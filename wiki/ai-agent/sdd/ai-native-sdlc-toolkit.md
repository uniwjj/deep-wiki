---
title: AI-native SDLC 的 Claude Code 工具链
description: AI 原生 SDLC 中 Claude Code 侧的五类工程手段——CLAUDE.md（会话上下文）、Skills（机构知识可操作化）、Hooks（确定性护栏）、并行会话与 subagent（并行化）、反馈闭环与 evals（自检与配置回归测试），及 plan mode → auto mode 的演进路径
aliases: [AI-native SDLC toolkit, SDLC 工具链, CLAUDE.md 与 skills 与 hooks, auto mode 演进]
tags: [ai-agent, sdd, tool, practice]
sources: ["2026/10/12/The AI-native SDLC playbook.html"]
created: 2026-10-12
updated: 2026-10-12
---

# AI-native SDLC 的 Claude Code 工具链

> 来源：Anthropic《The AI-native SDLC playbook》（2026-08-21）Build / Test 阶段的工具侧要点整理。play 全流程位置见 [[ai-native-sdlc-plays]]，框架总览见 [[ai-native-sdlc]]。本文聚焦"每个会话与每个工程师手上直接用的东西"。

## 全景：五类手段与一句话分工

| 手段 | 层 | 一句话 | 确定性 |
|------|-----|--------|--------|
| [[claude-code-memory-system|CLAUDE.md]] | 会话上下文 | 新人手册：约定/命令/架构/常见错误，每次会话都读 | 建议性 |
| Skills | 机构知识 | 必须一致执行的政策编码为 `SKILL.md`，版本化集中更新 | 建议性（advisory） |
| Hooks | 执行护栏 | Claude 行动前运行的脚本，allow / ask / block | **确定性** |
| 并行会话 + subagent | 并行化 | 一个工程师驱动多条流；重复工作固化为 subagent | — |
| 反馈闭环 + evals | 自检 | 会话自检证据 + agent 配置的回归测试 | 证据来自工具链 |

核心原则（原文）：**skill 让违规罕见，hook 让违规近乎不可能**——"必须无例外成立"的政策，必须在 skill 背后配确定性层。

## CLAUDE.md：agent 每次会话读的"新人手册"

- 覆盖：约定、命令、架构、团队最常见的错误。曾在人脑/wiki 里的知识变成文件，由全团队维护、每次犯错后迭代。
- 执行：`/init` 生成初稿 → 削减到"新人第一天所需" → 提交到仓库根部 git（全团队同一版本、变更像代码一样 review）→ **同一错误犯两次就写进 CLAUDE.md** → 保持一页以内（会话开始时会读完全文，过时内容白占上下文）。
- 与 review 的联动：review findings 第二次出现的错误写回 `CLAUDE.md`；review 也会标出哪个变更让 `CLAUDE.md` 过时。
- 治理：受版本控制即可 review、可审计，code owner 在 PR review 中批准其变更。
- 度量：Leading = Claude 重复犯"CLAUDE.md 本应拦住"的错误的频率；Lagging = 新成员首个 merged PR 的时间。
- 展开参考：[[claude-md-best-practices]]。

## Skills：机构知识可操作化

- 形态：一个含 `SKILL.md` 的文件夹——frontmatter 说明**何时触发**，正文说明**做什么**。放仓库 `.claude/skills/<name>/` 随代码分发，或经 plugin 组织级分发。
- 边界（原文经验法则）：为"必须一致应用的机构知识"写 skill；属于 `CLAUDE.md` 或一次性 prompt 的内容不要写成 skill。
- 生命流程：从政策 owner 的 source of truth 撰写 → 测试触发（用不同方式让 Claude 做相关任务，确认每次都加载）→ 政策变化时改 skill 并由政策 owner 签字 → 工程师下次会话自动获得新版本。
- 定位：**建议性控制**——让 Claude 在写代码时"很可能"应用政策，没有机制强制。必须永远成立的政策要在背后配 hook 或 PR review pass。
- 治理：skill 调用记录在会话轨迹；政策 owner 像 review 代码一样 review skill 变更。
- 度量：Leading = 政策批准到 skill merge 的耗时；Lagging = 引用该政策的 PR review findings 数（应趋近零；不降则要么没触发、要么文本漂移）。

## Hooks：确定性护栏与审批门

**build 期护栏**（对每个匹配动作都运行，故要快、限定到变更文件）：

- 阻断对受保护路径的编辑（生成的类、冻结的包）；
- 编辑后跑 formatter/linter，不让漂移累积；
- 让凭据不进 diff；
- 保护测试：fix 任务期间阻断对测试文件的编辑（"修代码的 agent 不能削弱对这段代码的检查"）。

**审批门（ask）**：暂停动作直到特定人批准——发布 gate 的最清晰用例；也拦 build 期无变更单的 migration/infra 编辑。团队 hook 放 `.claude/settings.json` 入 git；**不可协商的 hook 放 managed settings**（管理员拥有，工程师无法关闭）。阻断要能解释自己：原因与获批路径出现在 Claude 输出里。

配置形态（原文示例，`.claude/settings.json`）：

```json
{
    "hooks": {
      "PreToolUse": [
        {
          "matcher": "Bash",
          "hooks": [
            { "type": "command",
              "command": "${CLAUDE_PROJECT_DIR}/.claude/hooks/production-gate.sh" }
          ]
        }
      ]
    }
}
```

受管设置全量与逐键解释见 [[ai-native-sdlc-governance]]。

## 并行会话与 subagent

- **并行会话**：另一个完整 Claude Code 实例、独立 git worktree、独立任务；会话间互不知晓，唯一共享是驾驶的工程师。
  - worktree 是独立分支上的独立 checkout，防止会话在文件上相撞（`claude --worktree feature-auth`、`claude --worktree fix-rate-limit`…）；
  - **起点 2–3 个会话**；实际上限是"一个人能认真 review 多少条流"——只在 review 跟得上时加；
  - 前提：`CLAUDE.md`（所有会话都读）、反馈闭环（会话能自检则需更少监督）、git 仓库、调好的权限设置（不让会话卡在组织认为安全的命令的审批上）。
- **subagent**：单会话内部的限定助手，有自己的上下文窗口与工具限制；适合跨任务重复的工作。定义在 `.claude/agents/` 的 markdown（name、何时使用、可触及工具），提交 git 共享。
- 原文给出的三类典型 subagent：code simplifier（主 agent 完成后剥除多余复杂度）、verifier（跑应用验证行为）、researcher（探索代码库回报而不淹没主上下文）。
- 分工：并行会话提高"在飞"任务数，subagent 让每个会话聚焦；工程师的工作是 steer 与 review 全部。
- 治理：会话越多产出越多，**控制必须来自仓库配置**（hook 与权限设置作用于所有会话）；会话所做之事记录并归属到运行它的工程师。

## 反馈闭环与 evals（Test 阶段工具侧）

- **反馈闭环**：给 Claude 一个自检方式——单一入口命令（`make test`，失败非零退出）、`CLAUDE.md` 中列命令与健康输出样例、可量化目标（"截图与 mock 相符"）；修 bug 先写失败测试再修；UI 用截图对比迭代（两三轮常态）；验证作为"done"的一部分。
- 与 verifier subagent 的区别：反馈闭环在整个任务中运行任意多次；verifier 是完成后用**全新上下文**打包最终检查，结论不被产出代码时的假设染色。
- **continuous evals**：agent 配置的回归测试——20–50 个真实任务 + 可接受性检查；在 `CLAUDE.md`/skills/hooks 任何变化时与按 schedule 在 CI 非交互运行；用通过率 gate 配置变更；**每次生产事故沉淀为一条 eval**。
- 完整步骤、示例工作流（agent-evals.yml）与度量见 [[ai-native-sdlc-plays]] Test 章。

## 演进路径：plan mode 是起点，auto mode 是结果

文章的两级跳跃：

1. **plan mode 作默认起点**：工程师给 Claude `spec.md`，让它采访自己、迭代计划——**接受计划前 Claude 不能编辑文件**（plan mode 自身强制"代码前先设计 review"）；计划 commit 为 `plan.md`。
2. **auto mode 作成熟态**：护栏成熟后（调好的 `CLAUDE.md`、编码政策的 skill、阻断不安全动作的 hook、Claude 可运行的测试套件），**auto-accept 成为例常工作的默认**——前提是收窄的 `spec.md`、小爆炸半径、测试已覆盖的代码。重心从"盯着每次编辑"转向"更长自主会话后审查 artifact"；配合 worktree 支撑并行，**是自主运行 SDLC、闭合循环（Stage 6）的基础**。

## 相关页面

- [[ai-native-sdlc]] — 总览：bottleneck 迁移、六阶段对照
- [[ai-native-sdlc-plays]] — 工具在六阶段 play 中的完整上下文
- [[ai-native-sdlc-governance]] — 受管设置、控制分层与合规
- [[claude-code]] — Claude Code 总体架构
- [[claude-code-memory-system]] — CLAUDE.md 记忆系统机制
- [[claude-code-permission-system]] — 权限系统机制
- [[claude-md-best-practices]] — CLAUDE.md 最佳实践（独立来源）
- [[agentic-skills]] — Agentic Skills 范式（数据场景）
