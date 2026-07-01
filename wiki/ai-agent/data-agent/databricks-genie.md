---
title: Databricks Genie
description: Databricks 的企业级 Data Agent，通过 specialized knowledge search、parallel thinking 和 Multi-LLM 设计，在内部 benchmark 上将准确率从 32% 提升至 90%+；2026 年衍生为面向业务团队的 Genie One 与面向工程的 Genie Code
aliases: [Databricks Genie, Genie, Genie One, Genie Code, Pushing the Frontier for Data Agents with Genie]
tags: [big-data, ai-agent, tool]
sources: [2026/05/18/Pushing the Frontier for Data Agents with Genie.html, 2026/06/30/从Databricks产品发布会看数据平台的演进方向.html, 2026/06/30/Databricks官方博客-Introducing Genie One Genie Ontology Genie Agents.md]
created: 2026-05-18
updated: 2026-06-30
---

# Databricks Genie

Genie 是 Databricks 面向 Lakehouse 全量资产（结构化表、dashboard、notebook + 非结构化 workspace files、Google Drive、SharePoint 文档）的企业级 Data Agent。博文《Pushing the Frontier for Data Agents with Genie》于 2026-05-08 发布。

## 核心数据

在 Databricks 内部真实数据分析 benchmark 上：

| 阶段 | 准确率 |
|------|--------|
| 领先 coding agent baseline | 32% |
| 加入 specialized knowledge search | +40% 表搜索提升 |
| 加入 parallel thinking | 显著提升 |
| 加入 Multi-LLM + 优化 prompt | **90%+** |

## 四个技术阶段

原文通过一个真实（匿名化）用户案例展示 Genie 的执行轨迹：

1. **并行多 Agent 资产发现** — 多个搜索子 Agent 并行扫描 tables、dashboards、documents
2. **数据调查** — SQL 提取、对比分析、根因调查
3. **自我修正循环** — 中间计算结果揭示初始假设错误，Agent 自动纠正
4. **最终验证** — 确认结论可复核

案例场景：用户发现两个企业 dashboard 对同一产品收入显示矛盾的数据尖峰，Agent 需要跨系统发现（表 + 内部文档 + dashboard），推理多日报告的设置方式，挖掘企业定价细节找到合同费率，并在中间计算发现初始假设错误时自动修正。

## 三大技术突破

详见单独页面：

- [[specialized-knowledge-search]] — 利用企业数据资产（表、notebook、dashboard、文档）构建语义搜索索引，表搜索性能提升 40%
- [[parallel-thinking]] — 采样多条轨迹、聚合跨轨迹信息计算最终答案，弥补缺少确定性测试的不足
- [[multi-llm-design]] — 不同子 Agent 使用不同 LLM + 优化 prompt（含 GEPA 成本优化方法）

## Data Agent 的三个独特挑战

与 [[code-agent-vs-data-agent|Code Agent]] 对比：

1. **数据发现规模** — 百万级结构化+非结构化资产让传统搜索失效
2. **确定"真实来源"业务知识** — 多源信息（表元数据、公司文档、内部消息）往往过时、矛盾或被取代，Agent 必须判断最权威信息
3. **缺少可验证测试** — 没有单元测试式的 oracle，"规格"仅是高层用户提问；而且有些问题本身就不可回答（数据不完整）

## 2026 演进：Genie One / Genie Code 与 Agent Bricks 规模化

在 2026 年 Data+AI Summit 上，Genie 从单一 Data Agent 演化为产品系列（官方博客 2026-06-16），详见 [[databricks-2026-summit]]：

| 产品 | 定位 |
|------|------|
| **Genie One** | 面向业务团队的数据感知型 AI 同事（data-smart AI coworker），正式 GA，跨 Web/iOS/Android/Slack/Teams，从洞察走向行动 |
| **Genie Agents** | 从 Genie Spaces（客户已创建超 100 万个）演化的领域特定自主 Agent，单 Prompt 创建，可推理非结构化数据，多步自主行动 |
| **Genie Code** | 面向数据工程与 ML 工作流 |

Genie One 的核心能力：跨整个数据资产连接（Lakehouse Federation/Lakeflow Connect/双向集成 Gmail/Slack/Teams）、完整 agentic-cowork 能力（调度/告警/监控/文档创建/自定义技能/自定义 MCP）、嵌入 Slack 和 Teams @mention、移动应用、Genie MCP App（让已有 AI Agent 的组织无需改变工作流即受益）。

Genie 系列采用**按用量计费（Pay-as-you-go）**模式，标志着 AI 原生时代"按调用价值计费"正在取代传统席位制收费。

承载 Genie 的 **Agent Bricks** 托管式 Agent 平台已承载超 **10 万个生产 Agent**，年 Token 消耗量超 **10¹⁵**。Genie 的语义底座升级为 [[genie-ontology|Genie Ontology]]——以"人工定义＋自动生长"双轨制构建企业知识图谱，借鉴 PageRank 思路识别权威业务定义，将复杂业务首问命中率提升至 84.5%（官方内部 28 题基准，最强通用编码 Agent 仅 52.4%，且 Genie 快 2 倍）。

> Foot Locker 客户证言："Genie isn't just a tool; it's the engine driving self-service insights across our organization."

## 相关页面

- [[databricks-2026-summit]] — Databricks 2026 四层架构变革，Genie 系列演进的背景
- [[genie-ontology]] — Genie Ontology：PageRank 式企业语义资产，Genie 的语义底座
- [[code-agent-vs-data-agent]] — Data Agent vs Code Agent 的维度对比
- [[specialized-knowledge-search]] — 技术突破一：语义搜索索引
- [[parallel-thinking]] — 技术突破二：多轨迹采样聚合
- [[multi-llm-design]] — 技术突破三：多模型分工架构
- [[openai-data-agent]] — OpenAI 内部 Data Agent
- [[dataworks-data-agent]] — DataWorks Data Agent
- [[infinisynapse]] — InfiniSynapse 对三个挑战的回应
- [[data-agent-practice-guide]] — 火山引擎数据智能体实践指南
