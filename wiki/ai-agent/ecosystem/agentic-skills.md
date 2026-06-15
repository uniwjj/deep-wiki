---
title: Agentic Skills（预制数据技能库）
description: 封装标准化数据任务执行逻辑的可复用能力包——AI Agent 原生数据工程的核心抽象单元
aliases: [Agentic Skills, 预制数据技能库, Data Agent Skills, agent-skills]
tags: [ai-agent, big-data, concept]
sources: [2026/06/15/谷歌开源 Data Agent Kit_数据工程正式进入原生 Agent 时代.html]
created: 2026-06-15
updated: 2026-06-15
---

# Agentic Skills

## 定义

Agentic Skills 是将标准化数据任务执行逻辑封装为可复用能力包的范式。AI Agent 加载这些 Skills 后，无需重复编写底层规范代码，即可执行专业数据工程操作。

概念的首次官宣来自 Google 2026-06-11 开源的 [[google-data-agent-kit|Data Agent Kit]]，但类似思路已在多个数据平台中出现。

## 核心理念

传统模式：开发者手写 SQL/调度/校验逻辑 → 重复劳动多、质量不一致

Skills 模式：预制能力包可组合调用 → Agent 自动编排执行

## 典型 Skills 清单（Data Agent Kit 内置）

- SQL 查询优化
- 数据校验（字段、行数、指标分母）
- 数据漂移检测
- dbt 流水线构建
- BigQuery/Spark 开发
- 数据治理规则执行
- 故障排查

## 与 MCP 的关系

Agentic Skills 定义 **做什么**（任务逻辑），[[mcp]] 定义 **怎么连**（通信协议）。二者配合形成"能力层 + 连接层"两层架构。

## 跨平台对比

| 平台 | Skills 形态 | 备注 |
|------|------------|------|
| Google Data Agent Kit | Agentic Skills（Python/声明式） | 开源，跨 IDE |
| [[maxcompute-skills\|MaxCompute]] | MCP Skills（SQL 模板 + Python） | 阿里云专有 |
| [[hologres-skills\|Hologres]] | CLI Skills + AI Function | 阿里云专有 |
| [[flink-skills\|Flink]] | 四层 Skills 体系 | 阿里云专有 |
| [[emr-skills\|EMR]] | AI Native Skills | 阿里云专有 |

## 趋势判断

Agentic Skills 正在成为云厂商 Data Agent 生态的标准能力单元。Google 以开源方式推动，国内云厂商以平台绑定方式推进。标准化 vs 平台锁定将是未来竞争焦点。

## 相关页面

- [[google-data-agent-kit]] — Agentic Skills 的原始出处与最完整实现
- [[code-agent-vs-data-agent]] — 为什么 Code Agent 需要 Skills 层才能处理数据问题
- [[data-agent-practice-guide]] — 上下文能力占 Data Agent 成功的 70%，Skills 正是上下文承载层
