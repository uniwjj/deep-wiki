---
title: Databricks Genie
description: Databricks 的企业级 Data Agent，通过 specialized knowledge search、parallel thinking 和 Multi-LLM 设计，在内部 benchmark 上将准确率从 32% 提升至 90%+
aliases: [Databricks Genie, Genie, Pushing the Frontier for Data Agents with Genie]
tags: [big-data, ai-agent, tool]
sources: [2026/05/18/Pushing the Frontier for Data Agents with Genie.html]
created: 2026-05-18
updated: 2026-05-18
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
- [[multi-llm-design]] — 不同子 Agent 使用不同 LLM + 优化 prompt（含 [[GEPA]] 成本优化方法）

## Data Agent 的三个独特挑战

与 [[code-agent-vs-data-agent|Code Agent]] 对比：

1. **数据发现规模** — 百万级结构化+非结构化资产让传统搜索失效
2. **确定"真实来源"业务知识** — 多源信息（表元数据、公司文档、内部消息）往往过时、矛盾或被取代，Agent 必须判断最权威信息
3. **缺少可验证测试** — 没有单元测试式的 oracle，"规格"仅是高层用户提问；而且有些问题本身就不可回答（数据不完整）

## 相关页面

- [[code-agent-vs-data-agent]] — Data Agent vs Code Agent 的维度对比
- [[specialized-knowledge-search]] — 技术突破一：语义搜索索引
- [[parallel-thinking]] — 技术突破二：多轨迹采样聚合
- [[multi-llm-design]] — 技术突破三：多模型分工架构
- [[openai-data-agent]] — OpenAI 内部 Data Agent
- [[dataworks-data-agent]] — DataWorks Data Agent
- [[infinisynapse]] — InfiniSynapse 对三个挑战的回应
- [[data-agent-practice-guide]] — 火山引擎数据智能体实践指南
