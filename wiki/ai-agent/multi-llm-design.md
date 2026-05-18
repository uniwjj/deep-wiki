---
title: Multi-LLM 设计
description: 在 Agent 系统中为不同子 Agent 分配不同 LLM 的架构模式——利用各模型的互补能力同时优化准确率、延迟和成本
aliases: [Multi-LLM Design, Multi-LLM, 多模型架构, 多 LLM 路由]
tags: [ai-agent, architecture, concept]
sources: [2026/05/18/Pushing the Frontier for Data Agents with Genie.html]
created: 2026-05-18
updated: 2026-05-18
---

# Multi-LLM 设计

Multi-LLM 是 [[databricks-genie|Databricks Genie]] 的架构选择之一，核心理念是：**不同子 Agent 使用不同 LLM，各自发挥互补能力**。

## 设计动机

不同 LLM 擅长不同的能力维度，不存在一个在所有子任务上都最优的模型。与其选一个"全能模型"用在所有环节，不如为每个子 Agent 匹配合适的模型。

## Genie 的分工示例

| 子 Agent / 阶段 | 模型特点要求 | 可选模型 |
|------|----------|--------|
| 规划阶段 (Planner) | 高层次推理、任务分解 | GPT-5.4 / Opus-4.6 |
| 搜索子 Agent | 语义匹配、快速检索 | 优化过的中等模型 |
| 代码生成 (Code Gen) | 精确 SQL/代码生成 | 代码能力强的模型 |
| 评判器 (Judge) | 批判性评估答案质量 | 评估能力强的模型 |

Databricks 平台支持无缝切换前沿模型（Opus、GPT、Gemini）、开源模型以及自定义训练模型。

## 成本优化：GEPA

不同 LLM 在延迟和成本上差异巨大。Genie 使用 **GEPA** (Generative Prompt Adaptation) 方法进一步优化。在表搜索任务上，通过 GEPA 对不同 LLM 进行 prompt 优化，在保持准确率的同时显著降低成本。

GEPA 论文：`https://arxiv.org/abs/2507.19457`

## 架构意义

Multi-LLM 打破了"一个 Agent 绑定一个模型"的简单范式：

- Agent 系统中的**不同认知阶段**需要不同的模型能力
- 模型选择成为 Agent 架构设计的一部分，而非实现细节
- 成本不是模型单价 × 调用次数的简单乘积，而是任务 × 模型 × prompt 优化的组合优化问题

## 相关页面

- [[databricks-genie]] — 该架构在产品中的应用
- [[parallel-thinking]] — 与 Multi-LLM 配合降低成本和延迟
- [[specialized-knowledge-search]] — 搜索子 Agent 的技术基础
- [[agent-architecture-patterns]] — Agent 架构模式全景
- [[agent-design-paradigms]] — Agent 设计范式
- [[llm-cost-optimization]] — LLM 成本优化方法
