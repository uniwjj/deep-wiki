---
title: Inline & Spill：把上下文窗口当 Cache 管理
description: Super-agent 的上下文组织策略——用 Inline（内联高概率信息）与 Spill（超额信息留在窗口外、只留引用与摘要）两个基本操作，借局部性逐步展开关联对象，并由此形成按需多跳的链式召回（Chain of Recall）
aliases: [Inline & Spill, Inline and Spill, 链式召回, Chain of Recall, 上下文窗口即缓存, 局部性上下文注入]
tags: [ai-agent, context-engineering, concept]
sources: [2026/11/03/Super-agent 设计探索：迈向更自主的 Agent 系统.html]
created: 2026-11-03
updated: 2026-11-03
---

# Inline & Spill：把上下文窗口当 Cache 管理

在 [[super-agent]] 中，系统的输入从"有限的人类提示词"变成了"环境持续产生的半结构化事件"，于是**工具调用与事件注入的结构设计变得比提示词本身更重要**：

- 注入过多 → 挤占上下文窗口、稀释模型注意力；
- 注入过少 → 迫使模型反复调用工具读取，增加执行步数。

> 浪费上下文窗口，就是浪费模型的智能。

原文提出的做法是：**把有限的上下文窗口当作 Cache 来管理**，并把上下文注入归纳为两个基本操作。

## 两个基本操作

| 操作 | 定义 | 目的 |
|------|------|------|
| **Inline** | 把近期很可能用到的信息直接内联进当前上下文窗口 | 省去额外的工具调用往返 |
| **Spill** | 把超出窗口预算或低频使用的完整信息留在上下文窗口之外，窗口内只保留引用或摘要 | 让模型按需读取，同时守住窗口预算 |

原文说明这对概念借自编译优化与计算机系统；具体借用了哪套机制，原文未展开。

## 借助局部性逐步优化

结合计算机系统中的**局部性（Locality）**思想，Inline & Spill 对事件注入和工具调用可以分三步落地：

1. **从最少信息开始**：最初只提供当前对象的 ID 和内容摘要；
2. **利用局部性逐步 Inline 关联信息**：先把紧密关联对象的 ID 附进上下文，再附加时空相邻的内容——如内容前 50 字、IM 消息的前后相邻消息与所回复的消息、代码评论所在评审讨论中的相邻评论；
3. **超出上限则 Spill**：对无法全部 Inline 的长内容设定较小的长度上限，把超出部分 Spill 到窗口之外。做法可以是**截断并保留续读入口**，模型需要完整内容时再调用工具读取。

每一步都是"先给索引、再按需展开"，而不是一次性灌入。

## 链式召回（Chain of Recall）

在大量后续事件直通 Worker、且系统长期运行累积了许多历史任务后，[[super-agent-dual-layer-architecture|Router 层]] 面临一对矛盾：既不能把所有任务的全量历史放进自己的上下文窗口，又需要在用户跨信道提起某个旧任务时找回上下文。

把 Inline & Spill 一致地应用到系统中的各类对象后，就得到一种**按需展开的多跳检索机制**，原文称为**链式召回（Chain of Recall）**，它缓解了 Router 层在长程运行中丢失先前信息的问题。

机制很简单：**为 Trigger、Thread 和 Worker Session 等对象赋予唯一 ID，并在注入时内联紧密关联对象的 ID**，这些对象在底层因此构成一张引用图。由于每一次工具召回的对象自身同样遵循 Inline & Spill，其返回结果中又会内联下一跳关联对象的摘要与 ID，模型便可以**从当前事件出发，顺着引用关系逐跳展开上下文**。

原文给出的逐跳示例（同时也是图示中的 ID 链路）：

```
Trigger #42（内联注入）
├─ 前 50 字与相邻消息（超出部分 Spill 截断）
├─ thread_id: #08 ──read_thread(#08)──► Thread #08（Inline & Spill）
│                                        ├─ 近期事件摘要（更早历史 Spill 截断）
│                                        ├─ trigger_ids: [#39, #40] ──read_trigger(#40)──► Trigger #40 详情
│                                        └─ worker_ids: [#02, #03] ──read_worker(#03)──► Worker #03 状态
└─ worker_id: #03（与上面同一条路径）
                                          Worker #03 状态
                                          ├─ 任务进度与状态摘要
                                          └─ thread_id: #15 ──read_thread(#15)──► Thread #15 详情（关联 Pull Request）
```

文字描述即：从一条当前消息读取所在 Thread，再从 Thread 内联的 ID 追踪到相关的 Worker Session，进而找到该 Worker 关联的另一个代码评审 Thread。每一跳的返回结果都"同样遵循 Inline & Spill"，所以展开是**渐进且有预算约束**的。

## 在架构中的位置

| 环节 | 归属 | 作用 |
|------|------|------|
| 事件注入的密度与结构 | Worker 会话内部 | 决定单次任务要调用多少次工具、上下文是否被稀释 |
| Router 的长程上下文找回 | Router 层 | 链式召回让 Router 不必持有全量历史，仍能跨信道找回旧任务 |
| 事件流组织 | [[agent-event-driven-triggering]] | Thread 是引用图的主要骨架之一 |

原文把长程记忆列为开放问题：链式召回主要解决**近期任务与会话历史**的按需检索；当运行周期拉长到跨月甚至更长时，长程记忆仍是制约可扩展性的主要瓶颈（见 [[agent-memory-system]]）。

## 相关页面

- [[super-agent]] — Super-agent 总览：上下文组织对应"连续"与"高效"的底层支撑
- [[super-agent-dual-layer-architecture]] — 双层架构：Router 的全局记忆与 Worker 的隔离上下文
- [[agent-event-driven-triggering]] — 事件驱动：Trigger 与 Thread 构成引用图的骨架
- [[speculative-execution-agent]] — 投机执行：另一种用后台算力换人类等待的取舍
- [[agent-memory-system]] — Agent 长短期记忆：链式召回之外的长程记忆问题
- [[google-context-engineering]] — 上下文工程白皮书：上下文预算与注意力的一般原则
- [[manus-context-engineering]] — Manus 上下文工程实践：另一条"按需加载"的工程路线
- [[ai-agent/context-eng/index]]
