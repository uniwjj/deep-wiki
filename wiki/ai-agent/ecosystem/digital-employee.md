---
title: 数字员工
description: AI Agent 作为数字员工的概念、生态与演进趋势
aliases: [digital-employee, Digital Employee, 数字员工]
tags: [ai-agent, concept]
sources: [2026/05/10/lint-stub.md, "2026/09/07/重构协同：关于AI Native团队的思考.html", "2026/10/10/Multica 是什么——从源码看一个把 Agent 变成同事的系统.md"]
created: 2026-05-14
updated: 2026-10-10
status: draft
---

# 数字员工

数字员工是将 AI Agent 定位为组织中的虚拟劳动力——能够自主完成任务的智能体，而非仅仅是辅助工具。

## 核心理念

- **自主性**：从“工具”到“同事”的角色跃迁
- **可管理性**：与人类员工一样有角色、权限、绩效评估
- **规模化**：可快速复制和部署到不同业务场景

## 关键能力

- 理解复杂业务指令
- 调用企业系统与数据
- 人类-数字员工协作模式

## 协同形态视角：跳出数字员工本身（2026-09 补充）

天猫技术团队书牧在《重构协同：关于AI Native团队的思考》中对"数字员工"方案提出批评性视角：公司内已有不少数字员工方向的探索（Aone 团队的 Devix、Qteam 团队的 Qoder 系列、企业智能的数字员工账号、ideawork 等），但都还不完善、属于探索阶段。**现在很多号称"数字员工"的方案，应该跳出数字员工本身，核心把重心放在 Agent 和人的协同形态上**——数字员工对应的是协同形态中"参与者"这一个概念（空间里的人类成员和各类 Agent，关联技能和知识），而不是全部；完整的协同形态还包括业务空间、任务、频道、产物与知识底座等概念雏形，见 [[ai-native-team-refactor]]。

## 系统化形态：Multica「把 Agent 变成同事」（2026-10 补充）

Multica（约 52.3k stars、驱动 26 种 Agent CLI、source-available）把数字员工理念落成了一套系统（官方 slogan「智能体，也在看板上」）：Agent 在系统里有**身份**（看板上可被指派的成员）、执行有**常驻 runtime**（daemon 让注册的机器随时领活，不需要一台开着的终端）、状态在**共享 issue** 上（跨人、跨会话可回溯）。其对比"终端里跑 CLI"的核心论点：前者是工具，后者是"可被派活的成员"——"能调用工单系统"和"能被工单系统调用"是两件事。同时它保持"Agent 不会自己开工"的原则（指派 / @ / 聊天 / Autopilot 才触发），run 完成 ≠ issue 完成。详见 [[multica]]。

## 相关页面

- [[openclaw]] — 自托管 AI Agent 网关，从个人助手到数字员工原型
- [[claude-managed-agents]] — 托管式 Agent 部署
- [[multica]] — 数字员工的系统化产品实例：Agent 有身份、执行有常驻 runtime、状态在共享 issue
- [[ai-native-team-refactor]] — "数字员工方案应跳出本身、聚焦 Agent 与人的协同形态"的出处文章
- [[ai-plus-software-press-conference]] — 政策语境：智能体软件作为软件新形态的官方定义与培育路径
