---
ingested: 2026-10-10
wiki_pages: [easydata-ops-diagnosis]
source_url: https://km.netease.com/v4/detail/blog/267701
author: 罗霄
---

# 猛犸(EasyData)智能运维诊断工具

> 基于 Claude Code + 猛犸 OpenAPI + Spark History Server，实现大数据任务执行异常的自动化诊断与报告生成

---

## 1. 背景与痛点

在日常大数据运维中，我们经常面临以下问题：


| 痛点            | 传统方式                                   | 耗时     |
| --------------- | ------------------------------------------ | -------- |
| 基线破线告警    | 人工逐个点开任务查日志，逐条分析           | 30min~2h |
| 单任务变慢/失败 | 登录猛犸平台 → 找实例 → 查日志 → 看 SHS | 15~30min |
| 定位根因        | 依赖经验判断 OOM/倾斜/网络等问题           | 因人而异 |
| 输出诊断报告    | 手写文档汇总问题和建议                     | 30min+   |

**目标**：将上述流程自动化，**从告警到诊断报告 5 分钟内完成**。

- 猛犸基线诊断 [mammut-baseline-analysis  https://raven.netease.com/st/sdd/skills/detail?id=internal-1991](https://raven.netease.com/st/sdd/skills/detail?id=internal-1991)
- 猛犸任务诊断 mammut-task-analysis [https://raven.netease.com/st/sdd/skills/detail?id=internal-1990](https://raven.netease.com/st/sdd/skills/detail?id=internal-1990)
---

## 2. 整体架构

```
                          ┌─────────────────────────────────────────────────┐
                          │              Claude Code CLI                    │
                          │  (自然语言交互入口)                              │
                          └──────────┬──────────────────┬──────────────────┘
                                     │                  │
                    ┌────────────────┴──┐        ┌──────┴────────────────┐
                    │ 基线破线诊断 Skill │        │ 单任务诊断 Skill      │
                    │ mammut-baseline   │        │ mammut-task-analysis  │
                    │ -analysis         │        │                       │
                    └────────┬─────────┘        └──────┬────────────────┘
                             │  逐个调用                │
                             │  --json-only             │
                             └──────────┬───────────────┘
                                        │
                    ┌───────────────────┬┴──────────────────┬───────────────┐
                    ▼                   ▼                    ▼               ▼
            猛犸 OpenAPI          执行日志 API          Yarn API        SHS REST API
            (实例详情)            (节点日志)           (Application)   (Stage/SQL/Config)
                    │                   │                    │               │
                    └───────────────────┴────────┬───────────┴───────────────┘
                                                 │
                                        ┌────────┴────────┐
                                        │  诊断引擎        │
                                        │  (20 条规则      │
                                        │   + 8 条 SQL 规则)│
                                        └────────┬────────┘
                                                 │
                                        ┌────────┴────────┐
                                        │  HTML 报告生成   │
                                        │  + HTTP Server   │
                                        └─────────────────┘
```

### 2.1 核心组件


| 组件                 | 职责                                       | 文件                        |
| -------------------- | ------------------------------------------ | --------------------------- |
| **基线诊断 Skill**   | 解析基线告警，筛选变慢任务，编排多任务诊断 | `mammut-baseline-analysis/` |
| **单任务诊断 Skill** | 单个 execId 的深度诊断（日志 + SHS + SQL） | `mammut-task-analysis/`     |
| **诊断引擎**         | 12 条日志规则 + 8 条 SQL 规则的模式匹配    | Python 脚本内置             |
| **报告生成器**       | 自动生成可视化 HTML 报告并启动 HTTP 服务   | Python 脚本内置             |

---

## 3. 数据采集层

### 3.1 猛犸 OpenAPI

通过 AKSK 鉴权调用猛犸 OpenAPI 获取任务执行数据：

```
Base URL: http://easyopenapi-gy.service.163.org/openapi/easytaskops
鉴权方式: AKSK (sig = MD5(SK + timestamp))
```


| API                             | 用途                        | 关键字段                                     |
| ------------------------------- | --------------------------- | -------------------------------------------- |
| `GET /sched/v1/exec/detail/get` | 获取实例详情及所有节点      | `execJobs[].job, status, startTime, endTime` |
| `GET /log/v1/job/get`           | 读取节点执行日志            | `result.data`（单次最大 512000 字节）        |
| `GET /yarn/v1/applications`     | 获取关联的 Yarn Application | `applications[].applicationId`               |
| `POST /sched/v1/exec/list`      | 按任务名+日期查实例         | `result.list[0].execId`                      |

**API 字段映射注意点**（与直觉命名不同）：

```
API 字段         →  语义
job              →  jobId (节点 ID)
jobAliasName     →  jobName (节点名)
type             →  jobType (节点类型)
status           →  execStatus (执行状态)
attempt          →  attemptCount (重试次序，0-indexed)
```

### 3.2 Spark History Server REST API

**核心价值**：平台日志 API 只返回调度器轮询信息（FlowRunner 的 Yarn 状态轮询），**不含** Spark driver/executor 执行级日志。实际的 OOM、FetchFailed、IOException 等错误只能从 SHS 获取。

```
Base URL: https://hadoop-gy.netease.com/gateway/gy-cluster1/sparkhistory/api/v1/applications
```


| Endpoint                                                         | 用途           | 关键信息                                 |
| ---------------------------------------------------------------- | -------------- | ---------------------------------------- |
| `/{appId}/1/stages?status=failed`                                | 失败 Stage     | `failureReason`（实际错误堆栈）          |
| `/{appId}/1/stages`                                              | Stage 概览     | completed/failed/skipped 统计            |
| `/{appId}/1/environment`                                         | Spark 配置     | executor.memory, shuffle.partitions 等   |
| `/{appId}/1/sql/`                                                | SQL 执行信息   | SQL 源码 + 物理执行计划 + 算子级 Metrics |
| `/{appId}/1/stages/{stageId}/{attemptId}/taskList?status=FAILED` | 失败 Task 详情 | 故障节点 hostname/IP                     |

### 3.3 日志读取策略

```
第 1 轮: offset=0,      length=512000   → 读取日志头部
第 2 轮: offset=512000,  length=512000   → 读取日志中部（仅当第 1 轮接近上限时）
第 3 轮: offset=1024000, length=512000   → 读取日志尾部（仅当第 2 轮仍接近上限时）

单节点最多读取约 1.5MB，避免内存爆炸
```

---

## 4. 诊断引擎

### 4.1 日志模式匹配规则（12 条）

诊断引擎通过正则表达式匹配日志和 SHS 数据中的异常模式，按严重程度分级：

#### 严重（Critical）


| # | 类别              | 匹配模式（部分）                                           | 诊断结论                                   |
| - | ----------------- | ---------------------------------------------------------- | ------------------------------------------ |
| 1 | **OOM**           | `OutOfMemoryError`, `beyond physical memory limits`        | Executor/Driver 内存不足，被 YARN 强制终止 |
| 2 | **Executor 丢失** | `ExecutorLostFailure`, `Lost executor`, `Container killed` | Executor 被 YARN 杀死，通常是内存超限      |
| 3 | **磁盘空间不足**  | `No space left on device`, `DiskBlockManager Error`        | 本地磁盘满，无法写 Shuffle 文件            |

#### 高（High）


| # | 类别              | 匹配模式（部分）                                   | 诊断结论                                 |
| - | ----------------- | -------------------------------------------------- | ---------------------------------------- |
| 4 | **数据倾斜**      | `Speculative task launched`, `tasks pending`       | 部分 partition 数据量远超平均            |
| 5 | **Shuffle 溢写**  | `spilling in-memory map`, `Shuffle write: GB`      | 内存不足以容纳排序数据，频繁溢写磁盘     |
| 6 | **网络/连接问题** | `Connection refused`, `SocketTimeout`, `timed out` | 节点故障、Shuffle Service 断开或网络抖动 |

#### 中（Medium）


| #  | 类别              | 匹配模式（部分）                              | 诊断结论                            |
| -- | ----------------- | --------------------------------------------- | ----------------------------------- |
| 7  | **GC 压力**       | `Full GC`, `spent Nms on GC`                  | 频繁 GC 占用大量 CPU 时间           |
| 8  | **队列等待**      | submitTime 与 startTime 差 > 10min            | YARN 队列资源紧张                   |
| 9  | **Task 重试**     | `FetchFailed`, `Task failed N times`          | 节点失败重试导致耗时翻倍            |
| 10 | **慢 Task**       | `Speculative task launched`                   | 存在 straggler 拖慢整体进度         |
| 11 | **Hive 分区过多** | `numPartitions=NNNN`                          | 缺少分区过滤，导致 Metastore 查询慢 |
| 12 | **SQL 解析异常**  | `AnalysisException`, `Planning took too long` | SQL 复杂度过高，Catalyst 优化器超时 |

### 4.2 SHS 深度诊断

除了日志模式匹配，诊断引擎还从 SHS 的 `failureReason` 中进行**结构化错误分类**：

```
failureReason 解析流程:

IOException + "Connecting to" / "timed out"
  → Shuffle 网络故障（提取故障节点 hostname/IP）

FetchFailedException + "not registered"
  → Executor 注销（被 YARN 杀死或动态缩容移除）

OutOfMemoryError
  → OOM（提取内存使用详情）

FetchFailedException (通用)
  → Shuffle 数据拉取失败

requests outstanding when connection closed
  → Shuffle 连接异常关闭（提取故障节点）

其他
  → Stage 失败（保留原始错误信息）
```

**故障节点定位**：对于网络类问题，脚本会进一步查询失败 Task 列表，从 `errorMessage` 中提取故障节点的 hostname/IP 及出现次数，帮助精确定位问题机器。

### 4.3 SQL 执行分析规则（8 条，mammut-task-analysis 独有）

基于 SHS `/sql/` endpoint 返回的 **SQL 源码 + 物理执行计划 + 算子级 Metrics**，给出 SQL 层面的优化建议：


| #  | 类别             | 检测方式                                    | 严重度 |
| -- | ---------------- | ------------------------------------------- | ------ |
| S1 | **聚合爆炸**     | SQL 中`COUNT(DISTINCT)` >= 3 个             | 高     |
| S2 | **Shuffle 过大** | shuffle records written > 10 亿             | 高     |
| S3 | **内存溢写**     | spill size 累计 > 100 GiB                   | 高     |
| S4 | **多层 JOIN**    | SortMergeJoin >= 3 层                       | 中     |
| S5 | **Expand 膨胀**  | Expand 算子输出 > 10 亿行                   | 高     |
| S6 | **笛卡尔积**     | CartesianProduct 或 BroadcastNestedLoopJoin | 严重   |
| S7 | **大表全扫描**   | Scan 行数 > 100 亿                          | 中     |
| S8 | **聚合耗时**     | 聚合算子累计 > 10 小时                      | 高     |

**Metrics 解析**：脚本内置了 SHS Metrics 数值解析器，支持 `44,212,259,371`（逗号分隔）、`5.3 TiB`（带单位）、`82.99 h`（时间单位）等多种格式。

---

## 5. 两种诊断模式

### 5.1 单任务诊断

**触发方式**：

```
用户: 诊断任务 41170433
用户: 分析日志 播放指标 2026-04-22
```

**执行流程**：

```
1. 解析输入 → 获取 execId
2. 调用实例详情 API → 获取所有节点信息
3. 识别关键节点（Top 3 慢 + 失败 + 重试）→ 读取执行日志
4. 获取 Yarn Application → 查询 SHS（Stage/配置/SQL）
5. 运行 12 条日志规则 + 8 条 SQL 规则
6. 生成单任务诊断 HTML 报告
```

**报告包含**：

- 执行概览卡片（状态、总耗时、节点数、队列等待、问题数）
- 诊断结论（按严重程度排序的问题列表）
- 节点耗时分布（Top 15 慢节点条形图）
- 优化建议汇总（P0/P1/P2 分级）
- SHS 诊断详情（失败 Stage、Spark 配置）
- SQL 分析（SQL 源码 + 执行指标 + 物理执行计划）

### 5.2 基线破线诊断

**触发方式**：

```
用户: 基线破线了，风控14点基线，以下任务变慢：
- 播放指标统计 execId=41170433 耗时7.5h（历史均值2.2h）
- 风控特征计算 execId=41170500 耗时3.2h（历史均值1.1h）
```

**执行流程**：

```
1. 解析基线告警 → 生成 baseline_analysis JSON
2. 筛选 change_pct > 50% 的显著变慢任务
3. 按 execId 去重
4. 逐个调用单任务诊断（--json-only 模式）
5. 汇总所有诊断结果
6. 生成基线汇总 HTML 报告
```

**报告额外包含**：

- 基线概览卡片（告警线、破线、关联任务数、上游依赖数）
- 破线原因（变慢任务条形图：今日耗时 vs 历史均值 + 变化百分比）
- 各任务诊断详情（按任务分组的完整诊断）
- **共性问题分析**（跨任务出现的相同问题类别 → 怀疑集群级别问题）
- 全部任务耗时排名表

---

## 6. 诊断报告示例

### 6.1 单任务报告结构

```
┌─────────────────────────────────────────────────┐
│           猛犸任务执行诊断报告                     │
│  诊断时间 | 任务名 | execId | 业务日期             │
├─────────────────────────────────────────────────┤
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐  │
│  │SUCC  │ │7.5h  │ │12个  │ │2.1min│ │3个   │  │
│  │任务状态│ │总耗时 │ │节点数 │ │队列  │ │问题数 │  │
│  └──────┘ └──────┘ └──────┘ └──────┘ └──────┘  │
├─────────────────────────────────────────────────┤
│  诊断结论                                        │
│  ┃ [严重] Shuffle网络故障 — 节点: xxx            │
│  ┃   节点 host1 不可达（超时 9.5s）               │
│  ┃   建议: 检查故障节点、增大 memoryOverhead      │
│  ┃                                               │
│  ┃ [高] SQL优化-聚合爆炸 — 节点: yyy             │
│  ┃   SQL 中 5 个 COUNT(DISTINCT)，触发 Expand    │
│  ┃   建议: 拆分为独立子查询分别聚合               │
├─────────────────────────────────────────────────┤
│  节点耗时分布                                     │
│  节点A ████████████████████████████████ 4.2h     │
│  节点B ████████████████████░░░░░░░░░░░ 2.8h     │
│  节点C ████████░░░░░░░░░░░░░░░░░░░░░░ 1.1h     │
├─────────────────────────────────────────────────┤
│  优化建议汇总 (P0/P1/P2)                         │
├─────────────────────────────────────────────────┤
│  SHS 诊断详情 (Stage 失败 / Spark 配置)           │
├─────────────────────────────────────────────────┤
│  SQL 分析 (源码 / 执行指标 / 物理执行计划)        │
└─────────────────────────────────────────────────┘
```

### 6.2 优化建议分级


| 级别   | 含义     | 典型建议                                           |
| ------ | -------- | -------------------------------------------------- |
| **P0** | 必须修复 | 增大 executor.memory/memoryOverhead、检查故障节点  |
| **P1** | 强烈建议 | 启用 External Shuffle Service、拆分 COUNT DISTINCT |
| **P2** | 建议优化 | 错峰调度、增加 shuffle.partitions、开启推测执行    |

---

## 7. 关键设计决策

### 7.1 为什么需要 SHS？

```
平台日志内容（/log/v1/job/get）:
  [2026-04-22 08:00:05] yarnApplicationState RUNNING
  [2026-04-22 08:00:15] yarnApplicationState RUNNING
  [2026-04-22 08:00:25] yarnApplicationState RUNNING
  ...（只有 Yarn 状态轮询，无任何错误信息）
  [2026-04-22 15:30:00] yarnApplicationState FINISHED

SHS failureReason 内容:
  org.apache.spark.shuffle.FetchFailedException:
  java.io.IOException: Connecting to host1/10.x.x.x:7337
  failed in the last 9500 ms. Caused by:
  java.nio.channels.ClosedChannelException
  → 这才是真正的错误根因！
```

### 7.2 --json-only 模式

基线诊断需要批量调用单任务诊断。通过 `--json-only` 标志，单任务脚本只输出 JSON 到 stdout（进度信息走 stderr），基线脚本解析 JSON 后汇总：

```python
# 基线脚本调用方式
proc = subprocess.run([
    "python3", "mammut_task_analysis.py",
    "--exec-id", str(exec_id),
    "--json-only"
], capture_output=True, text=True, timeout=600)
result = json.loads(proc.stdout)
```

### 7.3 错误容忍

单个任务诊断失败不影响其他任务，报告中标注 `DIAG_ERROR`：

```python
try:
    result = call_task_analysis(exec_id, product, cluster)
except Exception as e:
    result = {"execStatus": "DIAG_ERROR", "all_issues": [], ...}
```

### 7.4 报告分发

生成 HTML 后自动启动轻量 HTTP 服务，输出内网 IP + 端口的可点击链接：

```python
# 自动获取内网 IP 和空闲端口
subprocess.Popen(["python3", "-m", "http.server", port, "--bind", "0.0.0.0"])
print(f"http://{local_ip}:{port}/{filename}")
```

团队成员可直接在浏览器中打开报告，无需传文件。

---

## 8. 使用示例

### 8.1 单任务诊断

```bash
# 方式1: 直接提供 execId
> 诊断任务 41170433

# 方式2: 任务名 + 日期
> 分析日志 播放指标统计 2026-04-22

# 方式3: 命令行直接运行
python3 mammut_task_analysis.py --exec-id 41170433 --product music_security --cluster hz11
```

### 8.2 基线破线诊断

```bash
# 方式1: 粘贴告警信息
> 基线破线了，风控14点基线，今天2026-04-22，以下任务变慢：
  - 播放指标统计 execId=41170433 耗时7.5h（历史均值2.2h）
  - 风控特征计算 execId=41170500 耗时3.2h（历史均值1.1h）

# 方式2: 提供已生成的 JSON 文件
> 基线诊断 baseline_analysis_292.json

# 方式3: 命令行直接运行
python3 mammut_baseline_analysis.py --baseline-json baseline_analysis_292.json
```

---

## 9. 常见问题诊断速查表


| 现象               | 常见原因                             | 首选优化                              |
| ------------------ | ------------------------------------ | ------------------------------------- |
| 任务耗时翻倍       | 数据倾斜 / Shuffle 网络故障导致重试  | 开启 AQE + External Shuffle Service   |
| 任务 OOM 失败      | executor.memory 不足                 | 增大 memory 到当前值 x1.5~2           |
| 多任务同时变慢     | 集群级别问题（节点故障/队列紧张）    | 检查共性问题分析章节                  |
| SQL 耗时极长       | 多个 COUNT DISTINCT 导致 Expand 膨胀 | 拆分为独立子查询                      |
| Stage 反复失败重试 | 网络抖动 / Executor 被杀             | 增大 memoryOverhead + network.timeout |
| 队列等待超 10 分钟 | YARN 资源紧张                        | 错峰调度 / 申请增加配额               |

---

## 10. Spark 常用调优参数速查


| 参数                                  | 默认值     | 作用                 | 调整建议                         |
| ------------------------------------- | ---------- | -------------------- | -------------------------------- |
| `spark.executor.memory`               | 1g         | Executor 堆内存      | OOM 时增大 x1.5~2                |
| `spark.executor.memoryOverhead`       | memory*0.1 | 堆外内存             | Executor 丢失时提到 20%~30%      |
| `spark.driver.memory`                 | 1g         | Driver 堆内存        | Driver OOM 时增大                |
| `spark.executor.cores`                | 1          | 每个 Executor 核数   | 内存压力大时减少                 |
| `spark.sql.shuffle.partitions`        | 200        | Shuffle 分区数       | 数据量大时增到 1000~5000         |
| `spark.sql.adaptive.enabled`          | false      | 自适应查询执行       | 建议开启                         |
| `spark.sql.adaptive.skewJoin.enabled` | false      | AQE 倾斜处理         | 数据倾斜时开启                   |
| `spark.speculation`                   | false      | 推测执行             | 有 straggler 时开启              |
| `spark.network.timeout`               | 120s       | 网络超时             | 网络问题时增大                   |
| `spark.shuffle.service.enabled`       | false      | 外部 Shuffle Service | 建议开启，防 Executor 退出丢数据 |

---

## 11. 两个工具的关系与分工


| 特性             | mammut-task-analysis<br>(单任务深度诊断) | mammut-baseline-analysis<br>(基线汇总诊断) |
| ---------------- | :--------------------------------------: | :----------------------------------------: |
| **适用场景**     |            单个任务变慢/失败            |                基线破线告警                |
| 平台日志分析     |                    Y                    |             Y（通过调用前者）             |
| SHS 深度分析     |                    Y                    |                     Y                     |
| SQL 源码分析     |                    Y                    |                     Y                     |
| 物理执行计划分析 |                    Y                    |                     Y                     |
| 算子级 Metrics   |                    Y                    |                     Y                     |
| 故障节点定位     |                    Y                    |                     Y                     |
| 诊断规则数       |             12 + 8(SQL) = 20             |                    同左                    |
| 共性问题分析     |                    -                    |                   **Y**                   |
| 跨任务建议去重   |                    -                    |                   **Y**                   |
| --json-only 模式 |            Y（供基线版调用）            |                     -                     |
| HTML 报告        |                单任务报告                |                基线汇总报告                |
| HTTP 服务        |                    Y                    |                     Y                     |

**核心关系**：`mammut-baseline-analysis` 是编排层，内部逐个调用 `mammut-task-analysis --json-only` 获取每个变慢任务的诊断结果，再汇总为基线报告。

**使用场景**：

- **单个任务变慢/失败** → `mammut-task-analysis`（深度诊断，含 SQL 分析）
- **基线破线** → `mammut-baseline-analysis`（批量诊断 + 共性分析 + 汇总报告）

---

## 12. 后续规划

- [ ]  接入更多集群（当前仅 hz11）
- [ ]  支持 MR 任务的 History Server 分析
- [ ]  历史趋势分析（多日对比）
- [ ]  告警自动触发诊断（基线破线 → 自动生成报告 → 推送通知）
- [ ]  诊断知识库积累（新增规则自动发现）
