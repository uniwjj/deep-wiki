---
title: Agent MCP 协议
description: Model Context Protocol——基于 JSON-RPC 2.0 的工具发现与调用标准，8 种传输、7 个配置来源、OAuth 认证链
aliases: [agent-mcp-protocol, MCP, Model Context Protocol, MCP协议]
tags: [ai-agent, concept]
sources: [2026/05/10/Dive into Claude Code.txt]
created: 2026-05-10
updated: 2026-05-17
---

# Agent MCP 协议

MCP（Model Context Protocol）定义了 AI Agent 与外部工具、数据源之间的标准通信协议。基于 JSON-RPC 2.0，核心只有两个操作：`tools/list` 发现工具 + `tools/call` 执行工具。

## 传输类型

| 传输 | 适用场景 |
|------|---------|
| **stdio** | 本地子进程，默认方式 |
| **HTTP Streamable** | 远程服务 |
| **WebSocket** | 双向实时通信 |
| **SSE** | 旧版遗留，逐步淘汰 |

## 配置来源（按优先级）

local `.mcp.json` > user `~/.claude.json` > project > enterprise > managed > claudeai > dynamic

按签名去重，内置工具优先于 MCP 工具。

## 工具包装流程

1. **名称规范化**：`mcp__serverName__toolName`
2. **描述截断**：2048 字符
3. **inputSchema 透传**：不修改参数定义
4. **注解映射**：权限级别等元信息

## 安全设计

- **分层超时**：连接 30s → 每请求 60s → 工具调用无上限 → OAuth 30s
- **OAuth 完整 RFC 链**：8414 → 6749，支持 XAA 跨应用联合令牌
- **MCP skills 永不执行内联 shell 命令**：信任边界是绝对的
- **InProcessTransport**：queueMicrotask 防栈深度，级联 close 防半开状态

## 相关页面

- [[claude-code-mcp]] — Claude Code MCP 实现详解
- [[claude-code-extensibility]] — MCP 在扩展机制中的位置
- [[agent-skills-system]] — Skills 与 MCP 的关系
