---
title: Superpowers + OpenSpec 组合踩坑实录
description: 双框架组合的 7 个坑及解决方案——自定义衔接 Skill、双层审查机制、跳过 apply 的替代方案
aliases: [7 pitfalls, 双框架踩坑, OpenSpec Superpowers 衔接, spec-compliance-check]
tags: [ai-agent, practice, decision]
sources: [2026/05/17/Superpowers+Openspec两个AI编程框架一起用，我踩了7个坑.html]
created: 2026-05-17
updated: 2026-05-17
---

# Superpowers + OpenSpec 组合踩坑实录

> OpenSpec 管"做什么"，Superpowers 管"怎么做"。它们互补但不自动配合——你需要一个衔接层。

## 框架分工再澄清

| 维度 | OpenSpec | Superpowers |
|------|----------|-------------|
| 回答的问题 | 做什么、为什么做 | 怎么做、按什么标准做 |
| 核心产物 | proposal/specs/design/tasks | 代码、测试、审查报告 |
| 控制对象 | 需求和规范 | 行为和纪律 |
| 失败模式 | 做了不需要的东西 | 需要的东西做错了 |
| 类比 | 建筑蓝图 | 施工规范 |

**缺少任何一个的后果**：
- 只有 OpenSpec：规范写得好，AI 实现时跳测试、不做审查、一次性提交 500 行——规范对了但代码质量烂
- 只有 Superpowers：TDD、审查做得好，但做出来的东西和需求对不上——代码质量好了但方向错了
- 两个都没有：方向错 + 代码烂

## 完整联合工作流（8 步）

### 需求阶段（OpenSpec 主导）

**第 1 步：`/opsx:explore`** — 先讨论方案，不要上来就 propose。explore 花 5 分钟，能省 30 分钟返工。

**第 2 步：`/opsx:propose add-jwt-auth`** — 生成 4 个文档：
- `proposal.md`：为什么做、范围+**排除范围**
- `specs/`：增量规范，"假设/当/则"场景
- `design.md`：技术方案与架构决策
- `tasks.md`：实现步骤清单

**第 3 步：人工审核** — 框架不会替你做的环节。重点检查：范围是否完整、场景是否覆盖边界（如并发刷新 token）、技术决策是否合理、任务是否遗漏安全测试。

### 衔接阶段（你主导）

**第 4 步：关键衔接** — 把 OpenSpec 的产物喂给 Superpowers：

```
对 AI 说：
请读取以下 OpenSpec 规范文档，基于规范用 Superpowers 的 writing-plans 拆分实现计划：
1. openspec/changes/add-jwt-auth/design.md 作为技术方案
2. openspec/changes/add-jwt-auth/specs/ 下的场景作为测试依据
3. openspec/changes/add-jwt-auth/tasks.md 作为任务列表
4. openspec/changes/add-jwt-auth/proposal.md 的排除范围作为审查依据
```

**重要**：跳过 OpenSpec 的 `apply` 步骤——用 Superpowers 的 TDD 流程替代，因为 Superpowers 有测试纪律、代码审查、验证机制。同时，如果 OpenSpec 的 explore 和 propose 已完成，跳过 Superpowers 的 brainstorming 以避免重复劳动。

### 实现阶段（Superpowers 主导）

**第 5 步：TDD 实现** — 每个 task 走测试驱动开发循环，子代理隔离执行。

### 验证阶段（两者配合）

**第 6 步：双层审查**
1. 代码质量审查（Superpowers 的 requesting-code-review）
2. 规范合规审查（自定义 spec-compliance-check Skill）
3. `/opsx:verify` — 从规范维度全面验证

**第 7 步：全量测试** — `go test ./...`（或其他语言对应的全量测试命令），Superpowers 的 verification-before-completion 强制要求。

**第 8 步：`/opsx:archive` + Superpowers 收尾** — 归档更新主规范，选择合并或创建 PR。

## 7 个坑及解决方案

| # | 坑 | 现象 | 根因 | 解法 |
|---|-----|------|------|------|
| 1 | **跳过 explore** | 生成的 proposal 方案和需求对不上，来回改 3 次 | 直接 propose 让 AI 猜需求 | 先 explore 再 propose |
| 2 | **排除范围没写细** | AI 顺手改了前端 Web 端的登录，而需求只要求移动端 JWT | proposal 没写"不做什么" | 写清楚排除范围，AI 就像有了围栏 |
| 3 | **tasks 粒度和 plan 不匹配** | tasks.md 是需求视角，"实现 JWT 签发函数"是一句话；Superpowers 的 plan 是实现视角，需要 TDD 步骤 | 两个框架管不同层面 | 衔接层做粒度转换，把"说什么"转为"怎么做" |
| 4 | **实现方式与 design.md 不一致** | AI 用内存 map 存黑名单，而 design.md 定了 Redis SET | Superpowers 没读到规范上下文 | 把 design.md 喂给 Superpowers 作为 brainstorming 输入 |
| 5 | **代码审查不检查规范合规** | 审查只看了代码质量，没发现和规范不一致 | Superpowers 审查代理只看代码质量 | 自定义 spec-compliance-check Skill |
| 6 | **verify 和代码审查二选一** | 以为两者等价，只做一个 | 代码审查看质量，verify 看合规 | 两个都做：先代码质量 → 再规范合规 → 最后 verify |
| 7 | **archive 前没跑全量测试** | verify 通过但测试没跑，有回归 bug | verify 只检查规范合规，不跑测试套件 | archive 前强制跑全量测试 |

## 自定义衔接 Skill

以下 Skill 直接解决了 7 个坑中的 5 个（#3、#4、#5、#6、#7）。

### Skill 1: openspec-superpowers-bridge

```yaml
# ~/.claude/skills/openspec-superpowers-bridge/SKILL.md
---
name: openspec-superpowers-bridge
description: 开始实现 OpenSpec tasks 时自动加载，衔接两个框架
---
# OpenSpec-Superpowers 衔接规范

## 铁律
实现任何 OpenSpec task 之前，必须先加载对应的规范文档。

## 执行步骤
1. 读取 openspec/specs/ 下主规范和 openspec/changes/ 下最新未归档变更
2. 如果 OpenSpec 的 explore 和 propose 已完成 → 跳过 brainstorming → 直接进入 planning
3. 把 design.md 作为 planning 的输入
4. 把 specs/ 中的 "假设/当/则" 场景转换为 TDD 测试用例
5. 把 tasks.md 中每个任务拆成 Superpowers plan 粒度
6. 每个任务完成后用 spec-compliance-check 审查规范合规
7. 全部任务完成后运行 /opsx:verify
8. verify 通过后运行 Superpowers verification（跑测试命令）
9. 两个验证都通过才能 /opsx:archive
```

**场景转测试用例规则**：每个"假设/当/则"场景至少需要 2 个测试——正常路径 + 错误路径，涉及数值/时间的加边界值测试。

### Skill 2: spec-compliance-check

```yaml
# ~/.claude/skills/spec-compliance-check/SKILL.md
---
name: spec-compliance-check
description: 代码实现后、代码审查前检查是否符合 OpenSpec 规范
---
# 规范合规审查

## 铁律
任何实现必须与 openspec/specs/ 中的规范一致。不一致就是 bug。

## 审查步骤
1. 读取 openspec/specs/ 下的主规范（检查是否违反已有需求）
2. 读取本次变更对应的 delta 规范
3. 逐条检查每个"假设/当/则"场景是否有对应实现
4. 检查 design.md 中的架构决策是否被遵守
5. 检查 proposal.md 的排除范围是否被违反

## 常见问题
- "AI 顺手多改了代码" → 检查排除范围
- "实现方式和 design.md 不一致" → 这是 bug，不是优化
- "场景没有覆盖" → 测试不完整
```

## 四组合效果对比

同一项目（Go 微服务添加 JWT 认证）实测：

| 维度 | 两个都不用 | 只用 OpenSpec | 只用 Superpowers | 两个都用 |
|------|-----------|--------------|-----------------|---------|
| 需求对齐 | 口头说了算 | proposal+specs 确认 | 口头说了算 | proposal+specs 确认 |
| AI 多做功能 | 频繁 | 排除范围控制 | 偶尔(1-2次) | 排除范围控制(0次) |
| 测试覆盖 | 0 个测试 | 0 个测试 | 10 个测试 | 10 个测试 |
| 提交粒度 | 1 次大提交 | 1 次大提交 | 8 次小提交 | 8 次小提交 |
| 规范合规 | 无法验证 | verify 检查 | 无法验证 | verify+合规审查 |
| 返工次数 | 3-4 次 | 1-2 次 | 2-3 次 | **0-1 次** |
| 长期维护 | 新对话忘光 | 主规范累积 | 新对话忘光 | 主规范累积+纪律一致 |

双框架组合的核心价值：消除"方向对了但做错了"和"做对了但方向错了"这两个最常见的失败模式。

## 使用时机判断

| 场景 | 用什么 |
|------|--------|
| 改按钮颜色、修 typo | 只用 OpenSpec |
| 一次性脚本（写完就扔） | 只用 Superpowers |
| 团队协作项目 | 两个都用 |
| 长期维护项目 | 两个都用 |
| 复杂功能开发 | 两个都用 |
| 已有代码库上叠加新功能 | 两个都用 |

判断标准：**代码要活多久 + 有多少人碰代码**。活 1 天、1 个人碰，两个都不用。活 1 个月、3 个人碰，两个都用。

## 3 分钟快速上手

```
1. /opsx:explore 讨论方案
2. /opsx:propose 功能名 生成规范
3. 审核四份文档（proposal、specs、design、tasks）
4. 对 AI 说："读取 openspec/changes/ 下的规范，用 Superpowers 的 writing-plans 拆计划"
5. Superpowers 逐个 task 走 TDD
6. 每个 task 完成后：代码审查 → spec-compliance-check → /opsx:verify
7. 跑全量测试（go test ./... 或对应命令）
8. /opsx:archive → Superpowers 收尾合并
```

**第 4 步是关键**——把 OpenSpec 的规范文档喂给 Superpowers，不要让 Superpowers 空手上阵。

## 相关页面

- [[superpowers-framework]] — Superpowers 框架详解与实测基准
- [[superpowers-openspec-legacy-workflow]] — 老旧项目四阶段协同开发实战
- [[sdd-custom-workflow]] — SDD 薄编排架构：sdd-* 统一入口，委托 OpenSpec/Superpowers
- [[sdd-openspec-superpowers]] — SDD 双框架对比
- [[openspec-sdd-practice]] — OpenSpec 七步工作流实战
- [[superpowers-vs-openspec]] — 哲学层面深度对比
