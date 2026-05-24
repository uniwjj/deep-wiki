---
title: Superpowers 仓库架构
description: Superpowers 仓库的内部架构分析——skills/agents/commands/hooks 四层结构、跨平台分发设计、Skill 作为软件资产的工程化思维
aliases: [Superpowers 仓库结构, Superpowers Repo Architecture, Superpowers 内部架构]
tags: [ai-agent, tool, architecture]
sources: [2026/05/24/开源项目 superpowers 深度解读：把 AI Coding Agent 变成遵守工程流程的协作伙伴.html]
created: 2026-05-24
updated: 2026-05-24
---

# Superpowers 仓库架构

Superpowers 不是一个典型前端应用或 Node 服务。其 `package.json` 极其简洁（仅声明 `version: 5.0.6` 和插件入口），真正的核心内容在 skill 文档与平台配置中。

## 目录结构

```
superpowers/
├── skills/          # 核心——14 个结构化技能模块（SKILL.md）
├── agents/          # 专用角色定义（如 code-reviewer.md）
├── commands/        # 快捷命令入口（逐步收敛至技能式入口）
├── hooks/           # 会话生命周期钩子（如 SessionStart）
├── docs/            # Codex/OpenCode 适配文档、历史设计
├── tests/           # 技能行为的自动化测试
├── .claude-plugin/  # Claude Code 插件元信息
├── .cursor-plugin/  # Cursor 插件元信息
├── .codex/          # Codex 安装说明
├── .opencode/       # OpenCode 插件入口
├── gemini-extension.json  # Gemini CLI 扩展声明
└── GEMINI.md        # Gemini 上下文文件入口
```

## 四层能力模块

### skills/ — 行为操作系统

每个 skill 是一个结构化 Markdown 文件，包含：

| 组成部分 | 说明 |
|---------|------|
| frontmatter 元数据 | `name` + `description`（仅描述触发条件） |
| 触发条件 | 何时使用该技能 |
| 核心原则 | 不可违反的铁律 |
| 执行流程 | 步骤化的行为指导 |
| 红线与反模式 | 必须禁止的行为 |
| 示例与注意事项 | 正反案例 |

**关键设计细节**：`description` 只能描述"何时使用"，不能总结流程。因为模型会走捷径——如果 description 直接总结了流程，模型可能只读 description 而不读正文。

### agents/ — 专用角色

定义可被当作子代理调用的专门角色，如 `code-reviewer.md`。

### commands/ — 快捷入口

提供 `/brainstorm`、`/write-plan`、`/execute-plan` 等命令快捷方式。部分已标记 deprecated，体现从"命令式"向"技能式"入口的收敛趋势。

### hooks/ — 生命周期注入

通过 `SessionStart` 等钩子将行为前置到会话启动时，而非仅靠手动调用。这让技能体系不再是被动等待调用，而是主动介入。

## 跨平台分发设计

Superpowers 面向六个平台，通过分层抽象实现：

| 层级 | 内容 |
|------|------|
| 核心层 | skill 内容（与平台无关的 Markdown） |
| 元数据层 | frontmatter（平台通用的发现与匹配） |
| 角色层 | agents/ 目录下的角色定义 |
| 配置层 | 各平台 plugin.json / extension.json |
| 入口层 | hooks / commands / GEMINI.md |

各平台接入方式不同：
- **Claude Code**：通过 plugin marketplace 注册
- **Cursor**：通过 `.cursor-plugin/plugin.json` 声明 skills/agents/commands/hooks 入口
- **Codex**：通过 `~/.agents/skills/` 符号链接发现
- **OpenCode**：通过 plugin 数组 + hook 注入
- **Gemini CLI**：通过 `GEMINI.md` 指定上下文文件

## using-superpowers：总控开关

这是整套体系的入口技能，核心要求极为激进：

> **只要有 1% 的可能某个 skill 适用，就必须先检查并调用 skill。**

它明确将以下想法列为 red flags（模型绕开流程的常见借口）：
- "我先问点问题再说"
- "我先看看代码再决定"
- "这任务太简单，不需要 skill"

没有这个总控 skill，其他 skills 容易沦为"存在但不使用"的摆设。

## 两条执行路径

| 路径 | 模式 | 特点 |
|------|------|------|
| `subagent-driven-development` | 团队协作模式 | 每任务新子代理 + 规格审查 + 质量审查，精度最高 |
| `executing-plans` | 单人施工模式 | 当前会话按计划逐步执行，更通用但精度略低 |

前者强调 `fresh subagent per task` + 先规格合规再代码质量；后者作为无子代理支持时的替代方案。

## receiving-code-review：反表演式认同

这个 skill 明确反对以下表达：
- "你说得对"
- "非常好的建议"
- "我这就改"

要求模型收到 review 后：先完整阅读 → 理解反馈 → 回到代码验证 → reviewer 对则改，不对则做技术性反驳。

> review 不是情绪表演，而是技术判断。

## writing-skills：把 Skill 当软件资产

写 skill 的流程遵循类似 TDD 的方式：

1. **设计压力场景**：看看没有这个 skill 时模型会怎么失败
2. **写 skill 修复失败模式**：堵住漏洞
3. **验证 skill 是否真起作用**：测试

同时要求控制 token 长度、优化搜索关键词、避免 description 取代正文。

## 核心设计洞察

1. **Prompt Engineering 软件工程化**：模块化、可分发、可测试、可持续演化
2. **围绕 AI 弱点设计**：爱跳步骤、爱拍脑袋、爱跳过验证、爱过早实现、爱模糊发挥
3. **流程治理优于模型能力**：提升 AI 编码质量不一定靠更强的模型，可以靠更强的流程
4. **多代理是流水线不是噱头**：何时该拆、怎么给上下文、review 顺序、何时并行都有明确原则
5. **证据先于结论**：TDD、debugging、verification、review 底层都在强调"先拿证据，再下结论"

## 局限

- **流程重**：对简单任务显得"官僚"
- **依赖平台能力**：子代理、skill 发现、hook 完善度直接影响效果
- **不适合快速试错**：原型优先的场景会觉得被束缚
- **强规则可能导致机械执行**：过度拘泥、对简单任务反应迟缓

本质上是一种取舍：用灵活性换稳定性，用速度换可控性。

## 相关页面

- [[superpowers-framework]] — Superpowers 框架综述与工程纪律
- [[superpowers-workflow-scenarios]] — 7 步工作流与场景裁剪
- [[superpowers-iron-laws]] — 三条铁律详解
- [[superpowers-vs-openspec]] — Superpowers vs OpenSpec 哲学对比
- [[sdd-openspec-superpowers]] — SDD 双框架操作层面对比
