---
title: Agent Context 的三重支柱：开放语义、Ontology 与统一元数据
description: 堵俊平（Datastrato）在 DACon 2026 的演讲——Agent 从 Answer、Reason 到 Act 演进时暴露四类上下文缺口，需要统一元数据、开放语义层、业务本体三重支柱共同支撑，并由治理贯穿；落地路径是 Top-down 与 Bottom-up 结合的混合闭环
aliases: [Agent Context 三重支柱, Agent Context 的三重支柱, 开放语义 Ontology 元数据, 三重支柱, Datastrato Agent Context]
tags: [ai-agent, big-data, architecture, concept]
sources: [2026/10/29/2026DACon北京站/主论坛/堵俊平-开放语义、Ontology 与元数据：Agent Context 的三重支柱.pdf]
created: 2026-10-29
updated: 2026-10-29
---

# Agent Context 的三重支柱

> 核心主张：**当 Agent 开始参与决策和执行，企业需要为它提供可理解、可追溯、持续更新的上下文。** 访问能力只是起点；任务越接近真实业务，错误上下文的影响越大。

来源：DACon 2026 北京站主论坛，堵俊平（Datastrato CEO）《开放语义、Ontology 与元数据：Agent Context 的三重支柱》。演讲把企业 Agent 的上下文基础设施拆成「资产在哪里」「数据意味着什么」「对象如何关联并受什么约束」三个问题，分别对应统一元数据、开放语义层与业务本体。

## 背景与问题

### Agent 任务演进的三阶段

演讲用一条递进链条描述 Agent 任务：访问能力只是起点；任务越接近真实业务，错误上下文的影响越大。

| 阶段 | 任务 | 需要解决的问题 | 对应上下文类型 |
|------|------|----------------|----------------|
| 01 回答问题 Answer | 找到相关信息与数据 | 能否访问 | Data access |
| 02 推理决策 Reason | 理解定义、关系与适用条件 | 如何理解 | Business meaning |
| 03 执行操作 Act | 检查权限、规则与操作前提 | 什么行动成立 | Operational constraints |

三阶段在图中以箭头相连，构成同一条任务链：Answer 对应 Data access，Reason 对应 Business meaning，Act 对应 Operational constraints。原文的判断是——**任务越接近真实业务，错误上下文的影响越大**。

### 一个收入问题，暴露四种上下文缺口

演讲用一个示意案例展开：同一个问题「上季度按产品类别统计的收入是多少？」，在四个系统里得到四种不同答案。

| 系统 | 计算方式 | 隐含假设 | 暴露的缺口 |
|------|----------|----------|------------|
| BI Dashboard | `SUM(order_total)` | 含税费与运费 | 口径 Definition |
| Notebook | `SUM(qty × unit_price)` | 未扣折扣与退货 | 净额 Netting |
| Finance Model | 按 ship date 确认 | 退款另行处理 | 时间 Time |
| AI Agent | 选择首先发现的表 | 关联方式每次推断 | 关联 Join |

四个系统都能访问同一份数据，却给出了不同的业务答案。演讲由此给出结论：**相同的数据入口，并不保证相同的业务答案**。要得到一致答案，上下文至少要覆盖四件事：

> Business definition + time grain + relationship + evidence

原文在此页标注「示意案例」，未说明来自客户实测。

## 方案/架构

### 三重支柱：资产、含义与业务关系

| 支柱 | 回答的问题 | 覆盖内容 |
|------|------------|----------|
| Unified Metadata 统一元数据 | 数据在哪里，如何访问？ | 资产身份 · 结构 · 来源 · 接口 |
| Open Semantics 开放语义层 | 数据在业务上意味着什么？ | 指标 · 维度 · 口径 · 定义 |
| Ontology 业务本体 | 对象如何关联，受到哪些约束？ | 实体 · 关系 · 概念 · 约束 |

演讲把三者放在一条从「Business intent」到「真实数据与受治理执行」的链条上：业务意图要先被翻译成业务含义，再落到真实资产，最后才能在受治理的前提下执行。

三者之上还有一层横向能力：

> **Governance 贯穿三者**：身份权限、版本、校验、审计与反馈。

后面「治理：业务理解与操作权限的衔接」一节会展开它如何把「定义是否正确」和「当前调用者能否安全使用」接在一起。

### 统一元数据：异构资产的共同基础

演讲用 Apache Gravitino 作为统一元数据层的代表实现。它接入四类异构资产：

| 资产类型 | 代表系统 |
|----------|----------|
| Lakehouse | Iceberg、Hive |
| Database | MySQL、PostgreSQL |
| Streaming | Kafka、Events |
| Files & AI | Fileset、Model |

Gravitino 在中间提供三项统一能力：

- **Stable identity 统一标识**：给不同系统的资产一个稳定身份
- **Structure & lineage 结构与血缘**：描述结构并记录血缘
- **Access interface 访问接口**：提供统一的访问入口

向上，这层统一元数据支撑三类消费者：

- **Semantic model**：定义映射到 `orders` / `items` / `products`
- **Ontology**：概念绑定到具体字段与资产
- **Agent retrieval**：用 Domain 缩小搜索范围

演讲特别强调统一的边界：

> 统一的是**资产身份与访问入口**，不是把所有系统搬到同一个地方。

演讲列出的全球用户（部分）包括 OpenAI、Uber、Apple、Pinterest、Microsoft、Cisco、J.P.Morgan、Roku TV、Toast、Tencent、中国移动、阿里云、小米、理想、哔哩哔哩，覆盖 AI、云、金融、互联网、通信、汽车、消费科技等行业；这是采用情况的展示，演讲未给出部署规模或效果数字。

### 开放语义层：跨工具共享业务定义

开放语义层要解决的问题是：一份经过确认的定义，可以被 BI、应用与 Agent 共同使用。

演讲用 Apache Ossie 的语义模型举例，定义 `net_revenue`：

| 组成 | 内容 |
|------|------|
| 指标 | `net_revenue = SUM(order_items.net_amount)` |
| 时间口径 | `orders.order_date` |
| 维度 | `products.category` |
| 覆盖范围 | 指标 · 维度 · 时间口径 · 关联关系 · 业务术语 |

三类消费者接入同一份定义：

- **BI**：导出副本，需要同步（Sync required）
- **App**：接入该语义模型（原文未展开具体接入方式）
- **Agent**：直接编译，运行在受治理的运行时里（Governed runtime）

这里的对比是：BI 走「导出副本 + 同步」，Agent 走「直接编译 + 受治理运行」。同一份定义，不同工具不必各自维护一套口径。

### 业务本体：实体、关系与约束的表达

Ontology 支柱用一个个人数据问题展开：「哪些表包含客户个人数据？这些数据是否受到保护？」

演讲给出的本体结构是：

- 概念 `PersonalData`（个人数据）
- 子类 `EmailAddress`、`PhoneNumber`，两者都是 `subClassOf PersonalData`
- 概念绑定到跨 catalog 的真实资产：
  - `lake.crm.customers.email`
  - `mysql.loyalty.member_email`
  - `kafka.events.contact_phone`

本体把「概念—关系—资产」串起来，支撑四步操作：

| 步骤 | 动作 | 机制 |
|------|------|------|
| 1 | 解释概念 | Glossary + class |
| 2 | 查询关系 | SPARQL path |
| 3 | 定位资产 | Cross-catalog |
| 4 | 检查策略 | Mask / deny |

四步操作把业务概念、关系路径、物理资产和保护策略放进同一条可查询的链路。

### 三者协同：业务意图如何转化为数据操作

演讲给出两条任务路径，说明三重支柱按任务组合：

**Path A · Revenue：按产品类别统计收入**

> Glossary 解释 revenue → Semantic model 选择 net_revenue → Metadata 定位真实资产 → Engine 编译并执行

**Path B · Personal Data：找到个人数据及其保护情况**

> Domain 限定 Commerce → Ontology 查询概念关系 → Metadata 返回字段资产 → Policy 检查脱敏与权限

两条路径的共享底座是：

> Shared foundation: Domain · Glossary · governed assets · identity

演讲明确提醒：**按任务组合所需上下文，三者并非每次都完整经过的固定流水线。** 收入问题走的是「术语 → 指标 → 资产 → 执行」，个人数据问题走的是「域 → 本体 → 资产 → 策略」，中间用到的支柱和顺序都不同。

### 治理：业务理解与操作权限的衔接

治理要同时回答两个问题：定义是否正确，以及当前调用者能否安全使用。演讲把它拆成四道门：

| 关卡 | 问题 |
|------|------|
| 01 Model access | 谁能查看或使用定义？ |
| 02 Asset access | 谁能读取模型引用的表与字段？ |
| 03 Business rule | 当前任务满足哪些业务条件？ |
| 04 Execute & audit | 由哪个系统执行限制并记录过程？ |

两个执行侧的例子说明了治理如何落地：

- **缺少 `products` 表权限**：拒绝编译，并明确缺失资产
- **执行成功时**：引擎应用 row filter / column mask，返回结果同时带上 `Answer + definition + model version + owner`

由此得到一条重要边界：

> **能使用模型，不等于拥有所有底层资产权限。**

语义模型可以被授权使用，但它引用的物理表、字段仍要单独过权限检查；两层授权不能互相替代。

## 关键工程做法

### 上下文基础设施的四个落地挑战

| 挑战 | 表现 |
|------|------|
| 01 语义建模碎片化 | BI、应用与 Agent 各自维护定义 |
| 02 业务含义难以获取 | 字段结构清楚，业务口径仍不明确 |
| 03 异构资产难以连接 | 标识、结构与访问方式不一致 |
| 04 上下文持续失效 | 结构、规则与使用方式不断变化 |

第四项与前三个不同：它描述的是上下文建好之后仍会失效。演讲用一句话概括：

> **Build once ≠ stay correct.**

### 自上而下 Top-down：应用任务驱动

Top-down 从一个高价值任务开始，用业务结果验证上下文是否可用。它的链路是：

> Business task（收入分析 / 客户服务）→ Required context（指标、实体、规则）→ Bound assets（有限范围的数据与工具）→ Validated outcome（参考问题、答案与角色）

| 优势 | 风险 |
|------|------|
| 范围清晰 | 重复建模 |
| 业务团队易参与 | 定义嵌入应用 |
| 验证较快 | 跨场景复用困难 |

演讲给了一个具体的重复建模例子：收入 Agent 定义完成后，另一个应用可能再次定义 `revenue`、`customer` 与 `order`。这对应 Top-down 的风险项「定义嵌入应用」与「跨场景复用困难」。

### 自下而上 Bottom-up：元数据治理驱动

Bottom-up 先建设共享资产与治理基础，再为多个场景提供统一入口。它的三层结构是：

| 层 | 内容 |
|----|------|
| Unified catalog | 资产目录、标识、结构与来源 |
| Identity & policy | 身份权限、标签、审计 |
| Shared access | 跨引擎与跨格式访问 |

Bottom-up 的典型缺口是「有结构、没含义」。演讲用 `order_date` 与 `ship_date` 举例：

> 哪一个用于收入确认？发现字段关联，并不能自动确定指标应使用哪条路径。

这页的结论是：**共享底座覆盖广，但技术结构无法替代业务判断。**

### 两种策略的比较与结合点

| 对比项 | Top-down | Bottom-up |
|--------|----------|-----------|
| 起点 | 业务任务 | 资产与治理 |
| 初期成果 | 可用应用 | 共享底座 |
| 主要风险 | 定义分叉、复用不足 | 业务意义不足、验证较慢 |
| 需要补足 | 资产绑定与共享发布 | 业务参与与任务反馈 |

两种策略在中间汇合到一点：**Verified knowledge（经过验证的共享知识）**。演讲的表述是「业务场景决定优先级，共享基础设施承接经过验证的知识」——先用业务场景判断什么值得做，再把验证过的知识沉淀到共享层。

### 混合路径：场景建模与共享发布的闭环

演讲给出的落地闭环有六步：

| 步骤 | 动作 | 说明 |
|------|------|------|
| 01 | 选择场景 | High-value use case |
| 02 | 共同确认定义 | Business + engineering |
| 03 | 绑定真实资产 | Unified metadata |
| 04 | 验证问题与角色 | Questions + roles |
| 05 | 发布共享对象 | Approved model |
| 06 | 反馈修订 | Usage feedback |

四个配套机制：

- **Import existing**：导入已有指标与术语
- **Generate candidates**：从元数据与查询历史生成候选
- **Git workflow**：工程团队做版本维护
- **Steward review**：业务负责人审核发布

核心原则是：**自动生成降低建模成本；业务校验决定候选内容能否成为可信定义。**

### 上下文持续保鲜：变化检测与反馈迭代

演讲把上下文维护拆成「三类变化输入 → 四步审核流水线 → 两类上下文状态」。

三类变化输入：

| 输入 | 内容 |
|------|------|
| Technical change | 字段、结构、映射与权限 |
| Business change | 指标口径、分类与适用范围 |
| Usage feedback | 失败查询、歧义与人工纠正 |

四步审核流水线：

1. **Impact analysis**：影响分析
2. **Validate**：验证
3. **Approve**：批准
4. **Publish + history**：发布并保留历史

产出分为两种状态：

- **Candidate context**：保留来源与置信信息，尚未成为正式定义
- **Curated context**：维护者审核后发布，进入正式术语或模型

最关键的一条工程纪律是：

> **Learned evidence 不能静默写入 curated objects.**

系统从使用中学习到的证据，不能绕过审核直接改写正式定义；学习结果要先进入候选区，保留来源和置信信息，再由维护者审核发布。

### AI Context Platform 的参考架构

演讲给出四层参考架构，右侧是贯穿的治理列：

| 层 | 内容 |
|----|------|
| USE | Agent · BI · Business Apps |
| CONTEXT | Open Semantics · Ontology · Domain · Glossary |
| ASSET | Unified Metadata · Mapping · Access APIs |
| EXECUTE | Query Engine · Business API · Workflow |

治理列包含六项：Identity、Policy、Version、Validation、Audit、Feedback。

两条任务路径在架构上的投影是：

- **Revenue path**：`term → metric → assets → SQL`
- **Personal data path**：`concept → assets → policies`

架构的定位是「共享定义与资产映射，按任务与调用者身份组织为可执行上下文」。

### Datastrato 2.0：从数据目录到上下文平台

演讲最后给出产品视角，把平台能力分成四个模块：

| 模块 | 能力 |
|------|------|
| DISCOVER / 发现 | 统一元数据连接异构资产；Domain 缩小检索范围；Glossary 解释业务术语 |
| MODEL / 建模 | Ossie semantic models；指标、维度与关联；Ontology sidecar practice |
| GOVERN / 治理 | Model and asset grants；审核、认证与版本；Policy checks before execution |
| SERVE / 服务 | REST & MCP tools；Compile for caller's engine；Explain model, owner and evidence |

发布边界也写得很明确：

| 阶段 | 内容 |
|------|------|
| Today · Gravitino 1.3 | Tags、policies、MCP、Jena sidecar |
| 2.0 OSS · planned | Ossie models、domains、glossary、semantic MCP |
| Datastrato 2.0 · planned | Generation、stewardship、imports、OPA |
| After 2.0 / 后续 | Native ontology、Context Graph、discovery |

这张表把「已经可用」和「计划中」分开标注。阅读时要注意：Ossie models、语义 MCP、OPA、原生本体与 Context Graph 都属于 planned 或后续，不是演讲时点已经交付的能力。

## 经验与边界

演讲用三个判断收束：

1. **三重支柱共同支撑业务理解**：资产描述、共享定义与实体关系必须连接起来
2. **上下结合支持可持续建设**：场景验证价值，共享模型积累知识
3. **可信性依赖长期维护**：持续校验、版本管理与治理执行共同发挥作用

结尾的判断是：

> 企业 Agent 需要持续获得**正确的业务定义、真实的数据映射和有效的操作约束**。将这些知识沉淀为共享基础设施，是数据目录向 AI Context Platform 演进的核心价值。

边界也要看清楚：

- 这是一场**厂商演讲**，参考架构和产品模块带有 Datastrato / Gravitino 的产品视角；Ossie、Gravitino 是演讲选用的代表实现。
- 收入口径案例在原文中明确标注为「示意案例」，未说明来自客户实测。
- 演讲没有给出量化效果（如准确率提升、建模周期缩短），只给了架构、流程和发布路线；路线图中的 2.0 能力明确标注为 planned。
- 原文没有把「三重支柱」称为行业标准，也没有声称三者有唯一实现顺序，反而强调「按任务组合所需上下文，三者并非每次都完整经过的固定流水线」。

## 相关页面

- [[ontology]] — 本体的基础概念与五要素框架
- [[open-semantic-interchange]] — OSI 开放语义标准与指标治理
- [[apache-ossie]] — Apache Ossie 语义层项目
- [[gravitino]] — Apache Gravitino 统一元数据湖
- [[ontological-semantic-layer]] — 本体化语义层：数仓之上的业务语义控制面
- [[dataagent-semantic-layer]] — DataAgent 语义层的实物层面
- [[data-agent-vs-ontology-agent]] — 指标驱动与本体驱动的推理范式对比
- [[google-context-engineering]] — 上下文工程的白皮书视角
- [[model-fullstack-moat]] — 同场主论坛：医疗 Agent 的全栈工程与 Harness 实践
- [[wing-pay-ontology-trusted-ai]] — 同场金融分论坛：语义映射与对象存储的本体落地取舍
