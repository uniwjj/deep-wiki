---
title: Superpowers 框架
description: AI 编程 Agent 的工程纪律框架——7 步工作流（头脑风暴→Worktree隔离→任务拆分→子Agent驱动→强制TDD→代码审查→收尾），覆盖从需求到交付的完整链路
aliases: [Superpowers, AI编程纪律框架, Superpowers Skills]
tags: [ai-agent, tool, practice]
sources: [2026/04/02/Superpowers框架.md, 2026/05/15/AI 编程三剑客：Spec-Kit、OpenSpec、Superpowers 深度对比与实战指南.html, 2026/05/17/Claude Code安装Superpowers后代码通过率58%→91%：AI编程工程化拐点来了.html, 2026/05/24/Superpowers 实战指南：7 步流程 + 14 个技能 + 3 条铁律，搭建让 AI 编程更稳、更守规矩的工作流.html]
created: 2026-05-09
updated: 2026-05-24
---

# Superpowers 框架

## 定位

Superpowers 不是单点功能插件，而是一套**给 AI 编程 Agent 灌输工程纪律的方法论**。核心价值不是让 AI "会更多"，而是让 AI "不乱来"。

问题根源：AI 编程 Agent 像"手很快但不太稳的初级工程师"——不问需求、不做设计、不写测试就直接开写。

## 四大工程纪律

- 需求没清楚 → **不准直接写**
- 没做计划 → **不准直接冲**
- 没写测试 → **不算真正开始**
- 没验证通过 → **不准宣布完成**
- 没 review → **不算真正交付**

## 7 步工作流

完整流程及三种场景（新项目、老项目加功能、修 Bug）的裁剪指南见 [[superpowers-workflow-scenarios]]。

```
brainstorming → using-git-worktrees → writing-plans → subagent-driven-development → test-driven-development → requesting-code-review → finishing-a-development-branch
```

## 三条铁律

Superpowers 的硬性工程纪律，不可违反，详见 [[superpowers-iron-laws]]：

1. **没有失败测试就不写生产代码** — 写不出失败测试说明还没想清楚
2. **不做根因调查就不修 Bug** — 没查根因的修复是在碰运气
3. **没有新鲜验证证据就不做完成声明** — "刚验证过"才算数

## 内置技能分类

| 类别 | 技能 |
|------|------|
| **测试** | test-driven-development, verification-before-completion |
| **调试** | systematic-debugging |
| **协作执行** | brainstorming, writing-plans, executing-plans, dispatching-parallel-agents, subagent-driven-development |
| **Code Review** | requesting-code-review, receiving-code-review |
| **Git 管理** | using-git-worktrees, finishing-a-development-branch |
| **元技能** | writing-skills, using-superpowers |

## 安装

### Claude Code（推荐方式）

```bash
# 在 Claude Code 中执行
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

验证：看到 `brainstorm`、`write-plan`、`execute-plan` 三个命令即成功。

### OpenCode

```bash
git clone https://github.com/obra/superpowers.git ~/.config/opencode/superpowers
mkdir -p ~/.config/opencode/plugins ~/.config/opencode/skills
ln -s ~/.config/opencode/superpowers/.opencode/plugins/superpowers.js ~/.config/opencode/plugins/superpowers.js
ln -s ~/.config/opencode/superpowers/skills ~/.config/opencode/skills/superpowers
```

### Codex

```bash
git clone https://github.com/obra/superpowers.git ~/.codex/superpowers
mkdir -p ~/.agents/skills
ln -s ~/.codex/superpowers/skills ~/.agents/skills/superpowers
```

### 更新

```bash
cd ~/.config/opencode/superpowers && git pull
# 或
cd ~/.codex/superpowers && git pull
```

## 技能触发机制

Superpowers 的技能根据上下文**自动触发**，无需手动指定技能名称：

```
说出需求 → 分析请求类型 → 自动加载对应技能
    ├─ 新想法/需求 → brainstorming
    ├─ 有计划/要实施 → subagent-driven-development
    └─ 要审查代码 → requesting-code-review
```

## 自定义技能

### 技能优先级
1. **项目级技能**（`.opencode/skills/`）— 最高优先级
2. **个人级技能**（`~/.config/opencode/skills/`）— 中优先级
3. **Superpowers 技能**（内置）— 默认技能

### 团队技能示例
可在 `.opencode/skills/` 下创建 SKILL.md，定义团队专属规则（如数据库规范、API 设计约定），整个团队自动遵守。

详见 [[agent-skills-system]]。

## 核心技能详解

### brainstorming：苏格拉底式需求澄清

在任何代码出现之前，强制 AI 进入提问模式。你说"做个用户认证系统"，它追问：用户规模多大？支持哪些登录方式？要不要 2FA？密码策略？session 还是 JWT？

### writing-plans + executing-plans：2-5 分钟小块拆分

实测对比：让 AI 写带分页和权限过滤的 REST API。没装 Superpowers 时一口气写完，路由参数搞错一个，花 15 分钟定位 bug。装上后自动拆成数据模型、查询逻辑、路由层、测试 4 个子任务，第二个子任务跑单元测试时就自动发现参数缺失并修正。

### dispatching-parallel-agents：多核 AI 并行

子代理 1 搞用户模块、子代理 2 搞商品模块、子代理 3 搞订单模块，同时写代码，主代理最后整合。坑：子代理间依赖关系必须在 writing-plans 阶段提前定义好，否则 API 接口会不一致。

### TDD：红-绿-重构自动循环

这是代码通过率从 58% 拉到 91% 的核心原因。全程自动执行 RED → GREEN → REFACTOR。

### systematic-debugging：科学化诊断五步

复现问题 → 提出假设 → 逐一验证 → 找到根因 → 修复验证。实测：故意埋 N+1 查询问题，没装 Superpowers 时 AI 猜了两个方向全错。装上后先用 profiling 确认查询次数异常，定位到未使用 eager loading，一处修改解决。

### receiving-code-review：提交前自动拦截

代码写完后自动扫描：规范遵守、安全漏洞、性能隐患。问题在提交前被拦截，而非推到 CI 流水线再报错。

## 实测基准

| 指标 | 无 Superpowers | 有 Superpowers |
|------|---------------|---------------|
| CRUD 接口一次通过率 | 58% | **91%** |
| 人工修复轮数 | 平均 2 轮 | 平均 **0.2 轮** |
| 测试覆盖率 | 不保证 | **96%** |
| 8 接口 REST 项目耗时 | ~2.5h | **47 分钟** |

核心机制：AI 自己写的测试发现了自己写的 bug，人工修复变成了 AI 自修。

## 三种最佳场景

| 场景 | 效果 | 关键数据 |
|------|------|---------|
| **新项目从零开始** | brainstorming → plan → TDD → review 全流程 | 8 接口 REST 项目 47 分钟，覆盖率 96%，零人工调试 |
| **遗留代码仓库** | debugging 技能强制 AI 先理解再动手 | 2300 行 Node.js 老项目加新功能：零回归 bug |
| **团队协作** | 子代理并行 + 任务清单标准统一 | 代码风格一致、测试覆盖相同 |

## 局限

1. **平台兼容性**：目前 Claude Code 集成度最高，Codex 和 OpenCode 或多或少有技能触发不稳定的问题
2. **学习成本**：14 个技能不是"装了就变强"，需要理解每个技能的触发时机——比如 brainstorming 在改小 bug 时也跳出来提问会拖节奏
3. **Token 消耗**：完整跑通 brainstorming + plan + TDD + review 流程，Token 消耗是直接让 AI 写代码的 **3-4 倍**。对频繁使用的小团队，API 费用翻倍
4. **强流程化对小任务显得重**：修一个 typo 也走全流程就过了
5. **效果吃模型执行力**

## AI 编程工程化的拐点

Superpowers 标记了一个分水岭：AI 编程从"让 AI 写代码"进化到"让 AI 按工程标准交付软件"。

- 2023：代码补全
- 2024：对话式编程
- 2025-2026：Agent 自主编程——但自主不代表质量

Superpowers 给 AI 的自主能力套上软件工程的纪律约束。未来所有 AI 编程助手都会内置类似的流程引擎，比的不是代码生成速度，而是工程流程规不规范。

> "Superpowers 不是让 AI 更炫，而是让 AI 更像一个能长期合作的工程师。"

## 适用场景

Superpowers 强调**个体赋能**，适合快速原型、个人项目和探索性开发。在需要团队一致性的场景中，需搭配 [[openspec-sdd-practice]] 或 [[spec-kit]]（SDD 工具二选一），参考 [[ai-programming-tools-comparison]] 全面对比。

## 相关页面

- [[superpowers-workflow-scenarios]] — 完整 7 步工作流与三种场景裁剪指南
- [[superpowers-iron-laws]] — 三条铁律：测试、根因、验证
- [[superpowers-openspec-legacy-workflow]] — 与 OpenSpec 协同改造老旧项目的四阶段实战
- [[superpowers-openspec-pitfalls]] — 双框架组合的 7 个坑及自定义衔接 Skill
- [[ai-programming-tools-comparison]] — Spec-Kit、OpenSpec、Superpowers 三剑客全面对比
- [[spec-kit]] — GitHub 官方 SDD 框架（与 Superpowers 互补）
- [[superpowers-vs-openspec]] — Superpowers vs OpenSpec 哲学对比
- [[agent-tdd-workflow]] — Agent TDD 开发流程
- [[sdd-openspec-superpowers]] — SDD 规范驱动开发
- [[agent-skills-system]] — Agent Skills 能力体系
- [[claude-code]] — Claude Code 开发工具
