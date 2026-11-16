---
title: NVIDIA Auto-Ontology
description: NVIDIA 以 Apache 2.0 开源的语义层框架——Generate（从元数据自动生成本体）→ Certify（人工逐项认证）→ Answer（用受审 SQL 属性组装查询），配 10 种连接器、9 个 MCP 工具与 Ossie 导入导出；含三条语义层路线对位、对我方 L1–L4 的影响、五个风险与五项建议（内部竞品调研，2026-11-14）
aliases: [Auto-Ontology, NVIDIA Auto-Ontology, auto-ontology, NVIDIA 语义层, Generate Certify Answer]
tags: [ai-agent, big-data, tool, comparison]
sources: ["2026/11/14/NVIDIA Auto-Ontology 技术调研（2026-11-14）/index.html",
          "2026/11/14/NVIDIA Auto-Ontology 技术调研（2026-11-14）/upstream/README.md",
          "2026/11/14/NVIDIA Auto-Ontology 技术调研（2026-11-14）/upstream/docs-architecture.md",
          "2026/11/14/NVIDIA Auto-Ontology 技术调研（2026-11-14）/upstream/connector-base.py",
          "2026/11/14/NVIDIA Auto-Ontology 技术调研（2026-11-14）/upstream/skill-management.md"]
created: 2026-11-16
updated: 2026-11-16
---

# NVIDIA Auto-Ontology

> 来源：内部竞品调研《NVIDIA Auto-Ontology 技术调研（2026-11-14）》（观策·行业与竞品情报，编制者岱玹/蒋鸿翔；POPO 云文档 + 桌面 HTML 导出版，两份为同一报告）。方法为直接抓取 GitHub README/Tags/Commits 逐段核对，**未实际部署运行**，官方也未公布准确率。页内保留报告的【推断】标记，把「官方事实」「报告推断」「报告判断」分开写。**2026-11-16 对照上游仓库（README、docs/architecture.md、connectors/base.py、管理 skill）做了一次核实**，结论与修正见「抽取边界」一节；上游原始文档已归档在同目录 `upstream/`。

## 一句话定位

NVIDIA 用 Apache 2.0 开源了一个「从企业元数据自动生成本体、经人工认证后供 Agent 消费」的语义层框架，把行业正在争论的三条语义层路线中的第三条——**自动生成 + 人工认证**——做成了可复现的开源实现。报告的直接结论：语义层的「生成能力」正在商品化，剩下的护城河是「认证与治理流程」。

## 项目基本事实

| 项 | 内容 |
|----|------|
| 仓库 | `NVIDIA/auto-ontology`（配套评测仓 `NVIDIA/auto-ontology-eval`） |
| 官方描述 | 在数据之上构建语义、图结构推理层的 Python 框架，支持确定性查询生成、更好的上下文检索、更可解释的 LLM 与 Agent 工作流 |
| 许可 | Apache License 2.0（可商用、可修改、可再分发） |
| 活跃度 | 报告时最近提交 2026-11-14；Issues 66、PR 54 |
| 版本状态 | 尚无正式 Release（Tags 仅 `pre-main-merge-backup`），早期项目 |
| 贡献政策 | 明确不接受外部贡献——厂商主导的标准卡位，不是社区共建 |
| 技术栈 | Next.js 前端 + FastAPI 后端 + PostgreSQL 17/pgvector + FastMCP + 可选 Vault；Docker Compose 一键起 |
| 定位表述 | 「**补充**客户已有的语义模型」，不主张替换既有语义层 |

## 它解决什么问题

官方判断：Agent 能检索文档、能查数据库，但**可靠性**卡住了 Agentic Analytics——一个算得更快的营收指标，如果数字是错的就毫无价值。所以它提供一个「上下文层」，回答 Agent 必须知道的六个问题：

| 问题 | 内容 |
|------|------|
| 数据在哪 | 定位到具体表与列 |
| 是什么意思 | 业务术语、同义词、口径说明 |
| 该不该用 | 是否经过认证、是否适用当前问题 |
| 怎么用 | 受审的 SQL 表达式、join 关系、桥表 |
| 从哪来 | 血缘与来源（provenance） |
| 什么相似 | 向量检索出的候选术语与字段 |

报告判断：这套问法和「指标语义层是智能问数可信前提」是同一套逻辑，区别只在于它用开源项目把这件事工程化了。

## 三段式机制

### Generate（自动生成）

读取源库（抽 schema、对字段做数据画像、采样值写入 catalog）→ 命名实体（逐表抽取术语并映射到具体列）→ 解析关系（从主外键与数据分布推断 join 与桥表，并从**查询历史**中建议 SQL 属性）→ 写入本体（术语/关系/SQL 属性入 Postgres，向量入 pgvector）。官方示例：对 Wide World Importers 库编译一次，产出 31 个术语。

### Certify（人工认证）

用真实问题在 Chat 里试，或经 MCP 的 `check_answerable` / `ask_question` 验证；在 UI 或 API 修订术语、描述、SQL 属性；确认无误后逐项认证，答案必须携带 provenance。

**一处需要按官方文档修正的口径**：报告写「只有认证过的名称、描述、SQL 属性才会被 Agent 当作可信依据」，但 `docs/architecture.md` 明确说认证只是把该条目标记为「已审核」（在 UI 和搜索结果里体现），**不改变 Agent 的检索行为**；管理 skill 也写明认证 API 不保证原子发布，系统没有认证历史与版本血缘。所以它是「认证工作流 + 人工修订」的骨架，而不是硬门禁——报告把「应当如此」写成了「已经如此」。

### Answer（回答）

意图分类 + 抽取问题中的实体 → 向量检索候选术语、列、SQL 属性 → **用受审的 SQL 属性组装查询**（而不是让模型自由发挥）→ 对 SQL 做校验与修复 → 无法从现有数据回答时明确返回「不可回答」。

### 三类定义（本体的最小构成）

| 定义 | 内容 |
|------|------|
| Terms（术语） | 业务概念（如 Customer、Invoice Line），含描述与同义词，映射到承载它的列 |
| SQL attributes | 术语下经审阅的 SQL 表达式（如毛利率、滚动合计）；Agent 复用同一表达式，保证同一度量每次算法一致 |
| Relationships | 表如何 join，含桥表；从键与数据推断 |

【推断】SQL attributes 是「口径统一」的工程化落点：不靠文档约束，而靠**可复用的受审表达式**强制一致——比常见的「指标定义 + 口径文档」更硬。

## 工程实现

| 组件 | 技术 | 职责 | 端口 |
|------|------|------|------|
| 前端 | Next.js、Better Auth、Prisma | Web UI、认证、公共 API 网关 | 3000 |
| 后端 | FastAPI（Python 3.12+）、LangChain | Text-to-SQL Agent、catalog、本体 API | 3001 |
| 摄入服务 | FastAPI | 数据摄入与语义编译 | 3002 |
| MCP 服务 | FastMCP | 对公共 API 暴露只读 Agent 工具 | 3003 |
| 存储 | PostgreSQL 17 + pgvector | Catalog、本体、应用数据、向量库 | 5432 |
| 凭据 | HashiCorp Vault（可选） | 连接凭据托管 | — |

**连接器 10 种**：PostgreSQL、MySQL、Databricks、Snowflake、ClickHouse、Trino、Apache Kyuubi、HeavyDB、DuckDB、SQLite。【推断】清单里既有 Snowflake/Databricks，也有 Trino、Kyuubi 这类国内大数据栈常见引擎，说明它具备进入国内混合数仓环境的现实基础。

**模型角色四类可替换**：

| 角色 | 默认模型 | 用途 |
|------|---------|------|
| REASONING | nemotron-3.5-lightning-30b-a3b | 构建本体、写答案 SQL 与解释、判定可回答性 |
| NON_REASONING | 同上 | 意图识别、实体抽取、候选筛选、SQL 校验修复 |
| EMBED | nemotron-3.5-embed-1b | 索引表/列/术语/SQL 属性并匹配问题 |
| RERANK | nemotron-3.5-rerank-8b | 编译桥表 SQL 属性时对候选表排序 |

可替换为自托管 NIM 或任意 OpenAI 兼容端点——报告判断这让国内客户可以换成国产模型，比闭源产品更容易进入国内政企。

**MCP 9 个只读工具**：就绪检查 `check_readiness`；术语检索 `search_terms`、`get_term`、`get_term_columns`、`get_term_sql_attributes`、`get_sql_attribute`、`describe_table`；可答性判断 `check_answerable`；提问 `ask_question`（返回答案、SQL 与数据行）。每次调用按登录用户权限执行——与腾讯 DataBuddy、阿里 DataWorks 一致，报告称这是 Agent 上下文服务的「事实标配」。

**互操作**：支持 Apache Ossie（语义模型交换标准），可经 Settings → Import/Export 或 API 导入导出 Ossie YAML，指向「语义模型可跨工具迁移」。**可选预测**：配置 `KUMO_RFM_API_KEY` 后，预测类问题路由到 NVIDIA 托管的 KumoRFM，需把连接的表上传到该服务，默认上限 20 张表；【推断】数据不出域场景下这是硬约束，国内政企基本不可用。

## 抽取边界：只有数据库侧证据（2026-11-16 核实）

用户追问「是不是纯数据库抽取、有没有吃数仓代码与业务文档」，逐项对照上游仓库后确认：**是纯数据库侧抽取**。

**唯一的抽取面就是 connector 接口**（`connectors/base.py` 的六个抽象方法）：`get_tables`、`get_columns`、
`get_views`、`get_pks`、`get_fks`、`get_queries`（近 N 小时查询历史，引擎不支持则返回空表）。

官方 `docs/architecture.md` 列出全部四类输入：

| 输入 | 来源 | 必需 |
|------|------|------|
| 表、列、视图、主键、声明外键 | 每个连接 | 是 |
| 查询历史 | connector（如 Snowflake `QUERY_HISTORY`），引擎暴露才有 | 否 |
| 自定义分析（保存的 SQL） | UI/API 手写，或导入 | 否 |
| 既有本体与语义模型 | Settings → Import/Export（Ossie YAML 或自有 YAML） | 否 |

**明确没有的**：ETL/调度任务代码的解析（dbt 工程、SQL 脚本仓、任务编排配置都没有入口）；看板与 BI 定义；
业务文档（钉钉/Confluence/PDF/Markdown）；跨系统的任务级血缘。语义来源只有四样：库内 schema 与采样值、
查询历史里的 SQL、人工手写的 glossary 与 custom prompt（Settings → Semantic Input）、以及认证阶段的人工修订。
视图定义（`view_definition`）会抓，但那是库内视图 SQL，不是数仓任务的加工逻辑。

**它和大淘宝直播数据那篇正好是互补的两极**（[[taobao-live-data-llm-wiki]]）：

| 知识载体 | Auto-Ontology | 直播数据 LLM Wiki |
|---------|---------------|-------------------|
| 表结构（DDL） | 抓 | 抓 |
| 任务代码（怎么算、过滤、聚合、上游依赖） | **不抓** | 抓（"只有 DDL 的 Wiki 仅是骨架"） |
| 业务文档（业务背景、口径说明） | 不抓（只能人工填 glossary） | 抓 |
| 看板（谁在用、消费场景） | 不抓 | 抓 |
| 查询历史（SQL 工作量） | 抓（SQL attributes 的唯一依据） | 未作为主输入 |
| 血缘 | 无任务级血缘，只有 join 关系与答案溯源 | 显式图（8 类边） |

据此判断它的适用面：关系型数仓、口径主要体现在 SQL 与既有语义模型里的场景，它能给出可用的骨架；
「口径写在 ETL 代码里、业务含义在文档里、资产消费在看板里」的企业，只能靠 certify 的人工补与 Ossie 导入既有模型来填，
这也解释了它为什么强调「补充客户已有的语义模型」而不是替换。

## 三条语义层路线的对位（报告最重要的一节）

| 路线 | 代表 | 核心机制 | 人工介入 |
|------|------|---------|---------|
| 纯自动生成 | 阿里云 DataWorks Context Graph | 从数据资产自动扫描推理生成六层语义 | 低（事后修正） |
| 自动 + 人工沉淀 | 数势、Aloudata、我方 | 人工建指标本体与语义层 | 高（事前建设） |
| 自动 + 认证 | 腾讯云（语义模型→本体模型，自动生成后需人工确认）、NVIDIA Auto-Ontology（Generate → Certify） | 机器生成初稿，人逐项认证后才可信 | 中（生成后认证） |

【推断，置信度中高】「自动生成 + 人工认证」已被一家国内头部云厂商和一家国际芯片与 AI 巨头**独立收敛到同一形态**，基本可判定为该赛道的主流工程范式。报告的判断是：继续把「人工沉淀」当核心卖点，会在 POC 里被直接对比下去——对手既保留人工环节（认证），又省掉了从零建设的时间成本；可保留的差异化不在「生成」，而在「认证之后」——谁认证、如何留痕、如何版本化、如何与权限血缘联动、如何跨系统生效。

## 对我方四层能力的含义

| 层 | 能力 | 报告判断 |
|----|------|---------|
| L1 | 数据存储计算 | 中性偏利好：它要对接 10 种引擎，反向证明「多引擎兼容」是语义层落地的前置条件；更完整的国产库/信创引擎兼容清单是加分项 |
| L2 | 数据开发治理 | 压力上升：Certify 本质是治理流程（审批、留痕、版本），开源项目把这一环留给使用方——机会在于做成「语义认证工作流」而非通用数据治理 |
| L3 | 指标语义 | 最大冲击：生成能力被开源商品化，客户技术团队可自建；定价逻辑需从「卖生成」转向「卖认证、治理、行业资产与交付」 |
| L4 | 智能问数 | 间接冲击：MCP 让语义层成为 Agent 上下文服务，任何 Agent 都能接；问数产品价值要往「可信闭环 + 业务动作」上移 |

## 五个风险与触发信号

1. **语义层被开源替代**，独立语义层模块定价空间被压缩（Apache 2.0 允许商用与再分发，云厂商可直接包装成托管服务）。触发信号：客户 POC 提「先用开源方案跑一版」；竞品宣传「兼容 Auto-Ontology / Ossie」。
2. **Ossie 若成事实标准，锁定效应消失**——语义模型可导入导出，客户迁移成本大幅下降，「语义资产沉淀在我们这里所以换不掉」的逻辑被削弱。触发信号：更多厂商采纳 Ossie；招标要求「支持语义模型标准格式导入导出」。
3. **「生成 + 认证」成为主流**，人工沉淀叙事受压。触发信号：客户问「能不能自动生成语义层」；竞品演示「导入资产当天可用」。
4. **NVIDIA 以开源做生态卡位**，后续可能被云厂商集成进托管服务（不接受外部贡献 + 无正式 Release + 高频更新 = 厂商主导标准打法）。触发信号：主流云推出「一键部署 Auto-Ontology」；NVIDIA 与某云签联合方案。
5. **我方至今没有可对外披露的语义层量化口径**。阿里云 93.24% / 99%，腾讯云 95.9% / 83.5%，NVIDIA 提供了可复现的双盲评测框架（schema-only 对照组 vs 本体组）但未公布数字；对手若用该框架自测并公布，我方将第三次被迫应战且仍无数字。

## 安全设计的硬伤（容易被忽略的加分项）

官方 README 自己写明三条：摄入服务的 `ingest` / `delete` / `reset` / `compile` 端点**没有任何认证**；后端 FastAPI 信任前端转发的 `x-auto-ontology-user-id` 头判断用户身份；必须把 3001 / 3002 端口放在私网，否则等同裸奔。【推断，置信度中高】这是「开发者可用、企业生产未就绪」的早期项目——而身份认证、端点鉴权、审计留痕、私有化交付恰是政企与金融客户可逐条验证的差距点。

## 五项可执行建议

1. **用官方评测框架做一次双盲自测**（最高优先级，两周内）：同一批问题、同一模型、两组对照（仅 schema vs 带本体）、确定性验证器判分，跑出「口径增强前后的准确率差」。
2. **支持 Ossie 语义模型的导入导出**：工程量小，可直接应答招标里「语义模型可迁移」条款，也是「不怕客户迁移」的信任信号。
3. **提供语义层的 MCP Server**（对齐三家）：腾讯 DataBuddy、阿里 DataWorks、NVIDIA 都已用 MCP 暴露语义层。
4. **把 Certify 做成独立产品能力**：认证人、认证版本、变更留痕、与权限血缘联动、认证失效回收——开源最难补齐、云厂商最不愿细做。
5. **把安全与私有化做成对比清单**：针对 NVIDIA 三处短板整理「生产就绪度对照表」。

## 待验证与报告局限

待验证（报告列出）：官方评测相对 BIRD / BEAVER 的提升幅度；仓库首个公开版本日期；Ossie 的基金会归属与厂商支持度；是否支持国产数据库（连接器清单无达梦、人大金仓、OceanBase、GaussDB）；中文业务术语（复购率、动销）抽取质量；万级表规模下的表现（示例仅 31 个术语）。

局限（报告自述）：未实际部署运行，能力判断基于 README 与代码结构；官方未公布任何准确率，效果描述均来自定性表述；仓库早期、能力可能快速变化，建议 2—4 周后复核。

## 相关页面

- [[apache-ossie]] — 它支持导入导出的语义模型交换标准（Ossie YAML）
- [[open-semantic-interchange]] — 另一条语义交换标准路线（OSI，Snowflake 发起）；两者都指向「语义模型可迁移」
- [[ontology-five-routes]] — 五条本体建模路线；Auto-Ontology 属「自动生成 + 人工认证」这一形态的工程实现
- [[semantic-layer-to-ontology]] — 语义层向业务本体演进的产品侧经验（认证、复用与场景优先）
- [[dataagent-semantic-layer]] — 数仓语义层的另一种供给方式（语义层产物直接给 Agent）
- [[table-knowledge-graph]] — 数帆 EasyData 的表知识图谱：自动从 DDL/血缘/文档蒸馏 + 人工审核入图，与本页同构但更重图谱
- [[shufan-ai-native-data-platform]] — 同一编制者（蒋鸿翔）的数帆 AI Native 数据平台分享；本报告的「我方 L1–L4」未写明公司归属，页内按原文
- [[data-agent-semantic-stack]] — 语义栈八层视角，可对照它的「六问」定位
- [[taobao-live-data-llm-wiki]] — 互补的另一极：五类载体（DDL+任务代码+文档+看板+接口）全抓，恰是本页缺的深度
