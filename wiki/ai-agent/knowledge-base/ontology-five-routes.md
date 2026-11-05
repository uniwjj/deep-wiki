---
title: 知识建模的五条路线
description: 知识（本体）目前没有统一标准——Palantir Ontology、Semantic View 家族、Apache Ossie、Databricks Genie Ontology、Agent Memory 不是同一标准的五个实现，而是五种本体/语义建模模型；按知识表示形态、概念范式、关键原语、运行方式、语义生产与裁决方式逐维对比，并用一条"强保证 ↔ 高灵活"的轴说明每向右一步都在用保证换自由
aliases: [五条路线, 本体五条路线, 知识建模路线, Ontology Five Routes, 五类知识建模模型, 知识表达路线]
tags: [ai-agent, concept, comparison]
sources: [2026/10/29/2026DACon北京站/关涛-从数据工程到知识工程：AI时代知识与本体的演进与思考.pdf]
created: 2026-11-05
updated: 2026-11-05
---

# 知识建模的五条路线

> **核心主张：知识（本体）建模目前有很多标准，但并不统一。** 准确地说：**有表达、查询和约束规范，但没有产业共同接受的企业本体建模、运行与维护标准。** 当前没有任何一套标准同时规定四件事——**建什么 · 怎么运行 · 如何裁决 · 怎样持续维护**。因此 Palantir Ontology、Semantic View 家族、Apache Ossie、Databricks Genie Ontology、Agent Memory **不是同一标准下的五个实现，而是五种不同的本体 / 语义建模模型**。

本页整理自关涛（云器科技）在 DACon 2026 北京站的演讲《从数据工程到知识工程》，是该演讲第二章的展开。演讲页来源：`2026/10/29/2026DACon北京站/关涛-从数据工程到知识工程：AI时代知识与本体的演进与思考.pdf`。

## 为什么没有统一标准

三个证据构成"不统一"的判断：

| 层面 | 现状 | 没解决什么 |
|------|------|-----------|
| 表达 | W3C 标准化的是表达：RDF / OWL / SHACL 规定机器如何表达、推理和校验 | 不规定企业应该采用哪一种业务对象与运行模型 |
| 实现 | 产业实现彼此不同：Palantir 建对象与 Action；Semantic View 建指标与 SQL；Genie 建知识片段并排序 | 三者并非同一标准的实现，无法互换 |
| 交换 | 开放交换仍在形成：Apache Ossie 仍处 Incubating，当前重点是跨工具交换 | 不提供统一运行时与企业本体方法论 |

演讲因此把讨论从"谁符合标准"转到"谁在什么约束下做了哪种建模选择"。

## 判据：先看 TBox / ABox / Action，再看语义从哪来

演讲给出一条读图判据：

> **"图"只是载体：先看有没有显式 TBox / ABox / Action，再看语义来自人工声明还是机器推断。**

这四个问题决定了后续所有差异：

1. **有没有显式 TBox**——概念、属性、关系、规则是否被显式声明？
2. **ABox 是否物化**——实例是物化并索引，还是留在源表、查询时编译？
3. **有没有 Action**——系统只能查询，还是能把决策写回业务系统？
4. **语义由谁生产**——人工建模 / 人工声明 + 机器提案 / 机器自动生成 + 人工增权 / LLM 自写自读？
5. **正确性靠什么裁决**——Schema + 权限 + Action、Schema + Compiler + RBAC、Schema 校验后交导入方执行，还是排序（排序不等于验证）？

## 路线一：Palantir Ontology——数字孪生与行动系统

**定位**：对象、属性、链接构成**语义元素**；行动与函数构成**动力元素**——Agent 不只查询业务，还能安全地改变业务。

| 维度 | 内容 |
|------|------|
| 建模中心 | 属性式对象模型 · 通用建模：Object / Property / Link / Action。把所有要素都建模了——包括行动，不只是数据和知识；实例物化为可操作对象 |
| 关键原语 | Object / Property / Link / Action；Action 类型、函数、动态权限；Action 写回源系统 |
| 实例与运行 | 实例物化并索引；运行引擎驱动 Action 写回 |
| 语义元素 | 对象类型 · 属性 · 链接类型；接口提供多态 |
| 强项 | 全面建模、行动包含层、本体运行引擎——引擎里包含驱动行动的部分，已经从表达层深入到运行层；语义、实例、权限、Action 与写回统一在一个平台里，全面性和深度是五条路线里最好的 |
| 边界 | 垂直领域、不开放、高成本：实施重、实例规模大、平台封闭；是高成本的治理闭环，不以开放语义交换为目标 |
| 路线判断 | 适合高治理的运营闭环；**不是起点** |

演讲用一张 Architecture Center 的架构图读出了三条输入与三条输出：输入是**数据源**（交易、IoT、地理、非结构化）、**逻辑源**（ML、优化器、规则）、**行动系统**（ERP、MES、SCM、调度）；本体居中，对象之间带 Action 与 Automation；输出是**分析与工作流**、**自动化**、**产品与 SDK**——形成读写闭环。

相关落地视角见 [[semantic-layer-to-ontology]]（悦点科技把 Palantir 列为业务本体的三条技术路径之一）与 [[ontology-semantic-infrastructure]]（本体作为智能体语义基础设施）。

## 路线二：Semantic View 家族——数据库中的分析语义对象

**定位**：用逻辑表、关系、维度和指标描述业务语义；不物化实例，查询时编译为 SQL。

| 维度 | 内容 |
|------|------|
| 建模中心 | 属性式对象模型 · 分析语义（对象 = 逻辑表）：Logical Table + Relationship + Fact / Metric / Dimension；实例仍留在源表，查询时编译为 SQL |
| 关键原语 | Table / Relationship / Fact / Metric / Dimension |
| 实例与运行 | 数据留源表；查询时编译 SQL |
| 语义生产 | 人工声明 + Autopilot 提案 |
| 正确性 / 裁决 | Schema + Compiler + RBAC：确定、准确 |
| 强项 | 确定性 + 准确性：分析口径确定、结果准确；原生 RBAC 与审计，对 NL2SQL 直接有效 |
| 边界 | 仅在结构化数据领域：主要覆盖仓内结构化分析语义，不承担业务实例写回与行动编排 |
| 路线判断 | 结构化分析语义的**主流起点**：确定、准确，改造成本适中 |

这一路线的代表生态是 Snowflake Semantic Views（演讲引用 Snowflake 官方文档 `views-semantic/overview`）与 Databricks 的 Metric View。它在本 wiki 的出现频率很高，见 [[data-agent-semantic-stack]]、[[360-data-agent-semantic-layer]]、[[open-semantic-interchange]]。

## 路线三：Apache Ossie——跨工具交换语义模型的开放规范

**定位**：厂商中立的 YAML 交换格式；最新规范把**本体层与核心语义层分开**——目前看得到的、从结构化语义向本体延伸的一条路径。

| 维度 | 内容 |
|------|------|
| 建模中心 | 实体模型（事实式，也是知识模型的一种）：本体层 EntityType / Role / Relationship 以**事实（关系陈述）**建模，没有"属性"，比属性式对象模型更宽泛；核心语义层仍是属性式（与 Semantic View 同构） |
| 本体层（偏业务定义） | 概念 EntityType / ValueType · 关系 + 角色 · 规则（requires / derived_by / identify_by / extends） |
| 映射 Mapping（两层之间的桥） | ontology_ref + semantic_model_ref；Object mapping → 字段 SQL 表达式；Link mapping → 连接树 |
| 核心语义层（偏数据定义） | Dataset / Field · Relationship · Metric / Dimension；YAML / JSON，实例留在源表 |
| 实例与运行 | YAML / JSON；**无运行时**；导入方执行 |
| 语义生产 | 人工声明 |
| 正确性 / 裁决 | Schema 校验：确定、准确；执行由导入方承接 |
| 强项 | 确定性、准确性 + 开放标准：同样是 Schema / SQL 编译，口径确定、结果准确；厂商中立的开放标准，一次编写、各处查询，不绑定任何引擎 |
| 边界 | 结构化范畴，最近增加 Ontology 层：和 Semantic View 一样目前局限在结构化数据；本体层近期才刚起步；不是查询引擎，也不是语义运行时 |
| 路线判断 | 企业的**另一个起点**：开放、无锁定 |

**查询接口 / MCP**：两层都向 Agent 和 BI 暴露，接口分三类——

- **维度式接口**：结构化查询对象，Agent 无法自造 join
- **关系式接口**：SQL + `MEASURE()`，治理过的指标
- **本体**：业务概念与含义，在指标之上

Agent 通过 MCP 工具调用，带类型化错误。生态示意图（数据源 → 语义模型 → 消费方，含 AI Agents）显示其消费方包括 Power BI、Microsoft Fabric、Snowflake、Databricks、AI Agents 与其他工具。

详见 [[apache-ossie]]（含蚂蚁落地的 1+3+N 实践）与 [[open-semantic-interchange]]（OSI 的 Agent 契约视角）。

## 路线四：Databricks Genie Ontology——从使用痕迹中推断上下文

**定位**：从查询、Dashboard、Pipeline 与协作系统中抽取知识片段，并按权威性与时效排序。

| 维度 | 内容 |
|------|------|
| 建模中心 | **混合模型**：既包含一部分结构化 Semantic（Metric View），也包含企业知识（Knowledge Snippet + 资产 / 用量关系）——比只做 Semantic 更全面；不声明统一概念模型 |
| 关键原语 | Snippet / Asset / Usage Signal / Rank |
| 实例与运行 | 片段库 + 排序服务；回答时裁决 |
| 语义生产 | 机器自动生成 + 人工增权 |
| 正确性 / 裁决 | **排序不等于验证** |
| 强项 | 覆盖面广 + 主打自动化生成：能从真实使用持续学习与更新，建模成本低 |
| 边界 | 难以表达非可加、基数等需要强契约的约束；排序不等于验证 |
| 路线判断 | 比较普适的模式：适合补齐长尾上下文；**认证真值仍需语义内核** |

演讲引用了 Genie Ontology 的五层架构栈（"从原始数据到对话式答案，五个协作的层"）：

| 层 | 内容 | 一句话 |
|----|------|--------|
| L5 Compound AI | Routing · SQL generation · Visualization · Clarification · Summarization | 每个子任务挑合适的模型，而不是一次巨型 LLM 调用 |
| L4 Genie Space | Topic scope · Instructions · Trusted assets · Benchmarks · Feedback loop | 一个有焦点、被评估的对话式产品 |
| L3 Business Semantics | Metric Views · Named entities · Relationships · Certified assets | 指标定义一次，BI 与 AI 复用 |
| L2 Unity Catalog | Schemas · Columns · Tags · AI descriptions · Lineage · Permissions | 面向所有下游消费者的统一治理 |
| L1 Data Platform（Lakehouse） | Delta tables · Storage · Lineage · Query history | 单一物理真相，无需搬迁数据 |

原文的注脚是 "Each layer is independently useful — together they form the Genie Ontology"——**每层可单独用，合起来才是 Genie Ontology**。

详见 [[genie-ontology]] 与 [[databricks-genie]]。

## 路线五：Agent Memory——自由文本中的个体知识与经验

**定位**：无需预定义结构；适合个体上下文，但不能承担组织级唯一真值。

| 维度 | 内容 |
|------|------|
| 建模中心 | 情景 / 语义记忆（episodic / semantic）：自由文本、个体偏好与经验 |
| 关键原语 | Markdown / Text Chunk |
| 实例与运行 | 目录树 / 检索 + LLM 理解 |
| 语义生产 | LLM 自写自读 |
| 正确性 / 裁决 | 无结构校验与口径裁决 |
| 强项 | 覆盖广、低建模、弱保证 |
| 边界 | 无结构校验、无冲突发现、无口径裁决；**不承担组织级真值** |

演讲展示了三类典型的记忆文件及其"适合保存"的内容：

| 记忆文件 | 例子 | 适合保存 |
|---------|------|---------|
| 项目记忆 `project-memory.md` | "本项目收入统一指剔除退款后的实付金额。" | 项目背景与临时约定 |
| 用户记忆 `user-preferences.md` | "老板看经营数据时偏好周环比，并先看异常。" | 个体偏好与表达习惯 |
| 会话记忆 `conversation.md` | "本轮分析聚焦华东区，暂不包含测试订单。" | 操作经验与历史反馈、跨会话连续上下文 |

组织边界被写得很直白：**无结构校验、无冲突发现、无口径裁决；因此必须由强 Schema 语义层提供认证真值。** 记忆机制的其他工程视角见 [[mem0]] 与 [[lycheememory-memory-scaling]]。

## 七维对比：五类系统真正不同的是知识表示与运行方式

| 模型维度 | Palantir | Semantic View | Apache Ossie | Genie Ontology | Agent Memory |
|----------|----------|---------------|--------------|----------------|--------------|
| **知识表示形态** | Schema based · 对象本体：TBox + ABox + Action 一体 | Schema based · 分析语义模型：TBox-like；ABox 留在源表 | Schema based · 交换契约：语义层 + 事实式本体层 | Semi-Schema · 推断上下文：片段 + 关系 + 排序 | Schema-Less · 自由文本记忆：无显式 TBox / ABox |
| **概念建模范式** | 属性式对象模型 | 属性式对象模型（对象 = 逻辑表） | 本体层：事实式实体模型；语义层：属性式（同 SV） | 混合：属性式分析对象 + 无 Schema 知识片段 | 无显式概念模型 |
| **关键原语** | Object / Property / Link / Action | Table / Relationship / Fact / Metric / Dimension | Dataset / Field / Metric + EntityType / Role | Snippet / Asset / Usage Signal / Rank | Markdown / Text Chunk |
| **实例与运行** | 实例物化并索引；运行引擎驱动 Action 写回 | 数据留源表；查询时编译 SQL | YAML / JSON；无运行时；导入方执行 | 片段库 + 排序服务；回答时裁决 | 目录树 / 检索 + LLM 理解 |
| **语义生产** | 人工建模 | 人工声明 + Autopilot 提案 | 人工声明 | 机器自动生成 + 人工增权 | LLM 自写自读 |
| **正确性 / 裁决** | Schema + 权限 + Function / Action | Schema + Compiler + RBAC：确定、准确 | Schema 校验：确定、准确；执行由导入方承接 | 排序不等于验证 | 无结构校验与口径裁决 |
| **最适场景 / 硬边界** | 高治理运营闭环；垂直、高成本；不是开放起点 | 主流起点：BI / NL2SQL；限结构化语义；无 Action | 开放标准：另一个起点；限结构化；无运行时 | 普适：长尾 + 部分结构化；推断不等于认证 | 个体记忆；不能承担组织真值 |

> 表格来源标注：Singdata Research《语义层与本体调研白皮书》3.1–4.3、《本体与语义模型对比》2–4。演讲据此整理，属二手归纳；具体产品的字段与能力应回到各厂商官方文档复核。

## 一条轴：每向右一步，用保证换一种自由

五条路线的排序不是能力高低，而是**设计目标不同**：

| 位置 | 路线 | 标定 |
|------|------|------|
| 强保证 | Palantir Ontology | 口径唯一 · 可验证 · 可写回 |
| 开放接口（居中） | Apache Ossie | 跨工具交换 · 不绑定引擎 |
| 高灵活 | Agent Memory | 覆盖广 · 低建模 · 弱保证 |

> **每向右一步：用一种保证，换一种自由。**

Semantic View 贴近"强保证"一侧（确定、准确、可编译，但只覆盖结构化分析语义、没有 Action）；Genie 介于"开放接口"与"高灵活"之间（覆盖广、自动生成，但排序不等于验证）。

## 结论：选择起点，而不是选"最好"

演讲的结论把选择权交回场景：

- 结构化分析语义的主流起点是 **Semantic View 家族**：确定、准确、改造成本适中。
- 想要开放、无锁定的另一个起点是 **Apache Ossie**。
- 需要高治理运营闭环时才考虑 **Palantir Ontology**——它是终点形态，不是起点。
- **Genie** 适合补齐长尾上下文，**认证真值仍需语义内核**。
- **Agent Memory** 承担个体上下文，不承担组织真值。

> 不同方案的设计目标和方向不同，所以在不同场景上会采用不同的知识建模形态。既然知识有多种建模形态，企业就需要一个通用的框架来建设知识。

这一"通用框架"在演讲的后续章节被展开为知识生产管线与 Eco-loop，见 [[data-to-knowledge-engineering]] 与 [[knowledge-eco-loop]]。

## 经验与边界

- **五条路线会互相组合**：Genie 自己就同时含 Metric View（Semantic）与知识片段；Ossie 的语义层与 Semantic View 同构、上方再挂本体层。把它们当"可组合的层"，比当"互斥的选型"更贴近原文图示。
- **"排序不等于验证"是本页最重要的一条判据**：覆盖面与自动化不能替代裁决，认证真值需要一个强 Schema 的语义内核。
- **标准仍在移动**：Ossie 仍处 Incubating，本体层是近期新增；Palantir、Snowflake、Databricks 的产品形态仍在演进，具体能力以官方文档为准。
- **演讲未给出的内容**：五条路线各自的成本量级、迁移代价与失败案例；白皮书的完整方法论。本页不做推断。

## 相关页面

- [[data-to-knowledge-engineering]] — 演讲主页：概念地图、两个挑战、两个案例、四条架构共识与云器 V2 平台
- [[apache-ossie]] — Apache Ossie 语义层的项目背景与蚂蚁落地实践
- [[genie-ontology]] — Genie Ontology 的双轨制、PageRank 式权威排名与 84.5% 首答命中率
- [[databricks-genie]] — Genie 从 Data Agent 到 Genie One / Genie Code 的演进
- [[open-semantic-interchange]] — OSI：语义层作为 Agent 契约语言、语义层之上长本体的两条路线
- [[semantic-layer-to-ontology]] — 业务本体路线：Palantir / Snowflake / Google Cloud 三条技术路径与 Knora 实践
- [[data-agent-semantic-stack]] — Data Agent 8 层语义栈（指标层 / 本体层 / 过程层 / 锚定层等）
- [[ontology-semantic-infrastructure]] — 本体作为智能体语义基础设施
- [[mem0]] — 记忆层的工程实现
- [[knowledge-eco-loop]] — 五条路线最终都要接入的知识生产闭环
