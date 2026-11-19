---
title: DataWorks 自带 Agent 与 MCP 路线（官方文档摘录）
url: https://help.aliyun.com/zh/dataworks/user-guide/dataworks-agent-with-mcp
author: 阿里云
date: 2026-11-19
retrieved: 2026-11-19
type: docs
ingested: 2026-11-19
wiki_pages: [dataworks-data-agent]
---

# DataWorks 的"自带 Agent"路线：MCP Server + 三方客户端

> 节选。原文《基于三方客户端的 DataWorks Agent》（用户指南；页面未标注更新时间，按抓取日 2026-11-19 记录）。关键点：DataWorks 不自建 Agent 托管，而是把平台能力 MCP 化，交给外部客户端。

## 定位与原理

> DataWorks Agent基于MCP协议（模型上下文协议），可对接DataWorks MCP Server及其他大数据MCP Server（如Hologres MCP Server），旨在通过自然语言交互在DataWorks中实现数据开发、任务运维和数据集成等能力。

> **重要** 该功能基于三方客户端实现，若您希望体验更便捷的Agent智能体服务，可参见Agent 智能体。

> DataWorks Agent通过大语言模型（LLM）解析需求，并智能调用MCP Server能力完成任务执行。

> 例如…输入"我有几个工作空间"，Agent会通过LLM解析并调用内置DataWorks OpenAPI的DataWorks MCP Server提供的 **ListProjects** 工具查询…当任务较复杂时，LLM将与MCP Server进行多次交互。

> DataWorks Agent不仅集成DataWorks MCP Server，还支持接入其他MCP Server。您也可按需选择自己的LLM（比如千问、DeepSeek、OpenAI等）。

## 运行形态

> DataWorks Agent 通过 MCP 客户端插件（例如 **Cline**）构建前端对话界面，并通过 MCP Server 配置接入 DataWorks MCP Server 等阿里云 MCP Server。

> **使用限制**：仅支持在已启用新版数据开发的工作空间中，于**个人开发环境**中使用。

> 个人开发环境重启后，需要**重新安装 MCP Server 功能**，请谨慎操作。

> 在创建个人开发环境实例时，如果您选择的镜像是 `dataworks-mcp:py3.11-ubuntu22.04`，则无需升级程序引擎和安装 Cline 扩展程序。

## 计费

> - **DataWorks OpenAPI 调用费用**：当 Agent 通过 MCP Server 调用 DataWorks OpenAPI 时，将按照 OpenAPI 计费标准收取相应费用。
> - **LLM Token 费用**：Agent 在解析用户意图…需调用您配置的大模型（如千问等）…如使用百炼中的 `qwen-coder-plus` 模型时，费用将依据百炼计费说明计算。

## 与"Agent 智能体"（全托管）的关系

> 该功能基于三方客户端实现，若您希望体验更便捷的 Agent 智能体服务，可参见 Agent 智能体。

即：**全托管的是 DataWorks 自己的 Data Agent；自带 Agent 走"MCP Server + 你自己的客户端 + 你自己的模型 key + 个人开发环境"**，平台不托管你的 Agent 代码，也不提供 Agent 级运行时 endpoint。
