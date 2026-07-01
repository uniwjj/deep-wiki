---
title: MaxCompute Skills
description: MaxCompute 通过 MCMCP 服务 + Information Schema 语义包 + MaxFrame Coding Skill 构建的 Agent 可调用能力层，配合 Delta Table/AutoMV/Blob 多模态存储与 MaxAgent 运营助手，覆盖数仓运维、多模态开发、数据分析全场景
aliases: [MaxCompute Skills, MCMCP, MC Skills, MaxAgent]
tags: [big-data, ai-agent, platform, tool]
sources: [2026/05/12/03. 基于 MaxCompute Skills 简化数仓运维加速多模态开发.pdf, 2026/05/12/DataWorks DataAgent分享录音.md, 2026/05/28/MaxCompute × Agent：从多模态存储到混合计算，突破数字员工的算力边界 20260528.pdf]
created: 2026-05-12
updated: 2026-05-31
---

# MaxCompute Skills

MaxCompute 通过 **MCMCP** 与 **Skills** 结合，将底层元数据、计算和存储服务封装成 Agent 可理解、可执行的统一能力接入层，让企业客户通过 Agent 完成超大规模数据分析、高性价比多模数据加工和智能化运维。

录音补充原话（2026-05-28 杭州虾聊日）：

- **元梗式开场**："现在的演讲目标不只是让大家听懂，更是：我讲的内容，让你的 Agent 转化成文字来听，作为你 Agent 的上下文来使用 MaxCompute"
- **为什么提供官方 MaxCompute 客户端**："就像看银行账户，Agent 可以读，但一定要有自己的网银去看真实数据"——客户端类似 Navicat，与 Agent 取数交叉校验
- **Time Travel 防误删**：最近 7 天任意时刻可回滚，上月版本可打快照（增量存储不膨胀）
- **AutoMV 多 Agent 同查节流**：第二、第三个 Agent 来查直接命中缓存，用户无感知
- **MaxAgent 移动端典型场景**：数据膨胀时手机端临时弹性资源、失败作业手机端触发重跑、多仓 Hive→MaxCompute 数据迁移自然语言交互

- 议题副标题：从多模态存储到混合计算，突破数字员工的算力边界
- 演讲人：**孙戴博（六稻）**｜MaxCompute PD（阿里云智能集团）
- 议题脉络：①MaxCompute 为数字员工提供丰富的接入方式 → ②通过语义层实现认知对齐 → ③多模态存储与混合计算方案

## 摄取说明

2026-05-31 按 CLAUDE.md 多模态资料首次摄取规则，对 `2026/05/28/MaxCompute × Agent：从多模态存储到混合计算，突破数字员工的算力边界 20260528.pdf`（共 14 页）使用 `pdftoppm -r 150 -png` 转出页面图片逐页视觉复核，不再以派生 TXT/OCR 为唯一可信来源。本次复核修正：议题三章脉络、MCMCP 方法分组与 4 个配套 Skills（Sql-Generation/Maxframe-Coding/Information-Schema/Project-Qouta-Manage）、第 3 个 Agent 接入口实为"龙虾实操 MaxCompute Specialist Sub-Agent · OpenClaw"实战示例（OpenClaw 总控 + Specialist Sub-Agent 执行）、Delta Table 细项、Blob Pipeline 架构对比、Information Schema 语义包结构与场景示例、Blob 多模态算子 SQL 示例。

## 逐页摄取索引

| 页码 | 主题 | 已摄取信息 |
|------|------|------------|
| P1 | 封面 | MaxCompute × Agent：从多模态存储到混合计算，突破数字员工的算力边界；孙戴博（六稻）｜MaxCompute PD；阿里云智能集团 logo |
| P2 | 目录 | 三部分：①MaxCompute 为数字员工提供丰富的接入方式 ②通过语义层实现认知对齐 ③多模态存储与混合计算方案 |
| P3 | Agentic 时代·MaxCompute 接入大图 | MCMCP + Skills 把数据/计算/存储服务封装成 Agent 可理解、可执行的统一能力接入层。架构自上而下：用户 Agent 生态（OpenClaw / DataWorks Agent / Qwen Code / QoderWork / MaxAgent）→ MC Skills 集合（Agent 通用：语义包、常用命令、开发模板、使用限制）→ MCMCP 服务（把 MaxCompute OpenAPI/StorageAPI/CatalogAPI 封装成 Agent 直接连接的结构化工具）→ MaxCompute（Metadata: Catalog/Schema/Table/Partition + 授权/审计/数据发现/敏感等级/脱敏/行级权限/数据共享；Compute Engines: MC SQL / MaxFrame / MC Spark + 异构算子 CU/GU、AI Function、模型；Storage: Table（Append / PK Delta Table）+ 数据类型（Blob/JSON/ARRAY/MAP/STRUCT）+ 冷热分层 / 多 AZ 容灾 / 复制 / Time Travel / 数据变更 / 数据分级） |
| P4 | MCMCP + Skills | MCMCP 是 MaxCompute 提供的 MCP 服务，用户可使用丰富的 Agent 生态配置接入，接入后用自然语言对话方式管理和分析 PB/EB 级数据。**方法分组**：①身份与权限确认 → `check_access`（支持 AK/SK、STS、Credentials URI 等多认证）②元数据 → `list projects/schemas/tables/partitions`（自然语言找表）③Knowledge Catalog → `search_meta_data`（语义搜索）④分析计算 → `cost_sql`、`execute_sql`、`get_instance_status`、`get_instance`（执行前可走成本评估审批，约束仅允许只读查询）⑤数据管理 → `create_table`、`insert_values`（建表时提供生命周期、主键、部分列更新等属性选项）。**生产就绪 / 安全·Agent 友好**：①丰富的认证方式 ②元数据搜索服务 ③执行 SQL 时可约束仅允许只读查询 ④执行 SQL 前可走成本评估审批 ⑤建表时提供表属性选项。**配套 Skills**：Sql-Generation、Maxframe-Coding、Information-Schema、Project-Qouta-Manage |
| P5 | 开放能力与 Agent 接入口（3 张产品图） | 同一套 MaxCompute 平台能力，不止一种 Agent 工作入口，用户可按任务类型与使用习惯选择接入工作流。**①MaxCompute CLI**——Agent 接入身份入口，含 `Run config (Auth: AKSK/STS)` 与 `Profile workspace default`；用法：`bootstrap`（自助引导）、`whoami --json`（结构化输出 `{"identity": "user_name", "auth": "xx"}`）、`AKSK / STS`（多认证方式）；要点：bootstrap 自动启动开关、配置环境依赖；`whoami --json` 结构化输出更适合 Agent 调用；常用动作：CLI 编排元数据操作、获取数据描述。**②MaxCompute AI 数据探索客户端**——桌面级客户端（左侧元数据树、右侧聊天/SQL/数据预览）；要点：快速进行项目数据查询、SQL 脚本、可视化分析；本地持久化工作现场，无须额外网络成本；多终端复盘；AI 驱动数据探索，AI 推断数据 DDL 作为 Specialist Sub-Agent 输入参数。**③龙虾实操 MaxCompute Specialist Sub-Agent · OpenClaw**——OpenClaw "总控"、Sub-Agent "专业分析执行"；Sub-Agent 中间证据链路：Plan → Act → Result；产物：方案、行动、原因、依据、审计 |
| P6 | 操作数据有保障，底层 Table Format 做支撑 | 副标题"Delta Table & AutoMV：面向 Agent 时代的近实时事务引擎和自动物化视图，让重复查询自动加速、数据回滚随时可用"。**Delta Table——无惧 Agent 破坏数据，TimeTravel 随时回滚**：ACID 事务表引擎、近实时写入与读取、存储成本持平或更低。基础 DML：INSERT/UPDATE/DELETE、MERGE INTO、Streaming Insert（流式写入）、Auto Partition、ACID 隔离；自动维护：TimeTravel/Snapshot、Incremental Read、Delta Live MV、Clustering（聚簇优化）、Schema Evolution、Realtime Compaction、Incremental ReClustering；性能提升、存储本地下降。**AutoMV——无惧 Agent 重复查询，自动物化、空间换时间降算力**：①作业特征分析（基于历史执行计划自动提取查询模式）②公共计算抽取（跨 SQL 语义识别公共子查询，识别高频/重复/共享逻辑）③全局最优推荐（综合收益/成本评估、自适应存储阈值，输出 AutoMV ↔ MV 智能推荐）④自动查询改写（自动发现 → 零人工干预 → 确定性收益） |
| P7 | MaxAgent 运营助手 | 副标题"覆盖资源管理、成本治理、业务分析全链路，免控制台登录、移动端操控闭环，打通钉钉、飞书、微信机器人通道，实现'聊天即服务'的交互"。**①数据开发日常运维**——痛点：作业失败/慢排查耗时长、查询性能瓶颈、全表扫描资源浪费；能力：作业运维与智能诊断、物化视图推荐与收益评估、Auto Cluster 聚簇优化。**②资源管理员移动运维**——痛点：项目配置繁琐、Quota 利用不平衡、外出告警无法及时处理；能力：项目配置与参数管理、Quota/路由/计划/分时配置、机器人移动端闭环处置。**③降本增效**——痛点：费用增长不知原因、存储大户不清、优化思路不可量化；能力：成本智能分析与趋势、计量成本归因与 Top 消费者、存储观测与分层推荐。**④智能问数**——痛点：不懂 SQL、依赖数据团队、无法定位目标表和业务逻辑、日常报表全表扫描；能力：NL2SQL 自然语言查数据、上下文追问深度分析、跨域联动（查询+诊断） |
| P8 | 通过语义层实现认知对齐：语义结构举例 | Information_Schema 是系统级运维（Ops）语义包，基于 MaxCompute 租户级的 INFORMATION_SCHEMA 元数据视图构建；为数据团队提供全方位的项目审计、用量分析与运维观测能力。**规模**：54 术语、34 实体（Table/Quota/User/Tunnel）、16 关联、35 指标、17 场景（playbook 直连）、38 SQL（已验证查询模板）。指标举例：`CU时 = SUM(cost_cpu / 100 / 3600)`；`任务 P99 延时 = PERCENTILE(DATEDIFF(end_time, start_time, 'ms'), 0.99)`。场景 Playbook 举例：存储趋势分析（盘点 TOP 存储成本占用情况）、检查分区裁剪、判定生命周期/分区策略风险、生命周期偏离分析。实体关系举例：UDFS、UDF_PRIVILEGES `Join udfs.udf_name = udf_privileges.udf_name`。"语义包不是 prompt，而是 Knowledge Catalog"，目录结构：information_schema/{terms.json, entities.json, metrics.json, joins.json, verified-queries.json, playbooks.json}。Skill 调用样例：用户提"为什么这个月成本上来了？"→ 调用类目 cost_pressure_diagnosis → 多轮"下一步分析" |
| P9 | Information Schema 语义包应用场景举例 | 结合 Information_Schema 语义包，不用铺垫即可让 Agent 直接回答真实运维和治理问题。六类场景与典型提问：**存储压力**——"哪些表最近占用的存储最高？""哪些项目下数据量增长快？"（识别 TOP 表/分区膨胀，定位生命周期异常与冷热失衡）；**成本压力**——"为什么这个月成本比上月涨得快？""按 owner / project / task_type 拆解 CU 时？"；**权限审计**——"哪些表的权限暴露面更大？"（审计 LABEL 保护表与字段级授权）；**传输审计**——"最近有哪些公网下载行为？"（基于 TUNNELS_HISTORY 与 client_ip 回溯）；**失败激增**——"最近 7 天失败任务为什么突然变多？"（结合 TASKS_HISTORY 对比）；**Quota 监控**——"哪些 Quota 已经接近资源瓶颈？"（quota_bottleneck 判断）。右侧黑色控制台截图：调用 `Maxcompute-catalog (get_instance)`，返回 ALTUNNEL_ANALYSIS 结果（task_344 失败率 70.6%、5614 CU Hours、9.371 MEM Hours），下方建议：①最近 7 天累计失败 344 次（5614.0 CU 时），为最大消耗源 ②失败率 70.6%（5614.0/7949.0 CU），高频但低成功 ③MEM 使用 9.371 MEM Hours ④全文审计将提供高频失败任务排查 |
| P10 | 多模态存储与混合计算方案 | MaxCompute 提供大规模多模态存储和计算能力，支持单表多模态数据一行多列混存，将音视频图文数据与元信息、标签统一存储，支持 AI 推理和 AIGC 应用等场景。**解决问题**：多模态数据的原始文件、元信息、标注信息经常分散在对象存储、混合主导多引擎中，多引擎架构维护成本高；数据获取与开发成本高，跨模态查询延迟高、缺乏 ACID 事务保障、数据一致性差。**Blob 数据类型**——应用场景：百万图片小文件大数据集、大规模 HTML 对话集、JSON 包含的文本数据集管理；5GB/cell（一个单元格可存放 5GB）、4X（IO 吞吐对 Object Table 提升 4 倍）、ACID（多模态数据集事务一致性）、2X（数据集标签点查场景性能优于 String/Binary 2X）；全托管优化方案：Compaction、Tiering、Index Build、Merge、Backup；支持 Append / PK Delta Table。**多模态计算**：MaxFrame + SQL UDF；多模态算子：IMG（图像 OCR）、DOC（文档解析）、AUDIO（音频 ASR）、VIDEO（视频切帧）。**多条数据接口**：开放原生 SQL、MaxFrame 多种路径读写 Blob；SDK 批量上传/下载；通过 Object Table 从 OSS 对象存储批量导入 |
| P11 | Blob 数据类型与多模态存储 Pipeline | Blob（Binary Large Object）数据类型是 MaxCompute 提供的一种用于在表中存储图片、音频、视频、文档等非结构化二进制文件的数据类型；通过 Blob 类型，可将多模态数据的原始文件、元信息和标注信息统一存储在同一张 MaxCompute 表中，使用 SQL 统一查询和管理，通过 MaxFrame 和 SQL UDF 批量加工处理。文档：help.aliyun.com/zh/maxcompute/user-guide/blob-data-type-and-multimodal-storage。架构对比：**传统模式**——多引擎多存储分散（左列 Tongyi/COS/WCM/影像中心/多个外部源）；**MaxCompute Blob 模式（多模态数据一体化存储）**——单表内 Image(BLOB)、label、meta(JSON) 列；ACID 事务（写入 + 读取一致性）+ Spark/Pipeline + 部分列更新 + 时间旅行；SQL UDF（如 `SELECT vad_thrx(image)... WHERE vad > x`）+ MaxFrame 多模态加工；Spark Pipeline（SQL 流批样式 + AI Pipeline + Object Table 直接挂载 OSS 文件 + 数据治理）。底部右侧四类业务场景：①AI 训练数据集管理 ②多模态数据处理流水线 ③多模态加工缓存层 ④非结构化数据入湖 |
| P12 | 多模态开发调试与加工体验 | Blob 具备文件系统式的可操作性，支持文件拖拽式批量导入、文件预览式查看和下载、挂载到用户操作系统等。Blob 多模态加工算子库已经覆盖图像、视频、音频、PDF、Excel、Word、PPT、Markdown、HTML、中文文本等多个方向。三张截图：挂载到文件系统式文件夹（左）、多模态加工 DAG 编排（中）、图像/视频/音频/文档 Blob 子库（右）。四个动作 Tab：**拖拽导入**（上传文件夹自动识别路径、名称、类型元信息）、**编排加工**（根据 DAG 数据流和数据元参数动态生成 SQL+UDF 执行语句）、**预览下载**（图片/视频/音频/PDF 都可在线预览，看到内容而非二进制编码）、**挂载共享**（WebDAV 挂载 Blob 表为操作系统文件夹，SQL CRUD 映射为 List/Get/Put）。底部多模态算子 SQL 示例：`音频Blob` →（语音转写文字）→ `英文记录String` →（AI_Function翻译）→ `中文记录String`；`原始图片Blob` →（图片去噪）→ `黑白图片Blob`；`图片水印加图Blob` → `带水印图片Blob` |
| P13 | MaxFrame Coding Skill 加速多模态算子开发与交付 | MaxFrame Coding Skill 可以集成到主流 AI 编程助手中，将 MaxFrame 分布式数据处理知识体系注入 Vibe Coding，包含从会话管理、数据读写、算子选择和结果输出的开发全流程。**已配能力**：在 Agent 生成更确定的代码，可分阶段标准化开发模式：Operator Selector（自动推荐更合适算子和处理方式）、API 校验（实时验证 API 是否真实存在）、3 阶段（确认需求 → 确认方案 → 生成代码）、10 模板（生产级示例覆盖常见开发场景）。生成代码模板基础生产约束：`session = new_session(...); df = df.mf.apply_chunk(...); result.execute(); session.destroy()`。**多模态算子举例**：逐行文件处理 `df.apply(axis=1)`、模型批推理 `df.mf.apply_chunk()`、视频切帧 `df.mf.flatmap()`、音频合并 `df.mf.map_reduce()`、并行调优 `df.mf.rebalance()`。**多模态开发模式**：访问 OSS 常配合 `@with_fs_mount`，模型加载使用 `_ctx` 缓存，减少重复加载，更适合分布式 Worker 批处理。**基座 + 专业 Skill**：已有 maxframe-job-coding 作为基座；多模态处理、AI Function、GPU/资源调度等专业方向 |
| P14 | 结束页 | THANKS |

## 接入架构

```
用户 Agent 生态（OpenClaw / DataWorks Agent / Qwen Code / QoderWork / MaxAgent）
              ↓
        MC Skills 集合（Agent 通用：语义包 + 常用命令 + 开发模板 + 使用限制）
              ↓
        MCMCP 服务（OpenAPI / StorageAPI / CatalogAPI → 结构化工具）
              ↓
        MaxCompute（Metadata + Compute Engines + Storage）
              ↓
    授权 / 审计 / 数据发现 / 敏感等级 / 脱敏 / 行级权限 / 数据共享
```

## MCMCP 服务

MCMCP 是 MaxCompute 提供的 [[agent-mcp-protocol|Model Context Protocol（MCP）]]服务，接入后用户可通过自然语言对话管理和分析 PB/EB 级数据。

### 方法分组

| 分组 | 方法 | 说明 |
|------|------|------|
| 身份与权限确认 | `check_access` | AK/SK、STS、Credentials URI 等多认证方式 |
| 元数据 | `list projects/schemas/tables/partitions` | 元数据搜索服务，自然语言找表 |
| Knowledge Catalog | `search_meta_data` | 语义搜索，关联业务术语 |
| 分析计算 | `cost_sql` / `execute_sql` / `get_instance_status` / `get_instance` | 执行前成本评估审批，约束仅允许只读查询 |
| 数据管理 | `create_table` / `insert_values` | 建表时提供生命周期、主键、部分列更新等选项 |

### 配套 Skills（Agent 接入即开即用）

- **Sql-Generation**
- **Maxframe-Coding**
- **Information-Schema**
- **Project-Qouta-Manage**（PDF 原文如此，对应 Project & Quota 管理）

## Information Schema 语义包 Skill

系统级运维（Ops）语义包，基于 MaxCompute 租户级的 `INFORMATION_SCHEMA` 元数据视图构建，将底层元数据转化为可自然语言查询的指标和对象，为数据团队提供全方位的项目审计、用量分析与运维观测能力。

### 规模

- **54 术语**：口语到治理概念映射
- **34 实体**：Table/Quota/User/Role/Catalog/Schema/Partition/Column/Tunnel
- **16 关联**：对象关系可串起来分析（如 `udfs.udf_name = udf_privileges.udf_name`）
- **35 指标**：存储/成本/资源/安全
  - 例：`CU时 = SUM(cost_cpu / 100 / 3600)`
  - 例：`任务 P99 延时 = PERCENTILE(DATEDIFF(end_time, start_time, 'ms'), 0.99)`
- **17 场景**：playbook 直连常见治理问题
- **38 SQL**：已验证查询模板可直接复用

语义包不是 prompt，而是结构化 Knowledge Catalog：

```
information_schema/
├── terms.json
├── entities.json
├── metrics.json
├── joins.json
├── verified-queries.json
└── playbooks.json
```

### 典型运维场景

| 场景 | 典型提问 | 能力 |
|------|---------|------|
| 存储压力 | "哪些表最近占用的存储最高？" / "哪些项目下数据量增长快？" | 识别 TOP 表和分区膨胀，定位生命周期异常与冷热失衡 |
| 成本压力 | "为什么这个月成本比上月涨得快？" / "按 owner/project/task_type 拆解 CU 时？" | 按 owner/project/task_type 拆解 CU 时 |
| 权限审计 | "哪些表的权限暴露面更大？" | 审计 LABEL 保护表与字段级授权 |
| 传输审计 | "最近有哪些公网下载行为？" | 基于 TUNNELS_HISTORY 与 client_ip 回溯 |
| 失败激增 | "最近 7 天失败任务为什么突然变多？" | 结合 TASKS_HISTORY 对比定位异常 |
| Quota 监控 | "哪些 Quota 已经接近资源瓶颈？" | 给出 quota_bottleneck 判断 |

P9 演示 Skill 调用：用户提"为什么这个月成本上来了？"→ Agent 调用 `cost_pressure_diagnosis` 类目 → 多轮"下一步分析"，控制台调用 `Maxcompute-catalog (get_instance)`，返回失败任务 Top（如 task_344 失败率 70.6%、5614 CU Hours、9.371 MEM Hours），并给出审计建议。

### 更多运维场景

- **作业诊断**：看 LogView → 调优建议，失败原因分析和改写建议
- **Quota 资源管理**：资源状态、资源规划、成本对比分析
- **MMS 数据迁移**：按需执行迁移任务，定时迁移

## 多模态存储与计算

MaxCompute 提供大规模多模态存储和计算能力，支持单表多模态数据一行多列混存，将音视频图文数据与元信息、标签统一存储，解决多引擎架构维护成本高、跨模态查询延迟高、缺乏 ACID 事务保障等问题。

### Blob 数据类型

| 特性 | 能力 |
|------|------|
| 单元格容量 | 5GB/cell |
| IO 吞吐 | 相对 Object Table 提升 4X |
| 事务 | ACID 多模态数据集事务一致性 |
| 点查过滤 | 数据集标签点查场景性能优于 String/Binary 2X |
| 托管优化 | Compaction、Tiering、Index Build、Merge、Backup |
| 表类型 | Append / PK Delta Table |
| 多写入接口 | SQL、MaxFrame 双路径读写，SDK 批量上传/下载，Object Table 从 OSS 批量导入 |

文档：`help.aliyun.com/zh/maxcompute/user-guide/blob-data-type-and-multimodal-storage`

### Pipeline 架构对比

**传统模式**——多引擎多存储分散：非结构化文件分布在对象存储（Tongyi/COS/WCM 等），元信息与标签零散维护，跨模态查询要跨多个引擎和接口。

**MaxCompute Blob 模式（多模态数据一体化存储）**——单表多模态一行多列混存：`Image(BLOB) | label | meta(JSON)` 等列在同一张表内，依托 ACID 事务（写入 + 读取一致性）、Spark/Pipeline、部分列更新、时间旅行；`SQL UDF` 与 `MaxFrame` 双路径加工；Spark Pipeline 把 SQL 流批模式 + AI Pipeline + Object Table（直接挂载 OSS 文件）+ 数据治理串联起来。

### 四大应用场景

| 场景 | 说明 |
|------|------|
| AI 训练数据集管理 | 图片/音频/视频与标注标签同表存储，SQL 过滤样本集 |
| 多模态数据处理流水线 | 视频切帧、音频转文本、内容打标等多步骤中间结果统一存储 |
| 多模态加工缓存层 | 作为 OSS 缓存层，提升 MaxFrame/SQL 并行计算吞吐，解决小文件 QPS 限制 |
| 非结构化数据入湖 | 分散外部文件统一导入，扩展元信息建立企业非结构化资产库，获得事务和权限管控 |

### 多模态开发体验

- **拖拽导入**：上传文件夹自动识别路径、名称、类型元信息
- **编排加工**：根据 DAG 和算子参数动态生成 SQL+UDF 执行语句
- **预览下载**：图片/视频/音频/PDF 在线预览，看到内容而非二进制编码
- **挂载共享**（WebDAV）：Blob 表挂载成操作系统文件夹，SQL CRUD 映射为 List/Get/Put

加工算子库覆盖：图像、视频、音频、PDF、Excel、Word、PPT、Markdown、HTML、中文文本。

P12 底部多模态算子 SQL 编排示例：

```
音频Blob → (语音转写文字) → 英文记录String → (AI_Function翻译) → 中文记录String
原始图片Blob → (图片去噪) → 黑白图片Blob
图片水印加图Blob → 带水印图片Blob
```

### 计算（多模态算子）

| 类型 | 算子 |
|------|------|
| 图像/OCR | `IMG` |
| 文档/解析 | `DOC` |
| 音频/ASR | `AUDIO` |
| 视频/切帧 | `VIDEO` |

## MaxAgent 运营助手

MaxAgent 覆盖资源管理、成本治理、业务分析全链路，免控制台登录、移动端操控闭环，打通钉钉、飞书、微信机器人通道，实现"聊天即服务"。

### 四大场景

| 场景 | 典型痛点 | MaxAgent 能力 |
|------|---------|-------------|
| **数据开发日常运维** | 作业失败/慢排查耗时长、查询性能瓶颈、全表扫描资源浪费 | 作业运维与智能诊断、物化视图推荐与收益评估、Auto Cluster 聚簇优化 |
| **资源管理员移动运维** | 项目配置繁琐、Quota 利用不平衡、外出告警无法及时处理 | 项目配置与参数管理、Quota/路由/计划/分时配置、机器人移动端闭环处置 |
| **降本增效** | 费用增长不知原因、存储大户不清、优化思路不通透不可量化 | 成本智能分析与趋势、计量成本归因与 Top 消费者、存储观测与分层推荐 |
| **智能问数** | 不懂 SQL、依赖数据团队、无法定位目标表和业务逻辑、日常报表全表扫描 | NL2SQL 自然语言查数据、上下文追问深度分析、跨域联动（查询+诊断） |

## Delta Table 与 AutoMV：Agent 时代的操作保障

副标题"Delta Table & AutoMV：面向 Agent 时代的近实时事务引擎和自动物化视图，让重复查询自动加速、数据回滚随时可用"。

### Delta Table

无惧 Agent 破坏数据，TimeTravel 随时回滚；ACID 事务表引擎，近实时写入与读取，存储成本持平或更低。

- **基础 DML**：INSERT/UPDATE/DELETE、MERGE INTO、Streaming Insert（流式写入）、Auto Partition、ACID 隔离
- **自动维护**：TimeTravel/Snapshot、Incremental Read、Delta Live MV、Clustering（聚簇优化）、Schema Evolution、Realtime Compaction、Incremental ReClustering
- 性能提升 + 存储成本持平或更低

### AutoMV（自动物化视图）

无惧 Agent 重复查询，自动物化、空间换时间降算力：

- **作业特征分析**：从历史执行计划自动提取查询模式
- **公共计算抽取**：跨 SQL 语义识别公共子查询，识别高频/重复/共享逻辑
- **全局最优推荐**：综合收益/成本评估、自适应存储阈值，输出 AutoMV ↔ MV 智能推荐
- **自动查询改写**：自动发现 → 零人工干预 → 确定性收益（透明加速，用户无感知）

## MaxFrame Coding Skill

集成到主流 AI 编程助手中，将 MaxFrame 分布式数据处理知识体系注入 Vibe Coding，覆盖会话管理、数据读写、算子选择和结果输出的开发全流程。

### 核心能力

- **Operator Selector**：自动推荐更合适的算子与处理方式
- **API 校验**：实时验证 API 是否真实存在
- **3 阶段开发**：确认需求 → 确认方案 → 生成代码
- **10 模板**：生产级示例覆盖常见开发场景

### 多模态算子举例

| 场景 | 推荐算子 |
|------|---------|
| 逐行文件处理 | `df.apply(axis=1)` |
| 模型批推理 | `df.mf.apply_chunk()` |
| 视频切帧 | `df.mf.flatmap()` |
| 音频合并 | `df.mf.map_reduce()` |
| 并行调优 | `df.mf.rebalance()` |

### 开发模式

访问 OSS 常配合 `@with_fs_mount`，模型加载使用 `_ctx` 缓存减少重复加载，更适合分布式 Worker 批处理。生成代码的最低生产约束包含 session 创建和销毁。

```
session = new_session(...)
df = df.mf.apply_chunk(...)
result.execute()
session.destroy()
```

### 演进方向

- 基座：`maxframe-job-coding`
- 专业方向：多模态处理、AI Function、GPU/资源调度

## 多 Agent 接入口

| 入口 | 能力 | 特色 |
|------|------|------|
| **MaxCompute CLI** | `bootstrap` 自助引导、`whoami --json` 结构化身份输出、AKSK/STS 多认证 | JSON 输出 Agent 友好；CLI 编排元数据操作、获取数据描述 |
| **MaxCompute AI 数据探索客户端** | 桌面级客户端，本地持久化工作现场、AI 推断 DDL | AI 驱动数据探索；多终端复盘；为 Specialist Sub-Agent 提供输入参数 |
| **龙虾实操 Specialist Sub-Agent · OpenClaw** | OpenClaw 总控 + Specialist Sub-Agent 专业分析执行 | 证据链：Plan → Act → Result；产物含方案、行动、原因、依据、审计 |

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 整体产品
- [[dataworks-2026-0528-xialiaori]] — 2026-05-28 虾聊日全部分享内容汇总
- [[maxcompute-data-ai]] — MaxCompute Data+AI 四层架构
- [[hologres-skills]] — Hologres 同类 Skills 体系
- [[flink-skills]] — Flink Agent Skills
- [[emr-skills]] — EMR Skills
- [[agent-mcp-protocol]] — MCP 协议原理
