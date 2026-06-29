---
title: Claude Code 指令失效分析——五层规则架构
description: 诊断 Claude Code 为何忽略 CLAUDE.md 指令，提出五层规则架构（入口文件→路径规则→Skills→Subagent→硬边界），以及从臃肿 CLAUDE.md 收身的实操方法
aliases: [instruction failure, 指令忽略, 规则分层, rule layering]
tags: [ai-agent, practice, concept]
sources: [2026/06/28/我终于搞明白了：Claude Code 为什么会忽略指令了.html]
created: 2026-06-29
updated: 2026-06-29
---

# Claude Code 指令失效分析——五层规则架构

## 问题定义

实践中常见现象：CLAUDE.md 里写的规则明明没有删，Claude Code 却开始忽略。不是完全没读——往往前面还遵守得好好的，任务跑到后面，某条规则就像被挤到了角落里。

**根因不是模型变差了，而是把不同性质的规则都塞进了同一个入口文件。**

## 指令被忽略的四类根因

| 根因 | 表现 | 诊断方式 |
|------|------|----------|
| **规则没加载** | 子目录 CLAUDE.md 或带路径的 rules，Claude Code 读到对应路径时才加载 | 用 `/memory`、`/context` 查看当前会话加载了哪些指令 |
| **规则太虚** | "注意安全""保持简洁""写高质量代码"——Agent 知道要注意但不知具体动作 | 检查规则是否有明确对象、动作、验证方式 |
| **上下文挤压** | 长任务中大量工具输出、文件读取、补充需求逐渐占据上下文窗口，早期规则权重下降 | 观察任务是否在前期遵守、后期偏离 |
| **不该由模型记** | "Never modify .env""Never deploy without approval"——应该是系统硬边界 | 评估漏掉后的破坏性（返工 vs 删数据/泄露密钥/绕过发布） |

### 具体 vs 空洞指令对照

**有效的指令**（有对象、有动作、可验证）：
- API handlers validate input and shape responses only
- Services own business orchestration and transactions
- External HTTP and LLM calls must stay outside DB transactions
- Separate code changes, local checks, deployment status, and production evidence in summaries

**无效的指令**（听起来没错，Agent 无法执行）：
- Write clean code
- Follow best practices
- Be careful with security
- Keep the project organized

## 五层规则架构

将模糊的"Claude 没听话"拆开为：这条规则到底是**给模型看的**、**给流程调用的**，还是**给系统执行的**。放错层就会表现为"忽略指令"。

| 层 | 机制 | 适用内容 | 示例 |
|---|------|---------|------|
| **L1 入口文件** | CLAUDE.md（常驻上下文） | 项目定位、技术栈、高频错误假设、目录边界、完成证据 | "本仓库是电商 monorepo，12 个微服务，Java + TS" |
| **L2 路径触发** | 子目录 CLAUDE.md、`.claude/rules/` | 特定目录/文件类型的局部约定 | API handler 校验规范、migration 回滚说明 |
| **L3 流程资产** | Skills | 超过 5 步的可复用流程 | 发布、review、迁移、排障、归档 |
| **L4 上下文隔离** | Subagent | 产生大量中间材料的探索工作 | 全仓搜索、日志分析、依赖对比、安全审计 |
| **L5 硬边界** | Hooks, Permissions, CI, 脚本 | 事故成本高、不能靠自然语言兜底的约束 | 密钥保护、危险命令拦截、部署审批、生产数据操作 |

### 规则分流决策树

拿到一条新规则，先问：
1. **每次会话都需要？** → 是 → 放 CLAUDE.md
2. **只对特定目录/文件类型有效？** → 是 → 放子目录 CLAUDE.md 或 `.claude/rules/`
3. **是一个超过 5 步的流程？** → 是 → 做成 Skill，入口文件只留路由语句
4. **会产生大量中间材料？** → 是 → 交给 Subagent，主会话只接收结论和证据
5. **涉及 always/never/must/forbidden + 事故成本？** → 是 → **必须下沉到硬边界**（权限、Hook、CI）

## 各层详解

### L1: CLAUDE.md —— 入口工作卡

CLAUDE.md 像一张"进仓库前的工作卡"，应包含：
- 项目做什么、不做什么
- 最容易猜错的技术栈和目录边界
- 常用命令
- 什么证据才算完成
- 哪些地方过去反复出问题

**Anthropic 建议控制在 200 行以内。** 越像百科，越容易把关键内容淹掉。不建议把 CLAUDE.md 写成项目知识库——知识库放在 `docs/` 里，让 Agent 需要时自己读。

### L2: 路径触发规则 —— 局部约定不污染全局

局部规则不该让每个任务都加载。应放在子目录 CLAUDE.md 或 `.claude/rules/`：

```yaml
---
paths:
  - "src/api/**/*.ts"
  - "**/*.handler.ts"
---
# API rules
- Validate request input before business logic
- Return the standard error response shape
- Keep external calls outside database transactions
```

### L3: Skills —— 流程离开入口文件

发布流程（检查分支→跑测试→打包→部署→smoke→changelog）不是规则，是一套过程。过程放 CLAUDE.md 的问题：不发布时也加载；步骤越补越长；每个团队都想加特殊情况。

做法：CLAUDE.md 里只留路由语句 `For release work, use the release-check skill`，真正步骤放在 Skill 里。

### L4: Subagent —— 上下文卫生工具

Subagent 不只是"多一个助手"，核心价值是**上下文隔离**。探索工作（全仓搜索、日志分析、依赖对比）产生的大量中间材料不进入主会话，Subagent 只带回结论、证据和建议。主会话继续保留决策线。

### L5: 硬边界 —— 自然语言做提醒，系统机制兜底

一句话原则：**能让系统兜住的，就不要只写成自然语言。**

| 风险等级 | 处理方式 |
|---------|---------|
| 漏掉只返工 | 写进上下文 |
| 漏掉会删数据/泄露密钥/绕过发布 | 权限、Hook、CI、仓库保护 |

## 臃肿 CLAUDE.md 收身方法

接手几百行的 CLAUDE.md，按四步归类：

1. **每次会话都该知道的内容** → 留在入口文件（项目定位、技术栈、常用命令、目录边界、完成证据、高频错误）
2. **只对某些目录/文件类型生效** → 拆到子目录 CLAUDE.md 或 `.claude/rules/`
3. **超过 5 步的流程** → 考虑 Skill（发布、review、迁移、验证、排障、归档）
4. **大量中间材料的工作** → 交给 Subagent，主会话只拿摘要和结论

看到 **always/never/must/forbidden/production/secret/deploy/payment/delete** 这些词，多停一下——如果关系到事故成本，不要只留在自然语言里。

## 诊断口诀

Claude Code 忽略指令时，先不问"怎么改措辞"，先问：
1. **它现在真的看到了这条规则吗？**
2. **这条规则够具体吗？**
3. **它是不是只在某些目录下才相关？**
4. **它是不是一个流程？**
5. **它失败以后有没有事故成本？**

## 相关页面

- [[claude-code-behavior-contract]] — CLAUDE.md 行为契约：从 41% 到 3% 的规则工程
- [[claude-code-configuration-guide]] — 大型代码库配置实操指南（分层编写、Hooks/Skills 实例）
- [[claude-md-best-practices]] — Karpathy 的 70 行 CLAUDE.md 实测效果与陷阱
- [[claude-code]] — Claude Code 综述
- [[agent-harness-overview]] — Harness 六承重层综述
- [[loop-engineering]] — Loop Engineering 持续清理系统
