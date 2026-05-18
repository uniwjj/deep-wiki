---
title: InfiniSynapse
description: WinClaw 提出的 Data Agent 基础设施方案——InfiniAgent + InfiniSQL + InfiniRAG，通过 Agent 友好的工作语言、知识基础设施和可审计工作流构建企业级数据智能体
aliases: [InfiniSynapse, InfiniSQL, InfiniRAG, InfiniAgent]
tags: [big-data, ai-agent, tool, architecture]
sources: [2026/05/18/为什么 Code Agent 无法解决企业数据分析问题.html, 2026/05/18/为什么 Code Agent 无法解决企业数据分析问题.md]
status: draft
created: 2026-05-18
updated: 2026-05-18
---

# InfiniSynapse

WinClaw 提出的面向企业数据现场的 Data Agent 基础设施。三件组合工具回应 [[databricks-genie|Databricks Genie]] 揭示的三个挑战，参见 [[code-agent-vs-data-agent]]。

> 状态说明：信息来源为单篇博客，方案细节、性能基准、实际部署案例尚未在多源核实，标记为 `draft` / `wip`。

## 三件组合

| 组件 | 解决的问题 | 角色 |
|------|----------|------|
| **InfiniAgent** | 缺少 oracle 的不确定性 | 规划、小步探索、自我纠错 |
| **InfiniSQL** | Code Agent 不断改代码导致中间证据消失 | Agent 友好的工作语言，让工具调用结果沉淀成"虚拟数仓" |
| **InfiniRAG** | source of truth 难判断 | 把业务知识/口径/偏好/历史分析/文档绑定到数据源和执行链 |

## InfiniSQL：Agentic 工具调用替代不断改代码

### 设计目标

不是发明新 SQL 方言，而是给 Data Agent 一门**适合 Agentic 分析的工作语言**——每次工具调用的结果可以被下一次工具调用直接使用。

### 命名结果即下一步起点

```sql
select region, sum(amount) as revenue
from orders
group by region
as region_revenue;

select *
from region_revenue
where revenue < 0
as abnormal_region_revenue;
```

理解为两次工具调用：

1. 产出 `region_revenue`（按地区汇总收入表）
2. 基于它筛出异常地区，产出 `abnormal_region_revenue`

随着工具调用次数增多，session 里逐渐沉淀出经过探索、清洗、聚合、命名的中间表（`region_revenue`、`abnormal_region_revenue`、`east_customer_detail`、`refund_adjusted_revenue`、`campaign_revenue_bridge`、`final_business_readout`），形成面向当前问题的**虚拟数仓**。

这个虚拟数仓不是事先建好的静态 schema，而是在 Agent 分析过程中自然生长出来的。

> 在 Python 里追问越多代码越复杂；在 InfiniSQL 里追问越多虚拟数仓越丰富，后续分析越容易。

### 异构数据源 Join 作为原生语义

```sql
connect jdbc where url="mysql://..." as mysql_biz;
connect jdbc where url="postgresql://..." as pg_ops;

load jdbc.`mysql_biz.orders` as orders;
load jdbc.`pg_ops.customers` as customers;
load excel.`/data/campaign.xlsx` as campaign;

select o.order_id, o.amount, c.level, campaign.channel
from orders o
left join customers c on o.customer_id = c.id
left join campaign on o.campaign_id = campaign.id
as order_customer_campaign;
```

**避免的风险**（相对 Code Agent 把数据拉到本地 DataFrame 再 merge）：

- 本地内存撑不住
- 数据被拉出生产环境，合规压力上升
- 计算无法下推
- 中间文件和缓存形成新的泄漏面
- 跨源 join 难以审计

Agent 只需表达业务关系，引擎负责执行细节——不需要变成数据工程师处理驱动、分页、缓存、内存、临时文件、跨源 merge。

## InfiniRAG：业务知识是分析基础设施

### 反对"RAG = 检索几段文档塞进上下文"

> 数据库只能告诉 Agent 发生了什么。知识库告诉 Agent 这件事意味着什么。

`metric_key` 字段能告诉你 `PAGEVIEW`/`DOWNLOAD`/`download:tool:windows:x64:agent_excel` 出现多少次，但**不能**告诉你：

- `DOWNLOAD` 是下载意图还是确认安装
- `agent_excel` 属于 Office 自动化还是文件处理
- 这个指标能否对外发布
- 哪些 key 应归入同一需求簇
- 当前业务认可的漏斗口径是什么

这些知识在数据字典、产品文档、历史分析、业务规则、用户偏好、团队经验、甚至某人反复纠正过 Agent 的对话记录里。

### 写 SQL 之前先问知识库

InfiniRAG 让这些知识变成 Agent 分析过程中的**可调用能力**：

- 这个指标应该怎么定义
- 哪个口径是最新的
- 这个字段有哪些业务禁区
- 过去类似问题怎么分析
- 用户喜欢什么样的图表和报告结构
- 哪些结论必须标注不确定性

然后再用 SQL 验证可计算事实。

> 结构化数据负责事实计算，非结构化知识负责业务解释、口径边界、历史经验、偏好约束。两者分开 Agent 只是会查数；两者绑定 Agent 才像分析师。

这与 [[openai-data-agent|OpenAI Data Agent]] 的"六层上下文"思路一致，与 [[dataagent-semantic-layer|DataAgent 语义层]]在思想上呼应——业务知识不是装饰性上下文，而是分析本身的一部分。

## 可审计工作流

> 企业不是不能接受 AI，企业不能接受的是没有证据链的 AI。

合格的企业 Data Agent 必须留下：

- 使用了哪些数据源
- 查询了哪些表
- 生成了哪些中间表
- 每一步 SQL 是什么
- 哪些结论来自数据库事实
- 哪些解释来自知识库
- 哪些口径被采用
- 哪些资产被判定为 source of truth
- 哪些地方数据不足
- 哪些结论需要人工确认
- 最终报告对应哪些图表、文件和查询结果

这是 InfiniSynapse 的 Task View、SQL 轨迹、具名表、图表、文件和报告设计的目的——不是为了界面热闹，而是为了让企业能复核 Agent。

> Code Agent 给你最终脚本。Data Agent 必须给你分析证据链。

## 对外触达：Command Tools

InfiniSynapse 提供 Command Tools 让 [[claude-code|Claude Code]]、Cursor、Codex、WinClaw 这类 Code Agent 调用 Data Agent 能力。

**触达形态**：下载单个二进制 Command Tool 放进 `PATH` 直接使用。
- ❌ 不是 `pip install` 的 Python 包
- ❌ 不是常驻 MCP 服务
- ✓ 单二进制 + PATH

体现 [[code-agent-vs-data-agent|Code Agent 与 Data Agent 协作]]的分工：Code Agent 负责工程任务，调用 Data Agent 处理多轮追问、业务口径、跨源分析、报告沉淀。

## 与既有方案的对照

| 方案 | 对应组件 | 差异 |
|------|---------|------|
| [[openai-data-agent\|OpenAI Data Agent 六层上下文]] | InfiniRAG | OpenAI 强调离线统一向量化 + 查询时 RAG 拉取，InfiniSynapse 强调知识可调用化 |
| [[dataworks-data-agent\|DataWorks Data Agent]] | InfiniAgent + 受控执行内核 | DataWorks 把 Code Agent / Data Agent 融合在同一受控内核，InfiniSynapse 主张分工协作 |
| [[dataagent-semantic-layer\|DataAgent 语义层]] | InfiniSQL 命名中间表 | DataAgent 通过节点类型/连接器/调度参数定义语义；InfiniSQL 让中间表的命名本身承载语义 |
| [[data-agent-practice-guide\|火山引擎实践指南]] | 整体架构 | 火山指南强调"分层隔离/多路径冗余/置信度驱动/人机协同"，InfiniSynapse 三件组合是对该框架的具体实现思路之一 |

## 待考察问题

- InfiniSQL 引擎如何实现跨源 push-down？语法看起来类似 [Byzer/MLSQL](https://github.com/byzer-org/byzer-lang) 的 `connect`/`load`/`as` 语义，需要核实是否同源
- 虚拟数仓的中间表生命周期、隔离、共享策略
- InfiniRAG 与 [[rag-query-optimization]] 描述的标准 RAG 优化技术之间的具体差异
- 真实企业部署的准确率/一致性数据（目前只有 Databricks Genie 的 32%→90% 自报数据）

## 相关页面

- [[code-agent-vs-data-agent]] — 提出 InfiniSynapse 的问题背景
- [[databricks-genie]] — 文章引用的行业信号
- [[openai-data-agent]] — 六层上下文体系（与 InfiniRAG 对照）
- [[dataworks-data-agent]] — DataWorks Data Agent 受控执行内核
- [[dataagent-semantic-layer]] — DataAgent 语义层
- [[data-agent-practice-guide]] — 火山引擎数据智能体实践指南
- [[rag-query-optimization]] — RAG 查询优化技术
- [[agent-design-paradigms]] — Agent 设计范式
