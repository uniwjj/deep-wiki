---
title: Watchdog：主系统之外的外部控制回路
description: 长期在线的 Agent 系统需要主系统之外的独立 Control Loop——不仅监控进程存活，还要由模型判断各个 Worker 是否有实质进展、方向是否偏离目标；Watchdog 本身是一个具备完整工具能力的 Agent Session，可用自然语言询问故障、在线修正提示词与触发规则
aliases: [Watchdog, Agent Watchdog, 外部控制回路, Control Loop, 看门狗 Session, 自愈回路]
tags: [ai-agent, architecture, practice]
sources: [2026/11/03/Super-agent 设计探索：迈向更自主的 Agent 系统.html]
created: 2026-11-03
updated: 2026-11-03
---

# Watchdog：主系统之外的外部控制回路

[[agent-compute-state-separation|存算分离]] 解决了"算力不随设备休眠而消失"，但"持久"还有另一半：**系统在长期运行中能否从异常中自动恢复**。

原文的判断是：**持久在线的分布式系统很难仅靠主系统内部逻辑保证长期可靠运行，需要在主系统之外引入独立的外部控制回路，即 Control Loop。**

## 为什么进程存活探测不够

在 Agent 系统里，传统的进程存活探测或监控脚本只覆盖了一半故障：

| 故障类型 | 传统监控能否发现 | 说明 |
|---------|----------------|------|
| 进程崩溃、接口报错、状态恢复失败 | 能 | 基础设施层面的故障 |
| 模型**陷入无效循环** | 不能 | 进程仍然活着，但在空转 |
| 模型**偏离目标** | 不能 | 进程仍然活着，但在做错的事 |

> 许多异常并非进程崩溃，而是模型在执行中陷入无效循环或偏离目标。因此，长期运行的 Agent 系统不仅需要监控基础设施的存活，还需要监控**任务本身的进展**。

## 设计：独立的 Watchdog Session

| 属性 | 设计 |
|------|------|
| 位置 | 主系统**外部**，独立于 Router / Worker |
| 形态 | 一个独立的 **Watchdog Session**，由**定时消息驱动**，构成外部 Control Loop |
| 基础设施职责 | 处理接口报错、状态恢复失败等基础设施故障 |
| 任务职责 | **由模型判断各个 Worker 是否有实质进展、方向是否偏离整体目标** |
| 处置方式 | 发现异常及时介入修复；遇到外部偶发故障则**持续复查直至恢复正常** |

按职责粗分：Router 负责"任务之间的协调"，Worker 负责"任务之内的执行"，[[agent-watchdog|Watchdog]] 负责"整个系统是否还在正确地往前走"。

## 它本身就是一个 Agent Session

原文特别指出：由于 Watchdog 本身也是一个**具备完整工具能力的 Agent Session**，维护者可以：

- 用**自然语言**向它询问故障原因；
- 让它**在线修正主系统的行为**，例如增删提示词、调整触发规则。

这一点把"运维"从读监控面板变成了对话式诊断与在线调参——Watchdog 不只是报警器，而是带着工具、能改系统的维护者。

## 在架构中的位置

```
[3] 环境事件 ──► 两级过滤 ──► Router 层 ──► Worker Sessions ──► Session 数据库
                                                                     ▲
                                          [6] Watchdog（定时消息驱动）┘
```

按原文架构图，Watchdog 位于主执行链路之外，由定时消息独立驱动，并对系统状态（如 Session 数据库）做检查与干预。

与 [[loop-engineering]] 讨论的"把反馈循环放进工程现场"相比，Watchdog 是反馈循环在**系统运维层**的实例化；与 [[harness-design-long-running]] 中通过独立 Evaluator 抑制自我评估偏差的思路相通，但关注点从"单个任务质量"转向"系统的长期存活与方向"。

## 原文未给出的部分

以下问题原文没有展开，属于开放边界：

- Watchdog 的判断频率（定时消息的间隔）如何设定；
- Watchdog 自身的失效如何被发现（谁来看守看门狗）；
- "实质进展"与"方向偏离"的具体判据（由提示词或模型临场判断）。

## 相关页面

- [[super-agent]] — Super-agent 总览："持久"特征的最后一块拼图
- [[agent-compute-state-separation]] — 存算分离：持久在线的前一半
- [[super-agent-dual-layer-architecture]] — 双层架构：Router/Worker 的职责划分
- [[loop-engineering]] — Loop Engineering：把反馈循环放进工程现场
- [[harness-design-long-running]] — 长任务 Harness：用独立 Evaluator 抑制自我评估偏差
- [[ai-agent/harness/index]]
