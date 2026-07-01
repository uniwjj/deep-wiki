---
title: Databricks Agent 控制面战略
description: Databricks 在 DAIS 2026 抢的不是单一模型或 BI 问答框，而是企业 Agent 运行时的控制点——业务入口、语义上下文、运行时治理、工程执行、实时底座、模型生态六层拼图，从"AI 数据底座"升级为"Agent 上下文与执行闭环"
aliases: [Databricks Agent 控制点, Agent 控制面, Databricks 战略, 企业 Agent 入口, control plane]
tags: [big-data, ai-agent, architecture, concept]
sources: [2026/06/30/Genie 之后，Databricks 真正想抢的是企业 Agent 入口.html, 2026/06/30/Databricks-DAIS2026-第二批四篇-图片识别.md]
created: 2026-06-30
updated: 2026-06-30
---

# Databricks Agent 控制面战略

Databricks 在 2026 Data+AI Summit 的主题是 "build apps and agents that work"。这轮发布不是一组 AI 功能，而是一张**控制面拼图**——Databricks 想抢的不是单一模型，也不是一个 BI 问答框，而是**企业 Agent 真正运行时需要的控制点**：Agent 到底相信什么上下文，能调用哪些工具，按谁的权限行动，怎样接入业务流程，出问题时如何治理和追踪。

## 六层控制点

Databricks 的原始主场是 Lakehouse / Spark / Delta / MLflow，以及围绕 Unity Catalog 的统一治理。这足以支撑"企业 AI 需要干净、可治理的数据"叙事。但 Agent 时代的问题不止是数据在哪里——更难的是业务用户是否愿意在 Databricks 里工作、Agent 能否理解企业内部相互冲突的指标定义、工具调用是否可控、AI 成本是否可管、执行动作是否能被追踪。

新的中枢是**企业上下文层**（[[genie-ontology|Genie Ontology]] / Unity Catalog），向外辐射六大控制点：

| 控制点 | 产品 | 作用 |
|--------|------|------|
| **1. 业务入口** | Genie One / Genie Agents / App Builder | 把问数、协作、应用和行动推到业务前台 |
| **2. 行业场景** | [[customerlake|CustomerLake]] / Profile Agents | 把业务入口落到营销和客户运营流程 |
| **3. 运行时治理** | [[databricks-2026-summit|Unity AI Gateway]] / Guardrails | 把模型、MCP、工具调用、成本纳入控制面 |
| **4. 工程执行面** | Genie Code / Lakeflow / MLflow | 让 Agent 进入开发、部署、监控和排障 |
| **5. 实时底座** | Lakehouse//RT / Streaming features | 为 Agent 提供低延迟上下文和持续反馈 |
| **6. 模型生态** | NVIDIA / OpenAI / Anthropic | 不押单一模型，押上下文和运行治理 |

**战略判断**：Databricks 不只想做 AI 数据底座，它在争 Agent 的上下文、工具和业务入口。

## Databricks 想补的两块短板

### 业务入口

Genie One 的价值不在于多了一个聊天入口，而在于 Databricks 终于开始正面争**业务工作入口**。过去 lakehouse 的主要用户是数据工程师、数据科学家和分析师；Genie 想进入销售、市场、财务、高管和一线业务流程。这是 Databricks 必须补的一块短板。

### 运行时治理

[[databricks-2026-summit|Unity AI Gateway]] 补另一块短板。传统数据治理主要管表、字段、模型、权限、血缘和审计。Agent 一旦开始调用工具，风险移动到运行时：prompt injection、数据泄露、危险工具调用、MCP tool poisoning、非人类账户权限扩张、模型调用成本失控。Databricks 把 CrowdStrike、Okta、Palo Alto、Zscaler 等安全、身份和可观测性伙伴拉进 Unity AI Gateway，说明它已不把治理只理解成 catalog，而是想推进到 Agent 执行动作发生的地方。

成本控制听起来偏 FinOps，但对 Agent 落地很关键——Agent 越自主，AI 支出越不可预测，没有成本边界，很多自动化工作流很难放进生产。

## Genie Ontology：最值得跟踪的产品假设

Databricks 这轮最重要的概念不是 Genie One，而是 [[genie-ontology|Genie Ontology]]。原因很简单：**如果没有稳定的企业上下文，Agent 入口越多，错误传播越快**。业务用户问"毛利为什么变了"，背后需要的不只是 SQL 生成，而是指标口径、时间口径、组织维度、认证 dashboard、数据质量、业务解释、权限边界和可追溯来源。

### 优势

Databricks 本身掌握数据工程、数据资产、MLflow、Unity Catalog 和 AI/BI 入口，可以看到很多上下文来源。它比单纯 BI 工具更接近数据生产链路，也比单纯治理工具更接近业务消费入口。Genie Ontology 用类似图排序的 authority ranking 处理企业内部多个定义之间的可信度。

### 风险（重要平衡视角）

企业里真正麻烦的业务语义，很多**并不天然存在于表、SQL、dashboard 和文档里**，而是在流程、组织、审批、Excel、邮件、会议和行业规则里。自动抽取可以加速起步，但**能否长期维护一个可信 ontology，仍然要看企业愿不愿意把业务定义、认证资产、owner、质量规则和变更流程持续纳入 Databricks**。

对 Genie Ontology 的判断要降一档：它已清楚表明 Databricks 要抢业务语义入口，但还不能证明它已解决企业语义治理。真正要看后续有没有更多产品文档、API 边界、客户案例和跨系统治理实践。

## 与 Snowflake 的差别：应用切入 vs 上下文切入

Databricks 和 Snowflake 都在讲企业 Agent，都想把业务 Agent 拉到自己的数据平台上，但切入口不同：

| | Snowflake | Databricks |
|---|-----------|-----------|
| 切入路径 | 从"业务应用怎么跑在数据云上"切入 | 从"Agent 怎么可信地使用数据平台里的上下文和工具"切入 |
| 起点 | Data Cloud、Horizon、Cortex、应用开发、行业伙伴、Accenture Context Graph | lakehouse、Unity Catalog、MLflow、Lakeflow、Feature Store、开放数据生态 |
| 核心逻辑 | 应用、行业方案、合作伙伴能力在 Data Cloud 上组合 | Genie/Ontology 让 Agent 读懂业务语义，Unity AI Gateway 管工具调用和运行风险 |
| 选型视角 | "已有数据云，想更快做业务应用/行业方案/流程 Agent" | "要让 Agent 可靠理解数据、继承权限、调用模型，并进入数据工程和 ML 工程流程" |

区别不是"业务应用在谁的平台上"——两家都想让业务应用靠近自己的数据平台。真正区别是：**Snowflake 从业务应用落地倒推数据云能力；Databricks 从数据与 AI 工程能力上推到业务入口**。这也是它们竞争越来越直接的原因：以前是 lakehouse vs cloud data warehouse，现在都在向上走——谁能让 Agent 读懂企业上下文、遵守权限、调用工具、进入业务流程，谁就更接近下一代数据平台入口。

## 与 OpenMetadata / Collibra 的差别：抢运行闭环

OpenMetadata、Collibra、DataHub 这类治理平台也在抢 Agent 上下文，但位置和 Databricks 不同：

- **OpenMetadata 1.13** — 把 glossary、ontology、RDF graph、hybrid search、data marketplace 和 MCP Services 组织成跨系统治理上下文层
- **Collibra** — 强调中立企业本体和跨平台治理，防止上下文被云厂商吸进各自平台

**Databricks 的强项不是中立，而是闭环**。它可以把数据生产、模型开发、业务问答、工具调用、运行时安全、成本控制、行业 Agent 和客户数据激活放在一个平台叙事里。它不一定适合成为所有企业的中立语义中枢，但会努力证明：如果数据、模型、语义、权限和执行都在 Databricks 里，Agent 落地会更快、更可控。

**对国内平台的警示**：治理平台如果只做目录，会被运行时平台绕开；运行时平台如果没有治理上下文，又会被安全和合规挡住。Agent 时代的控制点不在单一模块，而在"**上下文能不能进入执行闭环**"。

## 对国内数据平台的三层能力建议

Databricks 这轮发布对国内数据平台的参考，不是"也做一个 Genie"。要支撑 Agent 落地，至少补三层能力：

1. **业务上下文产品化** — 指标、术语、owner、认证报表、血缘、质量、权限、文档和业务规则，不能只散在几个页面里，要能被 Agent 稳定读取、排序、解释和追溯。否则问数入口越智能，越容易把错误口径放大。
2. **工具调用治理** — OpenAPI、MCP、SDK、CLI 只是入口。真正难的是 Agent 身份、用户代入权限、工具调用日志、审批、成本控制、失败回滚和安全拦截。生产 Agent 的风险发生在执行阶段，而不是 PPT 里的架构图里。
3. **数据平台自身的 Agent 化** — 数据开发、指标开发、质量检查、模型训练、特征工程、运维排障和补数，都是比"业务用户问一句话"更容易落地的场景。平台要把 job、pipeline、bundle、run history、log、lineage、quality result 和 deployment 做成连续工具面，而不是只开放零散 API。

如果只做一个聊天框，平台很快会被大模型应用层吸走入口；如果只做底层存算，平台又会被业务系统和 Agent workflow 平台绕开。Databricks 这轮真正值得看的地方，是它试图同时抓住底层数据、业务语义、运行治理和业务行动。

## 待验证的四件事

Databricks 的路线已清楚，但还没全部被证明。后续最值得看：

1. **Genie Ontology** 能否从自动抽取走向可治理维护——业务语义不是一次抽取就结束的资产，真正难的是变更、冲突、认证、owner 和跨部门协作。
2. **Unity AI Gateway** 能否成为跨模型、跨框架、跨 MCP 工具的真实运行时控制面——生产采用要看策略粒度、审计深度、性能开销和开发者体验。
3. **CustomerLake** 能否证明 lakehouse 内嵌 CDP 比独立 CDP 更高效——需要真实案例和迁移路径。
4. **Genie Code / Lakeflow / MLflow / AI Runtime** 能否把数据工程与 ML 工程 Agent 化——企业内部最先落地的 Agent，很可能不是高管问数，而是升级、修复、测试、训练、监控和成本优化。

## 相关页面

- [[databricks-2026-summit]] — Databricks 2026 四层架构变革综述
- [[genie-ontology]] — Genie Ontology，控制面的语义中枢（含本文的风险视角）
- [[customerlake]] — CustomerLake，控制面的行业场景层
- [[omnigent-meta-harness]] — Omnigent 元编排层，工程执行面的多框架调度
- [[ai-governance]] — AI 治理，Unity AI Gateway 运行时治理的理论框架
- [[ontological-semantic-layer]] — 本体化语义层，Genie Ontology 的概念源头
- [[agentic-data-cloud]] — Google Agentic Data Cloud，同类演进对照
- [[oceanbase-ai-database]] — OceanBase，国内"数据库→Agent 数据底座"演进
