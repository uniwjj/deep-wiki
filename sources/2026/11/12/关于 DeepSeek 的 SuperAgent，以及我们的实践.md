---
ingested: 2026-11-12
wiki_pages: [superagent-prototype-internals, flock-collaboration-space, digital-employee, agent-multi-agent-collaboration, super-agent]
source_file: 关于 DeepSeek 的 SuperAgent，以及我们的实践.html
source_type: article-html
url: https://mp.weixin.qq.com/s/d6JQ8D3K2CIeOZxfM0s_3Q
title: 关于 DeepSeek 的 SuperAgent，以及我们的实践
author: 郑天（阿里云；本文摘自《Alibaba Cloud AI Agent Handbook》）
published: 2026-11-11 18:30
---

本文件是 sidecar，仅用于 `llm-wiki` CLI 追踪摄取状态；原始正文（含 34 幅正文配图）在同名 `.html` 文件中，双击可在浏览器阅读。

正文结构：01 DeepSeek 想解决什么问题（人的四个瓶颈 → 四个特征；行业先行做法对照表）→ 02 原理拆解（双层架构 / 事件驱动 Fast-Slow Path / Inline-Spill 与链式召回 / 全双工 / 计算状态环境分离 / 实验结果怎么读）→ 03 原型代码分析（概念对象；一条消息进系统的五步；五条设计判断；原型边界）→ 04 作者思考（数字员工四个判定问题与四级划分；Router 三份工作可否拆开；中心化的成本结构；中心化四瓶颈；中心只关心结果，含 Anthropic 多 Agent 与 Cognition《Don't Build Multi-Agents》之争）→ 05 协同空间 Flock 实践（四层架构；Channel 网；Sessionless；Quest；任务链自然长出；Quest 链召回；主动推进与互相唤醒；记忆从推到拉）→ 写在最后（架构趋同的两个趋势）。

配图复核（抽检实际画面，均为文字化架构图/流程图）：

- 图 3：人的四个瓶颈与对应设计（要人发起→主动/事件驱动；要人盯着→高效/Worker 自主推进；要人传话→连续/Router+Thread 订阅；要人在线→持久/计算状态环境分离+Watchdog）。
- 图 5：Super-agent 简化架构（Trigger → 两级过滤 Fast/Slow Path → Router 层 → Worker Session 1..N 与订阅直通；执行环境 daemon；Session 数据库；Watchdog）。
- 图 12：原型核心对象关系（Trigger 事件 → Thread 事件流 → Ticket 工单 ← Policy 规则 / Mandate 授权 → Executor → Report）。
- 图 13：一条消息的旅程五步（进门登记 → 查 owner → Gate 门禁 → Router 研判 → 一次性开单），整条链路只有 Router 研判一步调模型。
- 图 14：模型负责提议 / 确定性核心负责裁决落账（唯一能改 Ticket 状态、一次操作要么全成要么全不成、人/模型/外部系统同一入口）。
- 图 15：先记账再动手（状态改动与待办外部动作记同一笔账，提交后后台逐条执行）。
- 图 16：规则优先级金字塔（代码强制 > 系统提示词 > 命中的规则 > 团队约定 > 外部文本）。
- 图 17：数字员工四级划分（问答机器人 → 带工具的 Agent → 后台任务 → Always-on 数字员工）。
- 图 18：Router 的三份工作拆给四样东西（Channel 时间线 / 派发器 / Coordinator / Quest 链+文件）。
- 图 24：Flock 四层架构与七步运行过程（产品层 / 协作控制面 / 执行面 / 基础能力）。
- 图 27：Quest 生命周期（待开始 → 执行中 ⇄ 等人回复 → 待验收 → 已完成，要求修改则再执行一次、上一版留着）。
- 图 33：记忆从推到拉（以前推上下文全量，现在只推任务 + 唤醒消息 + 文件清单，其余用工具自查）。

图 1、2、4、6–11、19–23、25–26、28–32、34 为示意性架构图/场景图，已确认其存在与编号，未逐张写入 wiki 正文。
