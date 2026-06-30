---
title: LTAP 架构
description: Databricks 推出的 Lake Transactional/Analytical Processing 架构——在存储层统一 OLTP/OLAP/流处理，单一数据副本、零 ETL 管道、统一治理，走 HTAP 与零 ETL 之外的"第三条路"
aliases: [LTAP, Lake Transactional/Analytical Processing, 湖事务与分析处理, LTAP 架构]
tags: [big-data, architecture, concept]
sources: [2026/06/30/Databricks 发布 LTAP：首个统一事务与分析处理的湖上架构.html, 2026/06/30/从Databricks产品发布会看数据平台的演进方向.html, 2026/06/30/Databricks-DAIS2026-第二批四篇-图片识别.md]
created: 2026-06-30
updated: 2026-06-30
---

# LTAP 架构

**LTAP（Lake Transactional/Analytical Processing，湖事务与分析处理）** 是 Databricks 在 2026 年 Data+AI Summit 推出的新一代数据处理架构，在**存储层**统一事务处理（OLTP）、分析处理（OLAP）、流处理与运营数据，实现**单一数据副本、零 ETL 管道、统一治理**。

Ali Ghodsi 将其定位为"等同于当年 Lakehouse 取代数据仓库"的架构变革：

> "过去十年，我们将现代数据栈的主要工作负载统一到了一个开放基础之上……现在，LTAP 消除了最后一道瓶颈。"

## 核心思想：第三条路

LTAP 的关键判断是——**不在同一个引擎内运行 OLTP 和 OLAP，而是在存储层统一数据**。这区别于此前业界的两种方案：

| 方案 | 统一方式 | 负载隔离 | 消除管道 | 开放标准 |
|------|---------|---------|---------|---------|
| **HTAP** | 同一引擎 | ❌ 无（两方面性能都打折） | ❌ | ❌ 专有 |
| **零 ETL** | 隐藏 CDC 管道 | ✅ 独立 | ⚠️ 只是隐藏，非真正消除 | 取决于后端 |
| **LTAP** | **存储层** | ✅ 严格隔离 | ✅ 真正消除 | ✅ Postgres + Iceberg + Delta |

HTAP 试图在同一引擎中运行两种负载，牺牲了工作负载隔离；零 ETL 只是隐藏了 CDC 管道而非真正消除。LTAP 走第三条路：事务与分析负载独立扩展、互不影响，但共享同一份存储层的数据。

## 三层构成

```
┌──────────────────────────────────────────┐
│  Lakebase（事务层）— 托管型 Postgres 兼容   │
│  直接向 Unity Catalog 写入开放格式          │
├──────────────────────────────────────────┤
│  Lakehouse（分析层）— 现有湖仓一体引擎       │
│  Reyden 引擎实时分析                       │
├──────────────────────────────────────────┤
│  Unity Catalog（统一治理层）— 元数据/权限/血缘 │
└──────────────────────────────────────────┘
```

- **Lakebase** 作为事务层——托管型 Postgres 兼容数据库，与 Databricks 平台深度集成，在对象存储上运行 Postgres-compatible 事务
- **Lakehouse** 作为分析层——现有的湖仓一体引擎，搭载 [[databricks-2026-summit|Reyden]] 引擎运行实时分析
- **Unity Catalog** 作为统一治理层——元数据、权限、血缘全链路统一

关键架构特性：存储层统一（Lakebase 直接写入 Iceberg/Delta）、消除 CDC（运营数据即时可查询）、工作负载隔离（事务与分析独立扩展）、开放标准（支持任何 Postgres 应用 + 任何 Iceberg/Delta 读取器）。

## 传统架构之痛：七个核心问题

过去二十年，企业数据架构几乎是固定模式：`应用数据库 → CDC 管道 → 落地层 → 转换管道 → Lakehouse/数仓 → 仪表盘/ML/AI Agents → 反向 ETL/API`。这带来七个问题：

1. **多副本** — 同一业务实体在多个系统中复制
2. **延迟** — 应用操作到分析可用之间存在时间差
3. **CDC 失败** — 管道断裂、Schema 漂移频繁发生
4. **对账困难** — 运营视图与分析视图反复对账
5. **反向 ETL 复杂** — 需要额外的 API 或同步层
6. **治理碎片化** — 不同系统有不同安全模型
7. **AI 数据陈旧** — AI Agent 基于过时或不完整的上下文运行

LTAP 的价值在于消除"仅仅因为运营和分析环境分离而存在的管道"。

## 对数据工程师的五个影响

Databricks 社区专家 Amit Kumar Singh 指出，**LTAP 不会消灭数据工程，而是让数据工程决策更加关键**：

1. **工作负载边界设计** — 并非所有数据都需要实时。月末财务对账可继续异步，实时欺诈检测则受益于 LTAP。数据工程师需判断哪些走 LTAP、哪些保持传统路径。
2. **数据契约（Schema Discipline）** — 当运营和分析共享同一存储层，应用侧的小 Schema 变更会直接影响分析报表、ML 特征、AI Agent 上下文、监管报告。Schema 纪律前所未有地重要。
3. **治理与访问控制** — 单一数据基座的价值取决于访问控制：谁可读写事务数据？哪些数据暴露给 AI Agent？敏感字段如何脱敏？访问如何审计？
4. **数据质量** — 源头脏数据仍是脏数据，靠近运营与分析交汇点的质量检查变得更重要。
5. **AI 操作的人为审批** — 当 AI Agent 从"回答问题"进化为"执行操作"：可信数据 → 验证上下文 → AI 建议 → 人工审查/策略检查 → 批准操作。

## 关键数据

| 指标 | 数据 |
|------|------|
| 支撑产品 | Lakebase（服务化 Postgres）+ Lakehouse |
| 已服务客户 | Block、Ensemble、Superhuman、Zillow 等上千家 |
| 日调度数据库数 | 1200 万次 |
| AI 应用产出提升 | ~50 倍于纯人工开发 |
| 全球客户 | 20,000+ 组织，含 70% 财富 500 强 |

Ensemble（医疗收入周期管理）在 Databricks 上管理 2PB+ 数据，其 CTO Grant Veazey 表示 LTAP 让 RCM-native AI 能实时访问运营数据。

## Lakebase 三项增强

LTAP 发布同时，Lakebase 推出三项重大增强：

- **跨云、跨地域灾备** — 为 Agent 驱动的关键业务提供弹性架构
- **Git 风格分支与快照** — 基于生产数据安全实验，Git 化工作流
- **自治数据库运维** — AI Agent 自动监控、检测瓶颈、建议索引、辅助恢复

## 边界与展望

LTAP 并非万能替代方案。企业仍将拥有 SaaS 应用、主机系统、第三方平台、遗留系统等无法迁移的数据源。LTAP 消除的是"一类仅仅因为运营和分析环境分离而存在的管道"。对于新建的 AI 应用和数据密集型企业，它提供了摆脱传统 `CDC → ETL → 反向 ETL` 复杂性的路径，为下一代 AI Agent 架构提供真正统一的数据基座。

## 相关页面

- [[databricks-2026-summit]] — Databricks 2026 四层架构变革，LTAP 是数据底座层的核心范式
- [[oceanbase-ai-database]] — OceanBase 湖库一体 AI 数据库，国内"事务+分析统一"同类演进
- [[iceberg]] — Iceberg 开放表格式，LTAP 存储层统一的对象之一
- [[agentic-data-cloud]] — Google Agentic Data Cloud，同类"数据平台→Agent 基础设施"演进
- [[ontological-semantic-layer]] — 本体化语义层，LTAP 之上的语义控制面
