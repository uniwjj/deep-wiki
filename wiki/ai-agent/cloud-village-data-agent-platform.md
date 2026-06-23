---
title: 云村数据 Agent 平台
description: 网易云村（Cloud Village）DAS Agent Platform 五层架构——自建 Agent + 三方 Agent 统一通过 das-mcp-cli 安全桥接访问 OmniSQL 数据底座与 Rossta 语义层
aliases: [云村DataAgent, DAS Agent Platform, 云村 DAS, Cloud Village Data Agent, das-mcp-cli]
tags: [ai-agent, big-data, architecture, platform]
sources: [2026/06/23/云村DataAgent.svg]
created: 2026-06-23
updated: 2026-06-23
---

# 云村数据 Agent 平台（DAS Agent Platform）

网易云村（Cloud Village）数据 Agent 平台，代号 **DAS Agent Platform**。其核心设计理念是：**自建 Agent 与第三方 Agent 共享同一套安全桥接层（das-mcp-cli），统一访问底层数据语义与查询引擎**。

## 五层架构

```
产品层（Data Agent · Dev Agent · Ops Agent · 三方 Agent）
    ↓
运行时层 · Aris（Agent Manager）
    ↓
桥接层 · das-mcp-cli（统一安全通道）
    ↓
数据语义层 · Das Rossta
    ↓
数据与查询底层 · OmniSQL
    ↓
外部数据源
```

## 第一层：产品层（Agent Products）

产品层分为自建 Agent 和三方本地 Agent 两类，均通过 das-mcp-cli 访问数据工具。

### 自建 Agent

| Agent | 面向用户 | 核心能力 |
|-------|---------|---------|
| **Data Agent** | 业务运营 · 数据分析师 | 业务域模式、全域搜索、SQL + Python 深度分析 |
| **Dev Agent** | 数仓开发人员 | 人机协作、SDD 全流程：需求→模型→ETL→DQC |
| **Ops Agent** | 运维 · 所有用户 | PoPo 值班机器人、智能答疑、异常诊断 |

### 三方本地 Agent

外部 Agent 产品通过标准 CLI / API 接入 das-mcp-cli：

| Agent | 说明 |
|-------|------|
| **Claude Code** | Anthropic 官方 CLI Agent，终端/IDE 多场景；das-mcp-cli 本地集成 → OmniSQL + Rossta |
| **Code Maker** | 代码生成类 Agent |
| **自定义 Agent** | Custom Agent、自建 Agent、本地部署均可通过标准 CLI / API 接入 |

> 三方 Agent 的核心价值：用户可以选择自己熟悉的 Agent 工具（如 Claude Code），通过 das-mcp-cli 安全地访问云村数据平台，无需为每个 Agent 单独开发数据访问层。

## 第二层：运行时层 · Aris（Agent Manager）

Aris 是 Agent 运行时管理器，负责 Agent 实例的全生命周期管理。

| 组件 | 能力 |
|------|------|
| **Container 沙箱** | Docker / Kubernetes + Claude Agent SDK 集成 |
| **凭证自动注入** | das-mcp-cli 集成、用户凭证绑定 |
| **动态插件机制** | Hook 技术、Plugin 注入、前端定制 |
| **生命周期管理** | Agent 实例管理、创建/销毁、弹性扩展 |

## 第三层：桥接层 · das-mcp-cli

das-mcp-cli 是整个平台的**核心设计亮点**——它是所有 Agent（自建 + 三方）访问数据平台的**统一安全通道**。

| 能力模块 | 详细说明 |
|---------|---------|
| **身份互信 & 安全通道** | Open-ID 统一身份、Token 设备绑定、端到端加密、后台审计、细粒度权限 |
| **智能数据桥接** | 无缝连接 OmniSQL + Rossta 语义层、自然语言查询、敏感黑名单、异常 SQL 拦截 |
| **Skill 市场与生态** | Skill 托管、分享、一键安装；三方集成包括 Muse、ABTest、猛犸、Overmind |

### 设计价值

- **一次接入，多 Agent 复用**：das-mcp-cli 封装了安全、权限、审计等横切关注点，Agent 无需各自实现
- **标准 CLI 接口**：任何支持 CLI 工具的 Agent（Claude Code、自建 Agent 等）都能直接集成
- **安全兜底**：敏感黑名单 + 异常 SQL 拦截，防止 AI 生成的危险查询直达生产数据

## 第四层：数据语义层 · Das Rossta

Das Rossta 提供 AI 可消费的数据语义，是 Text-to-SQL 准确率的关键支撑。

| 模块 | 内容 | 规模/价值 |
|------|------|----------|
| **知识库沉淀** | 按业务线 & 数仓分层结构化管理 | 300+ 物理表案例、3000+ 指标案例 |
| **AI 语义支撑** | N2SQL Schema 关联、业务上下文、Text-to-SQL | 高质量上下文 → AI 理解精度提升 |

## 第五层：数据与查询底层 · OmniSQL

OmniSQL 是统一数据查询服务，屏蔽底层引擎差异。

| 模块 | 说明 |
|------|------|
| **统一数据查询服务** | Impala、Doris、Hive、ClickHouse 多引擎统一入口；Apache Graviton 元数据管理；JDBC 标准协议 |
| **AI 场景专项优化** | 细粒度权限（表/字段级）、白名单快速申请、敏感黑名单防泄露、异常 SQL 拦截、自动优化改写 |

## 外部数据源

云村平台、猛犸平台、Muse、ABTest、Popo、Overmind 等外部系统通过标准接口接入。

## 架构特点与对比

### 与 DataWorks Data Agent 的对比

| 维度 | 云村 DAS | DataWorks Data Agent |
|------|---------|---------------------|
| **定位** | 网易内部数据 Agent 平台 | 阿里云 DataWorks 官方产品 |
| **桥接层** | das-mcp-cli（核心创新） | ACP Gateway + MCP Hub |
| **三方 Agent 支持** | 显式支持 Claude Code 等三方 Agent | 通过 MCP 协议开放 Skill 生态 |
| **语义层** | Das Rossta（知识库 + AI 语义） | Data Semantic Grounding |
| **查询引擎** | OmniSQL（多引擎统一） | MaxCompute / Hologres / EMR |
| **运维 Agent** | Ops Agent（PoPo 值班机器人） | DataClaw（IM 群运维 Agent） |

### 关键设计决策

1. **das-mcp-cli 作为统一桥接层**：将安全、权限、审计从 Agent 中解耦，任何 Agent 只需通过 CLI 即可安全访问数据
2. **自建 + 三方双轨制**：不强制使用单一 Agent，允许团队选择自己偏好的 Agent 工具
3. **Skill 市场机制**：与 DataWorks 的 MCP Skill 生态类似，通过可复用 Skill 降低 AI 能力建设成本

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 产品架构（对比参考）
- [[taobao-live-data-dev-paradigm]] — 淘宝直播 R2C 架构（另一种 Agent 平台实践）
- [[superetl]] — 菜鸟 SuperETL（Skill 编排 + Hooks 模式）
- [[data-agent-practice-guide]] — 数据智能体实践指南
- [[agent-mcp-protocol]] — MCP 协议（das-mcp-cli 的概念参照）
