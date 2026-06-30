---
title: Agent 基础设施厂商战略对照
description: 2026年5-6月三大科技巨头几乎同时发布 Agent 基础设施战略——Databricks 从数据与治理层切入、Microsoft Project Solara 从操作系统层切入、Google Gemini Spark 从搜索与助手层切入，路径截然不同
aliases: [Agent 基础设施对照, Databricks vs Microsoft vs Google, Project Solara, Gemini Spark, Agent 战略三方对标]
tags: [big-data, ai-agent, architecture, comparison]
sources: [2026/06/30/Databricks Data + AI Summit 2026 深度回顾：Genie One、Lakebase 与 Agent 全家桶.html, 2026/06/30/Genie 之后，Databricks 真正想抢的是企业 Agent 入口.html]
created: 2026-06-30
updated: 2026-06-30
---

# Agent 基础设施厂商战略对照

2026 年 5-6 月，三大科技巨头几乎同时发布了各自的 Agent 基础设施战略，但路径截然不同。理解三家切入路径的差异，是企业 Agent 平台选型的关键。

## 三方对标

| 公司 | 战略产品 | 切入路径 | 核心逻辑 |
|------|---------|---------|---------|
| **Databricks** | Genie Ontology + Unity AI Gateway | 数据与治理层 | Agent 质量 = 上下文质量，治理驱动 |
| **Microsoft** | Project Solara | 操作系统层 | Agent 原生 OS，嵌入 Windows 生态 |
| **Google** | Gemini Spark | 搜索与助手层 | 全天候 Agent 助理，搜索为核心 |

## Databricks：从数据与治理层切入

Databricks 的起点仍然是 lakehouse、Unity Catalog、MLflow、Lakeflow、Feature Store 和开放数据生态，然后用 [[genie-ontology|Genie/Ontology]] 让 Agent 读懂业务语义，用 [[databricks-2026-summit|Unity AI Gateway]] 管工具调用和运行风险，用 [[customerlake|CustomerLake]] 验证这些能力能不能进入营销和客户运营流程。

详见 [[databricks-agent-control-plane|Databricks Agent 控制面战略]]。核心赌注：**从数据和治理层切入 Agent 时代**——Unity Catalog 不仅是数据目录，更是 Agent 的"上下文操作系统"；Genie Ontology 不仅是知识图谱，更是 Agent 的"企业大脑"。

## Microsoft：从操作系统层切入

Microsoft Project Solara 走"Agent 原生 OS"路线，把 Agent 能力嵌入 Windows 生态。其优势是操作系统级的分发与嵌入能力，Agent 成为 OS 的一等公民。

## Google：从搜索与助手层切入

Google Gemini Spark 走"全天候 Agent 助理"路线，以搜索为核心。其优势是搜索入口和全天候助手场景的覆盖。

## Databricks vs Snowflake：应用切入 vs 上下文切入

除了上述三方对标，Databricks 与 Snowflake 的对比更具选型价值（两者都是数据平台厂商，竞争最直接）：

| | Snowflake | Databricks |
|---|-----------|-----------|
| 切入路径 | 从"业务应用怎么跑在数据云上"切入 | 从"Agent 怎么可信地使用数据平台里的上下文和工具"切入 |
| 起点 | Data Cloud、Horizon、Cortex、应用开发、行业伙伴、Accenture Context Graph | lakehouse、Unity Catalog、MLflow、Lakeflow、Feature Store |
| 选型视角 | "已有数据云，想更快做业务应用/行业方案/流程 Agent" | "要让 Agent 可靠理解数据、继承权限、调用模型，并进入数据工程和 ML 工程流程" |
| 方向 | 从业务应用落地**倒推**数据云能力 | 从数据与 AI 工程能力**上推**到业务入口 |

真正区别不是"业务应用在谁的平台上"——两家都想让业务应用靠近自己的数据平台。区别在于：**Snowflake 从应用倒推数据云，Databricks 从数据工程上推到业务入口**。以前是 lakehouse vs cloud data warehouse，现在都在向上走——谁能让 Agent 读懂企业上下文、遵守权限、调用工具、进入业务流程，谁就更接近下一代数据平台入口。

## 与治理平台（OpenMetadata / Collibra）的差别

OpenMetadata、Collibra、DataHub 这类治理平台也在抢 Agent 上下文，但位置不同：

- **OpenMetadata 1.13** — 把 glossary、ontology、RDF graph、hybrid search、data marketplace 和 MCP Services 组织成跨系统治理上下文层
- **Collibra** — 强调中立企业本体和跨平台治理，防止上下文被云厂商吸进各自平台

Databricks 的强项不是中立，而是**闭环**——把数据生产、模型开发、业务问答、工具调用、运行时安全、成本控制、行业 Agent 放在一个平台叙事里。

**对国内平台的警示**：治理平台如果只做目录，会被运行时平台绕开；运行时平台如果没有治理上下文，又会被安全和合规挡住。Agent 时代的控制点不在单一模块，而在"**上下文能不能进入执行闭环**"。

## 选型启示

| 企业问题 | 更顺的路线 |
|---------|-----------|
| 已有数据云，想更快做业务应用、行业方案、流程 Agent | Snowflake |
| 要让 Agent 可靠理解数据、继承权限、调用模型，进入数据工程和 ML 工程流程 | Databricks |
| 需要 OS 级 Agent 嵌入与分发 | Microsoft Solara |
| 需要搜索为核心的全天候助手 | Google Gemini Spark |
| 需要中立跨平台语义治理 | OpenMetadata / Collibra |

## 相关页面

- [[databricks-agent-control-plane]] — Databricks Agent 控制面战略，数据与治理层切入的详解
- [[databricks-2026-summit]] — Databricks 2026 四层架构变革综述
- [[agentic-data-cloud]] — Google Agentic Data Cloud，Google 侧的 Agent 数据架构实践
- [[ai-native-data-platform-report]] — 《AI原生数据平台研究报告》，覆盖 Databricks/Snowflake/Palantir 等全球头部厂商
- [[ai-governance]] — AI 治理，运行时治理层的理论框架
- [[ontological-semantic-layer]] — 本体化语义层，治理平台竞争的概念基础
