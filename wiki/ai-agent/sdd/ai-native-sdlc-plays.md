---
title: AI-native SDLC Playbook 详解（六阶段九 play）
description: Anthropic playbook 逐 play 详解——Plan（intent.md）/ Design（spec.md）/ Build（plan mode、auto mode、CLAUDE.md、skills、hooks、并行会话）/ Test（反馈闭环、continuous evals）/ Deploy（PR review、审批门、CI/CD）/ Maintain（闭环、定时扫描、Claude Tag），含传统对比、Getting started、执行步骤、治理与度量指标
aliases: [AI-native SDLC plays, AI 原生 SDLC 实践详解, SDLC playbook 详解, intent.md spec.md plan.md 工作流]
tags: [ai-agent, sdd, practice]
sources: ["2026/10/12/The AI-native SDLC playbook.html"]
created: 2026-10-12
updated: 2026-10-12
---

# AI-native SDLC Playbook 详解（六阶段九 play）

> 来源：Anthropic《The AI-native SDLC playbook》（2026-08-21）。总览与六阶段对照见 [[ai-native-sdlc]]；Build 阶段工具细节见 [[ai-native-sdlc-toolkit]]；控制与审批门见 [[ai-native-sdlc-governance]]。
>
> 阅读方式：每个 play 按原文五要素展开——**什么变了（Traditional vs AI-native）/ Getting started（前置与基础设施）/ 执行步骤 / 治理考量 / 如何度量（Leading 与 Lagging 指标）**。

## 目录

| 阶段 | Play | 核心产物 |
|------|------|---------|
| 01 Plan | 见下方 | `intent.md` |
| 02 Design | 见下方 | `spec.md` |
| 03 Build | plan mode / auto mode / CLAUDE.md / skills / hooks / 并行会话与 subagent | `plan.md`、guardrails |
| 04 Test | 反馈闭环 / continuous evals | 验证证据、eval 套件 |
| 05 Deploy | PR review / 审批门 / CI-CD | REVIEW.md、gates |
| 06 Maintain | 闭环 / 定时扫描 / Claude Tag | 新的 `intent.md` |

---

## 01 · Plan

> **阶段宣言（原文）**：想法不再等人写下来。意图一次捕获，用提出者自己的话，成为下一阶段可据以行动的版本化 artifact。

### Play：Capture as intent.md

`intent.md` 有多个入口：一个人有了想法、提了工单、或通过告警浮出的事故（见 Stage 6）。人有了想法时与 Claude brainstorm 产出 markdown proto-spec——传统 SDLC 中同一个人还得说服产品团队把它写出来；proto-spec 人类可读、版本受控、立即可被下一阶段消费，存为 `intent.md`。无论意图来自事件触发还是 agent，同样的步骤都适用：**产品负责人 review 并修正 agent 写的 `intent.md` 之后才 commit**。

**什么变了**

- Traditional：想法要经过 backlog 条目、用户故事、故事点、refinement meetings 才能行动；每次交接都转移 owner，到工程团队手里时已经和提出者本意隔了好几层。
- AI-native：提出者与 Claude brainstorm，结果写成 `intent.md`——用提出者自己的术语表达的 proto-spec，包含想要什么、为什么、受什么约束；**重复性流程编码为 skill**。

**Getting started**

- 前置：无。
- 基础设施：非工程师的 Claude 访问入口（claude.ai 或 Cowork）；商定的 `intent.md` 模板；一个产品负责人关注着的、共享且版本受控的 intent home。单一产品最简单的是产品仓库里的 `intent/` 目录（让 artifact 链紧挨由它派生的代码）；只有意图跨多个仓库时才值得独立 intent 仓库，monorepo 中就是目录。与 Jira/需求工具的关系见本文 Build 阶段"单一事实源"侧栏。

搭建是一次性的平台/工程团队任务：需要一名技术成员立起 intent home 并决定谁能写入（贡献者来自组织各处）。仓库建好后，**没有 git 经验的贡献者不必直接用 git**——到版本控制系统（如 GitHub）的连接器让 Claude 从 claude.ai 或 Cowork 代为提交 markdown。

**执行步骤**

1. 提出者用自己的话向 Claude 描述问题——可以是今天做不到什么、谁受影响、更好是什么样、什么不在范围内；无需正式语言。
2. brainstorm 到想法具体化：Claude 会问分析师式的问题——范围、用户、约束、成功标准。
3. 让 Claude 按组织模板写成 `intent.md`（模板可编码为 skill，由技术成员设立、lead 签字）——覆盖问题、期望结果、受影响用户与系统、约束、开放问题。
4. 提出者纠正 Claude 误解之处。
5. commit 到共享 home——作者与时间戳入档，产品负责人从这里接手。

**示例（原文代码块，节选）**

```markdown
# Intent: claims status self-service
Author: J. Ortiz (claims operations). Status: draft.

## Problem
Customers phone the contact center to ask where their claim is.

## Proposed outcome
Customers see claim status, next step and expected date in the portal.

## Constraints
No new PII in the portal session. Existing authentication only.

## Open questions
Do third-party loss adjusters need access too?
```

**治理考量**：证据是已提交的 `intent.md`——作者、时间戳、完整修订历史都在 intent home 的 git history 里。产品负责人批准；把 intent 送进 Stage 2 的 accept/reject 决策记录为 merge 或 closing review。

**如何度量**：Leading = 从第一次对话到 `intent.md` 提交的时间（从 intent home 的 git history 读取）——预期从数周的 elicitation/refinement 周期降到小时级；Lagging = intent 存活率（被产品负责人接受进入 Stage 2 的比例），accept/reject 记录为 artifact 的 merge 或关闭的 review；另有"同一变更首个 `spec.md` commit 之后仍发生的 `intent.md` 修改次数"。

---

## 02 · Design

> **阶段宣言（原文）**：需求与设计塌缩进一次会话。政策在写 spec 时就被应用，而不是几周后的 review 里才被发现。

### Play：Requirements and design

产品负责人批准后，Claude 拿 accepted `intent.md` 产出需求与设计 spec，由组织在 brand、security、compliance、UX 上的 skill 引导。**产品负责人 review 这份 spec，但不写它**；目标是造出工程团队可以据以规划、并带有 flagged 关注点的 spec。前端工作是最清晰的例子：`intent.md` 被接受后，产品负责人在 Claude Design（beta）里按 `intent.md` 做 mock、迭代，再导出到 Claude Code 构建。

**什么变了**

- Traditional：需求与设计是两个团队分别运行的独立阶段——分析师把想法形式化为需求，设计师再把它解析回设计；分离为问责而存在，但慢且有损。
- AI-native：两个阶段在**一次 prompted session** 内完成——Claude 拿 `intent.md` 产出需求与设计 spec，受组织 skill 约束、并标出关注区域。

**Getting started**

- 前置：写好 `intent.md`；把 brand、security、compliance、UX 政策写成 skill。
- 基础设施：一个有 Claude 访问权的产品负责人；**不要求工程技能**。

**执行步骤**

1. 产品负责人开会话（组织的 skill 可用），附上 `intent.md`。
2. prompt 指向 `intent.md`、点明约束、要求标出关注点。先是手工跑，然后编码为组织级 slash command；再进一步：把 intent home 中 `intent.md` 被接受作为触发器，merge 时跑一个非交互 job（加载组织 skill），把 `spec.md` 以 pull request 提交（管线详见 Stage 5 的 CI/CD play）。此后产品负责人第一次介入即是 review。
3. 同一产品负责人对照原始想法 review spec：spec 解决了所述问题吗？`intent.md` 的开放问题被回答还是被延续？
4. 先处理 flagged concerns——它们是分析师会升级的点；产品负责人在工程看到 spec 之前与各政策 owner 逐项解决。
5. `spec.md` 与 `intent.md` 一起 commit——这对文件记录了"要了什么"与"决定了什么"。
6. 产品负责人决定 spec 与 intent 是否进入 build（被组织归类为高风险的事项咨询技术 lead）；**这个决定永远由人类同事做**，接受 spec 即是启动 Stage 3 plan mode play 的动作。

**示例 prompt（原文代码块，节选）**

```text
Read the attached intent.md and produce a requirements and design spec for
integrating it into our existing codebase. Apply the skills available to you
so the plan conforms to our brand guidelines, security policies and UX
standards. Document the spec fully as spec.md, ready to hand to the
engineering team. Describe clearly any areas of concern, especially where you
cannot satisfy contradicting policies.
```

**治理考量**：活的政策在 spec 撰写时即被读取与应用，而不是几周后在 review 里被发现。组织 skill 作为 spec 的约束；spec、产出它的 prompt、生效的 skill 版本全部记录在版本控制里。产品负责人签署 spec，并把 flagged concerns 路由给具名政策 owner。

**如何度量**：Leading = 同一变更 `intent.md` commit 到 `spec.md` commit 之间的耗时（两个 git 时间戳），对比旧的"需求+设计"周期；Lagging = build 开始后的需求返工——数同一变更首个 `plan.md` commit 之后日期更晚的 `spec.md` commit 数（git log 直接可得）。

---

## 03 · Build

> **阶段宣言（原文）**：没有已接受的计划就不实现。机构知识变成 agent 读取的文件，护栏以代码运行而不是靠习惯。

本阶段的 play 依次展开（plan mode → auto mode → CLAUDE.md → skills → hooks → 并行会话），工具层的完整展开见 [[ai-native-sdlc-toolkit]]。

### Play：Claude Code plan mode 作为默认起点

工程师以 plan mode 开始 Claude Code 会话，把 Stage 2 批准的 `spec.md` 给 Claude，让它采访自己，迭代计划直到满意。

**什么变了**

- Traditional：工程师读了设计就开始写代码——变更怎么做的、动哪些文件、配哪些测试都留在工程师脑子里、顶多工单评论里，没人能 review；reviewer 见到的第一样东西是完成的 diff，返工已经很慢。
- AI-native：工作以 Claude 在 plan mode 产出的**书面计划**开始（可读代码库、不改任何东西）；工程师在写代码前修正计划，批准版本 commit 为 `plan.md` 供后续阶段对照。

**Getting started**：前置 = intent artifact（`intent.md` 或 `spec.md`，如有）与 `CLAUDE.md` 有帮助；基础设施 = 能访问仓库的 Claude Code。

**执行步骤**

1. 工程师以 plan mode 开会话。
2. 给出 `intent.md` 与 `spec.md`，要求一份实现计划：点名**哪些文件会变、工作顺序、证明它的测试**。
3. 拷问计划：这个变更可能破坏什么？哪一步最危险？Claude 选择不做的其他方案是什么？
4. 迭代到"没见过这段对话的工程师也能仅凭计划实现该变更"。
5. 批准计划 commit 为 `plan.md`——进入审计轨迹；PR review play（Stage 5）会用它对账最终 diff。
6. 接受计划让 Claude 实现——计划扎实时实现常是单次通过。
7. 实现偏离计划时，在同一个 commit 里更新 `plan.md`；可考虑用 hook 强制两者同步。

**示例（`plan.md`，原文节选）**

```markdown
# Plan: claims status self-service (from intent.md 2026-06-02)

## Files that change
portal/src/claims/StatusPanel.tsx (new), claims-api/routes/status.py,
claims-api/tests/test_status.py

## Order of work
1. Add the status endpoint behind existing auth.
2. Panel against the endpoint.
3. Wire into the portal nav.

## Risks
The claims-core API rate-limits at 50 rps; the panel must cache.
```

**治理考量**：设计 review 发生在生成任何代码之前——改方向只需编辑一份文档；plan mode 自身强制这一点（工程师接受计划前 Claude 不能编辑文件）。计划及其修订连同接受者一起记录。例常变更由工程师批准，高风险类事项走技术 lead 或架构师。

**如何度量**：Leading = 首次实现即 merge 的变更占比；计划批准到 merged PR 的时间（从 PR metadata）；Lagging = 每变更的返工轮数（PR metadata）与 merged diff 仍与 `plan.md` 相符的频率。

### Play：Claude Code on auto mode

auto mode 下工程师批准计划后，Claude **无需逐次编辑确认**地应用每一项变更。当后续 play 的护栏成熟（调校好的 [[claude-code-memory-system|CLAUDE.md]]、编码政策的 skill、阻断不安全动作的 hook、Claude 可运行的测试套件）后，**auto-accept 成为例常工作的默认**：收窄的 `spec.md`、小爆炸半径、测试已覆盖的代码。

文章描述的重心转移：从"用户盯着 agent 的每次编辑、审查动作"转向"更长自主会话之后对 artifact 的审查"。auto-accept 配合 worktree 进一步支撑个人与团队并行，是**自主运行 SDLC、闭合循环（Stage 6）的基础**。

### Play：The CLAUDE.md

`CLAUDE.md` 给 Claude 提供"新入职者需要的上下文"：约定、命令、架构、团队最常见的错误。曾经在人脑和 wiki 里的知识变成 agent **每次会话开始时都读**的文件，由全团队维护、每次犯错后迭代。

**执行步骤**

1. 在仓库运行 `/init`，Claude 依据所见生成起始 `CLAUDE.md`。
2. 削减到"新人第一天需要的"：保留 build/test/lint 命令、真正重要的约定、Claude 反复搞错的事。
3. 把 `CLAUDE.md` 提交到仓库根目录的 git——全团队共享一个版本，变更像代码一样被 review。
4. 一条 working rule：**Claude 犯同一个错误两次，修正就写进 `CLAUDE.md`**。
5. 保持在一页以内——Claude 会话开始时会读完它，任何过时内容都在白占上下文。

**示例（原文节选）**

```markdown
# Payments service

## Commands
- Build: make build
- Test: make test (unit), make itest (integration, needs docker)
- Lint: make lint (runs in CI; fix before pushing)

## Conventions
- Java 21, Spring Boot 3. No new Lombok.
- Money is always BigDecimal, never double.

## Things Claude gets wrong
- Do not bump dependency versions; the platform team owns them.
- The legacy v1/ package is frozen; changes go in v2/.
```

**治理考量**：`CLAUDE.md` 受版本控制，agent 遵循的指令可 review、可审计；团队约定通过该文件施加，变更记录在 git history，code owner 在 PR review 中批准这些变更。

**如何度量**：Leading = Claude 重复犯"CLAUDE.md 本应拦住的错误"的频率（在 git history 中追踪修正/变更）；Lagging = 新成员首个 merged PR 的时间（PR history）。

### Play：Skills as institutional knowledge

skill 是组织把**机构知识可操作化**的方式：指令显式、版本受控、广泛应用、政策变化时集中更新。经验法则（原文）：**为"必须一致应用的机构知识"写 skill；属于 `CLAUDE.md` 或一次性 prompt 的内容不要写成 skill**。

**执行步骤**

1. 挑一条今天执行不一致的知识——安全标准、API 设计约定、或品牌规则。
2. 写成 skill：一个含 `SKILL.md` 的文件夹——frontmatter 说明何时触发，正文说明做什么；由工程师依据政策 owner 的 source of truth 撰写（可用 Claude 辅助）。
3. 放进仓库 `.claude/skills/<name>/`（随代码分发）或通过 plugin 组织级分发。
4. 测试 skill 会触发：用不同方式让 Claude 做相关任务，确认每次都加载。
5. 政策变化时改 skill，并由政策 owner 签字。
6. 工程师在下一次会话自动获得新版本。

**示例（`.claude/skills/secure-api-review/SKILL.md`，原文节选）**

```markdown
---
name: secure-api-review
description: Apply the API security standard. Use whenever creating or
  modifying an external-facing endpoint, reviewing API code, or
  generating an OpenAPI spec.
---
# Secure API review

When you create or change an API endpoint:
1. Authentication: every endpoint requires the gateway JWT;
   no anonymous routes outside /health.
2. Input validation: validate request bodies against the OpenAPI schema.
3. Audit: every state-changing endpoint emits an audit event.
4. Data classification: fields tagged pii must never appear in logs.
```

**治理考量**：skill 是控制，但**是建议性（advisory）的**——它让 Claude 在写代码时"很可能"应用政策，但没有任何机制强制会话遵守。**必须永远成立的政策需要确定性层在 skill 背后**：阻断动作的 hook，或在 PR 处重查政策的 review pass——"skill 让违规罕见，hook 让违规近乎不可能"。skill 调用记录在会话轨迹中，政策 owner 像 review 代码一样 review skill 变更。

**如何度量**：Leading = 政策 owner 批准政策变更到更新后的 skill merge 的时间（skill 文件夹的 PR）；Lagging = 引用该政策的 PR review findings 数——应趋近于零；若不降，要么 skill 没触发、要么其文本已偏离官方政策。

### Play：Hooks as build-time guardrails

skill 是建议性控制，**hook 是它背后的确定性层**。Claude 在实现期的大部分动作是文件编辑与 shell 命令，所以 build 是 hook 触发最频繁的阶段。Build 期 hook 可以做（原文列举）：

- 阻断对受保护路径（如生成的类、冻结的包）的编辑；
- 文件编辑后跑 formatter 和 linter，不让漂移累积；
- 让凭据不进 diff。

原则：**任何"必须无例外成立"的政策，都要在 skill 背后配 hook**。hook 对每一个匹配的动作都运行，所以 build 期 hook 要快、范围限定在变更的文件；跑全量测试这类重检查属于 commit 或 PR 层级。**向人要审批的 hook 属于 Stage 5 的 gate**——build 期弹审批会把人重新放回所有并行会话的关键路径上。

### Play：并行会话与 subagent

一个工程师可以同时驱动多条工作流：

- **并行会话（parallel session）**：另一个完整的 Claude Code 实例，在自己的 git worktree 里做单独任务；每个会话对彼此一无所知，唯一共享的是驾驶它们的工程师。
- **subagent**：在单个会话内部运行的限定助手，有自己的上下文窗口与工具限制；适合跨任务重复出现的工作（如验证应用按预期运行）。

两者分工（原文）：并行会话提高工程师"在飞"的任务数；subagent 让每个会话聚焦自己的任务；工程师的工作是 steer 与 review 全部。

**执行步骤**

1. 用 plan mode 阶段的计划找出工作在哪彼此独立，把工作切成触及不同文件的任务；共享文件的任务在同一会话里串行。
2. 每个并行任务各得一个 worktree（如一个终端 `claude --worktree feature-auth`、另一个 `claude --worktree fix-rate-limit`）——worktree 是独立分支上的独立 checkout，防止会话在文件上相撞。
3. 二到三个会话是合理起点；实际上限是一个人能**认真 review** 多少条流——只在 review 跟得上时加会话。
4. 把重复工作变成 subagent（`.claude/agents/` 下的 markdown 文件，含名称、何时使用的描述、可触及的工具）：例如主 agent 完成后剥除多余复杂度的 code simplifier、跑应用验证行为的 verifier、探索代码库后回报而不淹没主上下文的 researcher。定义提交进 git，供全团队共享。

**示例（`.claude/agents/verifier.md`，原文）**

```markdown
---
name: verifier
description: Runs the app and checks the change works before the session
  reports done
tools: Bash, Read
---
Start the app with make run. Exercise the changed behavior and the two
nearest neighboring flows. Report what you ran, what you saw, and any
behavior that does not match plan.md. Do not fix anything; report only.
```

**治理考量**：会话更多意味着产出更多，**控制必须来自仓库里的配置**——其中的 hook 和权限设置作用于所有会话；会话所做之事被记录并归属到运行它的工程师。

**如何度量**：Leading = review 质量保持前提下的每工程师并发会话数（从 OpenTelemetry 导出统计）与一天中用于 steering 而非等待的时间占比；Lagging = 每工程师每周 merge 的变更数（结合 PR history 中的返工率）。

### 侧栏：存量系统与单一事实源

**适用于流程产出的每一个 artifact。** 既有 SDLC 大概率已经在追踪 artifact，只是不在 markdown 里：工作项在 Jira、需求在带合规追溯的工具、设计在 Figma、变更审批在变更委员会。这些系统难以被替代——**审计师和监管者已经接受它们、其他团队依赖它们**——所以 AI-native SDLC 必须绕着既有的存在来贴合。

迁移时，对流程产出的**每一个 artifact 命名一个"单一事实源"**，其余系统只持有副本或指向原件的链接。三种配置（原文）：

| 配置 | 内容 | 适用 |
|------|------|------|
| 仓库为事实源 | markdown artifact 是权威记录，存量系统引用 commit 中的文件 | 工程主导组织最干净的配置之一——全部记录在同一工具、同一时间戳权威 |
| 存量系统为事实源 | Jira / ServiceNow / 需求工具持有权威记录，markdown 是工作副本；Claude 会话开始读记录、并通过 [[claude-code-mcp|MCP]] 连接器在产出 spec/plan 的同一次会话里写回结果 | 监管追溯已在别的工具中建立 |
| 链接（linkage）为最低门槛 | 所有 artifact 注明记录 ID，所有存量记录含 markdown 文件的 commit SHA | 迁移起步——接受事实源有两个 |

原文结论：存量系统与 markdown-first 系统**可以共存**——只要两者之间有链接、或其中一个被声明为事实源。

---

## 04 · Test

> **阶段宣言（原文）**：每个会话在人看到之前先自检，而驱动 agent 的配置像它写的代码一样被回归测试。

### Play：给 Claude 一个反馈闭环

**永远给 Claude 一个验证自己工作的方式**——测试、构建、或 screenshot diff；会话在人看到之前自检并修掉自己的错误。

反馈闭环**不要和 verifier subagent 混淆**（Stage 3）：反馈闭环在整个任务中运行任意多次；verifier subagent 是"会话认为工作完成后用全新上下文窗口打包最终检查"的一种方式——这样结论不会被产生代码时的假设染色。

**执行步骤**

1. 如果今天检查工作要一串命令和环境知识，包成单一入口（`make test` / `npm test`），失败以非零退出。
2. 在 `CLAUDE.md` 的 Commands 区列出每个命令和健康输出的例子。
3. 陈述**可量化**的目标，让 Claude 无需询问即可自检："`test_status.py` 全部通过"、"截图与附上的 mock 相符"、"端点返回 200 带新字段"。
4. **修 bug 先写失败测试**：让 Claude 把 bug 复现为测试、运行确认按预期原因失败、commit 这个测试；然后才让它在不编辑测试的前提下修到通过（由最后的 test-file hook 强制）。"存在于修复之前、且 agent 无法改写的测试，就是 bug 已死的证明"。
5. UI 工作以视觉检查闭环：给 Claude 浏览器或截图工具 + mock，让它迭代——实现、截图、比较、调整；两三轮是常态，结果应逐轮变好。
6. 把验证做成"done"的一部分：指令住在 `CLAUDE.md`——报告任务完成前跑测试并展示输出。
7. **保护闭环本身**——修代码的 agent 不能削弱对这段代码的检查：fix 任务期间阻断对测试文件的编辑的 hook；替代方案是在 review 中检查 diff、拒掉任何碰测试的变更。

**示例（`CLAUDE.md` 验证块，原文）**

```markdown
## Verifying your work

- Build: make build (must finish with "Build succeeded")
- Test: make test (all green; never skip or delete a failing test)
- Lint: make lint (zero warnings)

Run all three before reporting any task complete, and paste the output.
If a test fails, fix the code, not the test.
```

**治理考量（原文结构化）**：执行什么 = 报告完成前验证 + fix 期间禁止 agent 编辑测试（组织要保证时都用 hook 实现）；证据是什么 = Claude 运行并粘贴的 `make test` 字面输出、构建日志、或 screenshot diff——证据来自工具链；记录在哪 = 会话 transcript（OpenTelemetry 导出到组织可观测栈）与 PR 的 check run（reviewer 和后续审计者都能看到）；谁批准 = review PR 的 code owner——机械证据已附上，他只需要聚焦意图与风险。

**如何度量**：Leading = agent 所写变更的首次 CI 通过率（CI 系统已支持）；Lagging = 每 PR review 时间（PR metadata，应在测试接管 reviewer 旧职责后下降）与来自事故追踪的变更失败率。

### Play：CI 中的 continuous evals

**evals 是 AI 时代的 stage-gate QA**：一套在**agent 配置变化时运行**的套件——换模型或重写 prompt 时，eval 套件回答"agent 是否仍以同一标准完成工作"。要当作**活的套件**：模型变强后曾能区分的用例会失效，必须从持续监控里补充新用例。原文注明：视用例，有些团队可能选择按固定节奏离线跑，以下步骤针对持续评测。

**执行步骤**

1. 平台工程师从近期工作中收集 **20–50 个真实任务**及期望/被接受的结果。
2. 每个任务写成一条 eval = prompt + 定义"可接受"的检查（测试通过、lint 干净、行为不变、政策被遵守）。
3. 套件在 CI 中非交互运行：按 schedule、以及在 `CLAUDE.md`、skills、hooks 发生任何变化时——**这份配置在 steer agent，值得和代码同等的回归测试**。
4. 用结果 gate 配置变更：把通过率拉低的 skill 变更要先 review 才 merge。
5. **每次生产事故各写一条 eval**（由负责该事故的团队写），永久留在套件里当回归测试。

**示例（`.github/workflows/agent-evals.yml`，原文节选）**

```yaml
name: Agent evals
on:
  pull_request:
    paths: ['CLAUDE.md', '.claude/**']
  schedule:
    - cron: '0 2 * * *'
jobs:
  evals:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install -g @anthropic-ai/claude-code
      - name: Run eval suite
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          for eval in evals/*.json; do
            claude -p "$(jq -r '.prompt' $eval)" \
              --allowedTools "Read,Edit,Bash(make test)" \
              --output-format json > result.json
            ./evals/check.sh "$eval" result.json
          done
```

**治理考量**：evals 给 QA 一道跟得上 agent 产出的 gate——通过率阈值作为 merge check 执行、运行被记录以便跨时间对比、拥有该配置变更的团队批准它。

**如何度量**：Leading = eval 通过率随时间的变化曲线（套件每次运行都报告）与"生产事故到成为永久 eval 的耗时"；Lagging = CI 中抓住的回归 vs 生产中发现的回归（来自事故追踪）之比。

---

## 05 · Deploy

> **阶段宣言（原文）**：review 双向运行，治理在 agent 行动时即被执行。agent 能做直到生产门之前的一切，门后什么都不做。

### Play：PR review 循环中的 AI

Claude **既给 review 也收 review**：按组织政策 review 进来的 PR，也处理自己 PR 上的 review 评论。这让工程师把 PR review 聚焦在行为上——归结为判断意图与风险。

**什么变了**

- Traditional：review 产能按人类产出规划——PR 等 reviewer 通读，review 质量随 reviewer 负荷波动，作者追进度而 backlog 增长。
- AI-native：所有 PR 得到同一组 review pass，findings 按严重度排序；人的注意力上一层级——这个变更是否实现了计划意图、风险是否可接受。

**Getting started**：前置 = Stage 3 更新过的 `CLAUDE.md`；若 review pass 执行书面政策则需要 skill；定义好的 subagent。基础设施 = 装好 Claude 集成的仓库——管理员启用的托管 Code Review（research preview）服务，或在自己 CI 里跑 claude-code-action（模型调用可走 AWS Bedrock / Google Vertex / Microsoft Foundry，见 CI/CD play）；以及值得配上的"branch protection 要求 code owner 批准"。

**执行步骤**

1. 托管 Code Review 服务是最快起步——管理员启用并选择仓库；需要控制流水线或让 API 调用走自己云合同时，用 claude-code-action 在自己 CI 里跑。
2. 技术 lead 把 review 政策写成仓库根部的 `REVIEW.md`，划分为组织关心的 pass：bugs/logical errors；security/vulnerabilities；合规（对照 `spec.md`、`plan.md` 与设计原则）。`REVIEW.md` 还定义什么算 Important（而非 Nit）、什么跳过。
3. 技术 lead 设定人工阈值：findings **本身不批准也不阻断** PR，branch protection 仍然要求 code owner 批准；想把 merge gate 在 findings 上的平台工程师可以读取 check run 发布的机器可读严重度计数。
4. reviewer 或作者在 review 评论里 @claude 时，Claude 处理评论并推送修复——PR 线程同时记录请求与变更（fix 循环走 claude-code-action；托管服务中 @claude review 是请求一次新 review）。对 Claude 开的 PR 可以更进一步：让 Claude **babysit PR 到 merge**——团队把循环包成自定义 slash command：扫未解决评论与失败检查、处理并推修复，直到 PR 全绿、只等 code owner 批准。
5. **review findings 反馈回 `CLAUDE.md`**：一个错误被 review 指出第二次时，修正作为该次 review 的一部分写进 `CLAUDE.md`；因为 review 会读 `CLAUDE.md`，从下一个 PR 起这个错误就会被抓住。review 也会标出某变更让 `CLAUDE.md` 过时。
6. 每月技术 lead 调校：给 findings 评级让 reviewer 变好、在 `REVIEW.md` 里封顶 Nit 数量；排除生成路径与 CI 已执行的内容。

**示例（`REVIEW.md`，原文）**

```markdown
# Review instructions

## Passes
Run three passes and tag each finding with its pass:
- Bugs: logic errors, broken edge cases, subtle regressions
- Security: injection risks, authentication gaps, PII in logs
- Compliance: the change matches spec.md, plan.md and our design principles

## What Important means here
Reserve Important for findings that would break behavior, leak data
or breach a policy. Style and naming are nits.

## Cap the nits
Report at most five nits per review; summarize the rest as a count.

## Do not report
Generated files under src/gen/ and anything CI already enforces.
```

**治理考量**：**职责分离被保留**——写代码的 agent 没有任何途径批准它。`REVIEW.md` 的 review 政策作用于所有 PR；findings、修复、评级、批准都记录在 PR history，**PR 即审计记录**；批准由人通过 branch protection 给出、由 findings 提供信息。原文另指向《how Anthropic secures its AI-native software development lifecycle》一文了解这些控制在生产规模下的组合。

**如何度量**：Leading = 首次 review 的时间（应降到分钟级）与"无人接触分支即解决的 review 评论占比"（数据直接存在 Git 上）；Lagging = merge 前抓住的缺陷/漏洞 vs 逃逸到生产的（PR history + 事故追踪）。

### Play：Hooks as approval gates

Build 阶段的 hook 是护栏（无人参与地放行/阻断）；hook 还能**ask**——暂停动作直到特定的人批准，这正是发布 gate 需要的。该 play 列在 Deploy 是因为发布 gate 是最清晰的情形，但 **hook 并非 deploy 专属**：它也拦 build 期间无变更单的 migration/infra 编辑、拦 fix 任务中 agent 编辑测试文件。

**执行步骤**

1. 工程领导层会同变更管理与合规，列出**必须存活的**人类审批门：变更管理签字、发布授权、受保护路径编辑等。
2. 平台工程师把每道门表达为 hook——Claude 行动前运行的脚本，可以 allow / ask / block。
3. 团队 hook 放 `.claude/settings.json`（入 git）；**不可协商的 hook 放 managed settings**（平台或 IT 管理员拥有，单个工程师无法关闭）。
4. **阻断要能解释自己**：hook 停下动作时，原因与获批路径出现在 Claude 的输出里。

**示例（`.claude/settings.json`，原文）**

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

**示例（`.claude/hooks/production-gate.sh`，原文）**

```bash
#!/bin/bash
# Production deploys require a named release authorization
cmd=$(jq -r '.tool_input.command' < /dev/stdin)
if [[ "$cmd" == *"deploy"* && "$cmd" == *"production"* ]]; then
   if [ -z "$RELEASE_APPROVAL" ]; then
     echo "Production deploys need a release authorization." >&2
     exit 2 # exit 2 blocks the action; the message goes to Claude
   fi
fi
exit 0
```

**治理考量**：hook 就是审批门——gate 条件对所有人、每一次都执行；allow/block 决策带时间戳记录；gate 同时定义了什么算"批准"（获批的变更单或发布经理签字）。**受管设置完整配置与逐键解释见 [[ai-native-sdlc-governance]]。**

**如何度量**：Leading = 每道审批门的等待时间（每个 hook 决策写入 OpenTelemetry 导出，带时间戳与 allow/block 结论）；Lagging = hook 上线前后到达生产的 gate 违规数（事故追踪）。

### Play：CI/CD integration and deployment

要点：在 CI/CD 流水线中**非交互地**运行 Claude Code；沙箱化执行让长时 agent 安全运行；通过 MCP 集成暴露部署；在 agent 需要之前排练回滚路径。

**什么变了**

- Traditional：流水线跑确定性脚本，任何需要判断的事等人——分诊 flaky 测试、写 changelog、搞清构建为什么坏；部署与回滚是人在压力下手跟的 runbook。
- AI-native：Claude 在流水线里非交互地跑判断步骤，处于带限定凭据的沙箱中；部署工具通过 MCP 暴露给 agent——写完并测过变更的工作流也能发布它、回滚它，处在组织按环境定义的 gate 里。

**执行步骤**

1. 平台工程师从只读判断步骤开始：流水线 job 里 `claude -p` 分诊构建失败、总结 flaky 测试、起草 changelog。
2. 写操作放在既有的 gate 之后：修 lint、更新生成的文档、处理 @claude 提到的 review 评论。agent 写的任何东西以 PR 形式经过 branch protection 到达——**agent 没有推 main 的路**。
3. 执行沙箱化：agent job 跑在容器里、受网络策略约束、用短时限定 token，**默认不持有生产凭据**。
4. 通过 MCP 暴露部署：deploy、status、rollback 变成工具、按环境限定——agent 的部署权力是一份 allowlist，而不是带凭据的 shell 脚本。
5. 按环境分层自治：development 中 agent 自由部署；production 中 agent 准备发布、**release manager 授权**、hook 执行生产 gate；staging 在中间。
6. **回滚应是流水线中排练最多的路径**——agent 可运行的单条命令，并定期在 staging 演练；Stage 6 闭环在 control band 破线时会调用它，因此必须事先被证明。

**示例（流水线步骤，原文）**

```yaml
- name: Triage failed build
  if: failure()
  run: >
    claude -p "Read the build log at out/build.log. Identify the most
    likely cause, say whether the failure looks flaky or real, and write a
    three-line summary for the PR thread." >> triage.md
```

**治理考量**：总原则是"**agent 可以行动直到生产门、不能越门**"。控制机制：branch protection 把 agent 写的一切变成 PR（没有直达 main 的路）；生产部署 hook 阻断发布直到具名 release manager 授权；**每次非交互运行以 agent 自己的身份行事**，流水线日志把 agent 做的与触发它的工程师做的分开；按环境的权限层级设定 agent 在门之前的行动上限。

**如何度量**：Leading = 无需 paging 人类即完成分诊的流水线失败占比（CI/CD 流水线日志）；Lagging = DORA 指标（CI 系统与部署工具已产出）。

---

## 06 · Maintain

> **阶段宣言（原文）**：循环闭合。触发器调用 Claude、路径中无人，它发现的东西以 `intent.md` 重新进入流水线。

### Play：Maintenance and closing the loop

前五个阶段仍需要人启动初始步骤；本阶段转向**自主运行 Claude 闭合循环**。例如持续运行的监控 agent 可以围绕一张 bug 工单创建 `intent.md`，流经需求、计划、构建、测试、review 各阶段。**Stage 6 以 headless 方式运行**，阶段之间有独立置信 gate——确定性检查或对抗性 reviewing agent——决定前一阶段输出是继续还是升级给人。

### Play：Closing the loop（control band → intent.md）

确定性脚本盯着生产，control band 破线时调用 Claude。（破线监控是循环自主运行的示例模式；消息渠道进来的工作见本阶段最后的 Claude Tag。）

**执行步骤**

1. 服务 owner 或平台工程师**挑一个有稳定滚动基线的指标**：CI 测试失败率、部署后 5xx 率、PR cycle time 等。
2. 写检测脚本：滚动窗口的均值与标准差 + 规则（Western Electric 或类似），让 band 既能抓尖峰也抓缓慢漂移。脚本版本受控、有单元测试；**检测完全确定性、不涉及模型**。
3. 响应层级定义在版本受控配置（`bands.yaml`）中：**1σ 只记录日志；2σ 只读调用 Claude 诊断；3σ Claude 可以行动**——但仅限开 PR 进入 review gate、或触发预批准的 runbook。
4. 触发层可以是 GitHub/GitLab 的定时 workflow、既有监控栈的 webhook、或网内 Cron Job。Claude **无状态运行**——CI runner 上的非交互步骤，或沙箱容器里 Agent SDK 服务。因为运行无状态且非交互，**循环可以无人启动而开始和结束**。
5. agent 把诊断写成 Stage 1 格式的 `intent.md`：异常与其证据、期望结果、受影响系统、开放问题。此后发现像其他任何东西一样走流水线。
6. 服务 owner / on-call 分诊队列：面向产品的发现路由给产品负责人；三选一——现在修、排期、驳回。**驳回会调校 band、帮助减少噪音**。
7. 修复上线后为该事故加一条 eval（continuous evals play）。

**示例（`bands.yaml`，监控 CI 测试失败率，原文）**

```yaml
metric: ci_test_failure_rate
baseline: rolling_30d
rules: western_electric
tiers:
  1sigma: { action: log }
  2sigma: { action: diagnose,
            tools: "Read,Grep,Bash(gh run view *)" }
  3sigma: { action: propose,
            routes: [pull_request, runbook:rollback-deploy] }
```

**治理考量**：层级边界由版本受控配置执行；权限与受管设置拒绝生产访问。调用、发现、分诊决策带时间戳记录。服务 owner 分诊并批准发现，由此产生的变更走常规 PR review gate，**agent 可触发的 runbook 是事先批准的**。

**如何度量**：Leading = band 破线到 `intent.md` 进入分诊队列的时间（对比旧的"事故到复盘行动"时间；检测脚本日志有破线时间戳与事故层级）；Lagging = 变成 merged fix 的发现占比（分诊队列 vs 实际 PR history）与同类事故重复数（应随修复往 eval 套件加用例而降）。

**原文给出的三个例子**（说明 harness 对生产指标和流程指标同样适用）：

- CI 测试失败率破 3σ：agent 隔离 flaky 测试或开 revert PR，由 review gate 决定；
- 部署窗口内 post-deploy 5xx 率破 3σ：agent 触发既有回滚流水线；
- PR cycle time 触发漂移规则：agent 为工程领导层写报告。

### Play：Recurring codebase scans

安全扫描是"某个模型下代码库的时点陈述"，**两个半份都会过期**：代码每周在变；每个模型代际都会发现上一代错过的新漏洞。AI-native 的答案：按计划跑扫描、调用路径中无人，发现走和其他代码变更相同的 gate。

**Claude Security** 是定时扫描的托管形态：连接 GitHub 仓库，扫描在 Anthropic 基础设施上以 **Claude Mythos 5** 运行；每条 finding 先验证再报告、附置信度评级；建议补丁在 Claude Code on the web 中 review 并应用；组织无需自己接触模型即可拿到发现。

**执行步骤**

1. 安全 lead 连接仓库并组织为项目（按 repo / service / team），从开始就明确 finding 的归属。
2. 对最关键仓库跑**首次全量扫描**——包括其他工具或更早模型扫过的；把首扫当基线，**首扫很可能在"被认为是干净的"代码里发现问题**。
3. 按项目设定计划：活跃开发的服务默认每周；仓库大或混杂时把扫描限定到目录或分支。
4. 带着置信度评级分诊；**驳回必须给原因**——驳回被记录，同一 finding 不会在下次运行以"新"身份回来。
5. 有界 finding：在 Claude Code on the Web 打开建议补丁、review、像任何变更一样送进 PR review gate（**提出修复的 agent 没有批准它的途径**）。
6. 超出单个补丁的（如架构弱点、跨服务重复的模式）：按 Stage 1 格式写成 `intent.md`，从 Plan 开始。
7. 修复发布到生产后，把该漏洞类别的 eval 加进套件。
8. 用 CSV/Markdown 导出 findings 或 webhook，让组织既有的 tracker 与审计系统继续做记录系统（审计师本来就在那里查）。

**什么变了**：Traditional——扫描是事件（发布前或审计前启动），报告进 tracker，人工消化到下一次事件；中间写下的代码靠 PR review 兜底。AI-native——对每个已连接仓库按计划跑、用可得的最强模型、finding 在被人读到前先验证；每条 finding 按"破线 control band"同款方式处理：一个 PR 装得下的修复走 review gate，更大的变成 `intent.md`；**覆盖率的日期从上次运行算起，而不是从第一次**。

**Getting started（基础设施要点）**：Claude Security 面向 Claude Enterprise 组织公开 beta——需要 Anthropic GitHub App 装在目标仓库（cloud-hosted github.com）、启用 Claude Code on the Web、开启 Extra Usage 并设消费上限、扫描操作者需 premium seats、管理员在 `claude.ai/admin-settings/claude-code` 打开功能；扫描按 Mythos 5 费率按量计费，消费上限应与仓库规模与数量匹配。

**治理考量**：扫描在组织管理员控制下运行——连接哪些仓库、谁持有 scan seat、消费上限都集中设定。每条 finding 有验证结果与置信度评级、每次驳回有原因——**扫描历史是"发现了什么、修了什么、有意识地接受了什么"的审计记录**。修复经 PR review gate 和 branch protection 到生产，而非从扫描直接落地；Claude Security 是既有静态分析与依赖扫描的增强——确定性检查留在 CI，模型驱动扫描覆盖那些检查设计上找不到的、依赖上下文的漏洞。

**如何度量**：Leading = 已连接仓库处于计划扫描中的占比、finding 被报告到补丁进入 PR review gate 的时间（扫描历史 + PR metadata）；Lagging = 定时扫描发现的漏洞 vs 生产中发现/外部报告的（事故追踪）、以及经过数次扫描的仓库的"每扫发现数"趋势（应随修复与 eval 累积而下降）。

### Play：Claude on call with Claude Tag

事故也会从工作沟通工具来——晚上 10 点的 Slack 消息要求紧急修复；**Claude Tag**（目前 public beta，Slack 可用）让 Claude 以自己的身份成为这些频道的一员，每条新事故得到第一响应者，且响应本身成为循环的一部分与未来事故的记忆。

对话与机构知识留在频道里，频道中任何人都能引导与推动响应；任何团队成员都能测试假设、探索新选项、实时调查，频道历史增加可审计性。**通过 MCP 访问**，Claude 验证指标回到基线并在线程里确认，把复盘写进版本受控的 lessons 文件供未来调查阅读。

Incident 不是 Claude Tag 唯一接的工作：在工单上被 tag（经 MCP）或在频道里被问，Claude 用同样方式分诊——小而界定清晰的修复以 PR 经 review gate 到达，更大的写成 `intent.md` 进入 Stage 1：此时循环开始自我供给。

> 配图注（原文）：channel 就是审计轨迹——请求、诊断、人类授权与修复全都留在事故被处理的地方。

---

## 闭环总览：一个变更走过全流程

```
想法/工单/告警/破线/频道
   │
   ▼
intent.md（Plan：提出者 brainstorm + 产品负责人修正）
   │  accepted（merge 或 closing review）
   ▼
spec.md（Design：Claude 依组织 skill 写，产品负责人 review）
   │  接受 spec（永远由人决定）
   ▼
plan.md（Build：plan mode 采访式迭代，工程师接受）
   │  实现（可能 auto-accept；CLAUDE.md/skills/hooks/worktree 护栏）
   ▼
验证证据（Test：反馈闭环自检 + CI 中的 continuous evals）
   │  PR
   ▼
review + 审批门（Deploy：REVIEW.md 三 pass、hook allow/ask/block、
   │        branch protection、按环境分层自治）
   ▼
部署 → 生产（回滚是被排练最多的路径）
   │  运行期：control band 检测（确定性）→ 破线触发 Claude（无人启动）
   ▼
新的 intent.md → 回到循环顶部
```

## 相关页面

- [[ai-native-sdlc]] — 总览：为什么代码不再是瓶颈、六阶段对照、采纳顺序
- [[ai-native-sdlc-toolkit]] — CLAUDE.md、skills、hooks、并行会话与 subagent 的完整展开
- [[ai-native-sdlc-governance]] — 分层控制、受管设置与逐键解释、审计与度量
- [[raven-sdd-data-development]] — 数据开发场景的 SDD 流程对照
- [[comet-gating-system]] — 三层门控体系对照
- [[openspec-sdd-practice]] — OpenSpec 实践（规范驱动主流工具链）
