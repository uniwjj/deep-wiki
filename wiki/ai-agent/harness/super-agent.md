---
title: Super-agent：持续在线的自主 Agent 形态
description: DeepSeek Harness 团队提出的 Super-agent 架构——打破 Agent 与单次会话的绑定，用双层架构、事件驱动、面向人的交互、计算/状态/环境分离与 Watchdog 五个模块，支撑"连续、高效、主动、持久"四个特征
aliases: [Super-agent, Super Agent, 超级智能体, 超级 Agent, 持续在线 Agent, 自主协作者]
tags: [ai-agent, architecture, concept]
sources: [2026/11/03/Super-agent 设计探索：迈向更自主的 Agent 系统.html, 2026/11/12/关于 DeepSeek 的 SuperAgent，以及我们的实践.html]
created: 2026-11-03
updated: 2026-11-12
---

# Super-agent：持续在线的自主 Agent 形态

**Super-agent** 是 DeepSeek Harness 团队提出的一种 Agent 形态及其系统架构：让 Agent 不再绑定于单次手动会话，而是成为**能跨会话保持上下文、同时推进多项复杂任务且互不干扰的连续实体**。

> 本文是一篇面向工程实践的架构思路探讨，并非对 DeepSeek Harness 现有产品功能的介绍。

原文由 DeepSeek Harness 团队的 Yujia Qiao 执笔，2026-10-31 发表于「DeepSeek 产品部」公众号。作者的自我定位是：**没有提出全新的理论，而是把计算机系统中经典的工程思想应用到 Agent 系统的构建中**（详见 [[super-agent-classic-cs-mapping|Super-agent 的经典计算机系统映射]]）。

## 问题起点：多任务的上限是人的注意力

单任务 Agent 已经证明了模型能力，变化发生在瓶颈的位置：

- 以 Coding Agent 为代表的单任务 Agent 证明模型能用工具解决复杂问题，且越来越少需要人类持续监控和逐步干预；多 Agent 协同能力同步提升，使跨会话的组织与调度变得可行——人的角色正从底层执行上移到更高层的决策与验收。
- 新形态出现：个人助手（Personal Agent）集中涌现，直接接入日常的消息、日程、邮件与工作平台，持续观察信息流并协助处理事务。这种**持续在线、事件驱动**的工作模式并不局限于个人场景，在软件开发等商业场景同样有价值。
- 但现有主流 Agent 设计仍以**单一 Session** 为基本单位，不同 Session 彼此独立。用户并行推进多项工作时，必须手动切换并记住各 Session 的上下文；据作者观察，**一个人通常只能同时跟进 3 到 5 个活跃 Session**。

> 单任务的上限取决于模型能力，多任务的上限取决于人的注意力。

在本地单会话 Agent 上借助 CLI、MCP 或外部连接器做简单扩展也不够——Agent 虽然能接入外部消息，仍受制于三个问题：

| 问题 | 表现 |
|------|------|
| 单会话同步阻塞 | 一个工具或任务未完成前，会话无法接受新指令 |
| 多任务上下文混乱 | 多项工作的中间细节混在同一条会话里 |
| 本地设备离线即停止 | Agent 与设备生命周期强耦合，且需要大量人工维护 |

## 四个基础特征

| 特征 | 定义 | 原文示例 |
|------|------|---------|
| **连续** | 用户直接在 IM 聊天、GitHub 评论等熟悉的工作界面与它交互，感觉始终只存在一个 Agent，不需要去想"Session X 的上下文中是否有 Y 信息" | 用户先在 IM 上提出采用方案 A，随后在 GitHub Pull Request 评论里改为方案 B，它能理解并调整执行方向 |
| **高效** | 交互响应迅速，同时能并行、高质量地完成多个复杂任务 | 无需等上一个命令或任务完成，就能接受下一条指令 |
| **主动** | 无需等待用户逐条下达指令，持续观察环境事件并主动推进值得处理的事项 | 不需要介入时保持安静 |
| **持久** | 始终在线、直接接入用户真实的工作环境，长期运行中遇到异常时自动恢复 | 原文本节未举例；跨设备与间歇在线设备的能力见第 5 节 [[agent-compute-state-separation]] |

**单个 ReAct Loop 无法同时支持上述四个特征**，因此底层需要一套相应的架构——这是整篇文章的出发点。

## 五个模块

| 模块 | 支撑的特征 | 核心机制 | 详见 |
|------|-----------|---------|------|
| **双层架构** | 连续 + 高效 | 常驻 Router 层统一承接人类交互与全局协调；上下文隔离的 Worker 层并发执行具体任务 | [[super-agent-dual-layer-architecture]] |
| **事件驱动** | 主动 | 环境事件（Trigger）成为主输入：两级过滤 → 订阅直通路由 → Inline & Spill 上下文组织 → 投机执行 | [[agent-event-driven-triggering]] |
| **面向人的交互** | 连续 + 高效 | 异步 ReAct Loop（`start()` / `stop()`）消除后台耗时操作对前台对话的阻塞；按信道带宽定制输出形态 | [[async-react-loop]] |
| **计算、状态与环境分离** | 持久 | 计算放入云端无状态弹性容器、状态入数据库；执行环境留在原地，由端侧轻量 daemon 按需接入 | [[agent-compute-state-separation]] |
| **Watchdog** | 持久 | 主系统之外独立的 Control Loop，定时消息驱动，同时监控基础设施存活与任务进展 | [[agent-watchdog]] |

整体系统架构（图中标号 [2]–[6] 对应原文第 2–6 节）：

```
 [3] 环境事件(Trigger)      [2] Router 层                 [4] 多信道交互
  群消息/PR/CI ──► [3] 两级过滤 ──► 共享上下文、全局        ──► IM/GitHub/邮件
                    Fast/Slow Path    路由与交互
                          │                    │
                          │ 订阅直通            ├──► [2] Worker Session 1（隔离上下文）
                          └────────────────────┤
                                               └──► [2] Worker Session N（隔离上下文）
                                                      │                      │
                                                      ▼                      ▼
                                          [5] 执行环境              [5] Session 数据库
                                          轻量 daemon 原地接入       持久化所有 Session 状态
                                                                              ▲
                                                [6] Watchdog（外部监控与自愈）┘
```

（示意图按原文架构图重绘：标号 [2]–[6] 对应原文第 2–6 节；虚线与实线的区分从略。）

## 两个贯穿全文的判断

1. **相似的设计约束会导出相似的架构选择。** 无论是面向个人的助手，还是面向软件开发等商业场景的 Agent，只要目标是"构建一个持续在线、并发响应环境事件的系统"，底层就可能采用相近的架构；具体落地会有不同变体。
2. **计算机系统的经典设计思想同样适用。** 这类系统面临的问题与传统计算机系统长期探讨的问题高度相似，因此 Fast/Slow Path、Cache 与局部性、投机执行、存算分离、外部控制回路等经典思想可以直接迁移。

## 落地状态

- **内部实例**：基于 DeepSeek Harness 运行的 Super-agent 实例已在持续运行，日常参与问题复现与修复、代码审查响应与 DeepSeek Harness 自身的迭代。原文给出的两个实例：同一个开发任务在 IM 群聊与 GitHub Pull Request 评审之间连续推进；持续关注公共群聊，在无人 @ 的情况下主动识别缺陷并提交修复 Pull Request。
- **原型**：部分设计已在 `dsh-superagent-prototype` 中实现（原文"阅读原文"指向 GitHub 仓库）。作者明确声明：这只是为验证技术路线而提供的原型，**并非 DeepSeek 的正式产品**。
- **异步任务机制**：已在 DeepSeek Harness 中实现，但尚未默认开启（见 [[async-react-loop]]）。

## 开放问题

原文第 8 节列出了架构在超出当前简化范围后需要解决的问题：

| 方向 | 问题 |
|------|------|
| 扩展双层架构 | 单 Router + 订阅直通 + Worker 间不直接通信是目前以较小工程代价覆盖多数任务的组合；规模扩大后 Router 何时水平切分、粒度如何划分、同层 Worker 何时需要直接通信、采用何种协作拓扑，均无定论。系统与模型的 co-design（针对事件判别、批量分派、上下文召回专项训练）值得探索；跨月以上的长程记忆是制约可扩展性的主要瓶颈 |
| 脱离 Session 后的人机协作 | 以聊天窗口为中心的交互在一定程度上是路径依赖；交互可能更多发生在领域特化界面（如直接在 GitHub Issue / PR 中工作）；界面上保留的"一个会话"在底层未必对应一个 Session；还需支持用户在后台执行过程中随时介入、接管或实时协同 |
| 上下文边界与 A2A | 组织在决定如何配备 Super-agent 时，实际上是在划分上下文共享与隔离的边界：需要共享的共用实例，需要隔离的各自配备。多个隔离实例之间（如个人助手与团队共用实例）的跨边界协同即 Agent 间通信（A2A，不特指某个协议），在互不共享上下文、各自代表不同权限边界的前提下如何安全可靠地通信仍需探索 |
| 安全与隐私 | 事件驱动扩大了不可信输入的暴露面。云端 Agent Loop + 端侧 daemon 的设计默认云端与端侧互信，一旦云端被入侵或提示词注入攻破，其连接的全部端侧设备都面临被控制风险；即使云端未被攻破，凭据进入模型上下文也有泄露风险，需要让 Agent **在不接触凭据本身的情况下使用凭据** |

## 定位归纳：注意力解耦与数字员工运行时底座

> 以下两点是对这套架构的归纳（非原文结论），用于对外说明时的快速定位。

1. **注意力与任务数解耦**：人只在决策或验收时出现，不必为每个任务分配一份注意力。原文的机制支撑是 Router 层作为唯一对外出口、Worker 隔离执行、IM 单次回复不超过 100 词。
2. **更贴近数字员工的运行时底座**：具备常驻、并发、自愈的运行能力；触发上有三条通道——**被动接收**（由人派任务）、**定时自主**（定时消息驱动，原文中 Watchdog 即此形态）、**主动识别**（环境事件 Trigger）。三条都有，才像一个在岗的员工；只有第一条，仍只是工具。

边界：原文的验证仍处于团队规模与合成负载（见 [[super-agent-dual-layer-architecture]]），身份、权限、考核等组织接口不在本架构范围内（见 [[digital-employee]]）。

## 相关页面

- [[super-agent-dual-layer-architecture]] — 双层架构：Router/Worker 的职责划分与消融实验数据
- [[agent-event-driven-triggering]] — 事件驱动：Trigger、两级过滤与订阅直通路由
- [[inline-and-spill]] — 上下文组织：把上下文窗口当 Cache，Inline & Spill 与链式召回
- [[speculative-execution-agent]] — 投机执行：不确定结果是否被采用时提前在后台尝试
- [[async-react-loop]] — 异步 ReAct Loop：全双工交互与多信道输出规范
- [[agent-compute-state-separation]] — 计算、状态与环境的两层解耦与 BYOM
- [[agent-watchdog]] — 外部 Watchdog：监控任务进展而非仅进程存活
- [[super-agent-classic-cs-mapping]] — 五个模块与经典计算机系统思想的逐项映射
- [[agent-harness-overview]] — Harness 六承重层：Super-agent 是 Harness 在"持续在线多任务"方向上的系统化
- [[superagent-prototype-internals]] — 原型的源码级拆解：Ticket/Mandate、五条设计判断与边界校正
- [[flock-collaboration-space]] — 协同空间 Flock：把 Ticket 做成 Quest 的企业实践与两处分歧
- [[sub-agent-vs-team]] — Sub-Agent vs Agent Team：Router/Worker 与 Agent Team 的边界
- [[distributed-agent-hosting]] — 腾讯云 Agent Runtime：同样以存算分离走向分布式托管
- [[claude-managed-agents]] — Anthropic 托管 Agent 的"脑手分离"，与本文第二层解耦同源
- [[digital-employee]] — 数字员工：身份、权限、绩效与组织化落地，与本页"运行时底座"定位互补
- [[ai-agent/harness/index]]
