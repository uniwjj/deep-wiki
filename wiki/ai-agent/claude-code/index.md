---
title: Claude Code 深度分析
description: Claude Code 源码级架构分析——六抽象架构、10 设计模式、Agent 循环、工具系统、多 Agent 机制
aliases: [claude-code-index, Claude Code 索引]
tags: [ai-agent, meta, summary]
sources: [2026/05/10/lint-stub.md]
created: 2026-05-18
updated: 2026-05-18
---

# Claude Code 深度分析

Claude Code 的完整源码级理解，共 34 篇分析。

## 架构全景

- [[claude-code-architecture]] — 架构全景（六抽象 + 10 设计模式）
- [[claude-code-bootstrap-pipeline]] — 启动管道（300ms 预算）
- [[claude-code-state-architecture]] — 两层状态架构
- [[claude-code-api-layer]] — API 层与 Prompt Cache
- [[claude-code-internal-mechanisms]] — 内部机制总览

## Agent 循环与工具

- [[claude-code-agent-loop]] — Agent 循环（async generator）
- [[claude-code-tool-system]] — 工具系统（14 步管道）
- [[claude-code-concurrent-tools]] — 并发工具执行
- [[claude-code-input-system]] — 输入系统
- [[claude-code-permission-system]] — 权限系统

## 多 Agent 机制

- [[claude-code-multi-agent-mechanism]] — 多 Agent 总览
- [[claude-code-subagent]] — 子代理委托
- [[claude-code-fork-subagent]] — Fork Subagent
- [[claude-code-coordinator-mode]] — Coordinator 模式
- [[claude-code-swarm]] — Swarm 多 Agent 协作
- [[claude-code-builtin-agents]] — 内置 Agent 类型

## 记忆与上下文

- [[claude-code-memory-system]] — 记忆系统
- [[claude-code-auto-dream]] — 自动梦境记忆
- [[claude-code-context-compaction]] — 上下文压缩
- [[claude-code-session-persistence]] — 会话持久化

## 扩展与集成

- [[claude-code-extensibility]] — 扩展机制
- [[claude-code-mcp]] — MCP 协议实现
- [[claude-code-harness]] — Harness 编排系统
- [[claude-code-remote-execution]] — 远程执行

## 前端与交互

- [[claude-code-terminal-ui]] — 终端 UI
- [[claude-code-search-strategy]] — 搜索策略

## 逆向与深度

- [[claude-code-hidden-features]] — 隐藏功能
- [[claude-code-sourcemap-reverse]] — Sourcemap 逆向
- [[dive-into-claude-code]] — 学术论文分析
- [[claude-code-performance]] — 性能优化

## 配置与对比

- [[claude-code-configuration-guide]] — 配置指南
- [[claude-code-large-codebase]] — 大型代码库实践
- [[claude-code-vs-opencode]] — vs OpenCode 对比

## 相关页面

- [[ai-agent/index]]
- [[opencode]]
- [[sdd]]
