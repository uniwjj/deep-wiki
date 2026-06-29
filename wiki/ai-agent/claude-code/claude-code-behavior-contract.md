---
title: CLAUDE.md 行为契约——从 41% 到 3% 的规则工程
description: 解析一份 CLAUDE.md 如何将 Claude Code 错误率从 41% 降至 3%——4 条基础规则、12 条扩展规则、四类工程问题映射、好规则的来源方法，以及可照做的最小实操流程
aliases: [behavior contract, 行为契约, 错误率降低, error reduction, rule engineering, 规则工程]
tags: [ai-agent, practice, comparison]
sources: [2026/05/11/Claude Code 错误率从 41% 到 3%：CLAUDE.md 到底改对了什么？.html]
created: 2026-06-29
updated: 2026-06-29
---

# CLAUDE.md 行为契约——从 41% 到 3% 的规则工程

## 关键数据说明

Forrest Chang 在 30 个代码库上 6 周测试，Claude Code 错误率从 41% 降至 3%。**注意：这是个人实验数据，非独立复现的论文。** 不同代码库、任务类型、模型版本、评判标准都会影响结果。把它当经验样本而非通用定律。

## CLAUDE.md 的工程定位

CLAUDE.md 不是神秘提示词，不是让模型强行听话的开关，而是一份**写在仓库里的工作约定**：把项目里反复出错的地方，提前告诉 Agent。

| 维度 | 提示词技巧 | 行为契约 |
|------|----------|---------|
| 作用范围 | 控制当下一次对话 | 仓库级别，每次会话继承 |
| 稳定性 | 随对话变化 | 随仓库演进 |
| 工程属性 | 临时提醒 | 持久上下文资产 |

Shopify CEO Tobi Lütke 更倾向于用 **context engineering** 而非 prompt engineering——核心技能是"给模型足够的上下文，让任务变得可解"。CLAUDE.md 属于最薄、最稳定的那层上下文。

## Agent 的五类典型偏差

CLAUDE.md 要防的，不是"语法写错"这类低级问题，而是**猜、扩、混、忘、装作完成**。

| 偏差类型 | 真实失败案例 |
|---------|------------|
| **猜**（静默假设） | Claude 把请求 body 当成 503 重试的决策上下文，重试策略抖动 |
| **扩**（过度复杂） | 存两套错误处理模式，Claude 写了一个"两边都照顾"的版本，结果错误处理器翻倍，错误被吞两次 |
| **混**（无关改动） | 文件附近已有旧函数做同一件事，Claude 没读到，又写了一个新的，新函数因 import 顺序取得优先级 |
| **忘**（缺乏验证） | auth 函数只返回一个常量，测试全绿；迁移脚本跳过命中约束冲突的记录，Agent 说"完成" |
| **装作完成** | 日志里有失败痕迹，Agent 没把这件事说出来，汇报为成功 |

## 规则体系

### 四条基础规则（Karpathy 原始约束）

1. **Think Before Coding** — 写代码前先说明假设，不要默默猜
2. **Simplicity First** — 先用最小方案解决问题
3. **Surgical Changes** — 只改必要范围，不顺手重构
4. **Goal-Driven Execution** — 围绕成功标准循环验证

这几条是资深工程师 code review 时常说的话。但人类被提醒一次会记住一阵子，Agent 换一个会话/目录/任务就可能从头开始猜——它无法稳定继承项目工作习惯。CLAUDE.md 把工作习惯固定到仓库里。

### 十二条扩展规则（Mnimiy 补充）

| # | 规则 | 一句话理解 |
|---|------|-----------|
| 1 | Think Before Coding | 不确定先说假设，不要默默猜 |
| 2 | Simplicity First | 先用最小方案解决问题 |
| 3 | Surgical Changes | 只改必要范围，不顺手重构 |
| 4 | Goal-Driven Execution | 围绕成功标准循环验证 |
| 5 | Model for Judgment Only | 模型做判断，确定性逻辑交给代码 |
| 6 | Token Budgets | 长任务要有预算，超了就停下来整理 |
| 7 | Surface Conflicts | 模式冲突要指出，不要折中平均 |
| 8 | Read Before Write | 写之前先读周边代码 |
| 9 | Tests Verify Intent | 测试要验证业务意图，不只验证返回值 |
| 10 | Checkpoint Steps | 多步骤任务要留下检查点 |
| 11 | Match Conventions | 跟随项目约定，不私自开新风格 |
| 12 | Fail Loud | 不确定、跳过、未验证，都要明说 |

### 四条工程维度重组

从 Harness 视角，12 条规则可以重组为四类工程问题：

| 工程问题 | 对应规则 | 解决的风险 |
|---------|---------|-----------|
| **控制改动半径** | 先思考、简单优先、手术式修改、先读再写 | 少猜需求、少顺手改、少重复造轮子 |
| **把确定性交还给代码** | 模型只做判断题、测试验证意图 | 不让模型充当随机 if-else，不让浅测试制造信心 |
| **长任务留下检查点** | token 预算、步骤检查点、冲突不平均 | 防止任务跑久后目标漂移、上下文漂移、模式漂移 |
| **失败显性化** | 匹配约定、失败要说清楚 | 不把跳过/未验证/不确定包装成完成 |

12 条规则不应照单全收——每个仓库的失败模式不同。没有长任务就别写检查点规则，没有多服务 monorepo 就别写跨代码库风格选择。

## 好规则的来源：摔出来的，不是写出来的

**写 CLAUDE.md，先翻 Agent 最近几次翻车记录：**

- 是不是没读调用方就写重复函数？
- 是不是修 bug 时顺手格式化了半个文件？
- 是不是测试没跑完就说"全部通过"？
- 是不是把 503 重试交给模型判断而不是 if-else？
- 是不是看到两套错误处理模式后各拿一半拼出第三种？

### 空洞 vs 具体指令对照

| 空洞（无效） | 具体（有效） |
|------------|------------|
| Be careful when running commands | Use `pnpm test --filter <package>` for package-level tests |
| Keep changes minimal | Do not reformat files outside the edited block |
| Write better tests | A test should fail if the business rule changes |

## 最小实操流程

给仓库补 CLAUDE.md 的推荐顺序：

1. **拉最小模板** — 用 Forrest Chang 模板或下方 80 行骨架
2. **删到 120 行以内** — 只留当前项目真的用得上的规则
3. **补 5 条项目命令** — 安装、类型检查、lint、局部测试、全量测试
4. **补 5 条项目边界** — 危险目录、只读文件、禁止命令、数据 dry-run、测试跳过汇报
5. **找一个真实小 bug 试跑** — 不用玩具任务
6. **观察 Agent** — 是否少问重复问题、少做无关改动、验证汇报更清楚
7. **只根据真实失败继续改** — 不因为"看起来更完整"加规则

### 80 行骨架模板

```markdown
# CLAUDE.md
## Working style
- State assumptions before making non-trivial changes
- Keep changes small and scoped to the request
- Read the surrounding code before editing
- Match existing conventions; if patterns conflict, choose the newer/better-tested one and explain why
- Do not refactor adjacent code unless the task explicitly asks for it
- If unsure, stop and ask instead of guessing

## Verification
- Run the smallest relevant test first
- If tests are skipped, say which and why
- Do not report success unless the requested behavior was verified
- Prefer deterministic tools for formatting, linting, renaming, and validation

## Project commands
- Install: TODO
- Type check: TODO
- Lint: TODO
- Unit test: TODO
- Integration test: TODO

## Project conventions
- TODO: Error handling pattern
- TODO: Logging pattern
- TODO: API/client pattern
- TODO: Test data and fixture pattern

## Safety boundaries
- Do not modify TODO without explicit approval
- Do not run destructive commands without explicit approval
- For migrations: produce a plan and dry-run output before changing data
```

TODO 部分才是真正要花时间填的——团队最容易漏掉的不是"要不要让 Agent 小心"，而是安装依赖命令、局部测试命令、哪些目录是生成代码、错误处理统一入口、migration dry-run、哪些命令改线上状态、测试跳过时怎么汇报。

## 三层文件架构

| 层 | 文件 | 内容 |
|---|------|------|
| L1 | 根目录 CLAUDE.md | 高频行为约定（80-120 行）：先读再写、小切片、少顺手改、测试要说明、失败要说清楚 |
| L2 | `docs/agent/project.md` | 项目事实：命令、目录边界、错误处理模式、日志约定、共享工具入口 |
| L3 | `docs/agent/migrations.md` 等 | 任务流程文件：迁移、API 端点、UI 组件、发布 checklist |

主文件只做索引：
```markdown
## Task references
- For database migrations, read `docs/agent/migrations.md`
- For new API endpoints, read `docs/agent/api-endpoint.md`
- For UI component changes, read `docs/agent/ui-components.md`
```

## 规则救不了一个没有验证的系统

CLAUDE.md 再好，也只是**事前引导**——Harness 里最轻的一层。外面还得接上：

- formatter、lint、类型检查 → 管格式和静态错误
- 单元测试、集成测试 → 管行为
- hooks 和权限 → 管动作边界
- CI 和 review → 管合并前检查
- 日志、指标、线上信号 → 管合并后的真实效果

Mitchell Hashimoto: **"Agent 犯错后，不能只修这一次，还要顺手补一个机制，让同类错误以后更难再发生。"** 有些机制是一条 CLAUDE.md 规则，更可靠的机制往往是一个工具（截图脚本、过滤测试脚本、边界写进 lint）。

Martin Fowler 的提醒：**非确定性一旦进了研发链路，确定性反馈反而更值钱。** 模型可以生成、解释、修复，但能不能交付出去，还得靠测试、类型、权限、日志和 review 兜底。

## 应避免的写法

| 反模式 | 问题 | 改进方向 |
|-------|------|---------|
| 身份提示 | "你是资深工程师"——模型本来就会装懂 | 告诉它在**这个仓库里**生产级代码长什么样 |
| 空泛提醒 | "注意安全"——人看着舒服，Agent 不知道做什么 | 换成具体命令、具体文件、具体模式 |
| 禁止清单 | 一连串"不要"让 Agent 变谨慎但未必变正确 | 写成"不要 X，请用 Y" |
| 永不过期 | 规则文件不是祖训 | 定期删除过时规则，比不断追加更重要 |

## 相关页面

- [[claude-code-instruction-failure]] — Claude Code 指令失效分析与五层规则架构
- [[claude-md-best-practices]] — Karpathy 的 70 行 CLAUDE.md 实测效果与陷阱
- [[claude-code-configuration-guide]] — 大型代码库配置实操指南
- [[claude-code]] — Claude Code 综述
- [[agent-harness-overview]] — Harness 六承重层综述
- [[loop-engineering]] — Loop Engineering 持续清理系统
- [[claude-code-large-codebase]] — CLAUDE.md 在大型代码库中的分层实践
