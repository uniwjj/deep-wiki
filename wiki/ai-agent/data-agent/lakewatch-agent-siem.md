---
title: Lakewatch 与 Panther 收购：Agent 驱动的安全湖仓
description: Databricks 推出 Lakewatch——构建在安全数据湖仓之上的 Agent 驱动 SIEM 系统，并收购 Panther Labs 补齐 AI 行为威胁检测，完成 AI 生产、调度、落地、安全、治理全栈闭环
aliases: [Lakewatch, Panther Labs, Agent SIEM, 安全湖仓, Databricks 安全收购]
tags: [big-data, ai-agent, platform, tool]
sources: [2026/06/30/Databricks Data + AI Summit 2026 深度回顾：Genie One、Lakebase 与 Agent 全家桶.html]
created: 2026-06-30
updated: 2026-06-30
---

# Lakewatch 与 Panther 收购：Agent 驱动的安全湖仓

Lakewatch 是 Databricks 在 2026 年 Data+AI Summit 推出的**构建在安全数据湖仓之上的 Agent 驱动 SIEM（安全信息与事件管理）系统**。同期 Databricks 收购 Panther Labs，补齐 AI 安全运营能力。

## 核心思路

Lakewatch 的核心思路是：将**安全数据、IT 数据和业务数据统一在治理化的湖仓中**，通过 AI Agent 实现检测和响应。

这呼应了 [[databricks-2026-summit|Databricks 2026]] 的核心论点——"AI 没有智能问题，它有上下文问题"。安全检测同样需要上下文：孤立的日志数据难以判断威胁，但当安全事件、IT 状态和业务行为在同一治理化湖仓中关联时，Agent 能做出更准确的检测和响应。

## 收购 Panther Labs

2026 年 6 月 16 日，Databricks 宣布收购 **Panther Labs**——一家领先的 AI 安全运营中心（SOC）平台公司。

| 发言人 | 角色 | 引述 |
|--------|------|------|
| Ali Ghodsi | Databricks CEO | "传统 SIEM 从未为 AI 设计过。" |
| Jack Naglieri | Panther CEO | "SOC 正处于转折点：AI 正在改变攻击的发起方式，防御者终于能够跟上 AI 攻击的步伐。" |
| Tim Nguyen | Anthropic 安全负责人 | "Panther 帮助我们为检测和响应带来了软件工程方法。" |

这是 Databricks 的**第三次安全收购**（此前收购了 Antimatter 和 SiftD.ai），标志着其向安全市场的战略进入。

## 在控制面中的位置

Lakewatch + Panther 是 [[databricks-agent-control-plane|Databricks Agent 控制面]]中**运行时治理**层的延伸——[[databricks-2026-summit|Unity AI Gateway]] 管控 Agent 的模型调用、工具使用、成本与安全，而 Lakewatch 把安全数据本身也纳入同一湖仓，让安全检测本身 Agent 化。

两者共同体现"**Agent 安全成为新型治理主战场**"这一趋势：安全防护从"边界防御"演进至"运行时纵深防御"，治理对象从静态数据资产扩展至动态 Agent 行为。

## 全栈闭环

至此，Databricks 完成 **AI 生产、调度、落地、安全、治理全栈闭环**：

- **生产** — Agent Bricks（10 万+生产 Agent）
- **调度** — Omnigent 元编排
- **落地** — Genie One / CustomerLake
- **安全** — Lakewatch + Panther
- **治理** — Unity AI Gateway + Unity Catalog

## 相关页面

- [[databricks-2026-summit]] — Databricks 2026 四层架构变革综述
- [[databricks-agent-control-plane]] — Databricks Agent 控制面战略，Lakewatch 是运行时治理延伸
- [[ai-governance]] — AI 治理，运行时纵深防御的理论框架
- [[ai-agent-security]] — AI Agent 安全，Prompt 注入/PII 泄露等风险治理
