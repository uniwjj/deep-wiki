---
title: EMR Skills
description: 阿里云 EMR AI Native 平台——从 Lakehouse 到 Agentic Lakehouse，三大子产品 Skills（Spark/StarRocks/集群）覆盖五大场景，多模态 AI Function + 混合检索，全栈 Serverless 免运维
aliases: [EMR Skills, EMR AI Native, EMR Agent, Agentic Lakehouse]
tags: [big-data, ai-agent, platform, tool]
sources: [2026/05/12/06. 基于 EMR Skills 实现智驾场景的自动化数据标注.pdf, 2026/05/12/DataWorks DataAgent分享录音.md, 2026/05/28/EMR Skills：从 Lakehouse 到 Agentic Lakehouse.pdf]
created: 2026-05-12
updated: 2026-05-31
---

# EMR Skills

EMR 定位为**面向 Agent 的数据与计算能力平台**，不再只是"集群产品"，而是对外提供三类核心能力。

- 议题副标题：数据智能化的新一站
- 演讲人：**畅含**｜EMR 产品经理｜计算平台事业部（阿里云智能集团）
- 议题脉络：①EMR AI Native 全景图 → ②EMR Skills 能力 → ③EMR StarRocks Skills 能力介绍（开发·诊断·管控，五大场景全覆盖）→ ④EMR AI Native 展望

## 摄取说明

2026-05-31 按 CLAUDE.md 多模态资料首次摄取规则重新复核 `2026/05/28/EMR Skills：从 Lakehouse 到 Agentic Lakehouse.pdf`：使用 `pdftoppm -r 150 -png` 转为 22 张页面图片，按 PPT 高密度页面逐页视觉检查，不以派生 TXT/OCR 为唯一可信来源。封面、目录、章节转场页、架构图、能力地图、场景页、应用架构、路线图和结束页均纳入逐页索引；模糊小字仅摄取可可靠辨认的部分，不臆测。

本次复核修正了首次摄取的若干偏差：演讲人姓名（"董亚军"/"畅舍" → 畅含）、副标题（"新站" → 新一站）、AI Function 体系子项、EMR AI 助手/智能开发助手/Agent Skills 子能力清单、能力展望六项分列、P4 Lakehouse 架构图、P8 应用场景层 16 个子能力。

## 逐页摄取索引

| 页码 | 主题 | 已摄取信息 |
|------|------|------------|
| P1 | 封面 | EMR Skills：从 Lakehouse 到 Agentic Lakehouse；数据智能化的新一站；畅含｜EMR 产品经理｜计算平台事业部；右上角阿里云智能集团 logo |
| P2 | 目录 | 四部分：01 EMR AI Native 全景图、02 EMR Skills 能力、03 EMR StarRocks Skills 能力介绍（开发·诊断·管控，五大场景全覆盖）、04 EMR AI Native 展望 |
| P3 | 章节页 | 01 / EMR AI NATIVE 全景图 |
| P4 | EMR 与 Lakehouse 定义 + Lakehouse 数据流图 | 阿里云 EMR：基于开源框架的一站式大数据处理服务，是运行大数据任务的"操作系统"或"基础设施平台"；Lakehouse：一份数据、多引擎协同，通过元数据层与事务机制实现数仓级管理能力（EMR Serverless Spark + StarRocks + DLF 统一元数据）。下半部分配图：MySQL/MongoDB/Kafka/PULSAR/PostgreSQL/Redis 等数据源 →（Flink）→ ODS/DWD/DWS/ADS 各层用 Paimon Table 承载 → 各层之间 Spark 处理；底部统一在 Apache Paimon（流批一体湖格式）+ Data Lake Formation 之上，标注"流批一体湖格式""元数据管理""数据管理""事务能力" |
| P5 | Agent 协作数据平台问题 | 过去十年大数据平台核心目标"把数据存下来、算出来、用起来"；今天面对的不再只是 BI/报表/数据工程师/分析师，而是越来越多的智能体（Agent），它们自动读取数据、调用算力、生成结果，甚至驱动业务决策；提出问题"如何构建一个面向'智能体协作'的大数据平台" |
| P6 | Agent 时代新要求 | 从"支持人分析"走向"支持 Agent 自主调用"；三项新要求左列：更低门槛的调用方式（从作业/SQL 配置 → API/能力调用）、更统一的能力抽象（数据处理、查询分析、系统运维统一对外抽象成能力调用）、更实时的响应能力（支持 Agent 即时决策与交互）；右列对应"能力 Skill 化"：数据处理 Skill、查询分析 Skill、系统运维 Skill |
| P7 | EMR：面向 Agent 的数据与计算能力平台 | 中央方框"统一接口层 EMR 平台（数据 + 计算 + 运维）"，对外输出三类核心能力：数据处理 Skill（全托管 Spark）、实时分析 Skill（全托管 StarRocks）、系统运维 Skill（集群管理与自动化运维）；底部说明 EMR AI Native 大图将回答 ①Agent 时代大数据平台需要哪些关键能力 ②EMR 如何把这些能力"Skill 化"对外提供统一接口 ③这种模式在真实业务场景中能带来什么变化 |
| P8 | EMR AI Native 产品功能大图（五层架构） | 副标题：SQL 层原生大模型调用·百炼 MaaS 深度集成·全栈 Serverless 免运维。**应用场景层**（智能数据分析/智能数据开发/智能运维管理/多模态数据处理）下含 16 个子能力：自然语言查数据/SQL 自动优化/SQL Copilot 辅助/查询结果可视化、作业性能调优/SQL 方言转换/代码生成、集群健康巡检/异常诊断/根因分析/成本优化建议/预测式扩缩容、文档/图像智能处理/企业知识库 RAG/多模检索/音视频内容理解。**AI 能力层**：AI Function 体系（图片打标、embedding、图像处理、数据脱敏、语义提取、音频处理、时序预测、内容安全）/多模态处理引擎（非结构化数据加工、批量模型推理、Embedding、全文检索 Beta、向量检索 Beta、混合检索 Beta）/Agent Skills（集群/实例管理、作业管理、弹性管理、资源管理、运维管理、SQL 执行、元数据查询、智能诊断）/EMR AI 助手（实例管理、任务诊断优化、错误根因分析、健康巡检、产品咨询、成本优化）/智能开发助手（SQL 智能生成、代码生成、SQL 方言迁移、执行计划诊断、参数优化）。**AI 基础设施**：模型服务/Token 管控/推理优化/安全合规（百炼 Qwen 系列、自定义模型接入、用量仪表盘、按量计费、批量推理、异步调用、模型内容安全、审计日志）。**计算引擎层**：EMR on ECS/ACK（集群化部署、大规模批处理、弹性伸缩、多引擎支持）/Serverless StarRocks（实时数据查询、毫秒级 OLAP、向量检索 Beta、全文检索 Beta、混合检索 Beta、AI Function 推理）/Serverless Spark（AI Function 推理、批量多模态数据加工、Ray 分布式计算、Daft 多模态处理、GPU/GPU 资源支持）。**数据底座层**：统一存储（OSS/NAS/CPFS）+ 统一 Catalog（DLF Data Lake Formation）+ 湖格式（Paimon、Iceberg、Fluss） |
| P9 | 章节页 | 02 / EMR SKILLS 能力介绍 |
| P10 | EMR Skill 能力全景图 | 副标题"三大子产品通过 Skills 接入 Agent，覆盖集群管理、计算作业、分析数据的全场景"。三列对照：**EMR Serverless Spark（alibabacloud-emr-spark-manage）**——管理 Spark 工作空间全生命周期：①工作空间管理 ②作业提交与监控（JAR/PySpark/SQL 提交、状态/日志查询、取消） ③Kyuubi 交互式查询（SQL 网关、JDBC、Token） ④资源队列扩缩容（CU 扩缩、按需弹性），用法示例"帮我建个 Spark 工作空间""跑个 PySpark 任务，数据在 OSS 上"，底部诊断优化（作业失败分析、性能瓶颈定位、参数优化建议）；**EMR Serverless StarRocks（alibabacloud-emr-starrocks-manage + assistant）**——管理实例全生命周期 + 开发运维助手：①实例生命周期管理 ②弹性扩缩容 ③SQL 开发与调优 [assistant]（慢 SQL 诊断、Profile 分析、Join 策略优化、改写验证） ④建表设计与数据导入 [assistant]（分区分桶策略、Stream/Routine Load、Schema 设计），用法"帮我建个存算分离的 StarRocks，再扩 2 个节点""这条 SQL 跑了 30 秒，帮我优化"，底部诊断优化（慢查询分析、Profile 定位、表结构优化、Colocate Join、调参建议）；**EMR on ECS 集群管理（alibabacloud-emr-cluster-manage）**——管理 EMR 集群全生命周期：①集群生命周期管理（5 种集群类型、HA/普通部署、存算分离/一体架构、状态查询、克隆、删除） ②节点组管理与扩缩容（5 种节点角色 MASTER/CORE/TASK 等、新建节点组、手动扩缩容） ③弹性伸缩（定时与指标策略 CPU/内存/YARN 负载触发、伸缩历史） ④运维与诊断（集群与节点巡检、续费管理、属性修改、创建失败诊断），用法"帮我建个高可用数据湖集群""请帮我扩容节点 G 到 10 台"，底部故障与诊断（智能定位、根因分析、优化建议、自动调参）。整页底部："已支持接入：EMR Agent/Dataworks Agent/Qoder/Claude" |
| P11 | 接入方式与预装 Skills | 标题"已支持多种接入方式，生态产品已预装 Skills"，副标题"阿里云 Skills 门户/Qoder/Claude Code/OpenClaw·DataWorks Data Agent·EMR AI 助手"。三张产品截图：左为阿里云 Skills 门户"让 Agent 用好阿里云 从 Skills 开始"，含安装 alibabacloud-emr-starrocks-assistant 的 CLI 选择面板（支持 Claude CLI/Qoder CLI、JetBrains/Qoder/Claude Code/OpenClaw/Cursor/Gemini CLI/GitHub Copilot 等多种工具），底部命令 `agent install alibabacloud-emr-starrocks-assistant --agent qoder`；中间为 DATA AGENT 截图（DataWorks Data Agent 的 Skills 列表）；右为 EMR AI 助手对话界面截图（Hello, I'm EMR AI 助手） |
| P12 | 章节页 | 03 / EMR STAROCKS SKILLS 能力介绍 |
| P13 | StarRocks AI Native 能力全景 | 四张能力卡片：**AI Function × 11**（一条 SQL 调用大模型，商用 SQL 原生 AI，已上线）/**AI 助手**（问题主动发现，根因自动定位，运维效率从天级降至分钟级，已上线）/**Agent Skills**（开放标准化运维 Skill，客户 Agent 即插即用，已上线）/**多模态混合检索**（全文 + 向量 + 标量，一个引擎统一三种检索模态，公测中）。底部四项关键指标：11 内置 AI 函数 ｜ <500ms 单次推理延迟 ｜ 100% MySQL 协议兼容 ｜ 3 多模态混合检索 |
| P14 | StarRocks Skill 能力地图 | 副标题"阿里云 Agent Skills 官方输出，按用户意图拆分，满足多角色用户（DBA/SRE、数据工程师/分析师、应用开发者）所需能力"。六大分类左侧标注"渐进披露"路径——SR 领域知识 - Assistant/「我想了解 StarRocks」、SR 开发管理 - Assistant/「我要构建」、SR 性能分析 - Assistant/「我要优化系统」、SR 诊断 - Assistant/「我遇到问题了」、SR 运维 - Manage/「我要执行运维操作」、SR 数据分析 - Assistant/「我要理解数据」。底部五项核心优势：Agent 原生集成（一键添加 Skill，开箱即用，无需额外开发）、核心场景覆盖（覆盖开发、诊断、运维、数据分析、领域知识、性能分析六大管理场景）、安全合规（基于 RAM 权限体系，细粒度访问控制）、Serverless 免运维（无需管理基础设施，弹性能力）、生产级稳定性（企业级可靠性保障，完善的容错与重试机制） |
| P15 | 场景一·金融实时风控 | 标题"金融实时风控 — 毫秒级交易异常检测"。痛点：反欺诈需实时聚合用户近 N 笔交易特征（金额突增/频次异常）T+1 跑批来不及；风控规则涉及多表 JOIN（交易流水 × 用户画像 × 黑名单）查询超时影响拦截；业务方天天改规则需 DBA 手动调 SQL+建物化视图响应慢。方案"StarRocks Skill 智能 SQL 调优 + 物化视图设计"：自动分析执行计划（Shuffle JOIN 瓶颈 → Colocate/Broadcast）、物化视图一键设计（高频查询模式自动推荐 MV，滚动窗口预聚合）、窗口函数即写即优化（LAG/LEAD/SUM OVER 实时计算交易特征）。右侧 Qoder 对话截图：用户输入"/starrocks-assistant 这条风控 SQL 跑了 8 秒，帮我优化下。需要实时算每个用户最近 10 笔交易的金额标准差"；助手回应"分析执行计划... 发现瓶颈：transaction_detail（50 亿行）与 user_profile（2000 万）使用了 Shuffle JOIN；建议 1：改为 Colocate JOIN（同分布键 user_id）；建议 2：创建异步物化视图预聚合近 10 笔特征"，给出优化后 SQL（窗口函数 STDDEV ROWS 9 PRECEDING），预计耗时：8s → 200ms |
| P16 | 场景二·集群健康巡检 | 标题"集群健康巡检 — 拯救运维小哥的发际线"。痛点：StarRocks 集群几十个节点逐个检查 FE/BE/CN 状态、Tablet、Compaction；半夜告警响打开控制台翻日志翻到天亮；新同事不熟悉诊断命令出问题只会重启大法。方案"StarRocks Skill 一键全链路诊断"：自动执行 SHOW FRONTENDS/BACKENDS 全套诊断、30 秒输出结构化巡检报告（节点状态 + Tablet 健康 + Compaction 积压）、异常项高亮 + 风险等级 + 修复建议。右侧 Qoder 截图回复样例："集群巡检报告——架构：Shared-Data（存算分离）；FE：3 节点 全部 Alive；CN：5 节点 cn-004 LastHeartbeat>60s；Tablet 健康：总计 128,394 ｜ 健康 128,102 ｜ 异常 292（292 tablets 副本不足）；Compaction：cn-003 积压 1,247 pending（阈值 500）；建议：①检查 cn-004 网络连通性 ②cn-003 SET compaction_max_concurrent=8 ③失败 Load 增大 timeout 或拆分分片" |
| P17 | 场景三·金融报表加速 | 标题"金融报表加速 — 告别 T+1，今天的数据今天看"。痛点：监管报表/经营日报依赖 T+1 跑批，领导早会看到的是"昨天的昨天"；MySQL 分库分表查不动聚合，BI 工具超时转圈；接一个新报表需求就要写 ETL+建宽表，排期两周起步。方案"StarRocks Skill 智能 Schema 设计 + 数据入湖选型"：一句话建表（自动推荐表模型、分区分桶策略）、入湖方式自动选型（MySQL CDC → Flink CDC + Connector；文件 → Broker Load）、物化视图替代宽表（无需手动 ETL，MV 自动增量刷新，报表实时可查）。右侧助手回复：用户提"MySQL 交易流水表，5000 万行/天，做实时报表"；Schema 设计建议——表模型 Primary Key（支持 CDC 实时更新）、分区 PARTITION BY RANGE(txn_date) 按天、分桶 DISTRIBUTED BY HASH(user_id) BUCKETS 32、排序键(user_id, txn_date, txn_time)；入湖：MySQL → Flink CDC → StarRocks Connector，全量 + 增量自动切换，Exactly-Once，延迟 <5s；报表加速 CREATE MATERIALIZED VIEW mv_daily_summary REFRESH ASYNC EVERY (INTERVAL 1 MINUTE) |
| P18 | 场景四·MySQL → StarRocks 报表迁移 | 痛点：团队 20 张业务报表跑在 MySQL（RDS）上，覆盖订单/商品/客户/营销等核心域，复杂聚合越来越慢，业务方要求做分析加速同时保留 MySQL 作为业务写入源；传统做法逐表导出 MySQL DDL（10 min）→ 手动改写 StarRocks 语法+调优分区分桶（30 min × 20 张）→ 编写迁移脚本（20 min）→ 全量导入并校验一致性（15 min），总计 2-3 小时，且 DDL 改写过程中极易遗漏 StarRocks 的分区裁剪、排序键、Colocate Join 等优化点。方案"StarRocks Skill 智能生成 StarRocks DDL + 自动比对"：读取 MySQL Schema 后自动生成最优 StarRocks DDL；无需担心网络等配置问题，AI 帮你搞定工程问题；根据业务特征自动生成分区裁剪、排序键、Colocate Join 等优化点。右侧附 Step 1/Step 2 截图（JDBC Catalog 配置 + StarRocks DDL 自动生成；细节小字按规则不强行摄取） |
| P19 | StarRocks AI Function + Agent Skills = 对话式智能分析 | 副标题"数据不出库，AI 分析在 SQL 内完成；运维操作对话触发，零界面切换"。应用架构四列：**[1] 控制与编排层 DataWorks Data Agent**（Skills Library: StarRocks Assistant、StarRocks Manage；Action: ①理解用户意图 ②生成 SQL/调用 API ③返回分析结果）→ **[2] 计算引擎 StarRocks**（接收 Agent 提交的 SQL；①AI Function 内置推理：ai_sentiment()、ai_classify()、ai_filter()、ai_complete()；②向量检索 + 全文检索（开发中）；③混合排序返回结果（开发中））→ **[3] 数据 & 向量存储**（retail_demo 数据库：客户评价、订单流水（分区表）、客户画像、商品信息、SQL 内文本字段、促销活动）→ **[4] 模型服务层**（Qwen3.6-Plus / 百炼；输入：SQL 内文本字段；输出：情感/分类/摘要）。核心流程横向五步：意图识别（用户对话输入·自然语言需求）→ SQL 生成（Agent 调用 Skills·生成 AI Function SQL）→ 引擎执行（StarRocks 执行·AI+向量+全文检索）→ 模型推理（内置调用 LLM·完成智能分析）→ 结果返回（结构化结果·回传 Agent 展示） |
| P20 | 章节页 | 04 / EMR AI NATIVE 展望 |
| P21 | 能力展望（六项） | 左列：①EMR AI 助手 全面对接 IM 应用，支持自定义推送范围和巡检范围 ②EMR Serverless Spark 多模态处理 GA ③EMR Serverless Starrocks 向量化检索 GA。右列：④EMR Serverless Starrocks 全文检索 GA ⑤EMR Serverless Spark 多模态算子市场 GA ⑥EMR AI 助手 支持成本与资源优化 |
| P22 | 结束页 | THANKS |

## EMR AI Native 全景图

### 四层架构

```
应用场景层：智能数据分析 / 智能数据开发 / 智能运维管理 / 多模态数据处理
    ↓
AI 能力层：EMR AI 助手 / 智能开发助手 / AI Function / Agent Skills / 多模态处理引擎
    ↓
AI 基础设施层：百炼 Qwen 系列 / Token 管控 / 安全合规 / 推理优化
    ↓
计算引擎层：EMR on ECS/ACK / Serverless StarRocks / Serverless Spark
    ↓
数据底座层：OSS/NAS/CPFS / DLF Catalog / Paimon/Iceberg/Fluss
```

### AI 能力层详情

| 能力 | 功能 |
|------|------|
| **EMR AI 助手** | 实例管理、任务诊断优化、错误根因分析、健康巡检、产品咨询、成本优化 |
| **智能开发助手** | SQL 智能生成、代码生成、SQL 方言迁移、执行计划诊断、参数优化 |
| **AI Function 体系** | 图片打标、embedding、图像处理、数据脱敏、语义提取、音频处理、时序预测、内容安全 |
| **Agent Skills** | 集群/实例管理、作业管理、弹性管理、资源管理、运维管理、SQL 执行、元数据查询、智能诊断 |
| **多模态处理引擎** | 非结构化数据加工、批量模型推理、Embedding、全文检索 Beta、向量检索 Beta、混合检索 Beta |

### 应用场景层（4 大场景 × 16 个子能力）

- **智能数据分析**：自然语言查数据、SQL 自动优化、SQL Copilot 辅助、查询结果可视化
- **智能数据开发**：作业性能调优、SQL 方言转换、代码生成
- **智能运维管理**：集群健康巡检、异常诊断、根因分析、成本优化建议、预测式扩缩容
- **多模态数据处理**：文档/图像智能处理、企业知识库 RAG、多模检索、音视频内容理解

### AI 基础设施

- 模型服务（百炼 Qwen 系列、自定义模型接入）
- Token 管控（用量仪表盘、按量计费）
- 推理优化（批量推理、异步调用）
- 安全合规（模型内容安全、审计日志）

## EMR Skill 能力全景

三大子产品通过 MCP Skill 接入 AI 助手，覆盖集群管理、计算作业、分析数据库全链路运维。

### EMR on ECS 集群管理 Skill

`alibabacloud-emr-cluster-manage`

管理 5 种集群类型（HA/普通部署、存算分离/一体架构）的全生命周期：

| 功能 | 能力 |
|------|------|
| 生命周期管理 | 创建、状态查询、克隆、删除 |
| 节点组管理 | 5 种节点角色（MASTER/CORE/TASK 等）、新建节点组、手动扩缩容 |
| 弹性伸缩 | 定时策略与指标策略（CPU/内存/YARN 负载触发）、伸缩活动历史 |
| 运维诊断 | 集群与节点巡检、续费管理、创建失败诊断 |

### EMR Serverless Spark 工作空间管理 Skill

`alibabacloud-emr-spark-manage`

| 功能 | 能力 |
|------|------|
| 工作空间管理 | 创建、查询、删除工作空间，查询可用引擎版本 |
| 作业管理 | 提交 Spark 作业（JAR/PySpark/SQL）、查询状态与日志、取消作业 |
| Kyuubi 交互查询 | 创建/启停 SQL 网关、JDBC 连接、Token 管理、取消查询 |
| 资源队列管理 | CU 扩缩容、查询队列状态 |
| 会话集群 | 创建/启停/删除会话集群，支持交互式开发 |

### EMR Serverless StarRocks 管理 Skill

`alibabacloud-emr-starrocks-manage`

| 功能 | 能力 |
|------|------|
| 实例管理 | 创建（存算一体/分离）、查询状态、释放、恢复暂停 |
| 弹性扩缩容 | 节点/磁盘扩缩容、计算组节点增减，均支持预检 |
| 配置管理 | 配置修改（预检+回滚）、版本升级、SSL、付费转换 |
| 运维管理 | 重启实例/计算组/节点、操作历史查询 |
| 网络管理 | 网关增删改查、隔离、公网 SLB 开关 |

## Agent 驱动智驾图片打标

### 架构

```
控制编排层           计算引擎层           数据源层              模型服务层
DW DataAgent  →     Serverless Spark  →  DLF & OSS    →    Qwen3.6-plus
  Skills Library                          camera_front/
  · Spark Skills                          ├─ img_01.jpg
  · Kyuubi Skills                         ├─ img_02.jpg
                                          └─ ...
```

### 执行流程

1. **Action**：Agent 理解用户意图 → 生成 SQL 代码 → 提交任务
2. **Spark 执行**：解析 Prompt → `read_files` 读取 OSS 图片 → `ai_query()` 调用 Qwen3.6-plus（可指定模型如 `model='qwen3.6-plus'`）
3. **输出**：结构化标注结果 `["警车", "吊车"]`

使用 Serverless Spark 内置的 `read_file` 表函数直接读取 OSS 文件：

```sql
SELECT ai_query(prompt, read_file(path)) FROM read_file('oss://bucket/path/*.jpg');
```

整个流程简化为标准 ETL 流水线：读取 OSS 文件 → SQL 定义 Prompt → 调用 AI 函数 → 输出结构化结果。Spark 侧提供 `model` 参数切换模型（如 Qwen 3.5/3.6/embedding），支持 JPG/PNG 后缀过滤。

### 核心优势

- **Agent 原生集成**：一键添加 Skill，开箱即用
- **核心场景覆盖**：工作空间、作业、查询、资源四大管理场景
- **安全合规**：基于 RAM 权限体系，细粒度访问控制
- **Serverless 免运维**：无需管理基础设施，按需弹性扩缩容
- **生产级稳定性**：企业级可靠性保障，完善的容错与重试

## EMR Serverless StarRocks Skills 详细能力地图

StarRocks Skills 按用户意图拆分六大类，满足 DBA/SRE、数据工程师、分析师、开发者多角色需求：

### 领域知识（Assistant）

| Skill | 功能 |
|-------|------|
| `knowledge-qa-skill` | 文档查询、概念解释、功能了解 |
| `config-guide-skill` | 配置项含义和推荐值 |

### 开发管理（Assistant）

| Skill | 功能 |
|-------|------|
| `sql-assistant-skill` | 写 SQL、改 SQL、理解 SQL |
| `schema-design-skill` | 建表、分区分桶、索引设计 |
| `data-import-skill` | 数据导入方式选择和实施 |

### 性能分析（Assistant）

| Skill | 功能 |
|-------|------|
| `query-tune-skill` | 单条 SQL 执行瓶颈分析 |
| `workload-tune-skill` | 系统级资源和负载调优 |
| `workload-analysis-skill` | 系统负载统计（Top SQL、pattern、趋势） |

### 诊断（Assistant/Manage）

| Skill | 功能 |
|-------|------|
| `error-fix` | 无法归类领域的通用报错兜底 |
| `bug-search` | 搜社区已知 bug 和修复 |
| `health-diagnose` | 深度根因分析与修复建议 |
| `health-inspect` | 主动巡检、发现潜在风险 |

### 运维（Manage）

| Skill | 功能 |
|-------|------|
| `instance-manage` | 查看/管理实例信息 |
| `migration-execute` | 执行数据或集群迁移 |
| `cluster-upgrade` | 执行版本升级 |
| `backup-recovery` | 执行备份和恢复 |
| `access-control` | 用户权限、认证、访问策略 |
| `ha-setup` | 高可用配置和管理 |

### 数据分析（Assistant）

| Skill | 功能 |
|-------|------|
| `schema-explore` | 库表结构与字段探索（实例数据） |
| `data-profile` | 数据分布、质量、统计 |
| `anomaly-detect` | 数据层面异常发现 |

## StarRocks AI Native 四大核心能力

| 能力 | 状态 | 指标 |
|------|------|------|
| **AI Function × 11** | 已上线 | 一条 SQL 调用大模型，MySQL 协议 100% 兼容 |
| **AI 助手** | 已上线 | 问题主动发现，根因自动定位，运维效率从天级降至分钟级 |
| **Agent Skills** | 已上线 | 开放标准化运维 Skill，客户 Agent 即插即用 |
| **多模态混合检索** | 公测中 | 全文 + 向量 + 标量，一个引擎统一三种模态，<500ms |

## StarRocks 实战场景

### 场景一：金融实时风控 — 毫秒级交易异常检测

**痛点**：50 亿行交易表 × 2000 万用户画像 JOIN 超时、风控规则天天改需 DBA 手动调优。

**方案**：Skill 智能 SQL 调优 + 物化视图设计：
- 自动分析执行计划：发现 Shuffle JOIN 瓶颈 → 建议 Colocate/Broadcast
- 物化视图一键设计：滚动窗口预聚合近 10 笔交易特征
- 窗口函数即写即优化：LAG/LEAD/SUM OVER 实时计算交易特征

**效果**：8s → 200ms。

### 场景二：集群健康巡检 — 拯救运维小哥的发际线

**痛点**：几十个节点逐个检查、半夜告警翻日志翻到天亮、新同事只会重启大法。

**方案**：`/starrocks-manage` 一键全链路诊断，30 秒输出结构化巡检报告：节点状态 + Tablet 健康 + Compaction 积压，异常项高亮 + 风险等级 + 修复建议。

### 场景三：金融报表加速 — 告别 T+1，今天的数据今天看

**痛点**：MySQL 分库分表查不动聚合、每接一个新报表需求排期两周、领导早会看到的是"昨天的昨天"。

**方案**：Skill 智能 Schema 设计 + 入湖选型：
- 一句话建表：自动推荐 Primary Key 模型、按天分区、HASH(user_id) 分桶
- 入湖方式自动选型：MySQL → Flink CDC + Connector（< 5s 延迟）
- 物化视图替代宽表：MV 自动增量刷新，报表实时可查

### 场景四：MySQL → StarRocks 报表迁移

**痛点**：20 张报表逐表导出 DDL (10min) → 手动改写 StarRocks 语法 (30min × 20) → 编写迁移脚本 (20min) → 全量导入校验 (15min)，总计 2-3 小时。

**方案**：Skill 智能生成 StarRocks DDL + 自动比对：
- 读取 MySQL Schema 后自动生成最优 StarRocks DDL
- 根据业务特征自动生成分区裁剪、排序键、Colocate Join 等优化点
- AI 搞定网络配置等工程问题

## 从 Lakehouse 到 Agentic Lakehouse

Agent 时代对大数据平台提出三个新要求：
1. **更低门槛的调用方式**：从作业/SQL 配置 → API/能力调用
2. **更统一的能力抽象**：数据处理、查询分析、系统运维 → 统一对外抽象成能力调用
3. **更实时的响应能力**：支持 Agent 的即时决策与交互

EMR 作为 **Agent 的数据与计算能力平台**，将三类核心能力 Skill 化：**数据处理 Skill**（Spark）、**实时分析 Skill**（StarRocks）、**系统运维 Skill**（集群管理）。

已支持的 Agent 接入方式：DataWorks Agent / EMR Agent / Qoder / Claude Code / OpenClaw。

## EMR AI Native 展望

P21 列出 6 项能力路线图（左右两列各 3 项）：

- EMR AI 助手 全面对接 IM 应用，支持自定义推送范围和巡检范围
- EMR Serverless Spark 多模态处理 GA
- EMR Serverless StarRocks 向量化检索 GA
- EMR Serverless StarRocks 全文检索 GA
- EMR Serverless Spark 多模态算子市场 GA
- EMR AI 助手 支持成本与资源优化

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 产品
- [[dataworks-2026-0528-xialiaori]] — 2026-05-28 虾聊日全部分享内容汇总
- [[maxcompute-skills]] — MaxCompute 同类 Skills 体系
- [[hologres-skills]] — Hologres Skills
- [[flink-skills]] — Flink Skills（Flink + EMR 联动）
- [[dataworks-stock-case]] — Agent 驱动数据处理的实践案例
- [[agent-mcp-protocol]] — MCP 协议原理
