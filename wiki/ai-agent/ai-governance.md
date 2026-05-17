---
title: AI 治理
description: AI 系统的治理框架——模型管理、记忆主权、合规与安全保障
aliases: [ai-governance, AI Governance, AI治理]
tags: [ai-agent, concept, draft]
sources: [2026/05/10/lint-stub.md]
created: 2026-05-14
updated: 2026-05-14
status: draft
---

# AI 治理

AI 治理涵盖 AI 系统在设计、开发、部署和运营中的规则、流程与责任划分，确保 AI 行为的可控、可信与合规。

## 核心维度

- **模型治理**：模型版本管理、评估基准、退役策略
- **记忆主权**：跨模型的知识记忆所有权、迁移与隐私保护
- **行为审计**：AI 决策的可追溯性与可解释性
- **合规对齐**：符合行业法规与企业安全策略

## 组织级 AI 治理模式

Anthropic 观察到大型组织的 AI 编码工具落地通常涉及：

- **DRI（单一负责人）**：负责 Claude Code 配置、权限策略、插件市场、CLAUDE.md 惯例
- **跨职能工作小组**：工程 + 信息安全 + 治理代表共同定义需求和推行路线图
- **受控推行**：先定义批准的 Skills、强制的代码审查流程、有限初始访问，逐步扩大
- **Agent Manager 角色**：新兴的混合 PM/工程师角色，专职管理 Claude Code 生态

## 相关页面

- [[claude-code-large-codebase]] — 组织级落地模式
- [[token-consumption-economics]] — Token 消耗经济学中的治理考量
- [[obsidian-claudian]] — 跨模型记忆与数据主权
