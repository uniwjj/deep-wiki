---
title: AI-native SDLC（AI 原生软件开发生命周期）
description: Anthropic Applied AI 团队的 AI 原生 SDLC playbook 总览——代码不再是瓶颈后瓶颈迁移到人速阶段、线性流程改为作品产物（intent.md/spec.md/plan.md）驱动 gate 的闭环、六大阶段对照、九大 play 与采纳顺序
aliases: [AI-native SDLC, AI 原生 SDLC, Agentic SDLC, AI SDLC, agentic software development, AI 原生软件开发生命周期 playbook]
tags: [ai-agent, sdd, concept, practice]
sources: ["2026/10/12/The AI-native SDLC playbook.html"]
created: 2026-10-12
updated: 2026-10-12
---

# AI-native SDLC（AI 原生软件开发生命周期）

> 来源：Anthropic 官方文章《The AI-native SDLC playbook》（claude.com/resources/articles，作者 Louis Claxton，2026-08-21）。文中自述这是 Anthropic **Applied AI 团队**在服务客户过程中、用于内部集成 Claude 的最佳实践合集。逐 play 细节见 [[ai-native-sdlc-plays]]，配套工具与治理见 [[ai-native-sdlc-toolkit]]、[[ai-native-sdlc-governance]]。

## 问题：代码不再是瓶颈

组织用 AI 写代码的速度一年前无法想象，但围绕代码的流程没有同步改变——多数工程团队仍保留同样的审批门、评审、交接和制度，把 agentic coding（如 [[claude-code]]）带来的生产力增益卡住了。

传统 SDLC 的六个阶段（planning、design、building、testing、deploying、maintaining）每个是独立阶段、由不同角色拥有：产品经理写需求，技术架构师转成设计，工程师实现，受监管企业的 QA 验证，发布团队发布，运维盯着线上；工作通过文档、工单、签字在阶段间流转。它的问题在于**为"写代码是最耗时最昂贵阶段"的时代设计**——PRD、估点仪式、产品安全评审都诞生于数周乃至数月的开发周期中用来强制对齐的手段；同时其控制机制**默认每一步由人执行**。

当 build 阶段的运行速度超过传统 SDLC 允许的范围时，文章给出三条断言：

1. **瓶颈迁移到 build 两侧**——主要是 plan、review/test、deploy 这些仍以人类速度运行的环节；
2. **控制机制与现实脱节、变得难以处理**——"逐行人工 review"在人写代码时成立，agent 写出大部分 diff 后就跟不上了；
3. **治理成本上升**——例外仍走每周/每月开会的委员会，成本增加。

文章以安全团队为例：安全团队按人类产出速度配置人员，agent 让代码产出成倍增加后，要么 review 队列堆积、要么代码未充分审查就上线；受监管组织两种都不能接受，所以安全与合规检查必须跟上 agent 的节奏。

配图（原文 figure）以时间轴呈现：Before agents 每个阶段都是人速；After agents build 塌缩到小时级，而 plan/design/test/deploy/maintain 长度不变，中间富余出 "cycle time reclaimed"。**结论：传统生命周期需要与实现阶段同等量级的转型。**

## 定义与核心机制

AI-native SDLC 是"**旧控制目标 + 新执行方式**"的重构流程：不是线性流程，而是**一个循环（loop）**，AI 嵌入每个点；它推动自动化交接与后续 play 的自动触发，解决传统 SDLC 阶段间手工、笨重交接的问题。文章指出同一趋势也被称作 agentic SDLC、AI SDLC 或 agentic software development，只是标签不同。

穿起整个循环的机制是**作品产物（committed artifact）**：

- 每个阶段以**向版本控制提交一个 artifact** 结束，下一阶段**读取它**开始：`intent.md` → `spec.md` → `plan.md` → diff 及其测试 → 带 review 结论的 PR → 事故记录；
- 早期阶段以 `.md` 文件为主，因为产品负责人和 agent 都能读同一份文件并行动；从 Build 起 artifact 是代码及其记录；
- **提交链即审计轨迹**：谁提出的、agent 产出了什么、谁批准的；
- 需要的判断永远由人负责，人的注意力随需要 review 的 artifact 走。

触发关系（原文）：accepted `intent.md` 触发 requirements & design pass；approved `spec.md` 触发 plan mode；merged PR 触发流水线；生产环境的 control band 破线写出下一条 `intent.md`——循环继续。

## 六阶段对照：传统 SDLC vs AI-native SDLC

原文对照表（"大多数组织处于两列之间的某处"）：

| 阶段 | 传统 SDLC | AI-native SDLC |
|------|----------|----------------|
| Plan | 委员会收集需求，工作坊和签字提炼，手工撰写 | Claude 直接从源头综合痛点，写入 `intent.md`——人类可读、机器可执行 |
| Design | 分析师写规格，设计师解析 | 需求与设计压缩进与 agent 的一次工作会话，由编码为 skill 的标准引导，git 版本化 |
| Build | 测试和代码手写，文档在主开发完成后补写 | 测试和代码由 AI 生成，机构知识以版本化、机器可读的 `CLAUDE.md` 和 skill 维护 |
| Test | 阶段边界的 QA 关卡 | 持续 evals 织入实现过程 |
| Deploy | 人逐行 review 代码，治理按 review 周期发生且经常不一致 | 多层 agentic review，人工只为受监管和关键代码保留；治理在 AI 行动时执行，hook 作为审批门 |
| Maintain | 人盯着生产环境找 bug | agents 监控线上部署；任何破线的 control band 被诊断并作为新 `intent.md` 写回循环 |

## 九大 play 与采纳顺序

playbook 的主体是分组在六个非线性阶段中的 **play（做法）**，每个 play 覆盖五件事：**什么变了 / 如何起步（Getting started）/ 具体执行步骤 / 治理考量 / 如何度量是否有效**。步骤是模块化的，组织可按自己的需要选择先转型哪些阶段；每个 play 在 "Prerequisites" 中列出依赖。

采纳顺序与阶段顺序**不是一回事**（原文配图注）：先采纳**"clay"（无前置依赖）的 play**——没有任何箭头指向它、不需要先做别的；其他 play 箭头指向的 play 就是要先采纳的。

| 阶段 | Play | 一句话 |
|------|------|--------|
| Plan | [[ai-native-sdlc-plays|Capture as intent.md]] | 想法一次捕获为版本化意图，取代 backlog/用户故事流转 |
| Design | Requirements and design | 需求与设计合并为一次会话，政策在写 spec 时即应用 |
| Build | Claude Code plan mode | 计划先行、批准后才动代码 |
| Build | Claude Code on auto mode | 护栏成熟后，例常工作默认 auto-accept，转向 artifact 审查 |
| Build | [[ai-native-sdlc-toolkit|The CLAUDE.md]] | 新人手册变成 agent 每次会话都读的文件 |
| Build | Skills as institutional knowledge | 机构知识编码为 skill（版本化、集中更新） |
| Build | Hooks as build-time guardrails | 确定性护栏：拦编辑、跑格式检查、防凭据进 diff |
| Build | Parallel sessions and subagents | 一个工程师并行多条工作流，重复工作转向 subagent |
| Test | Give Claude a feedback loop | 每个会话自检（测试/构建/截图）后再交给人 |
| Test | Continuous evals in CI | agent 配置的回归测试，成为 AI 时代的 stage-gate QA |
| Deploy | AI in the PR review loop | Claude 双向参与 review，人聚焦意图与风险 |
| Deploy | [[ai-native-sdlc-governance|Hooks as approval gates]] | 审批门表达为 hook（allow/ask/block），managed settings 兜底 |
| Deploy | CI/CD integration and deployment | 非交互运行、沙箱化、MCP 暴露部署、演练回滚 |
| Maintain | Closing the loop | 确定性检测脚本监测 control band 破线 → 无人在场触发 → 产出 `intent.md` |
| Maintain | Recurring codebase scans | 定时安全扫描（Claude Security），发现走正常 gate |
| Maintain | Claude on call with Claude Tag | Claude 以自己身份加入事故频道，第一响应 + 复盘 |

> 上表每一行的完整细节（传统对比、Getting started、执行步骤、示例、治理与度量）见 [[ai-native-sdlc-plays]]。

## 演进路径：从手工触发到闭环

- **起点**：每一步手工 prompt；终点是循环——每个 accepted artifact 触发下一道 gate，人的注意力集中在 gate 上、review agent 标出的内容，而不是从每个阶段从头开始；
- **Build 阶段的两级跳跃**（原文）：plan mode 是默认起点（Claude 在工程师接受计划前不能编辑文件）；当后续 play 的护栏成熟（调好的 `CLAUDE.md`、编码政策的 skill、阻断不安全动作的 hook、Claude 可运行的测试套件）后，**auto-accept 成为例常工作的默认**（前提：收窄的 `spec.md`、小爆炸半径、被测试覆盖的代码），工程师从"看 agent 每次编辑"转向"长自主会话后审查 artifact"；auto mode 与 worktree 结合进一步支撑个人与团队并行，是 Stage 6 闭环自主运行的基础；
- **Stage 6 的无头运行**（原文）：Maintenance 阶段以 headless 方式运行，阶段之间有独立置信 gate（确定性检查或对抗性 reviewing agent）决定前一阶段输出继续还是升级给人。

## 相关页面

- [[ai-native-sdlc-plays]] — 六大阶段 × 全部 play 的逐一详解
- [[ai-native-sdlc-toolkit]] — Claude Code 侧工具：CLAUDE.md、skills、hooks、并行会话与 subagent、反馈闭环、evals
- [[ai-native-sdlc-governance]] — 分层控制模型、受管设置、审计轨迹、存量系统与单一事实源
- [[ai-native-team-refactor]] — 天猫技术团队观点：串联者从人换成 Agent 的 AI Native 团队
- [[agent-multi-agent-collaboration]] — 多 Agent 协作机制（对照）
- [[claude-code]] — Claude Code 自身架构与能力
- [[claude-md-best-practices]] — CLAUDE.md 最佳实践
- [[harness-engineering-practice]] — 为 coding agent 构建受控工程系统
