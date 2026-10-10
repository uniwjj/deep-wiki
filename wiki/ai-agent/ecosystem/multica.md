---
title: Multica：把 Agent 变成同事的人机协作工作系统
description: 从源码与官方文档拆解 Multica——「人机工作的记录与行动系统」：Issue→Run→Runtime 工作循环、Server 管协调+机器管执行的 daemon 架构、wakeups 触发层调度（不是工作流引擎）、「daemon 用户即边界」的无沙箱安全模型与 Multica License
aliases: [Multica, multica.ai, 人机协作工作系统, 把 Agent 变成同事, Agents that show up on the board, 智能体也在看板上]
tags: [ai-agent, tool, architecture]
sources: ["2026/10/10/Multica 是什么——从源码看一个把 Agent 变成同事的系统.md"]
created: 2026-10-10
updated: 2026-10-10
---

# Multica：把 Agent 变成同事的人机协作工作系统

> 基于对上游仓库源码与官方文档的一手调研（数据截至 2026-10）。基本盘：约 52.3k stars、6.8k forks，驱动 26 种 Agent CLI，源码公开（source-available），出品方 Index Labs (Hong Kong) Limited。

## 痛点与定位

官方 README 的痛点描述：agent 各自关在终端标签页里，"会话一关就什么都不记得"；结论是"智能体越加越多，你越忙"。VISION 文档点出误判：*"The promise of AI is not a larger collection of tools. It is a more capable team."*（AI 的承诺不是更多的工具，而是一支更强的团队。）

官方定义一句话："The system of record and action for human-agent work"——**人机工作的记录与行动系统**。更直白地说：**一个 Server 管协调、你的机器管执行的团队工作区**——任务以 issue 为载体，Agent 是看板上可被指派的成员。官方 slogan：英文 "Agents that show up on the board"，中文「智能体，也在看板上」。

名字是个彩蛋：Multica = **Mult**iplexed **I**nformation and **C**omputing **A**gent，致敬 1960 年代的分时操作系统 Multics——当年让多人共享一台计算机，现在让人和 Agent 共享一套工作系统。

### 它刻意不是什么（四个最容易误判的点）

- **不是模型，也不是 Agent 框架**：不训练、不内置任何模型，只驱动你已安装并登录好的 CLI。"switching providers is a dropdown, not a migration"（换供应商是个下拉框，不是一次迁移）。
- **不是云端执行平台**：控制面（看板、任务、状态）可以在云上，但执行永远在你注册的机器上——"Multica records and coordinates the work; connected computers execute it." 代码、凭证、文件、实际改动不出你自己的机器。
- **不是工作流引擎**（被误解最多的一点）：官方工程文档原话 "This is not a workflow engine."。issue 状态只是看板语义——"A status's label does not grant behavior"，状态不驱动行为，驱动行为的是 wakeups 机制（见下文）。
- **不是 OSI 认证的开源**：采用自制的 "Multica License"（完整 Apache 2.0 + 附加条款，见下文）。

## 核心模型：一次任务从人走到 Agent 再走回来

官方文档给出的完整链路是五步：

1. **Issue 提供上下文**——描述、讨论串、指派人都在 issue 上；指派给某个 agent 时，该 agent 的 instructions、model、skills、runtime 配置一并生效
2. **创建 Run 进队列**——没有 runtime 在线时就在队列里等
3. **某个 Runtime 领取**——在线的 runtime（连着 daemon 的某台机器）领走任务，调用该 agent 配置的编码 CLI
4. **本地执行**——CLI 在机器上访问项目目录、执行命令、产出结果
5. **结果写回 Issue**——进度和产物流入 issue 时间线和执行日志

四个关键对象：

| 对象 | 含义 |
|------|------|
| **Issues** | 工作本体 |
| **Agents** | 定义在服务端的配置实体：名字 + instructions + model + skills + 绑定 runtime |
| **Runs** | 一次执行，被 runtime 领取 |
| **Runtimes** | 执行器，连着你的机器 |

三条容易忽略的规则：

- **Agent 不会自己开工**："Agents never start on their own"。直接触发方式：指派 issue、评论里 @、直接聊天、Autopilot（定时自动化）
- **run 完成 ≠ issue 完成**：执行日志显示 Completed 只代表一次 run 结束；issue 还可以继续讨论、加需求、再触发下一轮
- **能触发的不只是人**：事件订阅、定时器、条件、超时都可以

所以它不是"丢一条流水线"，而是一个**持续的工作循环**。

## 实现（一）：控制面与执行面

### 控制面（服务端）

- 后端 Go（Chi router + sqlc + gorilla/websocket），数据库 PostgreSQL 17（含 pgcrypto、pg_trgm）；Web 端 Next.js 16，桌面端 Electron，移动端 Expo / React Native（iPhone/iPad）
- 部署三选一：SaaS（multica.ai）、自托管（Docker Compose / Helm / 单二进制，镜像走 GHCR）、桌面端（装上即把本机自动注册为 runtime）
- 自托管服务端默认每天上报一条匿名部署快照（仅版本号 + 分桶计数），`DO_NOT_TRACK=1` 可关闭

### 执行面：daemon 机制

`multica` CLI 在你的机器上跑守护进程：

- **连接**：服务端通过 WebSocket 推唤醒信号，daemon 另有 30s 轮询兜底、15s 心跳；一次批量领取它名下所有 runtime 的任务
- **认证**：浏览器 OAuth 登录（签发 90 天个人 access token），无头环境用 `--token`
- **探测**：自动扫描 PATH 上装了什么 Agent CLI，逐个注册成 runtime（官方支持 26 种：Claude Code、Codex、Cursor Agent、Copilot、Kimi、Grok、Qwen、dsh……）；每种 CLI 可用环境变量覆盖模型和参数（如 `MULTICA_CLAUDE_MODEL`）
- **执行**：任务到达后建**每 run 隔离的工作目录**（默认 `~/multica_workspaces/`），拉起 CLI，流式回传结果；并发上限默认 20
- **运维**：二进制自更新"等当前任务跑完再重启"（不打断任务）；agent CLI 原地升级则重新探测、不重启即可注册

### 数据边界（重要）

服务端只存**协调数据**（workspaces、issues、评论、状态、agent 配置、skills、run 记录）；机器侧保留工具及凭证、代码目录、真实改动。官方强调这条边界"在云端和自托管下完全一致"。唯一例外是 `custom_env`（值存服务端再下发）——官方明确警示：别把"绝不能出本机"的密钥放进去。

## 实现（二）：wakeups 调度——预编排发生在哪一层

关键问题：**任务执行是固定编排还是动态规划？** 答案：**编排发生在触发层，执行层是 Agent 自主规划**。

Multica 的调度机制叫 wakeups：agent 或人可以给 issue 挂"订阅 / 条件 / 定时器"，触发时不启动常驻进程——"There is no sleeping process or second run lifecycle"——**触发一次 = 一次普通的 run**。四类触发器：

| 触发器 | 能力 |
|--------|------|
| **事件** | 25 种 issue 级订阅，覆盖 Run / Issue / Comment / Reaction / Attachment 五类变化，还能按"谁触发的"过滤（如只响应人类成员的评论） |
| **定时** | 一次性 `at`、重复 `every`、`cron`（带时区）；runtime 离线时一次性定时器保持排队，重复定时器把错过的周期合并成一次 |
| **条件** | 结构化谓词——issue 字段变化（状态 / 指派人 / 标签）、子 issue 全部完成、PR 的 checks 完成 / merged、另一个 issue 到达某状态；满足即产生一次唤醒 |
| **超时** | `expires_at` / `expires_in_seconds` 到期唤醒，目标"跑一次来做升级、延期或止损" |

派发机制的工程细节（按生产环境打磨过）：

- **事务性捕获**：匹配的输入以 `issue_wakeup_receipt` 落库，SQL 钩子在源事务内触发——不会出现"状态变了但触发丢了"
- **租约派发**：调度器约 30 秒轮询一次；每次派发 2 秒预算、50ms 锁超时，竞争失败顺延下个周期
- **严格串行**：同一 issue + agent 的 run 不并行——"a wakeup never runs beside another run of its agent"
- **合并与确认**："A claimed prompt is immutable"（已领取的提示词不可变，后续输入最多变成下一个排队任务）；agent 自己的操作产生的输入会被识别为"已知悉"而不是再触发一次
- **预算**：每次派发的 prompt 有 40,000 字节预算（instructions + 近期证据）
- **防失控三件套**：`max_fires`（默认 20 次）、循环链检测、限流（每小时最多 12 次）；容量上限 32 条规则 / issue、1000 条 / 工作区
- 处理完的 receipt 保留 7 天后清理；不支持全局事件重放（"no global event order or historical replay"）

**结论**：run 内部怎么做，完全交给底层 CLI 自主规划——这就是"不是工作流引擎"的含义。想要"固定步骤 + 逐步人工门闩"（例如：先出 spec → 人划线 review → approve → 开发 → 人审代码 → archive），需要在这一层之上自建。已知的实践中，网易内部版本正是在上游基础上做了二次开发（打通内部 IM、权限体系、GitLab），并实现了 fast development / SDD 这类固定步骤 workflow。

## 实现（三）：团队治理功能

**Skills（技能）**：官方定义 "A skill is a reusable set of working methods in a workspace"。核心文件是 `SKILL.md`（可附脚本、模板、参考资料），要写清楚：何时适用、动手前检查什么、步骤、产出形态、什么情况下停下来问人。用途：把"某个同事跑通的做法"变成全团队资产，一份 skill 可挂给多个 agent 复用。来源四种：手动创建、本地导入（`.skill` / `.zip`）、URL 导入（GitHub / ClawHub / Skills.sh，可持续 Update 刷新）、从 runtime 复制（快照，不自动同步）。安全提示非常坦诚："Multica does not review, sign, or sandbox them for you"——外部导入的 skill 可能含脚本和危险指令，风险自负。

**Squads（小队）**：把人和 agent 混编成一个组，有一个 leader 负责路由工作。

**Autopilots（自动机）**：按 cron 定时发起运行——站会、审计、报告这类周期性工作不用人点名。

**可观察与控制**：执行日志逐条回放每次工具调用、命令、报错（带时间戳）；可以给**正在跑的** agent 插话纠偏（目前支持 Claude Code、Codex、Grok）；按 run / agent / issue 统计成本；**review gate** 把代码挡在 main 之外，直到人批准。

**扩展与集成**：插件体系（`multica.plugin.json` 清单 + 单文件入口，走 Action API；权限按 scope 授权，不声明网络权限的插件 "literally cannot send data anywhere"）；集成覆盖 Slack / 飞书 / 钉钉 / 企业微信 / Telegram，Git 侧支持 GitHub / GitLab / Gitea / Forgejo（含自托管）。

## 安全模型：一个值得注意的刻意选择

官方安全文档的核心表述：**"daemon 的操作系统用户就是边界"（the daemon user is the boundary）**。具体含义：

- agent CLI 作为子进程运行，默认继承 daemon 用户的**全部权限**：能读写该用户所有文件、使用其凭证、无限制访问网络
- 官方明确不做文件系统沙箱保证："Multica makes no filesystem-sandbox guarantee"，并且要求用户 "Treat every run as unsandboxed and put the boundary outside the daemon"
- **run 是无人值守的，审批提示自动应答**：默认配置下 Codex 用 `danger-full-access`，Claude Code 用 `--permission-mode bypassPermissions`
- 官方坦承风险：如果 daemon 跑在个人账号下，一次 run 就可以"读取你的 SSH 密钥、改你的 shell 配置、删你的文档"

**为什么不内置沙箱？** 文档给了两个理由：一是 Agent 需要真实环境（装依赖、跑构建、调用 gh / aws / kubectl 等云 CLI，这些都依赖真实 HOME）；二是**部分沙箱防不住关键风险**——只写受限的沙箱"不能阻止一次 run 读取凭证并通过网络外传"，反而制造"安全了"的错觉。所以官方立场是：不假装自己是边界，"把边界放在 daemon 外面"。

它只做三件**缩小爆散半径**的事（官方强调"不是安全边界"）：每 run 隔离工作目录；Codex 使用 per-run 的 `CODEX_HOME`（不污染 `~/.codex/`）；**run 级 API token**（`MULTICA_TOKEN` 绑死在服务端的 agent + run 上，保证"一次 run 无法借 Multica API 冒充你或其他 agent"）。

落地建议（官方由轻到重）：专用 Unix 用户（只放该 agent 需要的仓库和凭证）→ 容器（只挂必要卷和密钥）→ VM（完全隔离）。凭证策略：把该环境里能碰到的凭证都视为"agent 可用"，用细粒度部署密钥替代个人 SSH key，生产凭证不要放进去。

## 常见误解

**Q1：Claude Code 本身就有编排能力，Multica 多了什么？**
"能调用工单系统"和"能被工单系统调用"是两件事。Claude Code 的编排是**单人会话级**的：状态在你的会话里，任务所有权挂在你的终端上，合上笔记本任务就不存在了。Multica 补的是：Agent 在系统里有**身份**（可被指派）、执行有**常驻 runtime**（有人在线就能领活）、状态在**共享的 issue** 上（跨人、跨会话可回溯）。前者是工具，后者是"可被派活的成员"。类比：bash 脚本也能跑测试，但组织还是需要 CI——不是 CI 里的编译器更强，是没人需要开着笔记本等它跑完。

**Q2：要那么多种 Agent CLI 干嘛？**
垂直角色靠 instructions + skills 就能塑形（所以"多 CLI"不是核心卖点）。支持 26 种 CLI 的真实原因是工程性的：① **不绑死**（换模型 / 换 CLI 不用推倒重来，"执行器可插拔"）；② **并发与配额**（限流按厂商算，多挂几个 CLI = 多几份并发额度）；③ **组织现实**（不同人本来就在用不同 CLI）；④ **交叉评审**（不同模型的盲区不重合，"另一个 agent 来 review"比自审更有效）。

**Q3：它有"预编排"能力吗？**
有，但在触发层：什么时候、因为什么、把哪个 issue 派给哪个 agent（事件 / 定时 / 条件 / 超时 + Autopilot）。执行层不预编排——一次 run 内部怎么干是 CLI 自主规划的。

## 什么时候值得用

价值成立需要满足至少一条：

- **派活的人 ≠ 跑 agent 的人**——同事 / 系统能把任务派给"某台机器上的某个 agent"，而不需要一台开着的终端
- **任务需要无人值守推进**——下班后、周末照常跑（daemon 常驻，run 实时回写）
- **审批必须异步、可追溯、角色化**——review gate、执行日志、成本统计对应企业审计诉求

反过来说：单人作战，或紧密小团队人人都泡在自己的终端里，**Claude Code + 几个脚本 + CI 触发（headless 模式）**就能搭出八成效果，Multica 的多人 UI、身份体系和调度层这时是过度设计。

## License 与商业化

Multica 采用自制的 "Multica License"（= 完整 Apache 2.0 + Part I 附加条款），关键边界：

- **内部使用免费**："Internal use within a single organization (including multiple workspaces) does not require a commercial license."
- **对外提供服务需要商业授权**："you may not use the Multica source code to provide a hosted service to third parties"；且 "whether or not you charge for it"——**免费对外提供也不行**，公开可访问、服务自己组织以外的人即触发
- **嵌入对外商业产品需要商业授权**：把 Multica 作为组件放进"卖出 / 授权 / 对外商业分发"的产品中
- **品牌豁免是单独一项**：不得去除 / 修改 Multica 的 LOGO、产品名、UI 版权署名；"A commercial license does not by itself waive branding"——商业授权本身不含品牌豁免，两项需分别取得
- 公开 fork 源码可以，但每个运营方要各自获得授权；商业授权与品牌豁免申请入口：https://www.multica.ai/contact-sales
- 授权方保留调整许可证措辞的权利，依赖前建议存档所用版本的 LICENSE

## 结语

Multica 的方向价值在于一个判断：**当 Agent 开始持续产出，协同层会成为标配中间件**——它占据的是当年 CI、工单系统、代码评审平台的位置。它自己不产生智能，但把"一群人和一群 Agent"变成了可调度、可审计、可复用的组织能力。当 Agent 足够快，瓶颈会从写代码转向审代码，HITL（人在环）将成为新的吞吐量上限。

一句话收尾：**Multica 的价值不在让 Agent 更强，而在让一群人共享同一群 Agent。**

## 相关页面

- [[ai-native-team-refactor]] — 协同工具层论述（业务空间/参与者/任务/频道概念雏形）：Multica 是外部产品侧对该层的一个具体实例
- [[digital-employee]] — 数字员工的系统化形态：Agent 有身份、可被指派
- [[claude-code-multi-agent-mechanism]] — 对比：Claude Code 的单人会话级编排 vs 共享 issue 工作系统
- [[claude-managed-agents]] — 对照：托管式（脑手分离）vs Multica「控制面在云、执行在你机器」
- [[harness-as-backend]] — Wakeups 的 Trigger 语义、Runtime 注册与 Worker 概念的对照
- [[agent-multi-agent-collaboration]] — 多 Agent 协作机制基础
- [[agent-skills-system]] — Skills 机制：Multica 工作区可复用资产的同类设计
- [[ai-agent-security]] — 「daemon 用户即边界」无沙箱化立场的落点
