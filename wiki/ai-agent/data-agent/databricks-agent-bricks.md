---
title: Databricks Agent Bricks
description: Databricks 的 Agent 开发平台（2025 首发、2026 扩展）——不绑定框架与模型，交付 Agent Runtime、DurableAgentServer、Databricks Sandbox、Lakebase 托管记忆、MCP 工具、MLflow 评测与 Unity Gateway 治理；2026 年承载 10 万+ 生产 Agent、年消耗 10¹⁵ tokens
aliases: [Agent Bricks, Databricks Agent 平台, Agent Runtime, Databricks Sandbox, Agent 工作台]
tags: [big-data, ai-agent, platform, tool]
sources: [2026/11/19/Databricks Agent Bricks 平台（官方文档摘录）.md, 2026/06/30/Databricks Data + AI Summit 2026 深度回顾：Genie One、Lakebase 与 Agent 全家桶.html]
created: 2026-11-19
updated: 2026-11-19
---

# Databricks Agent Bricks

Agent Bricks 是 Databricks 的 **Agent 开发平台**（官方定义："the Databricks agent developer platform"）。它的定位不是"帮你生成 Agent"，而是**给已经用任意框架写好的 Agent 提供生产化底座**：不限定框架（LangGraph / OpenAI Agents SDK / Claude Code SDK / Agno / CrewAI）、不限定模型，平台负责运行时、隔离执行、上下文、评测和治理。

## 平台给的五件事

| 能力 | 组件 | 关键点 |
|------|------|--------|
| 部署运行 | **Agent Runtime** | 跑在 Databricks Apps 上，暴露稳定的鉴权 endpoint；`agentbricks deploy` 自动配资源与授权 |
| 持续执行 | **DurableAgentServer**（AgentKit） | 同步/流式/后台三种调用，run state 持久化，崩溃可恢复 |
| 安全跑代码 | **Databricks Sandbox** | 隔离环境执行 Agent 自己写的代码，权限受 Unity Catalog 约束 |
| 上下文 | 托管记忆与会话（Lakebase）、MCP、AI Search、Genie One MCP | 会话状态 + 跨会话记忆 + 企业数据/工具 |
| 质量与治理 | MLflow Tracing/评测、Unity Gateway、Unity Catalog | 每步可追、可评、可限流与审计 |

## 2026 年的规模与扩展

2026-06 Data+AI Summit 口径：Agent Bricks 已承载 **10 万+ 生产 Agent**、年 Token 消耗超 **10¹⁵**；新增 Kimi、Grok 模型；多框架支持扩到 Claude Code SDK / LangGraph / Agno / CrewAI / OpenAI Agent SDK；用 Lakebase 做 Agent 托管内存。在四层架构里，它属于 **Agent 平台层**，与 [[databricks-agent-control-plane|Agent 控制面]]的"工程执行"控制点对应，详见 [[databricks-2026-summit]]。

## 与 Genie 家族的分工

官方把边界写得很清楚：

| 产品 | 面向谁 | 形态 |
|------|--------|------|
| **Genie One** | 业务用户 | 问数、看板、用应用，不写代码 |
| **Genie Agents** | 不想写代码的人 | 单 prompt 生成的领域 Agent，可被自定义 Agent 当工具调用 |
| **Agent Bricks** | 写代码的开发者 | 自带框架与模型，平台管运行时与治理 |
| **Omnigent** | 要编排多个 Agent 的人 | meta-harness，产物可部署到 Agent Bricks（见 [[omnigent-meta-harness]]） |

旧产品 Knowledge Assistant、Supervisor Agent 已不推荐用于新 Agent。

## 调度位置（容易误判）

Agent Bricks 的 Agent 部署后就是一个 **endpoint**，它**没有**进入 Lakeflow Jobs 的原生任务类型——Jobs 的任务清单里，AI 相关的只有 `Genie Code` 和 `AI Functions`（见 [[databricks-genie]] 的调度一节）。所以"调度自建 Agent"的现实做法是：

- 用 notebook / Python 任务调 Agent endpoint（SDK 或 HTTP），把重试、依赖、告警交给 Jobs；
- 或者由外部调度器（Airflow 等）直接打 endpoint。

换句话说，**Databricks 原生调度的"Agent"是 Genie Code；自建 Agent 是"被调度系统调用的服务"**，两者不是同一层。

## 执行沙箱：这里的"容器"是真的

[[databricks-genie|Genie Code]] 的 serverless 执行在文档里只承诺"Databricks 管理的隔离环境"，没承诺容器；但 Agent Bricks 的 **Databricks Sandbox**（Beta）不一样，它是明确的容器语义：

- **4 vCPU / 8 GB**，不可调；单次最长 **24 小时**；
- home 目录 `/home/sandbox-agent` **跨启停持久**，上限 100 GB；home 之外 10 GB，停机即删；
- 可挂载 `/Workspace` 与 `/Volumes`；DBFS 根与云存储路径不可用；
- **支持自定义容器镜像**（存 Artifact Registry），每次沙箱启动即带好依赖；
- 入站全关、出站不限且不可配；凭据按沙箱身份签发、1 小时刷新；
- Beta 期结束可能**一次性清空**沙箱数据，不能存不可恢复的东西。

## 相关页面

- [[databricks-genie]] — Genie 家族与"原生调度 Agent"（Genie Code）的细节
- [[databricks-agent-control-plane]] — 六层控制点战略，Agent Bricks 是平台层落地
- [[databricks-2026-summit]] — 四层架构与 10 万 Agent 的规模数据
- [[omnigent-meta-harness]] — 多 Agent 编排的 meta-harness
- [[ltap-architecture]] — Lakebase 与 LTAP，托管记忆的存储底座
- [[agent-harness]] — Harness 与框架、平台的边界
