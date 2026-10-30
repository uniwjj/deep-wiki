---
title: SORA——智能体的记录系统
description: Databricks 提出的"System of Record for Agents"——在湖仓之上把状态、本体、护栏与评估四个问题做成闭环，让智能体可在企业内自主行动并自我进化，被视为第四次"记录系统"王座之争
aliases: [SORA, System of Record for Agents, 智能体的记录系统, 记录系统, System of Record]
tags: [ai-agent, big-data, architecture, concept]
sources: [2026/10/30/《创业维艰》作者最新万字长文：AI让懂业务的人重新说了算.html]
created: 2026-10-30
updated: 2026-10-30
---

# SORA——智能体的记录系统

**SORA（System of Record for Agents，智能体的记录系统）** 是 Databricks CEO Ali Ghodsi 对其底座定位的命名：企业把**状态（state）**、**本体（ontology）**、**权限护栏（guardrails）**与**评估考卷（evaluation）**合为一体，做成一套智能体可以自主读写、并从结果中自我进化的记录系统。

> "如今，湖仓已经成为了记录系统。我称之为 SORA——智能体的记录系统。"

这一概念由 Ben Horowitz 与 Mike McGuiness 在 2026-10-27 的长文《Databricks & the Future of the Firm》中系统阐述，是理解 Databricks 产品布局（[[ltap-architecture|LTAP]] / [[genie-ontology|Genie Ontology]] / Unity Gateway / Agent Bricks）的上层叙事。

## 为什么需要新的记录系统

Ali 的判断是：**"AI 没有智力问题，它有的是上下文问题。"**

- 当今前沿模型已能解答"人类终极考试（Humanity's Last Exam）"中超过一半的题目（2,500 道题，由一百多个领域专家编写）；
- 但在大多数企业内部，人们仍不敢让 AI 可靠地更新 Salesforce 里的一条记录。

原因与"再聪明的人"一样：没有公司数据、没有行动权限、没有反馈、不掌握其他员工脑子里的知识，任何聪明人都干不成活。而在多数企业里，**本体、权限与评估往往都是不成文的**——

- 销售副总裁得知道：那笔最大的承诺订单之所以停滞，是因为客户方的内部推动者正在悄悄去别家面试；
- 得知道某个销售代表嘴里的"90% 把握"实际上只有 50%，并在预测报告呈交 CEO 之前把水分砍掉。

要让智能体自主行动，这些存在于人脑中的大部分本体、权限和评估，都必须迁移到企业的记录系统中。

## 四个承重问题

Databricks 押注湖仓是这个记录系统的天然归宿（公司其余数据都存储并治理在那里），并围绕四个问题同时发力：

| 问题 | 能力 | 实现 |
|------|------|------|
| **什么是真实的（What is true）** | 状态 | Lakehouse 与 Lakebase 保存状态 |
| **它意味着什么（What does it mean）** | 本体 | [[genie-ontology|Genie Ontology]] 持续构建组织知识图谱，用 OntoRank 排序知识权威性 |
| **什么是允许做的（What is allowed）** | 护栏 | Unity Gateway 为每个模型、智能体、AI 工具赋予身份、预算与护栏，用 traces 记录每一次操作、访问的数据与花费的成本 |
| **它奏效了吗（Did it work）** | 评估 | 业务团队在 Genie、工程师在 Agent Bricks 中自写"考卷"，定义什么可接受、什么绝不能犯 |

**OntoRank** 是工程师们致敬 Google PageRank 的算法：通过权衡"谁创建了某个定义""该定义被使用的广泛程度""最近一次修改的时间"，对哪些知识具有权威性进行排序。在 Databricks 的测试中，把智能体锚定在公司本体之上，回答准确率从约 **50% 提升到 84.5%**。

第四个问题（评估）是闭环的关键：Databricks 允许团队编写自己的考卷，并针对公司对特定任务"什么是好"的定义持续优化每个智能体——**这就是 AI 形成闭环的方式**。经验教训回流到企业自有的记录系统，而不是留在模型供应商那里（参见 [[sovereign-firm|主权企业]]）。

## 第四次记录系统战争

文章把这一命题放进计算机史的范式转移序列中：

| 时代 | 记录系统赢家 | 形态 |
|------|-------------|------|
| 大型机 | IBM | 集中式计算 |
| 客户端-服务器 | Oracle、SAP | 企业关系型数据库与 ERP |
| SaaS | Salesforce（销售）、Workday（人力资源）、ServiceNow（IT） | 按职能切分的云应用 |
| 智能体 | *未定*——"智能体的记录系统" | 跨全企业、智能体可自主读写的底座 |

> "每一次平台范式转移，都会引发一场围绕'记录系统'的新战争。智能体是下一次范式转移。"

作者强调，**哪家厂商会赢，远不如"智能体将如何改变企业本身"这个问题宏大**——这正是 [[managerial-revolution-ai|第二次经理人革命]] 与 [[theory-of-the-firm|企业的性质]] 两条线索的起点。

## 支撑数据

| 指标 | 数据 |
|------|------|
| 智能体查询量 | 三年前 Databricks 上每次交互都有人类参与；今天来自智能体的查询已超过人类 |
| 智能体启动的数据库 | 每天超过 **1,700 万个** |
| 新访问控制模型 | 员工每天启动数百个智能体，"每个员工一个登录账号"的旧模型崩溃 |
| 与 Block 的关系 | Databricks 管理与治理 Block 横跨 Square、Cash App、TIDAL 的 PB 级数据，并把其 AI 编程助手 goose 锚定在业务上下文 |

## 与 Dorsey "世界模型"的关系

Ali 的 SORA 类似于 Jack Dorsey 所说的公司**"世界模型（world model）"**——对组织运营、决策、能力和优先级的一个持续更新的表征。一线人员把模型锚定在现实世界中，模型则提供采取行动所需的共享上下文。

Block 的实践见 Dorsey 2026 年 3 月的宣言《从科层制到智能（From Hierarchy to Intelligence）》，详见 [[managerial-revolution-ai]]。

## 相关页面

- [[databricks-evolution]] — Databricks 从 Spark 到 Agents 的四条 S 曲线，SORA 是第四条曲线的落点
- [[ltap-architecture]] — LTAP 架构，为 SORA 提供事务与分析的统一存储底座
- [[genie-ontology]] — Genie Ontology，SORA 中"本体"一问的实现
- [[databricks-2026-summit]] — Databricks 2026 峰会：四层架构与 Agent 控制面
- [[databricks-agent-control-plane]] — Agent 控制面六层拼图，Unity Gateway 治理层
- [[sovereign-firm]] — 主权企业：为什么记录系统必须归企业自己所有
- [[ontological-semantic-layer]] — 本体化语义层：企业语义责任的归属
- [[enterprise-agi-framework]] — 企业级 AGI 框架，与"上下文先行"的判断同源
