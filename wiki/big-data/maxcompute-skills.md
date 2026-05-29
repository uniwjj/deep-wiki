---
title: MaxCompute Skills
description: MaxCompute 通过 MCMCP 服务 + Information Schema 语义包 + MaxFrame Coding Skill 构建的 Agent 可调用能力层，配合 Delta Table/AutoMV/Blob 多模态存储与 MaxAgent 运营助手，覆盖数仓运维、多模态开发、数据分析全场景
aliases: [MaxCompute Skills, MCMCP, MC Skills, MaxAgent]
tags: [big-data, ai-agent, platform, tool]
sources: [2026/05/12/03. 基于 MaxCompute Skills 简化数仓运维加速多模态开发.pdf, 2026/05/12/DataWorks DataAgent分享录音.md, 2026/05/28/MaxCompute × Agent：从多模态存储到混合计算，突破数字员工的算力边界 20260528.pdf]
created: 2026-05-12
updated: 2026-05-29
---

# MaxCompute Skills

MaxCompute 通过 **MCMCP** 与 **Skills** 结合，将底层元数据、计算和存储服务封装成 Agent 可理解、可执行的统一能力接入层，让企业客户通过 Agent 完成超大规模数据分析、高性价比多模数据加工和智能化运维。演讲人：孙戴博/六稻（MaxCompute PD）。

## 接入架构

```
用户 Agent 生态（OpenClaw / DataWorks Agent / Qwen Code / QoderWork）
              ↓
        MC Skills 集合（语义包 + 常用命令 + 开发模板 + 使用限制）
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
| 身份与权限 | `check_access` | AK/SK、STS、Credentials URI 等多认证方式 |
| 元数据 | `list projects/schemas/tables/partitions` | 元数据搜索服务，自然语言找表 |
| Knowledge Catalog | `search_meta_data` | 语义搜索，关联业务术语 |
| 分析计算 | `cost_sql` / `execute_sql` / `get_instance_status` | 执行前成本评估审批，约束仅允许只读查询 |
| 数据管理 | `create_table` / `insert_values` | 建表时提供生命周期、主键、部分列更新等选项 |

## Information Schema 语义包 Skill

系统级运维（Ops）语义包，基于 `INFORMATION_SCHEMA` 元数据视图构建，将底层元数据转化为可自然语言查询的指标和对象。

### 规模

- **54 术语**：口语到治理概念映射
- **34 实体**：表/字段/任务/Quota/Tunnel
- **16 关联**：对象关系可串起来分析
- **35 指标**：存储/成本/资源/安全（如 `CU时 = SUM(cost_cpu / 100 / 3600)`）
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
| 存储压力 | “哪些表最近占用的存储最高？” | 识别 TOP 表和分区膨胀，定位生命周期异常与冷热失衡 |
| 成本压力 | “为什么这个月成本比上月涨得快？” | 按 owner/project/task_type 拆解 CU 时 |
| 权限审计 | “哪些表的权限暴露面更大？” | 审计 LABEL 保护表与字段级授权 |
| 传输审计 | “最近有哪些公网下载行为？” | 基于 TUNNELS_HISTORY 与 client_ip 回溯 |
| 失败激增 | “最近 7 天失败任务为什么突然变多？” | 结合 TASKS_HISTORY 对比定位异常 |
| Quota 监控 | “哪些 Quota 已经接近资源瓶颈？” | 给出 quota_bottleneck 判断 |

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

### 四大应用场景

| 场景 | 说明 |
|------|------|
| AI 训练数据集管理 | 图片/音频/视频与标注标签同表存储，SQL 过滤样本集 |
| 多模态数据处理流水线 | 视频切帧、音频转文本、内容打标等多步骤中间结果统一存储 |
| 多模态加工缓存层 | 作为 OSS 缓存层，提升 MaxFrame/SQL 并行计算吞吐，解决小文件 QPS 限制 |
| 非结构化数据入湖 | 分散外部文件统一导入，扩展元信息建立企业非结构化资产库，获得事务和权限管控 |

### 多模态开发体验

- **拖拽导入**：上传文件夹自动识别路径、名称、类型元信息
- **画布编排加工**：根据 DAG 和算子参数动态生成 SQL+UDF 执行语句
- **预览下载**：图片/视频/音频/PDF 在线预览，看到内容而非二进制编码
- **WebDAV 挂载**：Blob 表挂载成操作系统文件夹，SQL CRUD 映射为 List/Get/Put

加工算子库覆盖：图像、视频、音频、PDF、Excel、Word、PPT、Markdown、HTML、中文文本。

### 计算（多模态算子）

| 类型 | 算子 |
|------|------|
| 图像/OCR | `IMG` |
| 文档/解析 | `DOC` |
| 音频/ASR | `AUDIO` |
| 视频/切帧 | `VIDEO` |

## MaxAgent 运营助手

MaxAgent 覆盖资源管理、成本治理、业务分析全链路，免控制台登录、移动端操控闭环，打通钉钉、飞书、微信机器人，实现"聊天即服务"。

### 四大运维场景

| 场景 | 典型痛点 | MaxAgent 能力 |
|------|---------|-------------|
| **数据开发日常运维** | 作业失败/慢排查耗时长、查询性能瓶颈 | 作业运维与智能诊断、物化视图推荐与收益评估、Auto Cluster 聚簇优化 |
| **资源管理员移动运维** | 项目配置繁琐、外出告警无法及时处理 | 项目配置与参数管理、Quota/路由/计划/分时配置、机器人移动端闭环处置 |
| **降本增效** | 费用增长不知原因、Quota 利用率不明、存储大户不清 | 成本智能分析与趋势、计量成本归因与 Top 消费者、存储观测与分层推荐 |
| **智能问数** | 不懂 SQL、无法定位目标表和业务逻辑、全表扫描 | NL2SQL 自然语言查数据、上下文追问深度分析、跨域联动（查询+诊断） |

## Delta Table 与 AutoMV：Agent 时代的操作保障

### Delta Table

无惧 Agent 破坏数据：TimeTravel 随时回滚、ACID 事务表引擎、近实时写入、增量读取、自动调优、存储成本持平或更低。

### AutoMV（自动物化视图）

无惧 Agent 重复查询：自动物化，空间换时间降算力：
- **作业特征分析**：从历史执行计划自动提取查询模式
- **公共计算抽取**：跨 SQL 语义识别公共子查询
- **全局最优推荐**：综合收益/成本评估，自适应存储阈值
- **自动查询改写**：透明加速，用户无感知，零人工干预，确定性收益

## MaxFrame Coding Skill

集成到主流 AI 编程助手中，将 MaxFrame 分布式数据处理知识体系注入 Vibe Coding：

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

### 演进方向

- 基座：`maxframe-job-coding`
- 专业方向：多模态处理、AI Function、GPU/资源调度

## 多 Agent 接入口

| 入口 | 能力 | 特色 |
|------|------|------|
| **MaxCompute CLI** | `bootstrap` 环境引导、`whoami --json` 结构化输出、`skill install` | JSON 输出 Agent 友好，多认证方式 |
| **数据探索客户端** | 桌面级客户端，本地持久化工作现场 | AI 驱动数据探索，推断 DDL |
| **OpenClaw Sub-Agent** | OpenClaw 做总控，Sub-Agent 做专业执行 | 证据链完整：Plan → Act → Result |

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 整体产品
- [[dataworks-2026-0528-xialiaori]] — 2026-05-28 虾聊日全部分享内容汇总
- [[maxcompute-data-ai]] — MaxCompute Data+AI 四层架构
- [[hologres-skills]] — Hologres 同类 Skills 体系
- [[flink-skills]] — Flink Agent Skills
- [[emr-skills]] — EMR Skills
- [[agent-mcp-protocol]] — MCP 协议原理
