---
title: AI-native SDLC 治理与受管设置
description: AI 原生流程的控制分层（建议性 skill / 确定性 hook / 审批门 / branch protection / managed settings）、受管设置完整配置逐键解释（sandbox 与 credentials 补权限的洞、requiredMinimumVersion 等）、分离职责与审计轨迹、度量方法，及存量系统与单一事实源的三种共存配置
aliases: [AI-native SDLC 治理, SDLC 控制与合规, managed settings, 受管设置, 审批门与审计]
tags: [ai-agent, sdd, practice, concept]
sources: ["2026/10/12/The AI-native SDLC playbook.html"]
created: 2026-10-12
updated: 2026-10-12
---

# AI-native SDLC 治理与受管设置

> 来源：Anthropic《The AI-native SDLC playbook》（2026-08-21）的治理侧要点——把散落在各 play"Governance considerations"里的控制机制汇总成一条主线。play 全流程见 [[ai-native-sdlc-plays]]，工具机制见 [[ai-native-sdlc-toolkit]]。

## 控制分层：从"建议"到"近乎不可能"

| 层 | 机制 | 确定性 | 原文定位 |
|----|------|--------|----------|
| 建议性 | Skills、CLAUDE.md | 无强制 | "让违规罕见" |
| 确定性（动作级） | Hooks：allow / block | 每次匹配动作必执行 | "让违规近乎不可能" |
| 确定性（流程级） | 检测脚本（control band）、CI 检查（evals 通过率） | 版本受控、可单测 | 检测完全无模型参与 |
| 审批门 | Hook ask：暂停到指定人批准；`.claude/settings.json`（团队）与 managed settings（管理员，工程师不可关） | 对所有人每次执行 | 变更管理签字、发布授权、受保护路径 |
| 边界 | Branch protection：agent 写的一切只能是 PR，没有推 main 的路 | 平台强制 | "agent 可行动直到生产门、不能越门" |
| 环境分层 | 按环境权限层级：dev 自由部署 / staging 居中 / production 由 release manager 授权 | | 每个非交互运行以 agent 自己身份行事 |

文章的核心论断（跨 play 反复出现）：**skill 是建议性控制，政策必须无例外成立时要在 skill 背后配确定性层**——hook 阻断动作、或 PR 处重查政策的 review pass；"skill 让违规罕见，hook 让违规近乎不可能"。

## 受管设置（managed settings）：受监管企业完整示例

原文的 worked example——由平台团队经 MDM 或 admin console 下发；**工程师不能编辑或覆盖其中任何一项**。完整配置：

```json
{
  "permissions": {
     "deny": [
        "Read(.env*)", "Read(./secrets/**)",
        "WebFetch", "Bash(curl *)", "Bash(wget *)"
     ],
     "allow": [
        "Bash(git *)", "Bash(make build)",
        "Bash(make test)", "Bash(make lint)"
     ],
     "disableBypassPermissionsMode": "disable"
  },
  "allowManagedPermissionRulesOnly": true,
  "sandbox": {
     "enabled": true,
     "failIfUnavailable": true,
     "allowUnsandboxedCommands": false,
     "network": { "allowedDomains": ["git.internal.example.com",
"registry.npmjs.org"] },
     "credentials": {
        "files": [
          { "path": "~/.ssh", "mode": "deny" },
          { "path": "~/.aws/credentials", "mode": "deny" }
        ],
        "envVars": [ { "name": "GITHUB_TOKEN", "mode": "deny" } ]
     }
  },
  "allowManagedHooksOnly": true,
  "disableSideloadFlags": true,
  "allowManagedMcpServersOnly": true,
  "strictKnownMarketplaces": [
     { "source": "github", "repo": "example-corp/approved-plugins" }
  ],
  "requiredMinimumVersion": "2.1.193"
}
```

**逐键解释（原文"What each line buys, in control terms"）**：

| 配置 | 买到什么控制 |
|------|--------------|
| `permissions.deny` | 把秘密挡在 agent 上下文之外；阻断通过工具的任意网络出口 |
| `permissions.allow` | 预批准安全的内循环，避免 deny 列表变成"prompt 疲劳" |
| `disableBypassPermissionsMode` + `allowManagedPermissionRulesOnly` | **没有工程师、项目文件或命令行 flag 能放宽规则** |
| `sandbox` | 补权限盖不住的洞——工具级禁 `WebFetch` 挡不住 shell 命令触网；OS 级域名 allowlist 直接断出口 |
| `failIfUnavailable` + `allowUnsandboxedCommands` | 把沙箱变成 gate：沙箱起不来时 Claude Code **拒绝启动**；沙箱内失败的命令不能在沙箱外重试 |
| `credentials` | 补 deny 规则留的洞——`permissions.deny` 只管 Claude 的文件工具，沙箱内 shell 命令默认仍能读 `~/.ssh`、`~/.aws/credentials`；此块拒绝这些读取，并从每个沙箱命令的环境里剥除指定秘密 |
| `allowManagedHooksOnly` | 审批门 play 中定义的 hook 是**唯一**运行的 hook——本地什么都不能添加或替换 |
| `disableSideloadFlags` + `strictKnownMarketplaces` | 工程师机器上的每个 skill、agent、hook、MCP server 都来自组织批准的插件市场，绝不来自 home 目录 |
| `allowManagedMcpServersOnly` | agent 的工具面是平台团队拥有的 allowlist |
| `requiredMinimumVersion` | 低于批准版本的 build 拒绝启动——控制由"组织实际评估过的构建"执行 |

原文提醒：**这是可裁剪的起点、不是拷贝建议**——每条 deny 都在与能力做交易，正确平衡取决于仓库的数据分级；设置参考文档列出每个键（含仅受管的键）。

## 分离职责与审计轨迹

- **写代码的 agent 没有任何途径批准它**：PR review 中 review 政策作用于所有 PR，findings 本身不批准也不阻断 PR；批准由人通过 branch protection 给出，findings 只提供信息。
- Claude Security 同理：提出修复的 agent 没有批准它的途径。
- **PR 即审计记录**：findings、修复、评级、批准全在 PR history。
- **通道即审计轨迹**（Claude Tag）：请求、诊断、人类授权与修复留在事故被处理的地方。
- **提交链即审计轨迹**：intent → spec → plan → diff 及测试 → 带 review 结论的 PR → 事故记录；"谁要了什么、agent 产出了什么、谁批准了"。
- 记录去向：会话 transcript（OpenTelemetry 导出到组织可观测栈）、hook 决策（带时间戳与 allow/block 结论）、非交互运行按 agent 自己的身份与触发它的工程师区分。

## 度量：治理本身怎么衡量

| 度量对象 | Leading | Lagging |
|----------|---------|---------|
| 审批门 | 每道 gate 的等待时间（hook 决策写入 OpenTelemetry） | hook 上线前后到达生产的 gate 违规数 |
| 验证 | agent 变更的首次 CI 通过率 | 每 PR review 时间；变更失败率 |
| evals | 通过率曲线；事故到成为永久 eval 的耗时 | CI 抓住的回归 vs 生产发现的回归 |
| review | 首次 review 时间；零人工接触解决的评论占比 | merge 前抓住的缺陷 vs 逃逸生产的 |
| PR cycle | — | PR cycle time 漂移也会被 control band 监测（见下） |

原文强调：harness 对**流程指标**同样适用——PR cycle time 触发漂移规则时，agent 为工程领导层写报告。

## 存量系统与单一事实源（Across all stages）

治理迁移绕不开的现实（原文侧栏，适用于流程产出的每一个 artifact）：既有 SDLC 的 artifact 大多不在 markdown 里——工作项在 Jira、需求在带合规追溯的工具、设计在 Figma、变更审批在变更委员会；**审计师和监管者已经接受它们、其他团队依赖它们**，所以 AI-native SDLC 必须绕着既有的存在来贴合。

对每个 artifact **命名一个"单一事实源"**，其余系统只持有副本或指向原件的链接。三种配置：

1. **仓库为事实源**——markdown artifact 是权威记录，存量系统引用 commit 中的文件；工程主导组织最干净的配置之一（全部记录同一工具、同一时间戳权威）。
2. **存量系统为事实源**——Jira / ServiceNow / 需求工具持权威记录，markdown 是工作副本；Claude 会话开始读记录、并通过 MCP 连接器在产出 spec/plan 的同一次会话里写回结果。
3. **链接（linkage）为最低门槛**——所有 artifact 注明记录 ID，所有存量记录含 markdown 文件的 commit SHA；迁移起步用，接受事实源有两个。

原文结论：**两者可以共存**——只要彼此有链接、或其中一个被声明为事实源。

## 相关页面

- [[ai-native-sdlc]] — 总览：bottleneck 迁移、六阶段对照
- [[ai-native-sdlc-plays]] — 各 play 的治理考量与度量（本文是其治理主线的汇总）
- [[ai-native-sdlc-toolkit]] — CLAUDE.md / skills / hooks 的工具机制
- [[claude-code-permission-system]] — Claude Code 权限系统机制
- [[ai-agent-security]] — Agent 安全治理（Multica"无沙箱"立场等对照）
- [[ai-governance]] — AI 治理相关（政策侧）
