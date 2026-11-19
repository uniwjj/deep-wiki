---
title: Databricks Genie
description: Databricks 的企业级 Data Agent，通过 specialized knowledge search、parallel thinking 和 Multi-LLM 设计，在内部 benchmark 上将准确率从 32% 提升至 90%+；2026 年衍生为面向业务团队的 Genie One 与面向工程的 Genie Code，后者可作为定时任务或 Lakeflow Jobs 任务被调度
aliases: [Databricks Genie, Genie, Genie One, Genie Code, Pushing the Frontier for Data Agents with Genie]
tags: [big-data, ai-agent, tool]
sources: [2026/05/18/Pushing the Frontier for Data Agents with Genie.html, 2026/06/30/从Databricks产品发布会看数据平台的演进方向.html, 2026/06/30/Databricks官方博客-Introducing Genie One Genie Ontology Genie Agents.md, 2026/11/19/Databricks Genie Code 调度能力（官方文档摘录）.md, 2026/11/19/Databricks Agent 代码执行环境（官方文档摘录）.md]
created: 2026-05-18
updated: 2026-11-19
---

# Databricks Genie

Genie 是 Databricks 面向 Lakehouse 全量资产（结构化表、dashboard、notebook + 非结构化 workspace files、Google Drive、SharePoint 文档）的企业级 Data Agent。博文《Pushing the Frontier for Data Agents with Genie》于 2026-05-08 发布。

## 核心数据

在 Databricks 内部真实数据分析 benchmark 上：

| 阶段 | 准确率 |
|------|--------|
| 领先 coding agent baseline | 32% |
| 加入 specialized knowledge search | +40% 表搜索提升 |
| 加入 parallel thinking | 显著提升 |
| 加入 Multi-LLM + 优化 prompt | **90%+** |

## 四个技术阶段

原文通过一个真实（匿名化）用户案例展示 Genie 的执行轨迹：

1. **并行多 Agent 资产发现** — 多个搜索子 Agent 并行扫描 tables、dashboards、documents
2. **数据调查** — SQL 提取、对比分析、根因调查
3. **自我修正循环** — 中间计算结果揭示初始假设错误，Agent 自动纠正
4. **最终验证** — 确认结论可复核

案例场景：用户发现两个企业 dashboard 对同一产品收入显示矛盾的数据尖峰，Agent 需要跨系统发现（表 + 内部文档 + dashboard），推理多日报告的设置方式，挖掘企业定价细节找到合同费率，并在中间计算发现初始假设错误时自动修正。

## 三大技术突破

详见单独页面：

- [[specialized-knowledge-search]] — 利用企业数据资产（表、notebook、dashboard、文档）构建语义搜索索引，表搜索性能提升 40%
- [[parallel-thinking]] — 采样多条轨迹、聚合跨轨迹信息计算最终答案，弥补缺少确定性测试的不足
- [[multi-llm-design]] — 不同子 Agent 使用不同 LLM + 优化 prompt（含 GEPA 成本优化方法）

## Data Agent 的三个独特挑战

与 [[code-agent-vs-data-agent|Code Agent]] 对比：

1. **数据发现规模** — 百万级结构化+非结构化资产让传统搜索失效
2. **确定"真实来源"业务知识** — 多源信息（表元数据、公司文档、内部消息）往往过时、矛盾或被取代，Agent 必须判断最权威信息
3. **缺少可验证测试** — 没有单元测试式的 oracle，"规格"仅是高层用户提问；而且有些问题本身就不可回答（数据不完整）

## 2026 演进：Genie One / Genie Code 与 Agent Bricks 规模化

在 2026 年 Data+AI Summit 上，Genie 从单一 Data Agent 演化为产品系列（官方博客 2026-06-16），详见 [[databricks-2026-summit]]：

| 产品 | 定位 |
|------|------|
| **Genie One** | 面向业务团队的数据感知型 AI 同事（data-smart AI coworker），正式 GA，跨 Web/iOS/Android/Slack/Teams，从洞察走向行动 |
| **Genie Agents** | 从 Genie Spaces（客户已创建超 100 万个）演化的领域特定自主 Agent，单 Prompt 创建，可推理非结构化数据，多步自主行动 |
| **Genie Code** | 面向数据工程与 ML 工作流 |

Genie One 的核心能力：跨整个数据资产连接（Lakehouse Federation/Lakeflow Connect/双向集成 Gmail/Slack/Teams）、完整 agentic-cowork 能力（调度/告警/监控/文档创建/自定义技能/自定义 MCP）、嵌入 Slack 和 Teams @mention、移动应用、Genie MCP App（让已有 AI Agent 的组织无需改变工作流即受益）。

Genie 系列采用**按用量计费（Pay-as-you-go）**模式，标志着 AI 原生时代"按调用价值计费"正在取代传统席位制收费。

承载 Genie 的 **Agent Bricks** 托管式 Agent 平台已承载超 **10 万个生产 Agent**，年 Token 消耗量超 **10¹⁵**。Genie 的语义底座升级为 [[genie-ontology|Genie Ontology]]——以"人工定义＋自动生长"双轨制构建企业知识图谱，借鉴 PageRank 思路识别权威业务定义，将复杂业务首问命中率提升至 84.5%（官方内部 28 题基准，最强通用编码 Agent 仅 52.4%，且 Genie 快 2 倍）。

> Foot Locker 客户证言："Genie isn't just a tool; it's the engine driving self-service insights across our organization."

## 调度：把 Agent 接进平台自己的定时体系

Databricks 支持"调度 Agent"——不是给 Agent 框架外挂一个 cron，而是让 Genie Code 成为调度系统里的一等任务。官方两条路径共用同一套运行时（权限、工具、MCP 连接），区别在编排粒度（文档 2026-10-30 / 2026-11-05）：

| | 定时任务（Scheduled tasks） | Job 任务（Lakeflow Jobs） |
|---|---|---|
| 位置 | Genie Code 会话内的 Schedules 面板 | Jobs 的 task type |
| 粒度 | 个人级后台任务 | 工作流中的一个 DAG 节点 |
| 每次运行产出 | 一个独立的可继续 chat | 一条可继续会话 + 运行记录里的输出 |
| 编排能力 | 仅调度、Run now、暂停 | 依赖、重试、超时、通知、If/else、For each |
| 配置方式 | 自然语言 / 建议任务 / 手动表单 | 仅 Jobs UI |

**定时任务**：可以说一句"每天早上 6 点汇总夜间 job 状态"让它自己建，也可以手动填 Title/Prompt/Schedule/Timezone。每次触发都把 prompt 当一次**完整会话**执行并生成新 chat（不是续同一会话），可在 Run history 里回看、继续对话。任务以**创建者权限**运行、受 Unity Catalog 管控；不能共享给他人（单次 chat 可以）；计入 Genie 用量。

**Job 任务**：在 Lakeflow Jobs 里新增 `Genie Code` 类型任务，prompt 可读上游任务输出，继承创建者的 Genie Code 设置与自定义 MCP 连接。prompt 支持双花括号插值：`{{job.id}}`、`{{job.start_time.iso_date}}`、`{{tasks.<task>.values.<key>}}`、For each 的 `{{input}}`、以及 job 参数 `{{region}}`；下游可用 `{{tasks.<task>.output.response}}` 取最终答复，接 If/else 或下一个 Agent 任务。于是"取数 → 汇总 → 判断 → 发报告"可以排进同一张 DAG，直接复用既有的重试、超时、告警与权限。

**执行环境**：Agent 本体（规划、模型调用、工具编排）是 Databricks 托管服务，不在工作区 compute 里；真正跑代码的是 Databricks compute——默认 **serverless**（Databricks 管理的 serverless compute plane，不在你的云账号里开机器），在 notebook / SQL editor 里则自动复用当前选中的 compute。两个任务的配置里都没有 compute 选项，等于固定走这条默认路径。三个推论：① 官方从未承诺 serverless 内部是容器（公开口径只有 "Databricks-managed" 与 "multiple layers of security"），按容器做假设会踩空——没有镜像入口、没有常驻 localhost 服务、本地磁盘不跨运行保留，产物要写 UC 表或 Volume；② serverless 是 **versionless**，运行时由 Databricks 自动升级，锁不住版本；③ 要确定性环境，只能自己写 notebook / Python 任务挂 classic compute，用 Databricks Container Services 指定 Docker 镜像——但那是**节点级**容器（一台 VM 一个容器），且失去 Genie Code 任务的原生编排。

**风险边界（关键）**：两条路径都**强制开启 auto-approve**，无法关闭，也没有运行前人工确认。Genie Code 用 AI 分类器按 prompt 范围审查每个工具动作、拦截越界的高风险操作，但官方明确写着"auto-approve 是生产力特性，不是安全边界"。无人值守时要提前划定的是：这个 prompt 能触达哪些生产数据、共享资源和外部系统（Slack/Jira/自定义 MCP）。

**典型用法**：每早汇总夜间 pipeline 结果并发报告；扫描新到数据标记异常；初查 Jira 工单并给出修复建议；生成周度合规审计。

**判断**：调度把 Agent 从"对话工具"变成"调度单元"——一旦进入 DAG，它继承了调度系统几十年积累的依赖、重试、告警、权限语义，也继承了相应的责任（谁在何时被自动执行了什么）。这与 Databricks 把 Agent 纳入运行时治理的方向一致（[[databricks-agent-control-plane]]），也对应 [[lakewatch-agent-siem]] 所述"AI 生产、调度、落地、安全、治理"闭环中的调度一环。

## 相关页面

- [[databricks-2026-summit]] — Databricks 2026 四层架构变革，Genie 系列演进的背景
- [[genie-ontology]] — Genie Ontology：PageRank 式企业语义资产，Genie 的语义底座
- [[code-agent-vs-data-agent]] — Data Agent vs Code Agent 的维度对比
- [[specialized-knowledge-search]] — 技术突破一：语义搜索索引
- [[parallel-thinking]] — 技术突破二：多轨迹采样聚合
- [[multi-llm-design]] — 技术突破三：多模型分工架构
- [[openai-data-agent]] — OpenAI 内部 Data Agent
- [[dataworks-data-agent]] — DataWorks Data Agent
- [[infinisynapse]] — InfiniSynapse 对三个挑战的回应
- [[data-agent-practice-guide]] — 火山引擎数据智能体实践指南
- [[sora-system-of-record-for-agents]] — SORA：Genie 作为面向业务的智能体入口，被定位为记录系统的交互层
- [[databricks-agent-control-plane]] — Databricks Agent 控制面：调度是"工程执行"层把 Agent 纳入运行时治理的入口
- [[lakewatch-agent-siem]] — Lakewatch + Panther：安全湖仓补上"调度"之后的检测与响应
