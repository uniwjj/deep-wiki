---
title: Agent 设计模式阶梯
description: 从 RAG 到 ReAct 的五种 Agent 设计模式——各自解决什么问题、边界在哪、怎么选与怎么组合，以及模式之上的工程体系
aliases: [Agent 设计模式, 五种设计模式, 五种经典设计模式, Agent 能力阶梯, design pattern ladder]
tags: [ai-agent, concept, comparison]
sources: [2026/11/12/AI Agent 五种经典设计模式：Multi-Agent、ReAct、Plan-and-Execute 一篇讲透.html]
created: 2026-11-12
updated: 2026-11-12
---

# Agent 设计模式阶梯

这五种模式不是可以互相替代的选项，而是一条能力台阶：**RAG 让模型访问外部知识 → 单 Agent 让它调用工具执行任务 → Multi-Agent RAG 让多个智能体分工检索 → Plan-and-Execute 把复杂任务拆开逐步完成 → ReAct 按执行反馈不断调整行动**。每上一级，能做的事更多，工程成本也更高。

## 一览

| 模式 | 核心思路 | 解决什么问题 | 典型场景 |
|------|---------|-------------|---------|
| RAG | 先检索外部知识，再交给模型生成 | 模型的知识只来自训练数据，访问不到企业内部最新资料 | 企业知识库问答、产品/技术文档查询、内部制度咨询、客服知识检索 |
| 单智能体 Agent | Memory + Planning + Tools 三组件 | 从「回答问题」走向「执行任务」 | 办公助手、数据分析、业务操作 |
| Multi-Agent RAG | 按数据域拆分检索，最后统一汇总 | 数据分散在 CRM / ERP / 库存 / 知识库等多个系统 | 多源数据分析、企业经营分析、综合查询 |
| Plan-and-Execute | Planner 拆任务，Executor 逐步执行 | 任务本身复杂，让模型一次生成容易遗漏、逻辑混乱 | 市场调研与竞品分析、多步骤数据分析、自动化报告、项目任务拆解、业务流程自动化 |
| ReAct | Reasoning → Action → Observation 循环 | 不确定性大，需要边做边看反馈再决定下一步 | 动态查询、复杂决策、工具操作 |

## 各模式的结构

**RAG** 分两段。知识准备段：接入文档（PDF/Word/TXT）、网页、数据库与内部知识库，解析、切分、向量化后存入向量数据库。查询段：Query 理解（把问题转成适合检索的表达）→ Embedding → Retriever 召回 → 可选 Reranker 重排 → 与问题拼成上下文 → LLM 生成答案。检索机制与优化见 [[knowledge-retrieval-paradigms]]、[[rag-query-optimization]]。

**单智能体 Agent** 在 RAG 之上加三个组件：Memory（历史对话、任务状态、必要上下文，避免每次重复输入）、Planning（分析目标、决定下一步做什么）、Tools（数据库、API、搜索引擎、ERP/CRM）。与 RAG 最实质的差别是：RAG 围绕检索结果生成答案，Agent 按任务需要选择工具，并根据执行结果决定是否继续处理。记忆与规划的深挖见 [[agent-memory-system]]、[[agent-autonomous-planning]]。

**Multi-Agent RAG** 四类组件：Router Agent（判断该派给哪些专业 Agent）、Retrieval Agent（各管一个数据源或领域，如 CRM / ERP / 库存 / 知识库）、Aggregation Agent（整合各路结果、信息汇总）、LLM（基于汇总证据生成最终答案）。协作范式与成本判断见 [[agent-multi-agent-collaboration]]。

**Plan-and-Execute** 两个核心角色：Planner（理解目标、拆解任务、制定执行计划）与 Executor（按计划逐步完成并返回结果）；可外加 Observer（观察执行状态）与 Reflection（判断是否需要补资料、重跑某步或调整后续计划）。

**ReAct** 三个环节循环：Reasoning（分析当前问题，判断该采取什么行动）→ Action（选工具执行：搜索、查库、调 API）→ Observation（把工具返回作为下一轮推理输入），持续到判定任务完成或触发停止条件。

## 边界与代价

| 模式 | 边界 | 代价与约束 |
|------|------|-----------|
| RAG | 只负责检索和生成。能回答问题，不代表能自动规划后续任务、调用业务工具或修改系统数据 | 原文例子：「查一下哪些客户合同即将到期」RAG 可以做；「把这些客户整理出来，给销售创建跟进任务」就超出 RAG |
| 单智能体 | 当一个 Agent 同时负责销售分析、库存查询、财务核算、客户管理和合同审批，工具理解、上下文管理和执行调度都会变难 | — |
| Multi-Agent | 不一定比单 Agent 更好。多一个 Agent，就多一份调度、通信、上下文传递和结果校验的成本；任务简单时拆成多个反而增加复杂度 | 适合专业领域差异明显、数据源分散、任务能合理拆分的场景。重点不在 Agent 数量，而在每个 Agent 是否有明确职责 |
| Plan-and-Execute | 任务拆解可能不合理；前一步的错误可能影响后续步骤；执行过程中可能出现新情况 | 所以执行结果的检查和计划调整非常重要 |
| ReAct | 可能反复调用相同工具；推理过程可能陷入循环；工具调用次数和执行成本难以预测；观察结果不准确会污染后续推理 | 落地需设最大执行轮数、工具调用预算、超时机制和结果校验 |

## 选型与组合

按业务真正需要的能力选，不必强行在五种里挑一个：

| 业务需求 | 可选模式 |
|---------|---------|
| 企业知识库问答 | RAG |
| 查询数据库、调用 API、操作业务工具 | 单 Agent |
| 跨多个业务系统检索数据 | Multi-Agent RAG |
| 任务需拆成多个明确步骤 | Plan-and-Execute |
| 大量不确定性、需按反馈调整 | ReAct |

模式可以组合：**Multi-Agent RAG + Plan-and-Execute**（Planner 先拆复杂任务，再把不同的检索任务分发给专业 Agent）、**RAG + ReAct**（Agent 按当前推理需要决定何时检索、检索什么、要不要再检索）、**Single Agent + ReAct**（单 Agent 通过工具调用与观察反馈完成动态执行）。

## 模式之上：Runtime 与 Harness

设计模式只解决 Agent 如何检索、规划、协作和行动；进生产环境还要六层工程配套（原文归纳）：Agent Runtime（生命周期、任务状态、上下文、工具调用与执行调度）、Harness Engineering（执行循环、错误处理、重试机制、结果验证、异常恢复）、Memory（区分短期上下文、长期记忆和任务状态）、MCP / Tools（统一工具接口，连接数据库、业务系统与第三方服务）、Evaluation（是否完成任务、结果是否准确、过程是否符合预期）、Security & Governance（工具权限、数据访问范围、审批流程、操作审计）。这些层的工程实现见 [[agent-harness-overview]]、[[agent-evaluation-framework]]。

> 「能不能调用工具是一回事，能不能在正确的权限范围内执行操作，又是另一回事。」

原文举的例子：Agent 可以查询客户信息，但是否允许修改客户资料、是否允许直接创建订单、是否需要人工审批，都要在架构设计阶段定清楚。

> 「企业级 Agent 是一套完整的工程系统：模型负责理解和推理，设计模式负责组织任务，Runtime 和 Harness 负责管理执行，权限与治理机制负责约束操作。」

## 结论

原文的建议是：**先把业务任务拆清楚，再决定采用什么架构；简单任务不要过度设计，复杂任务不要指望一个 Prompt 就能解决**。Agent 的能力上限取决于模型，能否稳定完成真实业务任务，取决于背后的架构设计与工程体系。

## 相关页面

- [[agent-design-paradigms]] — ReAct / Plan-and-Execute / Reflection 三种范式的机制细节与选型（本文只给能力边界，细节在那页）
- [[agent-multi-agent-collaboration]] — 多 Agent 的协作范式、Multi-Agent RAG 架构与「按上下文边界分解」的选型铁律
- [[agent-architecture-patterns]] — 另一套「架构模式」视角：Claude Code 源码提炼的 10 大工程模式
- [[agent-harness-overview]] — Harness 层：执行循环、上下文管理与错误恢复
- [[agent-autonomous-planning]] — Planning 组件的深挖
- [[agent-memory-system]] — Memory 组件：长短期记忆分层
- [[knowledge-retrieval-paradigms]] — RAG 在「三代知识检索范式」中的定位
- [[sub-agent-vs-team]] — 多 Agent 的组织形态对比
