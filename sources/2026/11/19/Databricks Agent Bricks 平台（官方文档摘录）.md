---
title: Databricks Agent Bricks 平台（官方文档摘录）
url: https://docs.databricks.com/aws/en/agents/
author: Databricks
date: 2026-11-17
retrieved: 2026-11-19
type: docs
ingested: 2026-11-19
wiki_pages: [databricks-agent-bricks, databricks-genie]
---

# Agent Bricks / Agent Runtime / Databricks Sandbox：官方文档摘录

> 节选，四份官方文档：① agents/（Develop agents，2026-11-12）；② agents/concepts/（What is Agent Bricks，2026-11-17）；③ compute/serverless/sandbox（Databricks Sandbox，2026-11-17，Beta）；④ 平台定位与组件表同源。

## 一、平台定义

> Agent Bricks is the Databricks agent developer platform. Use it to build and deploy the agents that power your agentic products and workflows. Agent Bricks is built for developers who write code. You bring an agent built with any framework or harness and choose any model.

核心工作流（原文四条）：

> - **Deploy your agent**: Agent Runtime hosts agents built with any framework or harness, stateful or stateless, and AgentKit adds durable execution so that runs survive restarts and crashes.
> - **Run code safely**: Agents run the code they write in Databricks Sandbox, an isolated environment whose home directory persists across sessions.
> - **Enrich your agent's context**: Agent memory and sessions let your agent recall what it learned in earlier conversations…
> - **Debug and test your agent**: MLflow Tracing records each step that your agent takes, and you can store and govern traces at scale in Unity Catalog.

## 二、计算栈三层

> The framework or harness, the agent server, and the agent runtime together form the agent compute stack.

> **Agent server**: `DurableAgentServer` wraps your agent loop and serves the invocation API, with synchronous, streaming, and background requests, persistent run state, and crash recovery.
>
> **Agent runtime**: Agent Runtime runs the agent server on **Databricks Apps** at a stable, authenticated endpoint. `agentbricks deploy` provisions the resources that your agent needs and grants it access to them.
>
> **Databricks Sandbox**: Databricks Sandbox is a tool that your agent calls to run the code it writes in an isolated environment, with access governed by Unity Catalog permissions.

## 三、与 Genie 家族的分工（原文）

> - **Genie Agents**: Genie Agents is for creating agents without code, such as agents that answer questions over your tables and documents. Agent Bricks is for developers who write their own agent code. A custom agent can call Genie Agents as tools.
> - **Genie One**: Genie One is for business users… Agent Bricks is for developers who build the agents themselves.
> - **Omnigent**: Omnigent is a meta-harness that composes and coordinates multiple agents, such as coding agents. You can use Omnigent to develop a custom agent that you deploy and run on Agent Bricks.
> - **Legacy agent builders**: Earlier products, such as Knowledge Assistant and Supervisor Agent, are no longer recommended for new agents.

## 四、Databricks Sandbox（容器边界的关键证据，Beta）

> Databricks Sandbox lets you run untrusted code in an isolated environment through the Databricks REST API, SDK, or Databricks CLI. The sandbox provides serverless, on-demand compute for running code dynamically.
>
> Each sandbox includes a persistent home directory. You can stop and start a sandbox at any time, and the data in your home directory persists.

> Databricks Sandbox comes with **4 vCPUs and 8 GB of RAM. This size is not configurable.**

存储：

> - Data in your home directory (`/home/sandbox-agent`) persists for the lifetime of your Databricks Sandbox…
> - Your home directory can store up to **100 GB**. You can't change this limit.
> - You can store up to **10 GB outside your home directory, but this data is deleted when you stop your sandbox**.
> - `/Workspace` 与 `/Volumes/<catalog>/<schema>/<volume>` 挂载为本地目录；DBFS 根与云存储路径不可用。

> **Custom images**: You can create a sandbox from your own container image stored in Artifact Registry, so every sandbox starts with your packages, tools, and configuration already installed.

网络与凭据：

> There are no restrictions on network egress from a sandbox, and egress is not configurable. Direct ingress to the sandbox's public IP is closed to all traffic, and you can't change this.
>
> Each sandbox is provisioned with a Databricks credential that authenticates as the sandbox's identity… Each token in the credential is valid for 1 hour and is refreshed automatically before it expires. Refreshing continues for up to 24 hours.

限制：

> - A sandbox can run for at most **24 hours** after each start…
> - Quotas: at most 200 active sandboxes per user (800 total), 400 active per workspace (2,000 total).
> - Databricks Sandbox is a compute primitive for running code, not a general-purpose Spark runtime.
> - **At the end of the Beta period, Databricks might perform a one-time purge of data stored in sandboxes.**

## 五、2026 规模（来自 2026-06 Data+AI Summit 复盘）

> Agent Bricks 已承载超 **10 万个生产 Agent**，年 Token 消耗量超 **10¹⁵**；新增 Kimi、Grok 模型支持，多框架支持 Claude Code SDK / LangGraph / Agno / CrewAI / OpenAI Agent SDK；通过 Lakebase 实现 Agent 托管内存。
