---
title: 猛犸（EasyData）智能运维诊断工具——mammut-task-analysis 与 mammut-baseline-analysis
description: Claude Code + 猛犸 OpenAPI + Spark History Server 的大数据任务异常自动诊断 Skill 对——12 条日志规则 + 8 条 SQL 规则的诊断引擎、单任务/基线破线两种诊断模式、P0/P1/P2 建议分级，目标从告警到诊断报告 5 分钟内完成
aliases: [猛犸智能运维诊断, mammut-task-analysis, mammut-baseline-analysis, EasyData 诊断工具, 猛犸任务诊断, 基线破线诊断]
tags: [ai-agent, big-data, practice, tool]
sources: ["2026/10/10/Raven Cloude Skill 猛犸(EasyData)智能运维诊断工具.md"]
created: 2026-10-10
updated: 2026-10-10
---

# 猛犸（EasyData）智能运维诊断工具

> 来源：KM 文章《Raven Cloude Skill 猛犸(EasyData)智能运维诊断工具》（作者罗霄），基于 Claude Code + 猛犸 OpenAPI + Spark History Server 实现大数据任务执行异常的自动化诊断与报告生成。两个 Skill：`mammut-baseline-analysis`（基线诊断）+ `mammut-task-analysis`（单任务诊断）。

## 背景与目标

| 痛点 | 传统方式 | 耗时 |
|------|---------|------|
| 基线破线告警 | 人工逐个点开任务查日志，逐条分析 | 30min~2h |
| 单任务变慢/失败 | 登录猛犸平台 → 找实例 → 查日志 → 看 SHS | 15~30min |
| 定位根因 | 依赖经验判断 OOM/倾斜/网络等问题 | 因人而异 |
| 输出诊断报告 | 手写文档汇总问题和建议 | 30min+ |

**目标**：将上述流程自动化，**从告警到诊断报告 5 分钟内完成**。

## 整体架构

```
Claude Code CLI（自然语言交互入口）
  ├── 基线破线诊断 Skill（mammut-baseline-analysis）
  └── 单任务诊断 Skill（mammut-task-analysis）
        │ 逐个调用 --json-only
        ▼
数据源：猛犸 OpenAPI（实例详情）｜执行日志 API（节点日志）｜Yarn API（Application）｜SHS REST API（Stage/SQL/Config）
        ▼
诊断引擎（12 条日志规则 + 8 条 SQL 规则）
        ▼
HTML 报告生成 + HTTP Server
```

核心组件：**基线诊断 Skill**（解析基线告警、筛选变慢任务、编排多任务诊断）、**单任务诊断 Skill**（单个 execId 的深度诊断：日志 + SHS + SQL）、**诊断引擎**（模式匹配，Python 脚本内置）、**报告生成器**（可视化 HTML 报告 + HTTP 服务）。

## 数据采集层

### 猛犸 OpenAPI

通过 AKSK 鉴权（`sig = MD5(SK + timestamp)`），Base URL：`/openapi/easytaskops`。

| API | 用途 | 关键字段 |
|-----|------|---------|
| `GET /sched/v1/exec/detail/get` | 获取实例详情及所有节点 | `execJobs[].job, status, startTime, endTime` |
| `GET /log/v1/job/get` | 读取节点执行日志 | `result.data`（单次最大 512000 字节） |
| `GET /yarn/v1/applications` | 获取关联的 Yarn Application | `applications[].applicationId` |
| `POST /sched/v1/exec/list` | 按任务名+日期查实例 | `result.list[0].execId` |

**API 字段映射注意点**（与直觉命名不同）：`job` → jobId（节点 ID）、`jobAliasName` → jobName（节点名）、`type` → jobType（节点类型）、`status` → execStatus（执行状态）、`attempt` → attemptCount（重试次序，0-indexed）。

### Spark History Server REST API

**核心价值**：平台日志 API 只返回调度器轮询信息（FlowRunner 的 Yarn 状态轮询），**不含** Spark driver/executor 执行级日志。实际的 OOM、FetchFailed、IOException 等错误**只能从 SHS 获取**。

| Endpoint | 用途 | 关键信息 |
|----------|------|---------|
| `/{appId}/1/stages?status=failed` | 失败 Stage | `failureReason`（实际错误堆栈） |
| `/{appId}/1/stages` | Stage 概览 | completed/failed/skipped 统计 |
| `/{appId}/1/environment` | Spark 配置 | executor.memory、shuffle.partitions 等 |
| `/{appId}/1/sql/` | SQL 执行信息 | SQL 源码 + 物理执行计划 + 算子级 Metrics |
| `/{appId}/1/stages/{stageId}/{attemptId}/taskList?status=FAILED` | 失败 Task 详情 | 故障节点 hostname/IP |

**日志读取策略**（防内存爆炸）：第 1 轮 offset=0 读头部，第 2 轮 offset=512000 读中部，第 3 轮 offset=1024000 读尾部（各 512000 字节，仅当上一轮接近上限时继续），单节点最多读取约 1.5MB。

## 诊断引擎

### 日志模式匹配规则（12 条，按严重程度分级）

**严重（Critical）**：OOM（`OutOfMemoryError`、`beyond physical memory limits`→Executor/Driver 内存不足被 YARN 强制终止）；Executor 丢失（`ExecutorLostFailure`、`Container killed`）；磁盘空间不足（`No space left on device`→本地磁盘满无法写 Shuffle 文件）。

**高（High）**：数据倾斜（`Speculative task launched`、`tasks pending`）；Shuffle 溢写（`spilling in-memory map`、`Shuffle write: GB`）；网络/连接问题（`Connection refused`、`SocketTimeout`、`timed out`）。

**中（Medium）**：GC 压力（`Full GC`、`spent Nms on GC`）；队列等待（submitTime 与 startTime 差 >10min→YARN 队列资源紧张）；Task 重试（`FetchFailed`、`Task failed N times`）；慢 Task（straggler 拖慢整体）；Hive 分区过多（`numPartitions=NNNN`→缺分区过滤）；SQL 解析异常（`AnalysisException`、`Planning took too long`→Catalyst 优化器超时）。

### SHS 深度诊断（failureReason 结构化错误分类）

`IOException + "Connecting to"/"timed out"` → Shuffle 网络故障（提取故障节点 hostname/IP）；`FetchFailedException + "not registered"` → Executor 注销；`OutOfMemoryError` → OOM；`FetchFailedException`（通用）→ Shuffle 数据拉取失败；`requests outstanding when connection closed` → Shuffle 连接异常关闭；其他 → Stage 失败（保留原始错误信息）。**故障节点定位**：进一步查询失败 Task 列表，从 `errorMessage` 提取故障节点 hostname/IP 及出现次数。

### SQL 执行分析规则（8 条，mammut-task-analysis 独有）

基于 SHS `/sql/` endpoint 的 SQL 源码 + 物理执行计划 + 算子级 Metrics：

| # | 类别 | 检测方式 | 严重度 |
|---|------|---------|--------|
| S1 | 聚合爆炸 | SQL 中 `COUNT(DISTINCT)` >= 3 个 | 高 |
| S2 | Shuffle 过大 | shuffle records written > 10 亿 | 高 |
| S3 | 内存溢写 | spill size 累计 > 100 GiB | 高 |
| S4 | 多层 JOIN | SortMergeJoin >= 3 层 | 中 |
| S5 | Expand 膨胀 | Expand 算子输出 > 10 亿行 | 高 |
| S6 | 笛卡尔积 | CartesianProduct 或 BroadcastNestedLoopJoin | 严重 |
| S7 | 大表全扫描 | Scan 行数 > 100 亿 | 中 |
| S8 | 聚合耗时 | 聚合算子累计 > 10 小时 | 高 |

脚本内置 Metrics 数值解析器，支持 `44,212,259,371`（逗号分隔）、`5.3 TiB`（带单位）、`82.99 h`（时间单位）等格式。

## 两种诊断模式

### 单任务诊断（mammut-task-analysis）

**触发**：`诊断任务 41170433` 或 `分析日志 播放指标 2026-04-22`。**流程**：解析输入获取 execId → 实例详情 API 取所有节点 → 识别关键节点（Top 3 慢 + 失败 + 重试）读日志 → 获取 Yarn Application 查 SHS → 运行 20 条规则 → 生成单任务 HTML 报告。

**报告包含**：执行概览卡片（状态/总耗时/节点数/队列等待/问题数）、诊断结论（按严重程度排序）、节点耗时分布（Top 15 慢节点条形图）、优化建议汇总（P0/P1/P2）、SHS 诊断详情（失败 Stage、Spark 配置）、SQL 分析（源码/执行指标/物理执行计划）。

### 基线破线诊断（mammut-baseline-analysis）

**触发**：粘贴基线告警（任务名 + execId + 今日耗时 vs 历史均值）。**流程**：解析告警生成 baseline_analysis JSON → 筛选 change_pct>50% 的显著变慢任务 → 按 execId 去重 → 逐个调用单任务诊断（`--json-only` 模式）→ 汇总所有结果 → 生成基线汇总 HTML 报告。

**报告额外包含**：基线概览卡片（告警线/破线/关联任务数/上游依赖数）、破线原因（变慢任务条形图：今日 vs 历史均值 + 变化百分比）、各任务诊断详情、**共性问题分析**（跨任务出现相同问题类别 → 怀疑集群级别问题）、全部任务耗时排名表。

### 优化建议分级

| 级别 | 含义 | 典型建议 |
|------|------|---------|
| **P0** | 必须修复 | 增大 executor.memory/memoryOverhead、检查故障节点 |
| **P1** | 强烈建议 | 启用 External Shuffle Service、拆分 COUNT DISTINCT |
| **P2** | 建议优化 | 错峰调度、增加 shuffle.partitions、开启推测执行 |

## 关键设计决策

- **为什么需要 SHS**：平台日志内容只有 `yarnApplicationState RUNNING/FINISHED` 轮询（无任何错误信息），SHS 的 `failureReason` 才有真正的错误根因（如 `FetchFailedException: Connecting to host1...failed in the last 9500 ms`）
- **`--json-only` 模式**：基线诊断批量调用单任务诊断时，单任务脚本只输出 JSON 到 stdout（进度走 stderr），基线脚本解析后汇总
- **错误容忍**：单个任务诊断失败不影响其他任务，报告中标注 `DIAG_ERROR`
- **报告分发**：生成 HTML 后自动启动轻量 HTTP 服务（`python3 -m http.server --bind 0.0.0.0`），输出内网 IP + 端口可点击链接，团队成员浏览器直接打开

## 速查表

**常见问题**：任务耗时翻倍（倾斜/Shuffle 网络故障重试→AQE + External Shuffle Service）；任务 OOM 失败（executor.memory 增大到 x1.5~2）；多任务同时变慢（集群级问题，看共性问题分析）；SQL 耗时极长（多个 COUNT DISTINCT 导致 Expand 膨胀，拆独立子查询）；Stage 反复失败重试（增大 memoryOverhead + network.timeout）；队列等待超 10 分钟（错峰调度/申请配额）。

**Spark 常用调优参数**：executor.memory（OOM 时 x1.5~2）、executor.memoryOverhead（Executor 丢失时提到 20%~30%）、driver.memory、executor.cores、sql.shuffle.partitions（数据量大时增到 1000~5000）、sql.adaptive.enabled（建议开启）、sql.adaptive.skewJoin.enabled（倾斜时开启）、spark.speculation（有 straggler 时开启）、network.timeout、shuffle.service.enabled（建议开启防 Executor 退出丢数据）。

## 两个工具的关系

`mammut-baseline-analysis` 是**编排层**，内部逐个调用 `mammut-task-analysis --json-only` 获取每个变慢任务的诊断结果，再汇总为基线报告（额外提供共性问题分析与跨任务建议去重）。使用场景：单个任务变慢/失败 → 单任务诊断（含 SQL 分析）；基线破线 → 基线诊断（批量 + 共性分析 + 汇总报告）。

**后续规划**：接入更多集群（当前仅 hz11）、支持 MR 任务 History Server 分析、历史趋势分析（多日对比）、告警自动触发诊断（基线破线→自动报告→推送通知）、诊断知识库积累（新增规则自动发现）。

## 相关页面

- [[bigdata-ops-agent-practice]] — 大数据运维 Agent 实践：同为运维诊断智能化（规则先行 + 大模型辅助），本文是其"单平台 Skill 化"的轻量形态
- [[raven-sdd-data-development]] — 同团队 SDD 数据开发：mammut-task-analysis 作为其性能验证环节的领域 Skill
- [[data-task-increment-optimization]] — 同团队配套实践：调用本工具做步骤 8 性能验证
- [[data-dev-skills-automation]] — 数仓开发 Skills 自动化：同为平台操作 Skills 封装的同类实践
- [[log-diagnosis-skill]] — 日志诊断 Skill：同为"日志模式匹配 + 诊断"的 Skill 思路
- [[agent-skills-system]] — Skills 体系机制
