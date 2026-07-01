---
title: Databricks 2026 Data+AI Summit 变革综述
description: 2026年Data+AI Summit上Databricks一次性发布18项新产品，将全量能力重构为"数据底座—治理语义—Agent平台—应用"四层架构，完成从"模型性能竞赛"到"AI Agent规模化受管控落地"的战略切换
aliases: [Databricks 2026, Databricks 四层架构, Data+AI Summit 2026, Databricks 范式重塑]
tags: [big-data, ai-agent, architecture, tool]
sources: [2026/06/30/从Databricks产品发布会看数据平台的演进方向.html, 2026/06/30/从Databricks产品发布会看-图片识别.md, 2026/06/30/Databricks 发布 LTAP：首个统一事务与分析处理的湖上架构.html, 2026/06/30/Databricks Data + AI Summit 2026 深度回顾：Genie One、Lakebase 与 Agent 全家桶.html, 2026/06/30/Genie 之后，Databricks 真正想抢的是企业 Agent 入口.html, 2026/06/30/Databricks Data + AI Summit 2026 重磅复盘：Databricks 正式迈入「AI 智能体湖仓」时代.html, 2026/06/30/Databricks 一次发了 18 件事：Lakehouse 下沉为底座，Agent Runtime 升到顶层.html, 2026/06/30/318| 企业级AGI：AI的真正战场.html]
created: 2026-06-30
updated: 2026-06-30
---

# Databricks 2026 Data+AI Summit 变革综述

2026 年 Data+AI Summit 上，Databricks 一次性发布 18 项新产品，完成了从"模型性能竞赛"到"**AI Agent 规模化受管控落地**"的战略叙事切换。

其演进主线是：**企业数据平台的角色正在发生根本性变化——从"支撑业务"走向"直接参与业务执行"，从"分析基础设施"进化为"AI Agent 运行基础设施"**。作为全球湖仓与 AI 融合领域的代表性厂商，Databricks 此次系统性架构重塑具有典型观察价值。

## 核心论点：AI 的问题是上下文问题

Databricks CEO Ali Ghodsi 在主题演讲中明确表示：

> "AI doesn't have an intelligence problem. It has a context problem."

AI 目前面临的主要问题是**上下文质量与受控访问**，而非模型智能本身。这一判断直接决定了 Databricks 2026 的产品布局重心——不在模型，而在治理、语义与 Agent 运行时基础设施。

## 峰会背景与财务数据

2026 年 6 月 15-18 日，旧金山 Moscone Center，超过 30,000 名参会者齐聚 Data+AI Summit 2026（DAIS 2026），150+ 国家线上同步直播。年度核心主题是 **Agentic Lakehouse（智能体湖仓）**。会前官宣的硬核财报：

| 指标 | 数据 |
|------|------|
| 年度 ARR | **54 亿美元**，同比增长 65% |
| AI 产品线收入 | 突破 **14 亿美元**，成为第一增长引擎 |
| 融资 | 完成 **70 亿美元**超大额融资 |

> ⚠️ 注：部分媒体提及"投后估值 1.34 万亿美元"，该数字异常偏高（Databricks 历史估值在百亿级），存疑，待官方核实。

2026 年被定义为"Data + AI 真正走向业务自动化的元年"。相比 2025 年的"搭好数据底座"，2026 年的核心是"实现业务全自动跑通"：

| 维度 | 2025 | 2026 |
|------|------|------|
| 核心主题 | 湖仓一体、向量检索、基础模型落地 | 智能体生产化、在线事务闭环、全栈治理 |
| 聚焦领域 | 数据 + 基础 AI | 自动化 + 业务落地 + 企业规模化 |
| 代表产品 | Delta Sharing、Vector Search | Agent Bricks、Lakebase、AI Search |
| 架构模式 | 湖仓底座 + 模型接入 | AI 智能体操作系统 |
| 落地阶段 | 试点探索、概念验证 | 规模化生产落地 |

## Ali Ghodsi 的"4C"叙事骨架

Day 1 Keynote 的叙事骨架是 Ali Ghodsi 抛出的"四个 C"，每个 C 对应具体产品落地：

| 关键词 | 含义 | 落地产品 |
|--------|------|---------|
| **Choice 选择** | 模型/框架/Agent harness 全开放 | Kimi、Grok（SpaceX 合作）、OpenAI、Anthropic、Gemini；LangGraph、CrewAI、Claude Code SDK、DSPy |
| **Control 控制** | 治理前置，Agent 在边界内运行 | [[databricks-agent-control-plane|Unity AI Gateway]]、Unity Catalog Glossary & Domains |
| **Cost 成本** | 全面去席位化、按用量计费 | Genie Pay-as-you-go、Lakehouse//RT 引导期 30% 折扣、Free Edition 升级 |
| **Context 上下文** | 把"上下文"做成一类新产品 | [[genie-ontology|Genie Ontology]]、[[customerlake|CustomerLake]] 的"Golden Context" |

Ali Ghodsi 在台上直言："This is going to get extremely expensive. We are just scratching the surface."——一个按用量计费的厂商 CEO 主动喊出"涨价"信号，背景是 Agent Bricks 上线一年客户已构建 100,000+ Agent、年消耗超 1 quadrillion（10¹⁵）tokens，企业 AI 的"用量曲线"正在跟"模型 benchmark 曲线"分道扬镳。

> ⚠️ **厂商口径警示**：本文及关联页面中的性能数字（如 84.5% 命中率、16x 提速、12,000 QPS、TCO 节省 80% 等）均为 Databricks 厂商或客户自报口径，部分基于厂商自选的内部 benchmark（如 28 题真实业务问题集），非第三方公开评测。宜按"宣传口径"阅读，待 GA 后客户真实生产数据验证。

## 整体架构：四层范式重塑

Databricks 将全量产品能力重构为"**数据底座层—治理/语义层—Agent 平台层—应用层**"四层架构（图1）。其中 **Unity AI Gateway** 与 **Unity Catalog** 共同构成统一的治理控制平面，置于 Agent 平台与数据底座之间，在运行时对模型调用、工具使用、成本与安全进行统一管控。

```
┌──────────────────────────────────────────────┐
│  应用层      Genie AI 业务助手 / 行业解决方案    │
│              （AI 驱动的业务应用与解决方案）      │
├──────────────────────────────────────────────┤
│  Agent 平台层  Agent Bricks / 工作流 / 多Agent协作 │
│                工具与连接器(MCP/API) / 运行时沙箱  │
│                可观测性评估 / Model Serving       │
│                开放框架兼容(LangChain/LlamaIndex) │
├──────────────────────────────────────────────┤
│  治理/语义层   Unity Catalog(统一数据与AI资产目录) │
│                Unity AI Gateway(策略/路由/安全/成本)│
│                语义层(业务术语/知识抽取)           │
├──────────────────────────────────────────────┤
│  数据底座层   Lakehouse 统一存储与计算            │
│              Delta Lake/Iceberg · Spark · HTAP   │
│              多云(AWS/Azure/GCP) · Finops        │
└──────────────────────────────────────────────┘
        平台级能力贯穿四层：安全与合规(SSO/IAM/加密/隐私)
```

四层架构的详细组件见下方各节。这一架构与 [[ai-native-data-platform-report|《AI原生数据平台研究报告》]] 提出的"统一底座是前置建设基础""治理与安全内嵌全流程""分层分步落地"等建设准则一致。

## 数据底座层：走向融合统一

Lakebase、Delta UniForm 与 Lakehouse/RT 共同构成 **LTAP（湖事务与分析处理，Lakehouse Transactional & Analytical Processing）** 架构范式——OLTP 与 OLAP 共享同一份对象存储中的开放格式数据，从根本上挑战"事务与分析必须分库"的传统认知。

| 产品/能力 | 定位 | 关键数据 |
|----------|------|---------|
| **Lakehouse/RT + Reyden 引擎** | 直接对受管控的 Delta Lake 和 Apache Iceberg 表运行实时分析；Reyden 是全新交互式查询引擎，向量+全文+SQL 混合查询毫秒响应，专为 AI 问答、智能体调用、实时分析提速 | 12000 QPS 压力下延迟稳定在 100ms 以内；小型工作负载响应低至 10ms |
| **Delta UniForm** | 统一计算路径，开放表格式（Delta、Iceberg）作为异构算力调度载体 | 验证开放表格式作为统一底座的可行性 |
| **Unity Catalog 原生 Iceberg 写入 GA** | 此前仅支持 Iceberg 读，本届开放完整读写；Delta + Iceberg 双格式原生统一治理，跨引擎无拷贝共享 | Databricks 面向开放中立数据生态最重要的升级，弱化厂商锁定 |
| **Zerobus** | 完全托管的无服务器推送式摄入 API | 直接将数据流式写入 Delta 表，无需中间消息总线 |
| **Lakebase Search / AI Search** | 向量与全文检索能力内嵌至 PostgreSQL 内核；AI Search 升级为向量+全文+结构化过滤三合一混合检索引擎 | 动摇独立向量数据库的工程必要性 |

LTAP 的完整技术拆解（HTAP/零ETL/LTAP 三方对比、传统架构七痛点、对数据工程师的五影响）见 [[ltap-architecture|LTAP 架构]]。LTAP 与向量检索内置化的实质，是宣告"专用单点组件"时代的终结——独立向量数据库、专用实时数仓等单点能力的工程必要性正在被快速稀释。

## 治理/语义层：向动态运行时延伸

### Unity AI Gateway

Unity AI Gateway 被官方定位为"**企业 AI 的治理解决方案（governance solution for enterprise AI）**"，提供四大能力：

1. **策略管控** — 策略执行、路由
2. **Token 预算** — 用量管控、成本分析/计费
3. **模型路由** — 模型调用管控
4. **运行时安全** — 安全防护、访问控制、Prompt 安全、PII 脱敏/审计

其核心突破在于将治理范围**从静态数据资产扩展至动态 Agent 运行时**——治理对象从"数据"扩展到"模型调用、工具使用、成本与安全行为"。详见 [[ai-governance]]。

### Genie Ontology

[[genie-ontology|Genie Ontology]] 以"人工定义＋自动生长"双轨制构建企业知识图谱，排名机制借鉴 Google PageRank 思路，通过权威性加权识别企业内最具权威的业务定义。据官方数据，该机制可将复杂业务首问命中率提升至 **84.5%**。通过 Unity Catalog 的强制权限继承，实现"安全内嵌"。

这是 [[ontological-semantic-layer|本体化语义层]] 在 Databricks 产品中的具体实例——把"表里的数据"翻译成"业务世界里的对象和变化"，让 Agent 在企业定义、审核、约束过的语义空间内工作。

## Agent 平台层与应用层：趋于一体化

| 产品 | 定位 | 关键数据/特征 |
|------|------|-------------|
| **Agent Bricks** | 托管式 Agent 平台，全生命周期管理 | 已承载超 **10 万个生产 Agent**，年 Token 消耗量超 **10¹⁵**（1+ 万亿）；新增 Kimi、Grok 模型支持，多框架支持 Claude Code SDK/LangGraph/Agno/CrewAI/OpenAI Agent SDK；通过 Lakebase 实现 Agent 托管内存 |
| **Omnigent** | 元编排层（meta-harness），Apache 2.0 开源 | 位于各类 Agent 框架之上，组合、控制、共享不同框架的 Agent。详见 [[omnigent-meta-harness]] |
| **Genie One** | 面向营销/财务/销售等业务团队的 AI 同事（AI Coworker） | 正式 GA，统一业务入口；SQL 锚定上下文（非 RAG，避免幻觉），跨工作流行动，全平台覆盖（Web/iOS/Android/Slack/Teams），MCP 工具集成 |
| **Genie Code** | 面向数据工程与 ML 工作流 | 自动写 SQL/Notebook/数据流水线/模型脚本，7×24 监控并自动修复管道故障与模型衰退 |

商业模式上，Genie 系列采用**按用量计费（Pay-as-you-go）**模式，每位用户每月赠送 150 DBU（约 $10.50），标志着 AI 原生时代"按调用价值计费"正在取代传统的席位制收费模式。

Databricks 这轮发布的战略意图——抢企业 Agent 运行时控制点（业务入口/语义上下文/运行时治理/工程执行/实时底座/模型生态六层拼图）——详见 [[databricks-agent-control-plane|Databricks Agent 控制面战略]]。其中 [[customerlake|CustomerLake]] 把 CDP 能力放进 lakehouse，验证 Agent 能否进入营销等业务流程；[[lakewatch-agent-siem|Lakewatch + Panther 收购]] 补齐安全湖仓，完成全栈闭环。

## 五大行业趋势

文章从单点变化中提炼出五条行业趋势：

1. **数据底座进入"范式融合期"** — LTAP 架构、向量检索内置化、实时分析直接基于开放格式数据，本质宣告"专用单点组件"时代终结。
2. **Agent 安全成为新型治理主战场** — MCP 工具调用管控、Prompt 注入、PII 泄露等风险的明确化治理，意味着数据治理从"边界防御"演进至"运行时纵深防御"，治理对象从静态资产扩展至动态行为。
3. **语义资产正在取代数据资产成为核心** — 从"黄金记录"到"黄金上下文"的转变，揭示 AI 时代企业核心资产形态的迁移——高质量、可被自动理解与持续生长的语义资产，将取代原始数据成为差异化竞争壁垒。
4. **Agent 生态竞争上移至编排治理层** — Omnigent 开源的实质，是将行业竞争从"谁的 Agent 框架更好"上移至"谁能统一调度与治理多框架 Agent"。平台型厂商正以元编排能力构建新的生态护城河。
5. **定价模式从席位制向用量制深度切换** — Genie 系列转向 Pay-as-you-go，"按调用价值计费"正在取代"按用户数计费"成为新基准。

## 竞争格局：9 条战线

这轮发布把挤压面铺到 9 个方向，按挤压强度分三档（基于发布内容的影响推演，非已发生的胜负）：

| 对手 | 挤压方式 | 强度 |
|------|---------|------|
| **Snowflake** | LTAP / Lakehouse//RT / CustomerLake 三线挤压；Cortex Agents vs Genie One 2026 H2 正面对撞 | 强挤压 |
| **Pinecone / Weaviate / Qdrant** | Lakebase Search 把 Hybrid Vector + Full-Text 内置 Postgres，蚕食独立向量库存在理由 | 强挤压 |
| **Splunk / QRadar / Sumo Logic** | [[lakewatch-agent-siem|Lakewatch + 拟收购 Panther]] 进军安全市场，TCO 优势 80% | 强挤压 |
| **Salesforce Data Cloud** | [[customerlake|CustomerLake]] 直打 Customer 360 + Activation 全链路 | 正面对撞 |
| **LangChain / CrewAI / AutoGen** | [[omnigent-meta-harness|Omnigent]] 作为 meta-harness 把框架降维成实现细节 | 蚕食 |
| **AWS / GCP** | Serverless GPU + 多节点训练对 SageMaker、Vertex AI 工程级挤压 | 工程级 |
| **Microsoft Fabric** | 表面深度合作，OneLake Federation 实际反向把 Fabric 收成"数据登记入口" | 反向利用 |
| **Tableau / Power BI** | Genie Code 内置 BI 迁移工具，正面"挖墙脚" | 蚕食 |
| **Adobe RT-CDP** | 被拉成 CustomerLake 启动伙伴而非竞品——"打不过就联盟" | 拉成生态伙伴 |

详细的厂商战略对照（Databricks/Microsoft Solara/Google Gemini Spark/Snowflake/Collibra）见 [[agent-infra-vendor-strategies]]。

## 与《AI原生数据平台研究报告》的四维对照

Databricks 2026 的变革路径可从四个维度梳理，详见 [[ai-native-data-platform-report]]：

| 维度 | Databricks 对应实践 | 验证的工程方向 |
|------|-------------------|--------------|
| **计算维度** | Lakehouse/RT 弹性扩缩容 + Delta UniForm 统一计算路径 | 开放表格式作为异构算力调度载体；训推一体与异构协同 |
| **供给维度** | Lakebase Search 统一向量与全文检索、Lakebase 支持 Agent 状态存储 | 全模态资产一体化供给；从"被动批量同步"走向"按需主动供给" |
| **治理维度** | Genie Ontology + Unity AI Gateway | 治理从静态数据资产延伸至 Agent 运行时；安全从边界防御走向全链路内生 |
| **消费维度** | Agent Bricks 规模化部署 + Genie One 统一业务入口 | 消费主体从单一人类用户扩展为"人＋Agent"双主体；价值链路闭环化 |

## 结语

Databricks 的变革集中呈现了上述演进主线：**数据平台正在超越传统分析工具的定位，真正成为 AI Agent 规模化运行的基础设施**。文章建议我国数据平台产业在借鉴海外经验的同时，立足本土业务刚需与信创适配要求，走出一条自主可控的 AI 原生数据平台建设之路。

## 相关页面

- [[databricks-genie]] — Databricks Genie 企业级 Data Agent（本次 Genie One/Code GA、Agent Bricks 规模数据的产品基础）
- [[databricks-agent-control-plane]] — Databricks Agent 控制面战略：六层控制点拼图与战略判断
- [[enterprise-agi-framework]] — 企业级 AGI 框架：SoI 四层堆栈 + 本体成熟度 1-9 级 + 数据层冰面化（理论制高点）
- [[genie-ontology]] — Genie Ontology：PageRank 式企业语义资产，84.5% 首问命中率（含风险视角与成熟度评估）
- [[omnigent-meta-harness]] — Omnigent 元编排层（meta-harness），开源多框架 Agent 调度
- [[ltap-architecture]] — LTAP 架构：存储层统一 OLTP/OLAP 的"第三条路"
- [[customerlake]] — CustomerLake：Agentic CDP，Agent 进入营销业务流程
- [[lakewatch-agent-siem]] — Lakewatch + Panther 收购：Agent 驱动的安全湖仓
- [[agent-infra-vendor-strategies]] — Agent 基础设施厂商战略对照（Databricks/Microsoft/Google/Snowflake）
- [[ai-native-data-platform-report]] — 《AI原生数据平台研究报告》6+2 架构与四维对照框架
- [[ontological-semantic-layer]] — 本体化语义层：Genie Ontology 的概念源头
- [[ai-governance]] — AI 治理：Unity AI Gateway 运行时治理的理论框架
- [[agentic-data-cloud]] — Google Agentic Data Cloud，同类"数据平台→Agent 基础设施"演进
- [[oceanbase-ai-database]] — OceanBase 湖库一体 AI 数据库，国内"数据库→Agent 数据底座"演进
- [[iceberg]] — Iceberg 开放表格式，Delta UniForm 统一的对象之一
