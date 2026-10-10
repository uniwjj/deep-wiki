---
title: AI Agent 安全
description: AI Agent 面临的安全风险与防护策略——指令注入、权限控制、数据隔离
aliases: [ai-agent-security, AI Agent Security, Agent安全]
tags: [ai-agent, concept]
sources: [2026/05/10/lint-stub.md, "2026/10/10/Multica 是什么——从源码看一个把 Agent 变成同事的系统.md"]
created: 2026-05-14
updated: 2026-10-10
status: draft
---

# AI Agent 安全

AI Agent 在获得工具调用与自主决策能力后，面临独特的安全挑战。

## 主要风险

- **指令注入**：恶意 prompt 劫持 Agent 行为
- **权限失控**：Agent 越权访问敏感资源
- **数据泄漏**：Agent 将内部信息暴露给外部
- **工具滥用**：Agent 对工具能力的不当使用

## 防护策略

- 最小权限原则：Agent 仅获得完成任务必需的工具与数据访问
- 人类审批节点：关键操作（部署、删除）要求人工确认
- 沙箱隔离：Agent 操作在受限环境中执行

## 无沙箱化立场：Multica「daemon 用户即边界」（2026-10 补充）

与"沙箱隔离"相反的一条路线：[[multica]] 明确**不内置文件系统沙箱**，核心表述是"**daemon 的操作系统用户就是边界**"（the daemon user is the boundary）：

- agent CLI 作为子进程默认继承 daemon 用户的**全部权限**（读写该用户所有文件、使用其凭证、无限制访问网络）；官方要求 "Treat every run as unsandboxed and put the boundary outside the daemon"，并把"把边界放在 daemon 外面"作为用户侧的第一责任
- **run 无人值守，审批提示自动应答**：默认配置下 Codex 用 `danger-full-access`，Claude Code 用 `--permission-mode bypassPermissions`；官方坦承：daemon 跑在个人账号下时，一次 run 就能读 SSH 密钥、改 shell 配置、删文档

**官方不内置沙箱的两个理由**：① Agent 需要真实环境（装依赖、跑构建、调用 gh / aws / kubectl 等云 CLI，都依赖真实 HOME）；② **部分沙箱防不住关键风险**——只写受限的沙箱"不能阻止一次 run 读取凭证并通过网络外传"，反而制造"安全了"的错觉。

它只做三件**缩小爆散半径**的事（官方强调"不是安全边界"）：每 run 隔离工作目录、Codex 使用 per-run 的 `CODEX_HOME`、run 级 API token（`MULTICA_TOKEN` 绑死在服务端 agent + run 上，防止一次 run 借 API 冒充）。落地建议由轻到重：专用 Unix 用户（只放该 agent 需要的仓库和凭证）→ 容器（只挂必要卷和密钥）→ VM；凭证策略：把该环境里能碰到的凭证都视为"agent 可用"，用细粒度部署密钥替代个人 SSH key，生产凭证不要放进去。

## 相关页面

- [[multica]] — 无沙箱化路线的代表：「daemon 用户即边界」安全模型的出处
- [[openclaw]] — OpenClaw 自托管方案的安全考量
- [[claude-code-permission-system]] — Claude Code 权限系统（bypassPermissions 等模式）
- [[ai-plus-software-action-plan]] — 政策侧要求：方案（十九）部署智能体身份标识、可信互联、行为管控与安全管理规范
