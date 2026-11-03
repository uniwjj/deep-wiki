---
title: 事件驱动：Trigger、两级过滤与订阅直通
description: Super-agent 把主输入从"人类逐条提示词"换成环境中的半结构化事件（Trigger）；用程序层 Fast Path + 语义层 Slow Path 两级过滤噪音，用 Thread 订阅与直通把后续事件绕过 Router 直接路由给 Worker
aliases: [事件驱动 Agent, Trigger 机制, 两级过滤, Fast/Slow Path, 订阅直通, Thread 订阅, Event-driven Agent]
tags: [ai-agent, architecture, concept]
sources: [2026/11/03/Super-agent 设计探索：迈向更自主的 Agent 系统.html]
created: 2026-11-03
updated: 2026-11-03
---

# 事件驱动：Trigger、两级过滤与订阅直通

要让 Agent 具备主动性，首先要改变系统的驱动方式：**驱动 Agent 的主要输入不再是人类在 Session 中逐条键入的提示词，而是环境中的半结构化事件**。在 [[super-agent]] 中，这些事件被称为 **Trigger**。

| Trigger 来源 | 典型事件 |
|--------------|---------|
| IM | 群聊消息、话题回复 |
| 邮件与日程 | 会议邀约、待办提醒 |
| 代码平台 | Pull Request 提交、代码审查评论、CI 运行结果 |

通过监听并响应 Trigger，Agent 不必再像单 Session 模式下那样等人类下达指令才被动执行一次。围绕一条环境事件从进入到处理的生命周期，原文按四个递进问题展开：**如何过滤噪音（两级过滤）→ 如何路由给 Worker（订阅与直通）→ 如何高密度注入上下文（Inline & Spill）→ 如何提前行动（投机执行）**。本页覆盖前两个；后两个见 [[inline-and-spill]] 与 [[speculative-execution-agent]]。

## 核心矛盾：高噪音、低触发率

接入真实的群聊与代码平台后，环境事件流呈现显著的**"高噪音、低触发率"**特征——绝大多数日常消息并不需要 Agent 介入。如果对每个事件都调用大模型从头判断，延迟和算力成本都会难以承受。

原文的类比是：**大模型推理是一条灵活但高成本的 Slow Path，而把已经明确且稳定的高频判断固化为确定性代码，则构成低延迟、低成本的 Fast Path**。事件进入系统后的第一步因此是过滤——在调用大模型之前先用确定性策略筛除噪音，只把需要语义理解的候选事件交给模型。

## 两级过滤

| 层级 | 角色 | 判据 | 能力 |
|------|------|------|------|
| **程序层策略** | Fast Path | 事件来源、群组范围、用户或机器人白名单等确定性条件 | 直接通过代码做快速准入检查；可对不同来源分别设置"仅响应明确请求""作为候选事件""直接忽略"，在调用模型前滤除大部分无关消息 |
| **语义层策略** | Slow Path | 由提示词编写的策略 + 上下文 | 判断是否需要行动；同时是**面向人类的自然语言扩展点**，方便随时通过提示词开关或新增策略 |

原文示例（三条 Trigger 的走向）：

| Trigger | 内容 | 程序层 | 语义层 | 结果 |
|---------|------|--------|--------|------|
| Trigger 1 | 非监听群消息："中午去食堂还是点外卖？" | 拦截（不在群组范围 / 白名单外） | 不调用 | 滤除，无需 LLM |
| Trigger 2 | 用户群日常消息："新版本界面看着挺清爽的" | 放行，作为候选事件 | 判定为无需介入 | 静默 |
| Trigger 3 | 用户群缺陷反馈："切换会话时偶发白屏" | 放行，作为候选事件 | 判定需要行动 | 开始路由 |

```
Trigger ──► 程序层策略（Fast Path）──候选事件──► 语义层策略（Slow Path）──► 开始路由
            确定性规则准入                        LLM 按提示词判断语义
            × 滤除无关来源（无需调用 LLM）          × 无需介入的讨论保持静默
```

设计要点：Fast Path 与 Slow Path 的分工不是"要不要用模型"，而是**把稳定的高频判断固化成代码，把需要理解的部分留给模型**。这样既守住延迟与成本，又保留了用自然语言调整策略的灵活性。

## 事件路由：订阅与直通

第 2 节引入的全局 Router 层负责维持跨信道的连续性；但如果**每个后续事件仍需经由 Router 转发**，这个全局节点依然会承受大量负载，推高系统响应延迟。因此 Fast / Slow Path 同样适用于过滤后的事件路由：

- **全局 Router 层仅负责新任务的冷启动与跨任务协调**；
- **已确定归属关系的后续事件由程序直接发往对应的 Worker 会话**，无需经由 Router 层转发。

### Thread：事件流的组织单位

同一个 GitHub Pull Request、同一个 IM 话题或同一封往来邮件串下的连续事件构成一条事件流，即 **Thread**。原文示例：

| Thread | Trigger 序列 |
|--------|-------------|
| Thread #08（IM 群话题） | Trigger #39 话题首条消息：张三"切换会话时页面偶发白屏" → Trigger #40 话题回复：李四"复现了，控制台报空指针异常" → Trigger #42 话题回复：张三"@Agent 帮忙定位修复一下" |
| Thread #15（GitHub Pull Request #128） | Trigger #51 PR 提交 `Opened: "fix: 修复会话切换空指针"` → Trigger #54 CI 状态变更 `CI Check: "unit-test 运行失败"` → Trigger #58 代码审查评论 `Review: "这里需要补充空值判断测试"` |

### 两条路径

Router 层根据某个 Thread 的初始事件创建 Worker Session 后，该 Worker 会**直接订阅对应的 Thread**，事件路由由此分为两条路径：

| 路径 | 流向 | 说明 |
|------|------|------|
| **已订阅（Fast Path）** | 同一 Thread 下的后续事件 → 程序直通已订阅的 Worker | 无需 Router 层再次调用工具转发。若新事件超出当前任务范围，Worker 也可取消订阅或交回 Router 层重新分派 |
| **未订阅（Slow Path）** | 新事件 → Router 层判断与分派 | Router 仍可观测所有事件以掌握全局状态，但仅负责判断和分派尚未被订阅的新事件 |

```
初始事件 ──Slow Path──► Router 层（创建 Worker 并建立订阅）
                              │
同一 Thread 的后续事件 ──Fast Path──► Worker Session（已订阅该 Thread）
```

### 多对多订阅与冲突消解

Thread 与 Worker 之间的订阅是**多对多**关系：一个 Worker 可以同时订阅多个相关 Thread，同一个 Thread 也可以被多个 Worker 订阅。

- **共享环境中的冲突**：当多个 Worker 在同一个工作环境或讨论中发生交集时，该 Thread 下的新事件会同时直通给所有订阅它的 Worker，使它们无需点对点通信，也有机会获知彼此的操作并自行解决冲突；
- **输出侧不走直通**：Worker 仍将关键结论汇报给 Router 层，由 Router 统一向用户回复并同步全局记忆。这与 [[super-agent-dual-layer-architecture]] 中"Worker 不直接与人交互"的约定一致。

## 与其他设计的关系

- **主动性的来源**：Trigger 驱动的意义是把 Agent 从"等指令"变成"观察环境、自主判断是否值得介入"，并在不需要时保持安静——这正是 [[super-agent]] 四个特征中的"主动"。
- **与投机执行的衔接**：过滤通过后，Agent 可以在结果是否被采用尚不确定时提前在后台尝试，见 [[speculative-execution-agent]]。
- **与上下文注入的衔接**：事件进入 Worker 后，注入多少信息由 Inline & Spill 决定，见 [[inline-and-spill]]。

## 相关页面

- [[super-agent]] — Super-agent 总览：事件驱动支撑"主动"特征
- [[super-agent-dual-layer-architecture]] — 双层架构：Router/Worker 的职责边界与直通的 token 收益（去掉直通后 Router 决定数 +30%、token +37%）
- [[inline-and-spill]] — 上下文组织：把上下文窗口当 Cache 的 Inline & Spill 与链式召回
- [[speculative-execution-agent]] — 投机执行：不确定结果是否被采用时提前在后台尝试
- [[agent-harness-overview]] — Harness 承重层：事件驱动对应"主循环"之外的驱动层重构
- [[ai-agent/harness/index]]
