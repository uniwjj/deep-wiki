---
title: Apache Paimon
description: 流式湖仓表格式——Streaming First 定位、LSM Tree、三大核心能力(主键表更新/Changelog Producer/Merge Engine)、批流一体表抽象、与 Iceberg/Hudi 对比
aliases: [Paimon, Apache Paimon, Flink Table Store, 流式湖仓, Streaming Lakehouse]
tags: [big-data, tool, concept]
sources: [2026/07/01/paimon-core-principle.html, 2026/07/01/paimon-douyin-optimization.html, 2026/07/01/paimon-ant-practice.html, 2026/07/01/paimon-flink-sql-lakehouse.html, 2026/07/01/paimon-starrocks-streaming-lakehouse.html, 2026/07/01/paimon-starrocks-ali-lakehouse.html, 2026/07/01/paimon-douyin-data-lake.html, 2026/07/01/paimon-xingfuli-practice.html, 2026/07/01/paimon-gravitino-collaboration.html, 2026/07/01/paimon-four-vendors-practice.html, 2026/07/01/paimon-iceberg-hudi-comparison.html, 2026/07/01/paimon-flink-advanced.html, 2026/07/01/paimon-event-sourcing.html]
created: 2026-07-01
updated: 2026-07-01
---

# Apache Paimon

Apache Paimon 是一个**流数据湖平台**（Streaming Lakehouse），兼具高速数据摄取、变更日志跟踪与高效实时分析能力。它创新地**把湖格式与 LSM 技术结合**，给数据湖带来实时流更新与完整的流处理能力，是 Flink 社区主导的流批一体表格式。

## 定位：Streaming First

Paimon 的根本定位是 **Streaming First**——面向 Flink 有最好集成的湖存储。理解这个定位要看大数据的"不可能三角"。

### 大数据不可能三角

大数据系统在**成本、查询延时、新鲜度**三者间做权衡（Trade-off），没有银弹，没有大一统系统，每个场景对应一种方案。Paimon 的选择是：在湖存储（低成本）上把**新鲜度**做到分钟级，同时保持可接受的查询延时。

### 流计算 + 存储的两代场景演进

来源 千千寰宇核心原理 梳理了流计算+存储的演进：

**场景 1：实时预处理（Queue + Flink + KV/OLAP + Online Query）**
- 早期 Storm（2010）解决"数据流起来、数据不丢"，但只有 At-least-once。
- Spark Streaming 用 mini-batch 复用批计算，达到 Exactly-once 但延迟分钟级。
- Flink（2014）靠内置状态 + 全局一致性快照，在纯流模式下兼顾 Exactly-once 与低延迟。
- 典型架构：流计算 + 在线 KV 服务（HBase/Redis/Tair）。优点：查询毫秒级；缺点：灵活性低、每新增业务要定制一条链路。

**场景 2：实时数仓（Queue + Flink + OLAP + Online Query）**
- ClickHouse（2016）以向量化计算带来超快查询，让业务数据可灵活即席查询。
- Flink + OLAP（ClickHouse/Doris/StarRocks/Hologres）成为新场景：流计算写 OLAP，OLAP 服务查询。

但这两代场景都有局限——要么灵活度不够，要么 OLAP 更新成本高。Paimon 的目标是**第三条路**：在湖存储上原生支持流式更新与 changelog，让数据湖本身具备"实时数仓"能力。

## 诞生：从 Flink Table Store 到 Paimon

Paimon 的诞生源于对现有湖格式的判断（来源 千千寰宇）。当时需要的是：

1. 一个有 Iceberg 良好建筑基础的湖存储。
2. 一个有强 **Upsert** 能力、用 LSM 结构（OLAP/流计算 State/KV 系统都用它）的存储。
3. 一个 **Streaming First**、面向 Flink 最佳集成的存储——地基直接考虑 Streaming & Flink，而非在复杂系统上修修补补。
4. 一个面向中国开发者、长期投入 Streaming + Lake 的社区。

关于 Hudi：Hudi 是好系统，但 **Hudi 不是为实时数据湖而生的**——其 Flink/流相关支持少，新 Feature 常只支持 Spark，Flink 要支持需天翻地覆重构。所以 Flink 社区决定造一个新的。

| 时间 | 事件 |
|------|------|
| 2022.01 | Flink 社区从零研发 **Flink Table Store (FTS)** |
| 2023.03.12 | FTS 进入 Apache 孵化器，改名 **Apache Paimon** |
| 2024.03.20 | Paimon 毕业为 Apache 顶级项目（TLP） |
| 2024.04.16 | ASF 正式宣布 Paimon 毕业 |

Paimon 在孵化一年内发布四个大版本，在阿里、字节、同程、蚂蚁、联通、网易、中原银行等企业生产实践。

## 核心：LSM Tree

Paimon 底层把列式文件存在文件系统/对象存储上，用 **LSM 树结构**支持大量数据更新与高性能查询。这正是它强 Upsert 能力的来源——LSM 是 OLAP 系统、流计算 State、KV 系统通用的结构，Paimon 把它引入湖存储，使数据湖具备数据库级的更新能力。

LSM 写入流程：内存排序 → Level0 落盘 → 异步 Compaction 合并。这一机制让 Paimon 主键表能高效处理 upsert/delete，远胜传统湖格式（靠重写文件或 delete file 实现 update）。

> 这也是 Paimon 与 [[iceberg]] 的根本差异：Iceberg 用快照 + delete file 实现更新（偏批/分析），Paimon 用 LSM 原生支持高频更新（偏流/实时）。详见 [[iceberg]] 四格式对比。

## 事件溯源视角：流原生存储

来源 事件溯源视角 提供了理解 Paimon 的独特角度——它不是"又一个兼容 Hive 的数据湖"，而是**第一个把"变更流"作为核心抽象的存储格式**，可称为**流原生存储（Stream-Native Storage）**。

### 表 = 数据 + 变更历史

传统视角与 Paimon 视角的根本分歧：

| 维度 | 传统视角 | Paimon 视角 |
|------|---------|------------|
| 表 | 数据集合 | 数据 + 变更历史 |
| 查询 | 读取当前状态 | 读取状态快照 **或** 消费变更流 |
| 写入 | 插入/覆盖 | 提交 changelog 记录 |

底层哲学接近 **Event Sourcing（事件溯源）**：每次更新不是"改数据"而是"记一笔账"，最终状态由所有变更合并得出，历史任意时刻状态都可重建。**Paimon 把数据库的 changelog 能力原生集成到了数据湖中**——这是它真正的创新。

### LSM-Inspired 而非 LSM-Based

Paimon 常被宣传为"基于 LSM 树"，但这容易误解（来源 事件溯源视角）。它实际是 **LSM-Inspired**：

- ❌ 没有内存表（MemTable）
- ❌ 不使用 WAL（Write-Ahead Log）
- ❌ 不在同一文件内做多层合并
- ✅ 实际是 **文件级 LSM**：Sorted Run + Compaction 模型，灵感来自 LSM 但实现于分布式文件系统之上

工作流程：Flink 将 changelog（+I/-U/+U/-D）以 Parquet/ORC 写入 `data/` → 按主键哈希分 bucket，bucket 内按主键排序（Sorted Run）→ 后台 Compaction 异步合并小文件、按主键去重保留最新 → 查询时通过最新快照获取有效文件、跳过被覆盖的旧记录。这种"文件级 LSM"既保留 LSM 高效更新特性，又适配对象存储的不可变性。

## 批流一体的表抽象

Paimon 提供**表抽象**，使用方式与传统数据库无异，但行为随执行模式变化：

| 模式 | 行为 |
|------|------|
| **批处理模式** | 像一张 Hive 表，支持 Batch SQL 各种操作，查询看到最新快照 |
| **流执行模式** | 像一个消息队列，查询行为像从"历史数据永不过期的消息队列"消费 changelog |

这种"一份数据、两种读法"是 Paimon 流批一体的核心——同一张表既可批查历史快照，也可流式消费变更。结合 Flink（流）+ Spark（批）提供完整流批处理，数据口径一致。

## 基本概念与文件布局

来源 核心原理和Flink应用进阶 补充的关键概念：

- **Snapshot（快照）** —— 捕获表某时间点状态。最新快照访问最新数据；时间旅行可访问历史状态。
- **Partition（分区）** —— 与 Hive 同概念，按列值分片。若定义主键，分区键必须是主键子集。
- **Bucket（桶）** —— 分区内的细分存储单元，按主键/记录哈希划分。**桶是读写最小存储单元**，桶数限制最大并行度；桶数不宜过大（小文件多、读性能降），建议每桶约 1GB 数据。
- **一致性保证** —— Paimon writer 用**两阶段提交**原子提交一批记录，每次提交最多生成两个快照。两个 writer 只要不同桶即可序列化提交；同桶则保证快照隔离（最终状态是混合但不丢更改）。

文件布局分层组织：从 snapshot 文件（JSON，含快照信息）开始，Paimon reader 可递归访问表中所有记录。基本目录下分层存放 snapshot/、manifest、data 等文件。

## 三大核心能力

来源 Paimon+StarRocks 流式湖仓 总结的三大能力：

### 1. 主键表更新（Primary Key Table）

底层 LSM Tree 实现高效数据更新。主键表可接受 upsert/delete，支持高频变更——这是流式湖仓的基础。

### 2. 增量数据产生机制（Changelog Producer）

Paimon 可为任意输入数据流产生**完整的增量数据**（所有 `update_after` 都有对应的 `update_before`），保证数据变更完整传递给下游。这让 Paimon 表本身成为 changelog 源——下游 Flink 可像消费 Kafka 一样消费 Paimon 表的变更，无需额外 Kafka 中转。

### 3. 数据合并机制（Merge Engine）

主键表收到多条相同主键数据时，Paimon 提供丰富的合并行为：
- **去重（deduplicate）** —— 默认，保留最新值。
- **部分更新（partial-update）** —— 用于宽表打宽，多个流各更新部分列。
- **预聚合（aggregation）** —— 用于指标计算，按聚合函数（sum/count 等）合并。

这三种 Merge Engine 直接对应数仓分层的需求：partial-update 打宽 DWD，aggregation 算 DWS 指标。

## 流式湖仓分层实践

来源 Paimon+StarRocks 给出典型流式湖仓分层架构：

```
MySQL --CDC--> Paimon(ODS) --Changelog--> Flink加工 --> Paimon(DWD) --Changelog--> Flink加工 --> Paimon(DWS) --外部表--> StarRocks(查询)
```

- **ODS**：MySQL 经 Flink CDC 实时写入 Paimon。
- **DWD**：Flink 订阅 ODS 的 Changelog，用 partial-update 打宽生成宽表，分钟级延时产出 Changelog。
- **DWS**：Flink 消费宽表 Changelog，用 aggregation 预聚合产出指标表。
- **应用**：StarRocks 读 Paimon 外部表，对外提供查询（交易大屏、行为分析、用户画像）。

优势：每层数据分钟级传递变更（离线数仓从小时/天级降到分钟级）；每层可直接接受变更数据，无需覆写分区，中间层数据易查易更新；模型统一、架构简化（全 Flink SQL + 统一 Paimon 存储）。

## 生态集成

Paimon 与 Flink 深度集成（最佳），同时支持 Spark、Hive、Trino、Presto、StarRocks、Doris 等引擎读取，统一存储、计算无边界。CDC 入湖支持 MySQL 等多种数据库，自动同步 Schema 变更，千万级数据规模下保持高效率低延迟。

## 国内落地

- **抖音集团**：多场景优化实践，维表 JOIN 性能优化、Merge Engine 扩展；基于 Paimon 重构实时数仓（ODS/DWD/APP 分层，Lookup/Broadcast Join）。
- **蚂蚁集团**：生产场景 Paimon 应用。
- **幸福里（字节房产业务）**：交易型场景用 Paimon 替代 Flink 状态算子做去重，附血缘管理、小文件踩坑。
- **阿里集团 + 饿了么**：StarRocks + Paimon 准实时 Lakehouse 改造，存储成本下降、查询性能提升。
- **小米/同程/网易** 等：大规模生产实践。Paimon 在国内采用度为第一梯队。

## 与 Iceberg / Hudi / Delta 的关系

Paimon 偏 Flink、Streaming First；Iceberg 中立开放；Hudi 偏 Hadoop；Delta 偏 Spark。详细对比见 [[table-format-selection]] 的横向机制对比表。Paimon 与 Gravitino 协同的元数据治理方案见 [[gravitino]]。

## 待补充

- Deletion Vectors 与索引机制细节
- Bucket Index 与 Hudi Flink State Index 的历史脉络（千千寰宇文中有，待精读补充）
- 物化表 / 格式表 / 对象表等多模态表类型的具体用法

## 相关页面

- [[iceberg]] — Apache Iceberg（中立开放表格式，Paimon 的对标基准；四格式对比）
- [[apache-flink]] — Flink 是 Paimon 的最佳集成引擎
- [[flink-cdc]] — CDC 数据入湖到 Paimon
- [[flink-state-backend]] — Paimon 用 LSM 结构（与 Flink RocksDB 状态后端同源）
- [[gravitino]] — Apache Gravitino 统一元数据（与 Paimon 协同）
- [[lakehouse]] — 湖仓一体（Paimon 流式湖仓是湖仓一体的实时方向）
- [[realtime-data-warehouse]] — 实时数仓（Paimon 流式湖仓分层成新主流）
