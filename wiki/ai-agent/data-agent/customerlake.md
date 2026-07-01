---
title: Databricks CustomerLake
description: Databricks 把 CDP 能力放进 lakehouse 的 Agentic CDP——Customer 360、身份解析、受众构建、活动自动化、激活和个性化都在受 Unity Catalog 治理的数据基础上完成，Profile Agents 和 Campaign Agents 把"客户数据平台"改写成"营销执行 Agent 的上下文和行动平台"
aliases: [CustomerLake, Agentic CDP, Databricks CustomerLake, 营销执行 Agent 平台]
tags: [big-data, ai-agent, tool]
sources: [2026/06/30/Genie 之后，Databricks 真正想抢的是企业 Agent 入口.html]
created: 2026-06-30
updated: 2026-06-30
---

# Databricks CustomerLake

CustomerLake 是 Databricks 在 2026 年 Data+AI Summit 推出的 **Agentic CDP（智能体化客户数据平台）**，把 CDP 能力放进 lakehouse。表面上是进入 CDP 市场，更深一层是验证 Databricks 的 Agent 路线能不能落到具体业务流程里。

## 核心判断

Databricks 对 CustomerLake 的表述很明确：传统 CDP 坐在核心 Data and AI platform 外面，会造成新的集成、治理和数据复制问题；**agentic marketing 需要在同一个地方拿到 customer identity、predictive models、business logic、activation endpoints 和 real-time performance signals**。

CustomerLake 把 Customer 360、身份解析（identity resolution）、受众构建（audience building）、活动自动化（campaign automation）、激活（activation）和个性化（personalization）放回 lakehouse，在受 [[databricks-2026-summit|Unity Catalog]] 治理的数据基础上完成。

## 两类 Agent

| Agent | 职责 |
|-------|------|
| **Profile Agents** | 把原始客户数据整理成可信 Customer 360 |
| **Campaign Agents** | 把受众构建、下一步行动、跨渠道激活和持续优化包装成 Agent 工作流 |

Profile Agents 和 Campaign Agents 的意义在于：它不只是把营销工具接进 lakehouse，而是把湖仓里的数据、模型、权限和业务行动放进一个闭环。

## 为什么先打 CDP

这背后是一个更大的判断：**Agent 不会永远停在"回答问题"，它最终要进入营销、供应链、客服、财务、风控等业务流程**。Databricks 选择先打 CDP，是因为客户数据天然跨系统、强治理、强实时、强业务动作，也最容易暴露"数据平台离业务行动太远"的问题。

CustomerLake 强调开放 martech/adtech 生态、Lakehouse Federation 和 Unity Catalog——Databricks 不可能替代所有营销工具，但它可以把自己放在**客户上下文、模型和激活动作的中间层**。

## 在控制面中的位置

CustomerLake 是 [[databricks-agent-control-plane|Databricks Agent 控制面]]的**行业场景层**——把业务入口（Genie One）落到营销和客户运营流程。它回答的问题是：Databricks 的 Agent 能力能不能从"问数"真正进入"业务行动"。

## 待验证

CustomerLake 能否证明 lakehouse 内嵌 CDP 比独立 CDP 更高效。营销技术栈很复杂，Databricks 要说服企业把客户上下文、模型和激活动作放回数据平台，需要真实案例和迁移路径。

## 相关页面

- [[databricks-agent-control-plane]] — Databricks Agent 控制面战略，CustomerLake 是行业场景层
- [[databricks-2026-summit]] — Databricks 2026 四层架构变革综述
- [[genie-ontology]] — Genie Ontology，CustomerLake 依赖的企业上下文层
- [[ai-governance]] — Unity Catalog 治理，CustomerLake 的数据治理基础
- [[agentic-data-cloud]] — Google Agentic Data Cloud，同类"数据平台进入业务流程"演进
