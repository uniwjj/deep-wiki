---
title: 多 Agent 协作
description: 多 Agent 系统的协作模式——Sub-Agent 隔离执行、Agent Team 实时协作、五种通用模式、设计原则与选型铁律
aliases: [agent-multi-agent-collaboration, Multi-Agent, 多Agent]
tags: [ai-agent, concept]
sources: [2026/05/10/Claude-Code-Source-Analysis.pdf, 2026/05/09/Agent-Harness综述同一模型体感差异在哪.md]
created: 2026-05-10
updated: 2026-05-17
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

## 选型铁律

**按上下文边界分解，不按角色拆分。** 最常见的错误：分 Planner → Developer → Tester 角色链。正确的做法是按**独立的上下文边界**切分，让每个 Agent 拥有完成任务所需的完整信息。

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
