---
title: Google Agentic Data Cloud（智能体数据云）
description: Google Cloud 的 AI 原生数据架构——将企业数据平台从静态仓库演进为动态推理引擎，关闭"思考"与"行动"之间的鸿沟
aliases: [Agentic Data Cloud, 智能体数据云, System of Action, Google 智能体数据架构]
tags: [big-data, ai-agent, architecture, concept]
sources: [2026/06/15/What's New in the Agentic Data Cloud - Google Cloud Blog.html]
created: 2026-06-15
updated: 2026-06-15
---

# Google Agentic Data Cloud

> 2026-04-23 Google Cloud Next 官方博客，Andi Gutmans（VP/GM）与 Yasmeen Ahmad（MD）联合发布。定位：从 System of Intelligence 到 System of Action 的代际跃迁。

## 核心定义

Agentic Data Cloud 是 AI 原生的数据架构，将企业数据平台从**静态存储库**演进为**动态推理引擎**。上一代"智能系统"仅为人规模构建，Agentic Data Cloud 是为 **Agent 规模**构建的行动系统。

关键区别：
- **System of Intelligence**：回答人类提出的问题
- **System of Action**：Agent 自主感知、推理、行动，关闭"思考"与"行动"之间的鸿沟

## 老旧架构的四大结构性失败

在遗留技术栈上扩展 Agent 会暴露：

1. **碎片化治理**（Fractured Governance）— Agent 跨系统访问时权限与语义不一致
2. **信任鸿沟**（Trust Gap）— Agent 不理解企业的业务定义（如"利润率"），被迫猜测
3. **推理断裂**（Broken Reasoning Loops）— 上下文不完整导致 Agent 推理链中断
4. **成本螺旋上升**（Cost Spiral）— 手动维护数据集成与语义层的成本随 Agent 数量指数增长

## 三大创新支柱

### 1. 通用上下文引擎（Universal Context Engine / Knowledge Catalog）

核心思想：**AI 的智能程度取决于其上下文的丰富程度**。Dataplex Universal Catalog 升级为 Knowledge Catalog，通过聚合、持续丰富、搜索三层框架映射业务语义。

| 层次 | 能力 | 关键细节 |
|------|------|---------|
| **聚合（Aggregation）** | 跨平台上下文汇聚 | Google Cloud 原生 + 第三方目录与应用（Palantir、Salesforce Data360、SAP、ServiceNow、Workday，Preview）；LookML Agent 自动化业务逻辑；BigQuery Measures 原生嵌入 |
| **持续丰富（Continuous Enrichment）** | 自动语义标注 | 分析组织使用日志、后台数据画像；Smart Storage 自动标记图片/PDF；Gemini 自动生成缺失 Schema、映射复杂关系 |
| **搜索与检索（Search & Retrieval）** | 混合语义+词法搜索 | Google Search 技术栈（语义匹配+词法匹配+ML 重排序）；访问控制感知搜索（IAM 原生实施，Agent 只能检索和操作已授权的资产） |

基于 Knowledge Catalog 的 **Deep Research Agent**（Preview）：跨 BigQuery、内部文档和 Web 资产的多步推理，带引用和精度保障。

### 2. Agentic-First 从业者体验

数据从业者角色演进为 **Agent 编排者**。核心载体是 [[google-data-agent-kit|Data Agent Kit]]。

| 组件 | 状态 | 说明 |
|------|------|------|
| **Data Engineering Agent** | GA | 带治理的流水线转换 |
| **Data Science Agent** | GA | 从数据 wrangling 到训练的完整模型生命周期 |
| **Database Observability Agent** | Preview | 7×24 根因分析与自动修复 |
| **Conversational Analytics** | GA（BigQuery/Looker）| 自然语言到 SQL，内嵌于 Gemini Enterprise |
| **MCP 协议支持** | GA/Preview | BigQuery、Cloud SQL、AlloyDB GA；Spanner、Looker Preview；复用现有 IAM 和 VPC 控制 |

### 3. AI 原生跨云 Lakehouse

消除数据孤岛，为 Agent 提供开放、低延迟的跨云数据访问。

| 能力 | 说明 |
|------|------|
| **Cross-Cloud Interconnect + Iceberg REST Catalog** | 跨 AWS/Azure 的专用私有网络与开放 catalog，最小化出站成本 |
| **双向联邦（Bi-Directional Federation, Preview）** | 引擎直接读取 Databricks Unity Catalog、Snowflake Polaris、AWS Glue Data Catalog（通过 Iceberg REST） |
| **Spanner Omni（Preview）** | Spanner 引擎跨云、本地、本地环境运行 |
| **Lakehouse Federation for AlloyDB（Preview）** | 零 ETL 同步，实时访问操作数据 |

## 性能与规模

- **Lightning Engine for Apache Spark**：相较专有替代方案 2x 性价比
- **Managed Lustre**：最高 10 TB/s 吞吐
- **Bigtable In-Memory Tier**：亚毫秒级读取，消除独立缓存层
- **BigQuery Fluid Scaling**：自动扩缩工作负载成本降低最多 34%

## 客户案例

| 客户 | 场景 | 效果 |
|------|------|------|
| **Vodafone** | 数百个客服 Agent | 预计每年节省数百万欧元 |
| **American Express** | 核心本地数仓 + 数百应用迁至 BigQuery | 可信 Agentic 商业规模 |
| **Virgin Voyages** | 1,000+ 专用 Agent（如大规模行程重新预订） | 从 6 小时缩短至 11 分钟 |

## 趋势分析

Agentic Data Cloud 代表了云数据平台的下一代范式：

1. **从被动到主动**：不再等人类查询，Agent 自主发现、推理、行动
2. **语义层成为核心基础设施**：Knowledge Catalog 不只是元数据管理，而是 Agent 的"世界模型"
3. **MCP 成为数据连接标准**：跨 BigQuery、Spanner、AlloyDB、Cloud SQL、Looker 的统一协议
4. **跨云互操作**：Iceberg REST Catalog 作为开放标准打破云边界
5. **数据从业者角色转型**：从"写代码的人"变为"定义意图的人"——意图驱动工程

## 相关页面

- [[google-data-agent-kit]] — Agentic Data Cloud 战略中"Agentic-First 体验"的核心落地产品
- [[agentic-skills]] — Agentic Skills 概念，Data Agent Kit 的核心抽象单元
- [[agentic-analytics-anthropic]] — Anthropic 的自服务分析平台，类似的 Agent 化数据分析思路
- [[agent-mcp-protocol|MCP]] — MCP 协议，Agentic Data Cloud 的统一连接标准
- [[iceberg]] — Apache Iceberg，跨云 Lakehouse 的开放表格式基础
