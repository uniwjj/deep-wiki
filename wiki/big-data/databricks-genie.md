---
title: Databricks Genie
description: Databricks 在 2026-05-08 发布的企业级 Data Agent，面向 Lakehouse 全量结构化与非结构化资产，通过 specialized knowledge search、parallel thinking 与 Multi-LLM 把内部 benchmark 准确率从 32% 提升到 90%+
aliases: [Databricks Genie, Genie, Pushing the Frontier for Data Agents with Genie]
tags: [big-data, ai-agent, tool]
sources: [2026/05/18/为什么 Code Agent 无法解决企业数据分析问题.html, 2026/05/18/为什么 Code Agent 无法解决企业数据分析问题.md]
status: draft
created: 2026-05-18
updated: 2026-05-18
---

# Databricks Genie

Databricks 在 2026-05-08 发布的博客《Pushing the Frontier for Data Agents with Genie》对应的企业级 Data Agent。本页内容来自 WinClaw 文章的二手转述（参见 [[code-agent-vs-data-agent]]），未直接读到原文，标记为 `draft`，待后续 `/research` 拉原文补全。

## 面向对象

不是单个 CSV，也不是孤立数据库，而是整个 Lakehouse 里的结构化与非结构化资产：

- tables
- dashboards
- notebooks
- files
- Apps
- Google Drive
- SharePoint
- 业务定义
- 元数据和历史分析资产

## 内部 Benchmark 数据

在 Databricks 内部真实数据分析 benchmark 上：

- 加入 **specialized knowledge search**、**parallel thinking** 和 **Multi-LLM** 之后
- Genie 相对领先 coding agent，准确率从 **32%** 提升到 **90%+**

注意：Databricks 自家内部数据，**不能直接当作全行业通用指标**。但至少说明：企业数据分析问题不是"让模型多写几行 SQL"就能解决的，需要搜索、语义、推理、执行、验证和成本控制一起进入系统设计。

## 揭示的三个独特挑战

详见 [[code-agent-vs-data-agent]]：

1. **百万级数据资产让传统搜索失效** — 资产规模大、结构不统一、命名不一致
2. **动态环境里真实来源很难判断** — source of truth 识别难，错口径比代码报错更危险
3. **缺少像代码测试那样的确定性验证** — 没有 oracle，必须靠系统设计建立信任

## 待补全

- [ ] 拉取原文 `https://www.databricks.com/blog/pushing-frontier-data-agents-genie`
- [ ] specialized knowledge search 的具体实现
- [ ] parallel thinking 的策略与 Multi-LLM 路由
- [ ] benchmark 的题型、数据分布、对比对象（领先 coding agent 是哪一个？）
- [ ] Genie 与 [[dataworks-data-agent]]、[[openai-data-agent]] 的产品形态对比

## 相关页面

- [[code-agent-vs-data-agent]] — Genie 揭示的三个挑战
- [[infinisynapse]] — 对三个挑战的另一种回答
- [[openai-data-agent]] — OpenAI 内部 Data Agent
- [[dataworks-data-agent]] — DataWorks Data Agent
- [[data-agent-practice-guide]] — 火山引擎数据智能体实践指南
