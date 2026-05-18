---
title: Claude Code
description: Anthropic 的 AI Coding Agent——架构、机制与工程实践全索引
aliases: [claude-code, CC, Claude CLI]
tags: [ai-agent, tool]
sources: [2026/05/10/lint-stub.md]
created: 2026-05-10
updated: 2026-05-18
---

# Claude Code

Anthropic 的生产级 TypeScript 单体 AI Coding Agent，约 500K 行代码。本子目录包含 34 篇深度分析页面。

## 架构全景

- [[claude-code-architecture]] — 架构全景：六抽象模型 + 10 大设计模式（**主入口**）
- [[claude-code-bootstrap-pipeline]] — 启动管道：5 阶段 / 300ms 预算
- [[claude-code-state-architecture]] — 状态架构：两层状态 / 粘性锁存器
- [[dive-into-claude-code]] — 学术论文：5 价值观 + 13 原则

## 核心机制

- [[claude-code-agent-loop]] — Agent 循环：1730 行 async generator
- [[claude-code-tool-system]] — 工具系统：40+ 工具 / 14 步执行管道
- [[claude-code-context-compaction]] — 上下文压缩：5 层渐进管道
- [[claude-code-permission-system]] — 权限系统：7 种模式 / 拒绝优先
- [[claude-code-memory-system]] — 记忆系统：4 类型 / 可推导性标准
- [[claude-code-search-strategy]] — 搜索策略：grep 为什么打败了 RAG

## 多 Agent 与扩展

- [[claude-code-subagent]] — Subagent 委托：10 步决策树
- [[claude-code-multi-agent-mechanism]] — 多 Agent 机制：三种模式总览
- [[claude-code-extensibility]] — 扩展机制：MCP / Plugins / Skills / Hooks
- [[claude-code-mcp]] — MCP 集成：8 种传输 / 7 配置来源

## 工程实践

- [[claude-code-large-codebase]] — 大型代码库最佳实践
- [[claude-code-configuration-guide]] — 配置指南与 CLAUDE.md 模板
- [[claude-code-harness]] — Harness 实现细节

## 相关页面

- [[agent-harness-overview]] — Harness 综述
- [[claude-code-vs-opencode]] — Claude Code vs OpenCode 对比
- [[opencode]] — OpenCode 架构分析
