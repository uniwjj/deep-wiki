---
title: OpenSpec SKILL 文件机制
description: SKILL.md 如何定义 Agent 的行为方式——触发条件、执行步骤、输出格式，以及与 schema.yaml 的协同关系
aliases: [SKILL.md, skill mechanism, Agent操作手册, SKILL文件]
tags: [ai-agent, concept, tool]
sources: [2026/06/17/openspec-custom-schema.html]
created: 2026-06-17
updated: 2026-06-17
---

# OpenSpec SKILL 文件机制

Schema 定义了工作流的结构（生成什么文档、什么顺序），但 **SKILL 文件**定义了 Agent 的行为方式——触发条件、执行步骤、输出格式。两者协同工作，互补不重叠。

## 生成与加载机制

```
openspec init --tools claude --profile core
        │
        ▼ 生成
.claude/skills/
├── openspec-propose/SKILL.md
├── openspec-apply-change/SKILL.md
└── openspec-archive-change/SKILL.md
        │
        ▼ 加载（会话启动时）
Claude Code 上下文
  → 所有 SKILL.md 注入上下文
  → Claude 知道如何响应 /opsx:* 命令
```

`openspec init` 根据 `--tools` 参数（如 `claude`）自动生成对应平台的 SKILL 文件。这些文件在 Claude Code 会话启动时被读取并注入上下文，使 Claude 在用户输入 `/opsx:propose` 时自动知道执行什么步骤。

## SKILL.md 内容结构

以 propose 为例，SKILL.md 是一段结构化的自然语言 + 伪代码：

```markdown
# SKILL: OpenSpec Propose

## Trigger
Use when: user runs /opsx:propose [change-name]
Also use when: user says "start a new change", "create change"

## Steps
1. Run `openspec new change $ARGUMENTS`
2. Loop until all apply.requires artifacts are done:
   a. Run `openspec status --change {name} --json`
   b. Find first artifact with status "ready"
   c. Run `openspec instructions {artifact-id} --change {name} --json`
   d. Generate artifact content based on instructions
   e. Write file to the path in artifact.outputPath
3. When all apply.requires done:
   Report completion, suggest /opsx:apply
```

### 关键元素

| 元素 | 说明 |
|------|------|
| **Trigger** | 定义何时激活该 SKILL——斜杠命令和自然语言触发词 |
| **Steps** | 结构化的执行步骤，包含 CLI 命令和决策逻辑 |
| **循环结构** | "Loop until" 模式驱动 Agent 反复检查状态、执行操作 |
| **输出指引** | 完成后做什么——报告、引导下一步 |

Agent 把 SKILL.md 当成**执行手册**，逐步推进，而不是自由发挥。

## Schema vs SKILL 的分工

| 维度 | schema.yaml | SKILL.md |
|------|------------|----------|
| **控制什么** | 生成什么文档、按什么顺序 | Agent 如何行动、何时触发 |
| **定义方式** | 声明式 DAG（artifact + requires） | 过程式步骤（Trigger → Steps） |
| **执行者** | CLI（`openspec status` / `instructions`） | Agent（读取 SKILL 后按步骤执行） |
| **可替换性** | 项目级定制（团队共享） | 平台级适配（Claude/Codex/Cursor 各自不同） |
| **存放位置** | `openspec/schemas/<name>/schema.yaml` | `.claude/skills/openspec-*/SKILL.md` |

**核心理解**：schema 控制"生成什么文档"，SKILL 控制"Agent 如何行动"。同一个 schema 在不同编码助手上可以有不同的 SKILL 实现（Claude Code 用 `.claude/skills/`，Codex 用 `.codex/skills/`）。

## 自定义 SKILL 扩展

SKILL 机制不仅用于内置命令，也可以用于扩展领域专用能力。例如数据仓库场景下的 `dw-verify` SKILL（详见 [[openspec-verify-extension]]）：

```
.claude/skills/dw-verify/SKILL.md
```

自定义 SKILL 遵循相同的结构模式：Trigger + Steps + Gate 逻辑，独立于 schema 但读取 schema 产出的 artifact 文件。

## 相关页面

- [[openspec-schema-mechanism]] — Schema 核心机制（DAG、状态机、命令映射）
- [[openspec-custom-schema-guide]] — 自定义 schema 创建与模板设计
- [[openspec-verify-extension]] — 利用 SKILL 扩展 verify 能力
- [[sdd-custom-workflow]] — SDD 薄编排中 SKILL 的 invoke 委托机制
- [[superpowers-framework]] — Superpowers 的 Skills 组合体系
