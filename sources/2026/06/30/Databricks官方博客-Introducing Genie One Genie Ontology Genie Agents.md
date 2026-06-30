---
title: Introducing Genie One, Genie Agents, and Genie Ontology
url: https://www.databricks.com/blog/introducing-genie-one-genie-ontology-and-genie-agents
author: Sydney Sundell, Ken Wong, Elise Georis
date: 2026-06-16
retrieved: 2026-06-30
type: blog
ingested: 2026-06-30
wiki_pages: [genie-ontology, databricks-2026-summit, databricks-genie]
---

# Introducing Genie One, Genie Agents, and Genie Ontology

**作者**：Sydney Sundell, Ken Wong, Elise Georis | **发布**：2026-06-16 | **来源**：Databricks 官方博客（一手来源）

> 原文：https://www.databricks.com/blog/introducing-genie-one-genie-ontology-and-genie-agents

## 1. Genie One：数据感知型 AI 同事

Genie 最初是 Databricks AI/BI 内的会话式分析助手。Genie One 是下一步演进——一个旨在让用户从洞察走向行动的 AI 同事（AI coworker）。

**核心能力：**
- **跨整个数据资产连接**：通过原生连接器、Lakehouse Federation、Lakeflow Connect，以及与业务工具（Gmail、Slack、Teams）的双向集成。可跨系统提取洞察并编排行动。
- **完整 agentic-cowork 能力**：调度、告警、监控、文档创建、自定义技能、自定义 MCP 支持。用例：结合日历/邮件/Lakehouse 数据生成每日客户会议简报；用最新库存数据和团队转写更新业务复盘文档。
- **嵌入 Slack 和 Microsoft Teams**：用户可在对话（含公共频道和线程）中 @mention Genie，获得受治理的安全自然语言答案。
- **iOS/Android 移动应用**：随时提问、接收告警、采取行动。
- **Genie MCP App**：让已有 AI Agent 的组织无需改变工作流即可受益于 Genie。

## 2. Genie Ontology：活的上下文图谱

Genie Ontology 是为 Genie One 和 Genie Agents 提供动力的**自动上下文层**。它自动从表、查询、仪表板、管道和连接的应用中提取知识片段，组织成一张代表"公司如何运作、数据意味着什么"的活图谱。

**捕获内容**：指标定义、业务术语、独特计算，以及概念/指标/表/团队之间的关系。

**权威/信任机制（类 PageRank 方法）**——Genie Ontology 从多个维度加权决定来源权威性：
- 定义来源何处
- 来源作者的相对权威性
- 人们依赖它的频率
- 与已认证、广泛使用资产的关联紧密程度
- 新鲜度

Genie 随后从最高权重的来源回答，并强制执行源原生权限，只显示用户有权访问的内容。**无需人工策展或单独的权限系统**。

**数据源**：表、查询、仪表板、管道、连接的应用。

## 3. Genie Agents：从 Prompt 到自主 Agent

Genie Spaces（客户已创建超 100 万个）正在演化为 Genie Agents——策展的、领域特定的 AI Agent。

**关键特性：**
- **采取自主行动**：使用 MCP 连接、调度任务、文档/工件生成、写入外部系统，无需监督完成多步工作流。
- **推理非结构化数据**：文档、文件、知识源，与结构化数据并列。
- **单 Prompt 创建**：在 Genie One 或 Genie Code 中描述一句，系统自动 scope、benchmark，并分享给队友使用或定制。

## 4. 基准数字与准确率声明

来自 Databricks 内部基准（28 题真实世界数据分析套件，2026 年 6 月）：

| Agent | 首次回答准确率 |
|---|---|
| **Genie（含 Ontology）** | **84.5%** |
| 最强通用编码 Agent | 52.4% |
| 最弱编码 Agent | 25% |

其他声明：
- Genie 同时实现高准确率**和**低延迟——比最强编码 Agent **快 2 倍**。
- 基准中的竞争编码 Agent 已匿名化。

## 5. 治理与安全

- 每个答案默认通过源原生 ACL 或 Unity Catalog 强制执行权限。
- MCP、工具和成本由 **Unity AI Gateway** 治理，为管理员提供单一治理面板。

## 6. 关键引述

**Uplight（Micaela Christopher，数据科学与工程总监）：**
> "This is the promise of data democratization - enabling a culture where curiosity, data-informed decision-making"

**Foot Locker（Matt Giunipero & Krish Lakshminarayanan，数据分析 VP）：**
> "Genie isn't just a tool; it's the engine driving self-service insights across our organization"

**关于解决的核心问题：**
> "When AI doesn't easily find the information it needs, it fills in the gaps with inference"（当 AI 找不到所需信息时，它会用推断填补空白）

## 三部分发布总结

1. **Genie One** — 跨所有企业数据、应用和界面（Slack/Teams/移动/MCP）工作的数据感知型 AI 同事
2. **Genie Agents** — 单 Prompt 创建的可分享自主 Agent，推理结构化与非结构化数据并采取多步行动
3. **Genie Ontology** — 自动、安全的上下文图谱，使用类 PageRank 权威系统解决企业上下文问题，无需人工策展
