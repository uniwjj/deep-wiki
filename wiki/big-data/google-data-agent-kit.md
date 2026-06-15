---
title: Google Data Agent Kit
description: 谷歌云开源的 AI 数据工程能力包——将数据技能、工具和插件嵌入 VS Code、Claude Code、Codex、Gemini CLI 等开发环境，实现意图驱动工程
aliases: [Data Agent Kit, Google Data Agent Kit, 谷歌数据智能体套件, agentic-skills]
tags: [big-data, ai-agent, tool, practice]
sources: [2026/06/15/谷歌开源 Data Agent Kit_数据工程正式进入原生 Agent 时代.html, 2026/06/15/Data Agent Kit brings data skills and tools to your IDE or CLI - Google Cloud Blog.html, 2026/06/15/Google Cloud Data Agent Kit 深度解析.html]
created: 2026-06-15
updated: 2026-06-15
---

# Google Data Agent Kit

> Google Cloud 2026-05-20 官方博客发布。定位：统一的开源数据工程与数据科学技能、工具和插件集合，直接嵌入开发者日常使用的 IDE 和 CLI 环境。

## 核心理念：从手动编码到意图驱动工程

传统模式：数据从业者手写 SQL/调度/校验逻辑 → 重复劳动多、质量不一致。

Data Agent Kit 模式：定义**期望的业务结果、约束条件和成功标准**，由 AI 增强系统自主决策如何执行。这解决了当前构建智能体应用时的"上下文窗口税"（context window tax）——开发者不再需要手动将大量 schema 元数据粘贴到 prompt 中。

**意图驱动开发示例**——不再逐行编写 ETL 代码，而是向 Coding Agent 描述业务目标：

> "把 GCS 里的原始交易日志摄入 BigQuery Iceberg 表 → 用 dbt 去重、清洗、关联身份表 → 在 Spark 上训练 LightGBM 欺诈检测模型 → 批量推断并写入 Spanner → 编排整条 Pipeline。"

在幕后，Data Agent Kit 自动规划多步骤编排：创建 Notebook 完成摄入、SQL 转换、模型训练、批量推断及调度；若 Pipeline 失败，自动 RCA + 起草修复代码 + 通过 Git 工作流验证部署。

## 架构组成

```
开发环境接入层（IDE/CLI）
  VS Code  Claude Code  Gemini CLI  Codex  Antigravity CLI
         ↓
  Data Agent Kit 核心（开源）
  ┌──────────────┬──────────────┬─────────────────┐
  │ Agentic      │  MCP Tools   │  预置 Agents     │
  │ Skills       │  BigQuery    │  Data Eng (GA)   │
  │ SQL优化/ELT  │  AlloyDB     │  Data Sci (GA)   │
  │ ML最佳实践   │  Spanner     │  DB Obs (Preview)│
  │ 治理/排障    │  GCS         │                  │
  └──────────────┴──────────────┴─────────────────┘
         ↓
  Google Cloud 数据底座（智能路由 → BigQuery / Spark / Spanner / AlloyDB / GCS）
```

三模块构成的一体化数据智能工具集：

| 模块 | 功能 | 详细说明 |
|------|------|---------|
| **Agentic Skills（预制数据技能库）** | 封装标准化数据任务 | 查询优化、ML 最佳实践、数据校验、数据漂移检测、数据治理、故障排查——以预编码路径形式注入工作流 |
| **MCP 工具层** | 安全连接 AI Agent 与数据平台 | 标准化封装 BigQuery、AlloyDB、Spanner、GCS 的连接参数，免手动管理 pipeline 代码 |
| **预置 Agents** | 开箱即用的数据智能体 | Data Engineering Agent (GA) / Data Science Agent (GA) / Database Observability Agent (Preview) |

### 预置 Agents 详情

| Agent | 状态 | 核心职责 |
|-------|------|---------|
| **Data Engineering Agent** | GA | 从头构建复杂 Pipeline，自动强制执行治理规则 |
| **Data Science Agent** | GA | 自动化模型全生命周期，跨 BigQuery Dataframes 和 Serverless Spark 弹性扩展 |
| **Database Observability Agent** | Preview | 7×24 基础设施守护，诊断根因并执行数据库自愈 |

## 统一数据中心（Unified Hub）

Data Agent Kit 将整个数据资产呈现为单一视图：

- **覆盖范围**：BigQuery、AlloyDB、Spanner 等数据库 + 数据工程/科学任务 + 编排流水线 + 作业
- **智能路由**：自动选择最优计算引擎——BigQuery 处理 SQL 原生分析和 ELT，Spark 处理自定义 Python 转换和分布式 ML 训练
- **全生命周期管理**：从数据发现到生产部署，无需切换上下文

## 解决的核心痛点

1. **Context Gap（上下文鸿沟）** — AI Agent 不理解企业对"毛利率"等业务概念的特定定义就可能犯代价高昂的错误；解决方案：Knowledge Catalog 提供企业特定业务语义
2. **工具割裂** — IDE、AI 助手、BigQuery 控制台、调度 CLI 来回切换；解决方案：统一嵌入开发环境
3. **上下文窗口税** — 开发者手动粘贴大量 schema 元数据到 prompt，消耗 token 限额、增加延迟；解决方案：MCP 标准化连接，Agent 自动获取上下文
4. **AI 不懂数据规范** — 通用 LLM 的 prompt 缺乏企业级数据工程最佳实践沉淀；解决方案：Agentic Skills 内置安全规则 + 自动校验

## 战略分析：逆向下沉（Reverse Cloud Strategy）

传统云厂商策略是"把数据搬上来"，而 Data Agent Kit 的策略是**"把能力送下去"**。

现实是大量企业数据根本不在 GCP 上——Hive/Spark 跑在自建机房，Oracle 跑在专线另一头，敏感数据因合规永远不会迁云。与其等数据上来，不如让 Agent 能力通过 MCP Tools 搭桥，直接操作本地/私有环境的数据资产，而调用记录和计费留在 GCP 侧。

**控制点的迁移：**

| 时代 | 控制点 | 手段 |
|------|--------|------|
| 传统云计算 | 算力与存储 | 数据必须上云 |
| Agentic 时代 | **开发者工具链** | SDK / MCP Server 注入 IDE |

谁的 MCP Server 被装进 VS Code 和 Claude Code，谁就在 Agent 每次调用数据时获得感知和影响力——不必真正拥有数据。

## 安装与使用

根据工作环境选择入口：

```
# GitHub（适用于 Gemini CLI、Claude Code、Codex）
git clone https://github.com/googleapis/data-agent-kit

# VS Code Marketplace → 搜索 "Google Cloud Data Agent Kit"
# Claude Code / Antigravity CLI → 通过各自插件市场安装
```

安装后通过 IAM 登录，Cloud Storage、数据库、Catalog 资产即刻出现在工作区。

## 行业影响

### 数据开发范式转移：代码驱动 → 意图驱动

工程师从写 Spark Job、配 Airflow DAG，转变为描述业务目标并审查 AI 生成的方案。

### MCP 成为 Agent-to-Data 标准协议

Data Agent Kit 全面押注 MCP，将 BigQuery、Spanner、AlloyDB 等包装为 MCP Server。对 Hive/Spark 等开源数仓而言，若无 MCP 接入层，未来与 AI Agent 的集成摩擦将显著增大。

### 开发工具生态重组

通过将数据平台能力以 Agentic Skills 形式注入 Claude Code、VS Code 等开发者工具，Google 打通了 AI Coding Agent 与企业数据底座之间的最后一英里，对 IDE 厂商和数据平台厂商均形成整合压力。

### HITL → HOTL 自动化加速

Pipeline 自愈能力（自动 RCA + 自动 Git PR）是 **HOTL（Human Out of the Loop）** 在数据运维领域的早期落地形态，代表 AI Agent 在数据系统中自主决策权限的持续扩张。

## 竞争格局

| 厂商 | 产品 | 路线特点 |
|------|------|---------|
| Google | Data Agent Kit | 开源套件 + 多 IDE 集成，生态开放 |
| AWS | Bedrock Data Automation | 平台封闭，与 AWS 生态深度绑定 |
| Databricks | Mosaic AI | 统一分析平台，Lakehouse 原生 |

Google 选择了**开源套件 + 多 IDE 集成**路线，而非封闭平台，对企业自定义和迁移成本更友好。竞品缺乏"本地 IDE + 自研大模型 + 云数仓"的完整链路。

## 实践启示：对开源数仓团队的建议

对于使用 Hive/Spark 技术栈的数据平台团队：

- **不需要迁移数据**：部署兼容 MCP 协议的本地 Server，Claude Code / Gemini CLI 即可感知本地 Schema、血缘、治理规则
- **YAML Spec 可作为 MCP 上下文投喂层**：SDD 系统中的 Spec 定义天然适合作为 Knowledge Catalog 的等价物，向 Agent 传递业务语义
- **开源优先**：Data Agent Kit 以 Apache 2.0 开源，可在私有化环境中自行部署和定制 Skills

## 国内云产品兼容性

结论：**基本不可用**。Data Agent Kit 未针对国内云厂商做专属适配。

- **可直接连接**：兼容 PostgreSQL/MySQL 协议的国内 OLAP（Hologres、TDSQL-C、GaussDB、ByteHouse）
- **有较大适配成本**：MaxCompute、WeData、DLI 等专有大数据平台，原生语法/调度体系差异大

三大痛点：鉴权不原生（需手动 AK/SK）、云特有语法无法自动生成、无法打通 DataWorks/WeData 的数据目录与血缘。

## 相关页面

- [[agentic-data-cloud]] — Data Agent Kit 是 Google Agentic Data Cloud 战略中"Agentic-First 从业者体验"的核心落地产品
- [[agentic-skills]] — Agentic Skills（预制数据技能库）概念页，Data Agent 核心抽象单元
- [[code-agent-vs-data-agent]] — Code Agent 与 Data Agent 的目标函数分叉
- [[openai-data-agent]] — OpenAI 的 Data Agent 方案，竞品对比
- [[databricks-genie]] — Databricks 的企业级 Data Agent 方案
- [[data-agent-practice-guide]] — 火山引擎数据智能体实践指南
- [[maxcompute-skills]] — MaxCompute 的 MCP + Skills 体系
