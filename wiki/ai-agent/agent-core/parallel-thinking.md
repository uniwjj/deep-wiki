---
title: 并行思考
description: 通过采样多条推理轨迹并聚合跨轨迹信息来提升答案准确率的 Agent 技术——弥补数据 Agent 缺少确定性测试反馈的问题
aliases: [Parallel Thinking, 并行思考, 多轨迹采样]
tags: [ai-agent, concept]
sources: [2026/05/18/Pushing the Frontier for Data Agents with Genie.html]
created: 2026-05-18
updated: 2026-05-18
---

# 并行思考

Parallel Thinking 是 [[databricks-genie|Databricks Genie]] 的核心技术突破之一。它解决了 Data Agent 的一个根本性问题：**缺少可验证的确定性测试**。

## 问题背景

Code Agent 有完善的测试反馈机制：

```
写代码 → 运行测试 → 测试不通过 → 修正代码 → 再测试 → 通过
```

Unit test、lint、type check、build、e2e 构成了确定性的验证闭环。但数据查询任务没有这种 oracle——用户问"上季度华东收入为什么下降"，没有"正确答案"可供比对。而且有些问题本身就不可回答（数据缺失、口径无法对齐）。

## 方案：多轨迹采样 + 聚合

Parallel Thinking 不依赖外部 oracle，而是通过**内部一致性**建立置信度：

1. 对同一个用户提问，采样多条独立的推理轨迹
2. 每条轨迹独立完成资产发现 → 数据调查 → 分析推理
3. 聚合所有轨迹的发现和推理结果
4. 综合计算最终答案

关键差异：这不是简单的 majority voting，而是**聚合跨轨迹的信息**——不同轨迹可能发现不同的数据源、采用不同的分析角度、发现不同的问题。

## 性能数据

在 GPT-5.4 和 Opus-4.6 上测试：

- 加入并行思考显著提升准确率
- 但伴随额外的延迟和 token 成本
- 结合 [[multi-llm-design|Multi-LLM]] 和 prompt 优化后，可大幅降低成本和延迟

## 与 Agentic Workflow 其他模式的关系

区别于：

- **Self-Reflection** — 单条轨迹的自我修正
- **ReAct** — 单条轨迹的交替推理与行动
- **Ensemble** — 多模型投票（不聚合跨轨迹信息）

Parallel Thinking 的核心在于**跨轨迹信息聚合**——不同轨迹不仅是"多投一票"，而是互补的发现过程。

## 相关页面

- [[databricks-genie]] — 该技术在产品中的应用
- [[specialized-knowledge-search]] — Genie 的另一技术突破
- [[multi-llm-design]] — 与并行思考配合的 Multi-LLM 架构
- [[code-agent-vs-data-agent]] — 确定性测试缺失的根本原因
- [[agent-design-paradigms]] — Agent 设计范式对比
