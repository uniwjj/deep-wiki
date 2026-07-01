---
title: 为什么 Code Agent 无法解决企业数据分析问题
url: https://zhuhailin.com/zh/blog/why-code-agent-cannot-solve-enterprise-data-analysis
author: WinClaw
date: 2026-05-16
retrieved: 2026-05-18
type: blog
ingested: 2026-05-18
wiki_pages: [big-data/infinisynapse, big-data/code-agent-vs-data-agent, big-data/databricks-genie]
---

# 为什么 Code Agent 无法解决企业数据分析问题

> 原始 HTML 见同目录 `.html` 文件。本 `.md` 为正文提取版，便于 ingest 与全文检索。

## 核心论点

- Code Agent 的目标函数是"把代码写出来并让它跑起来"。
- Data Agent 的目标函数是"在混乱、动态、语义密集的企业数据系统里，生产可信答案"。
- 这两个目标函数在企业现场会彻底分叉。

## Databricks Genie 给出的行业信号

Databricks 在 2026-05-08 发布《Pushing the Frontier for Data Agents with Genie》。Genie 面向整个 Lakehouse 的结构化与非结构化资产（tables、dashboards、notebooks、files、Apps、Google Drive、SharePoint、业务定义、元数据、历史分析资产）。

Databricks 自家内部 benchmark：加入 specialized knowledge search、parallel thinking 和 Multi-LLM 后，Genie 相比领先 coding agent，准确率从 32% 提升到 90%+。

文章把 Data Agent 的独特挑战概括为三点。

## 挑战一：百万级数据资产让传统搜索失效

代码世界有稳定的组织方式（路径/函数名/类型/import/测试/Git/README），可以靠搜索 + 跳转 + 依赖分析 + 测试反馈逐步理解。

企业数据资产则结构不统一、命名不一致、质量参差不齐：成千上万张业务表、多个数仓/湖仓/实时库、历史 dashboard、notebook、数据字典、飞书/Notion/Google Docs/SharePoint 文档、Excel 临时报表、API、业务团队的指标说明、多年前的脚本。

用户问"为什么两个收入 dashboard 的峰值日期不一样？" 真正相关的信息散落在订单事实表、财务确认收入表、dashboard 过滤配置、定价文档、notebook 转换逻辑、Slack/飞书讨论、最近改过的口径说明里。

关键词搜索容易失效：用户问"收入"，但表名 `rev_recognition_fact`、dashboard 写 `ARR`、文档写"确认收入"、财务口径叫"净收入"。

**结论**：企业 Data Agent 的第一能力不是写 SQL，而是找到相关数据资产，并判断它们之间的关系。

## 挑战二：动态环境里，真实来源很难判断

找到资产只是开始，真正难的是判断 source of truth。

典型冲突：
- 两张订单表（业务系统原始 vs 数仓清洗）
- 两个收入 dashboard（按下单时间 vs 按确认收入时间）
- 文档写旧口径，业务团队上月改过
- notebook 里有真实转换逻辑但没同步到数据字典
- 字段 `status` 不同业务线含义不同
- Excel 是老板临时改过的版本，不能进入正式分析

Code Agent 擅长"用已有上下文继续写代码"，不擅长"判断企业知识资产的权威性"。看到 `gmv` 直接拿来用，看到"收入看板"默认是正确口径，看到文档解释指标直接引用。

> 最危险的不是代码报错，而是：代码没报错，数字也算出来了，但用的是错口径。

Data Agent 必须能处理动态业务知识：哪个指标定义是最新版、哪个 dashboard 经过认证、哪个 notebook 藏着真实逻辑、哪些历史分析过期、哪些文档只是背景、哪些知识应优先于 schema、哪些结论只能说"不确定"。

## 挑战三：Data Agent 缺少像代码测试那样的确定性验证

Code Agent 可以靠 unit/integration test、lint、type check、build、e2e 闭环。

Data Agent 没有这种 oracle。"上季度华东收入为什么下降？" 隐含前提：上季度是自然/财务季度、华东按客户/门店/销售组织、收入是下单/支付/确认/净收入、下降是同比/环比/相对预算、数据是否完整入仓、是否有汇率/价格政策/退款周期/业务调整。

更麻烦的是有些问题本来就不可回答（缺关键数据源、口径无法对齐、历史数据断裂）。好的 Data Agent 必须能说"目前数据不足以支持这个结论"，而 Code Agent 默认会继续写代码尽量产出结果。

Data Agent 的可靠性不能只靠"模型聪明"，必须靠系统设计来补：搜索找到相关资产、知识判断 source of truth、执行过程留痕、中间结果可复核、报告区分数据库事实和业务解释、不确定性显式表达。

## 维度对比

| 维度 | Code Agent | Data Agent |
|------|-----------|-----------|
| 工作环境 | 代码库、文件系统、测试框架 | 动态企业数据现场 |
| 主要对象 | 代码文本和工程依赖 | 表、字段、指标、文档、dashboard、历史分析 |
| 成功标准 | 代码能运行、测试通过 | 答案可信、口径正确、过程可复核 |
| 反馈机制 | 报错、测试失败、构建失败 | 业务校验、证据链、口径一致性、不确定性表达 |
| 主要失败 | 不编译/测试不通过/行为错 | 找错表/信错口径/算对但解释错 |

数据分析里的错误很多时候不是语法/运行错误，而是语义错误。语义错误不会抛异常，会安静地生成一个看起来很专业的错误报告。

## "Code Agent + Excel / 数据库连接"的错觉

Code Agent 能做：小 CSV 一次性 EDA、Excel 清洗汇总、生成临时图表/dashboard、写简单 SQL、连一个数据库跑基础查询。共同点：数据规模小、上下文单一、风险有限、验证成本低。

但企业数据分析是连续追问：先看整体趋势 → 发现某区域下降 → 按行业下钻 → 找到异常客户 → 对比同期 → 检查价格变化 → 关联活动 → 排除节假日/汇率 → 重新计算净收入 → 形成结论与风险提示。

Code Agent 默认方式是不断改代码，注意力从"业务问题"转移到"工程问题"（包/API/驱动/类型/内存/临时文件/图表库）。最后代码可能能跑，但企业要的不是"能跑的代码"，而是"可信的分析"。

复杂代码本来就反人类，对 Agent 也难维护。探索过程被不断修改后，中间证据容易消失，企业最关心的问题反而回答不了：第一次为什么选这张表、第二次为什么改过滤、哪一步发现口径不一致、哪个中间结果支撑最终结论、下周重跑能否复现。

## InfiniSQL：用 Agentic 工具调用替代不断改代码

InfiniSynapse 的第一个回答是 InfiniSQL——给 Data Agent 一门适合 Agentic 分析的工作语言。

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

理解为两次工具调用：第一次产出 `region_revenue`（按地区汇总收入表），第二次基于它筛出异常地区，产出 `abnormal_region_revenue`。

随着工具调用次数增多，InfiniSQL session 里逐渐沉淀出经过探索/清洗/聚合/命名的中间表（`region_revenue`、`abnormal_region_revenue`、`east_customer_detail`、`refund_adjusted_revenue`、`campaign_revenue_bridge`、`final_business_readout`）——形成面向当前问题的"虚拟数仓"。

这个虚拟数仓不是事先建好的静态 schema，而是在 Agent 分析过程中自然生长出来。越往后 Agent 越不需要回原始明细表重新理解，可以站在已带业务语义的中间表上继续分析。

> 在 Python 里，追问越多代码越复杂；在 InfiniSQL 里，追问越多虚拟数仓越丰富，后续分析越容易。

### 异构数据源 Join 作为原生语义

Code Agent 拉数据到本地 DataFrame 再 merge 在 demo 没问题，企业里立刻遇到内存撑不住、合规压力、计算无法下推、缓存形成泄漏面、跨源 join 难审计等风险。

InfiniSQL 让不同数据源在同一个 session 里以表的形式共存：

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

Agent 不需要变成数据工程师处理驱动/分页/缓存/内存/临时文件/跨源 merge，只需表达业务关系，引擎负责执行细节。

## InfiniRAG：业务知识不是上下文，而是分析基础设施

很多人把 RAG 理解成"检索几段文档塞进上下文"，对企业数据分析远远不够。

> 数据库只能告诉 Agent 发生了什么。知识库告诉 Agent 这件事意味着什么。

`metric_key` 字段能告诉你 `PAGEVIEW`/`DOWNLOAD`/`download:tool:windows:x64:agent_excel` 出现多少次，但不能告诉你：`DOWNLOAD` 是下载意图还是确认安装、`agent_excel` 属于 Office 自动化还是文件处理、这个指标能否对外发布、哪些 key 应归入同一需求簇、当前业务认可的漏斗口径是什么。

这些知识在数据字典、产品文档、历史分析、业务规则、用户偏好、团队经验、甚至某人反复纠正过 Agent 的对话记录里。

InfiniRAG 要让这些知识变成 Agent 分析过程中的可调用能力：写 SQL 前先问知识库（指标定义、最新口径、字段业务禁区、过去类似分析、用户偏好的图表/报告结构、必须标注不确定性的结论），再用 SQL 验证可计算事实。

> 结构化数据负责事实计算，非结构化知识负责业务解释、口径边界、历史经验、偏好约束。两者分开 Agent 只是会查数，两者绑定 Agent 才像分析师。

## 可审计工作流

> 企业不是不能接受 AI，企业不能接受的是没有证据链的 AI。

合格的企业 Data Agent 必须留下：使用了哪些数据源、查询了哪些表、生成了哪些中间表、每一步 SQL、哪些结论来自数据库事实、哪些解释来自知识库、哪些口径被采用、哪些资产被判定为 source of truth、哪些地方数据不足、哪些结论需要人工确认、最终报告对应哪些图表/文件/查询结果。

这是 InfiniSynapse Task View、SQL 轨迹、具名表、图表、文件和报告这些设计的意义。

> Code Agent 常常给你一份最终脚本。Data Agent 必须给你一条分析证据链。

## Code Agent 与 Data Agent 的协作

两类 Agent 应该协作，而不是互相替代。InfiniSynapse 提供 Command Tools，让 Cursor、Claude Code、Codex、WinClaw 这类 Code Agent 可以调用 Data Agent 能力——下载单个二进制 Command Tool 放进 `PATH` 直接使用，不是 `pip install` 也不是常驻 MCP 服务。

| 场景 | 更适合的工具 |
|------|------------|
| 写代码、重构、修 bug、生成页面 | Code Agent |
| 小 CSV、一次性 EDA、临时图表 | Code Agent 可以胜任 |
| 多轮追问、业务口径、跨源分析、报告沉淀 | Data Agent |
| 企业数据库、权限、审计、私有化部署 | Data Agent |
| 结构化数据 + 非结构化业务知识联合分析 | Data Agent |
| 严肃决策、监管报告、金融风控、经营分析 | Data Agent |

## 结语

- Code Agent 的难点：在明确工程系统里把代码改对。
- Data Agent 的难点：在混乱、动态、语义密集的企业数据系统里，先找到该相信什么，再算出可信答案。

Data Agent 不是"会写 SQL 的 Code Agent"，需要自己的基础设施：

- **InfiniAgent**：规划、小步探索、自我纠错
- **InfiniSQL**：Agent 友好的数据分析语言，让工具调用结果不断沉淀成虚拟数仓
- **InfiniRAG**：把业务知识、计算口径、个人偏好、历史分析和非结构化文档绑定到数据源和执行链上

## 参考

- Databricks Blog, *Pushing the Frontier for Data Agents with Genie*, 2026-05-08. https://www.databricks.com/blog/pushing-frontier-data-agents-genie
