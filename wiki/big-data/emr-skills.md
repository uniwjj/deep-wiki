---
title: EMR Skills
description: 阿里云 EMR AI Native 平台——从 Lakehouse 到 Agentic Lakehouse，三大子产品 Skills（Spark/StarRocks/集群）覆盖五大场景，多模态 AI Function + 混合检索，全栈 Serverless 免运维
aliases: [EMR Skills, EMR AI Native, EMR Agent, Agentic Lakehouse]
tags: [big-data, ai-agent, platform, tool]
sources: [2026/05/12/06. 基于 EMR Skills 实现智驾场景的自动化数据标注.pdf, 2026/05/12/DataWorks DataAgent分享录音.md, 2026/05/28/EMR Skills：从 Lakehouse 到 Agentic Lakehouse.pdf]
created: 2026-05-12
updated: 2026-05-29
---

# EMR Skills

EMR 定位为**面向 Agent 的数据与计算能力平台**，不再只是“集群产品”，而是对外提供三类核心能力。演讲人：董亚军（EMR 产品专家）。

## EMR AI Native 全景图

### 四层架构

```
应用场景层：智能数据分析 / 智能数据开发 / 智能运维管理 / 多模态数据处理
    ↓
AI 能力层：EMR AI 助手 / 智能开发助手 / AI Function / Agent Skills / 多模态处理引擎
    ↓
AI 基础设施层：百炼 Qwen 系列 / Token 管控 / 安全合规 / 推理优化
    ↓
计算引擎层：EMR on ECS/ACK / Serverless StarRocks / Serverless Spark
    ↓
数据底座层：OSS/NAS/CPFS / DLF Catalog / Paimon/Iceberg/Fluss
```

### AI 能力层详情

| 能力 | 功能 |
|------|------|
| **EMR AI 助手** | 实例管理、任务诊断优化、错误根因分析、健康巡检、成本优化 |
| **智能开发助手** | SQL 智能生成、SQL 方言迁移、代码生成、执行计划诊断 |
| **AI Function 体系** | 情感分析、文本翻译、实体识别、图像分析、文档理解、音频处理、时序预测、内容安全 |
| **Agent Skills** | 集群/实例管理、作业管理、弹性管理、资源管理、运维管理、SQL 执行、元数据查询、智能诊断 |
| **多模态处理引擎** | Embedding、批量模型推理、非结构化数据加工、全文检索、向量检索、混合检索 |

## EMR Skill 能力全景

三大子产品通过 MCP Skill 接入 AI 助手，覆盖集群管理、计算作业、分析数据库全链路运维。

### EMR on ECS 集群管理 Skill

`alibabacloud-emr-cluster-manage`

管理 5 种集群类型（HA/普通部署、存算分离/一体架构）的全生命周期：

| 功能 | 能力 |
|------|------|
| 生命周期管理 | 创建、状态查询、克隆、删除 |
| 节点组管理 | 5 种节点角色（MASTER/CORE/TASK 等）、新建节点组、手动扩缩容 |
| 弹性伸缩 | 定时策略与指标策略（CPU/内存/YARN 负载触发）、伸缩活动历史 |
| 运维诊断 | 集群与节点巡检、续费管理、创建失败诊断 |

### EMR Serverless Spark 工作空间管理 Skill

`alibabacloud-emr-spark-manage`

| 功能 | 能力 |
|------|------|
| 工作空间管理 | 创建、查询、删除工作空间，查询可用引擎版本 |
| 作业管理 | 提交 Spark 作业（JAR/PySpark/SQL）、查询状态与日志、取消作业 |
| Kyuubi 交互查询 | 创建/启停 SQL 网关、JDBC 连接、Token 管理、取消查询 |
| 资源队列管理 | CU 扩缩容、查询队列状态 |
| 会话集群 | 创建/启停/删除会话集群，支持交互式开发 |

### EMR Serverless StarRocks 管理 Skill

`alibabacloud-emr-starrocks-manage`

| 功能 | 能力 |
|------|------|
| 实例管理 | 创建（存算一体/分离）、查询状态、释放、恢复暂停 |
| 弹性扩缩容 | 节点/磁盘扩缩容、计算组节点增减，均支持预检 |
| 配置管理 | 配置修改（预检+回滚）、版本升级、SSL、付费转换 |
| 运维管理 | 重启实例/计算组/节点、操作历史查询 |
| 网络管理 | 网关增删改查、隔离、公网 SLB 开关 |

## Agent 驱动智驾图片打标

### 架构

```
控制编排层           计算引擎层           数据源层              模型服务层
DW DataAgent  →     Serverless Spark  →  DLF & OSS    →    Qwen3.6-plus
  Skills Library                          camera_front/
  · Spark Skills                          ├─ img_01.jpg
  · Kyuubi Skills                         ├─ img_02.jpg
                                          └─ ...
```

### 执行流程

1. **Action**：Agent 理解用户意图 → 生成 SQL 代码 → 提交任务
2. **Spark 执行**：解析 Prompt → `read_files` 读取 OSS 图片 → `ai_query()` 调用 Qwen3.6-plus（可指定模型如 `model='qwen3.6-plus'`）
3. **输出**：结构化标注结果 `["警车", "吊车"]`

使用 Serverless Spark 内置的 `read_file` 表函数直接读取 OSS 文件：

```sql
SELECT ai_query(prompt, read_file(path)) FROM read_file('oss://bucket/path/*.jpg');
```

整个流程简化为标准 ETL 流水线：读取 OSS 文件 → SQL 定义 Prompt → 调用 AI 函数 → 输出结构化结果。Spark 侧提供 `model` 参数切换模型（如 Qwen 3.5/3.6/embedding），支持 JPG/PNG 后缀过滤。

### 核心优势

- **Agent 原生集成**：一键添加 Skill，开箱即用
- **核心场景覆盖**：工作空间、作业、查询、资源四大管理场景
- **安全合规**：基于 RAM 权限体系，细粒度访问控制
- **Serverless 免运维**：无需管理基础设施，按需弹性扩缩容
- **生产级稳定性**：企业级可靠性保障，完善的容错与重试

## EMR Serverless StarRocks Skills 详细能力地图

StarRocks Skills 按用户意图拆分六大类，满足 DBA/SRE、数据工程师、分析师、开发者多角色需求：

### 领域知识（Assistant）

| Skill | 功能 |
|-------|------|
| `knowledge-qa-skill` | 文档查询、概念解释、功能了解 |
| `config-guide-skill` | 配置项含义和推荐值 |

### 开发管理（Assistant）

| Skill | 功能 |
|-------|------|
| `sql-assistant-skill` | 写 SQL、改 SQL、理解 SQL |
| `schema-design-skill` | 建表、分区分桶、索引设计 |
| `data-import-skill` | 数据导入方式选择和实施 |

### 性能分析（Assistant）

| Skill | 功能 |
|-------|------|
| `query-tune-skill` | 单条 SQL 执行瓶颈分析 |
| `workload-tune-skill` | 系统级资源和负载调优 |
| `workload-analysis-skill` | 系统负载统计（Top SQL、pattern、趋势） |

### 诊断（Assistant/Manage）

| Skill | 功能 |
|-------|------|
| `error-fix` | 无法归类领域的通用报错兜底 |
| `bug-search` | 搜社区已知 bug 和修复 |
| `health-diagnose` | 深度根因分析与修复建议 |
| `health-inspect` | 主动巡检、发现潜在风险 |

### 运维（Manage）

| Skill | 功能 |
|-------|------|
| `instance-manage` | 查看/管理实例信息 |
| `migration-execute` | 执行数据或集群迁移 |
| `cluster-upgrade` | 执行版本升级 |
| `backup-recovery` | 执行备份和恢复 |
| `access-control` | 用户权限、认证、访问策略 |
| `ha-setup` | 高可用配置和管理 |

### 数据分析（Assistant）

| Skill | 功能 |
|-------|------|
| `schema-explore` | 库表结构与字段探索（实例数据） |
| `data-profile` | 数据分布、质量、统计 |
| `anomaly-detect` | 数据层面异常发现 |

## StarRocks AI Native 四大核心能力

| 能力 | 状态 | 指标 |
|------|------|------|
| **AI Function × 11** | 已上线 | 一条 SQL 调用大模型，MySQL 协议 100% 兼容 |
| **AI 助手** | 已上线 | 问题主动发现，根因自动定位，运维效率从天级降至分钟级 |
| **Agent Skills** | 已上线 | 开放标准化运维 Skill，客户 Agent 即插即用 |
| **多模态混合检索** | 公测中 | 全文 + 向量 + 标量，一个引擎统一三种模态，<500ms |

## StarRocks 实战场景

### 场景一：金融实时风控 — 毫秒级交易异常检测

**痛点**：50 亿行交易表 × 2000 万用户画像 JOIN 超时、风控规则天天改需 DBA 手动调优。

**方案**：Skill 智能 SQL 调优 + 物化视图设计：
- 自动分析执行计划：发现 Shuffle JOIN 瓶颈 → 建议 Colocate/Broadcast
- 物化视图一键设计：滚动窗口预聚合近 10 笔交易特征
- 窗口函数即写即优化：LAG/LEAD/SUM OVER 实时计算交易特征

**效果**：8s → 200ms。

### 场景二：集群健康巡检 — 拯救运维小哥的发际线

**痛点**：几十个节点逐个检查、半夜告警翻日志翻到天亮、新同事只会重启大法。

**方案**：`/starrocks-manage` 一键全链路诊断，30 秒输出结构化巡检报告：节点状态 + Tablet 健康 + Compaction 积压，异常项高亮 + 风险等级 + 修复建议。

### 场景三：金融报表加速 — 告别 T+1，今天的数据今天看

**痛点**：MySQL 分库分表查不动聚合、每接一个新报表需求排期两周、领导早会看到的是"昨天的昨天"。

**方案**：Skill 智能 Schema 设计 + 入湖选型：
- 一句话建表：自动推荐 Primary Key 模型、按天分区、HASH(user_id) 分桶
- 入湖方式自动选型：MySQL → Flink CDC + Connector（< 5s 延迟）
- 物化视图替代宽表：MV 自动增量刷新，报表实时可查

### 场景四：MySQL → StarRocks 报表迁移

**痛点**：20 张报表逐表导出 DDL (10min) → 手动改写 StarRocks 语法 (30min × 20) → 编写迁移脚本 (20min) → 全量导入校验 (15min)，总计 2-3 小时。

**方案**：Skill 智能生成 StarRocks DDL + 自动比对：
- 读取 MySQL Schema 后自动生成最优 StarRocks DDL
- 根据业务特征自动生成分区裁剪、排序键、Colocate Join 等优化点
- AI 搞定网络配置等工程问题

## 从 Lakehouse 到 Agentic Lakehouse

Agent 时代对大数据平台提出三个新要求：
1. **更低门槛的调用方式**：从作业/SQL 配置 → API/能力调用
2. **更统一的能力抽象**：数据处理、查询分析、系统运维 → 统一对外抽象成能力调用
3. **更实时的响应能力**：支持 Agent 的即时决策与交互

EMR 作为 **Agent 的数据与计算能力平台**，将三类核心能力 Skill 化：**数据处理 Skill**（Spark）、**实时分析 Skill**（StarRocks）、**系统运维 Skill**（集群管理）。

已支持的 Agent 接入方式：DataWorks Agent / EMR Agent / Qoder / Claude Code / OpenClaw。

## EMR AI Native 展望

- EMR AI 助手全面对接 IM 应用，支持自定义推送和巡检范围
- Serverless StarRocks 全文检索 GA + 向量化检索 GA
- Serverless Spark 多模态处理 GA + 多模态算子市场 GA
- EMR AI 助手支持成本与资源优化

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 产品
- [[dataworks-2026-0528-xialiaori]] — 2026-05-28 虾聊日全部分享内容汇总
- [[maxcompute-skills]] — MaxCompute 同类 Skills 体系
- [[hologres-skills]] — Hologres Skills
- [[flink-skills]] — Flink Skills（Flink + EMR 联动）
- [[dataworks-stock-case]] — Agent 驱动数据处理的实践案例
- [[agent-mcp-protocol]] — MCP 协议原理
