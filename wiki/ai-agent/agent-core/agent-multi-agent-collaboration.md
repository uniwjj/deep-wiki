---
title: 多 Agent 协作
description: 多 Agent 系统的协作模式——Sub-Agent 隔离执行、Agent Team 实时协作、五种通用模式、Multi-Agent RAG 架构、设计原则与选型铁律
aliases: [agent-multi-agent-collaboration, Multi-Agent, 多Agent, Multi-Agent RAG]
tags: [ai-agent, concept]
sources: [2026/05/10/Claude-Code-Source-Analysis.pdf, 2026/05/09/Agent-Harness综述同一模型体感差异在哪.md, 2026/11/12/AI Agent 五种经典设计模式：Multi-Agent、ReAct、Plan-and-Execute 一篇讲透.html, 2026/11/12/关于 DeepSeek 的 SuperAgent，以及我们的实践.html]
created: 2026-05-10
updated: 2026-11-12
---

# 多 Agent 协作

多个 AI Agent 协同工作需要回答两个问题：**怎么分工**（谁做什么）和**怎么通信**（信息如何流动）。

## 两种协作范式

| | Sub-Agent | Agent Team |
|---|---|---|
| **关系** | 父子层级，隔离执行 | 对等协作，实时通信 |
| **通信** | 只返回最终结果给父级，历史不进父上下文 | Agent 间直接通信，共享任务层 |
| **适用** | 独立任务，无依赖 | 互相依赖的任务 |

**Sub-Agent 三条铁律**：不能互相通信、不能自建新 Agent、信息流必须经父级。

## 五种通用模式

| 模式 | 适用场景 |
|------|---------|
| **提示链** | 串行分解：A 的输出是 B 的输入 |
| **路由** | 按任务类型分发到不同 Agent |
| **并行化** | 独立子任务同时执行，主 Agent 合成 |
| **编排-工作器** | 主 Agent 动态分配，Worker 执行后汇报 |
| **评估-优化** | 生成器→评估器→优化器循环 |

## Multi-Agent RAG：按数据域拆检索

企业的数据通常分散在 CRM（客户）、ERP（订单库存）、HR（员工）、MES（生产）与知识库（制度文档）。让一个 Agent 同时对接所有数据源，架构很容易失控；Multi-Agent RAG 的做法是**按数据域拆分检索任务，再统一汇总**。

| 组件 | 职责 |
|------|------|
| Router Agent | 理解问题，判断该分派给哪些专业 Agent |
| Retrieval Agent ×N | 各负责一个数据源或领域（如 CRM / ERP / 库存 / 知识库） |
| Aggregation Agent | 接收多路结果，整合去重、信息汇总 |
| LLM | 基于汇总证据生成最终答案 |

执行上 Router 并行分发、各自检索、最后由 Aggregation 汇总——每个 Agent 只专注自己那套数据和检索逻辑。

## 选型铁律

**按上下文边界分解，不按角色拆分。** 最常见的错误：分 Planner → Developer → Tester 角色链。正确的做法是按**独立的上下文边界**切分，让每个 Agent 拥有完成任务所需的完整信息。

**多 Agent 不一定比单 Agent 好。** 多一个 Agent，就多一份调度、通信、上下文传递和结果校验的成本；任务本身简单、一个 Agent 能完成时，拆成多个只会增加系统复杂度。适合拆分的条件是：专业领域差异明显、数据源分散、任务能合理拆分；重点不在 Agent 数量，而在每个 Agent 是否有明确职责。整条模式阶梯的选型见 [[agent-pattern-ladder]]。

**切分的判据是「能否被独立验收」。** 2025 年 6 月 Anthropic 公开多 Agent Research 系统（主 Agent 拆任务、子 Agent 并行搜索），几乎同时 Cognition 发《Don't Build Multi-Agents》反对，其例子是做 Flappy Bird——一个子 Agent 做了超级玛丽风格背景，另一个做了对不上的鸟，因为每个行动都带着隐含决策、彼此却看不到。两边都对：调研可以切开，一个游戏的美术风格切不开。所以问题不是该不该用多 Agent，而是切分边界在哪里——**只有能被独立验收的工作才能分出去**。企业侧的对应实现见 [[flock-collaboration-space]]。

## 设计原则

1. **字段粒度隔离**：不是一刀切隔离所有状态，而是按字段粒度决策哪些共享
2. **消息驱动通信**：Agent 间走消息队列，非函数调用
3. **工具权限分级**：子 Agent 的工具权限是父级的子集
4. **缓存友好架构**：Fork Subagent 复用父 Agent 字节级缓存，成本降至 10%
5. **并行优先 + 协调者合成**：能并行的全部并行，主 Agent 退化为纯协调者

## 相关页面

- [[claude-code-multi-agent-mechanism]] — Claude Code 三种多 Agent 机制源码分析
- [[sub-agent-vs-team]] — Sub-Agent vs Agent Team 深度对比
- [[claude-code-coordinator-mode]] — Coordinator 模式详解
- [[claude-code-swarm]] — Swarm 对等协作
- [[claude-code-subagent]] — 子代理委托的 10 步决策树
- [[claude-managed-agents]] — Anthropic 托管 Agent
- [[ai-plus-software-action-plan]] — 政策侧要求：方案（五）（七）提出提升多智能体协同决策与协同作业能力
