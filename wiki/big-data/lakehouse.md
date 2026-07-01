---
title: 湖仓一体（Lakehouse）
description: 数据平台演进（数仓→数据湖→湖仓一体）、湖仓一体定义与三层能力、流批一体（计算层统一）、CIDR 论文理论源头、大厂落地
aliases: [Lakehouse, 湖仓一体, Data Lakehouse, 流批一体, Stream-Batch Unification]
tags: [big-data, concept, architecture]
sources: [2026/07/01/lakehouse-data-platform-history.html, 2026/07/01/lakehouse-databricks-what-is.html, 2026/07/01/lakehouse-architecture-evolution.html, 2026/07/01/lakehouse-bytehouse-practice.html, 2026/07/01/lakehouse-tencent-practice.html, 2026/07/01/lakehouse-citic-practice.html, 2026/07/01/lakehouse-selectdb.html, 2026/07/01/lakehouse-tech-selection.html, 2026/07/01/lakehouse-roundtable.html, 2026/07/01/lakehouse-jdcloud.html, 2026/07/01/lakehouse-cidr-2021.pdf]
created: 2026-07-01
updated: 2026-07-01
---

# 湖仓一体（Lakehouse）

**湖仓一体（Lakehouse）** 是融合数据湖的低成本存储与数据仓库的数据治理/事务能力的新一代数据平台架构。它用开放表格式在低成本对象存储上提供 ACID 事务、Schema 演进、时间旅行与高性能查询，使一份数据同时服务 BI 分析与机器学习，消除数据湖与数据仓库的割裂。

> 概念理论源头是 CIDR 2021 论文 *Lakehouse: A New Generation of Open Platforms...*（Matei Zaharia 等，Databricks）。该 PDF 待逐页视觉复核（见 [[lakehouse-cidr-2021|论文 sidecar]]），本页论点暂以 Databricks 博客 [[lakehouse-databricks-what-is|What is a Data Lakehouse]] 及中文转述为据。

## 数据平台的组成

数据平台 = **存储系统 + 计算引擎 + 接口**（来源 [[lakehouse-data-platform-history|极简数据平台史]]）：

- **存储** —— 把原料存进来。特点：时间跨度长（存全历史）、来源分散（MySQL/日志/第三方）、集中存储（建立 single source of truth）。
- **计算引擎** —— 从存储提炼信息。无大一统引擎，按模型/时效/数据量选型：深度学习用 TF/PyTorch，离线用 MapReduce/Spark，BI 用 OLAP。
- **接口** —— 屏蔽底层差异，向上提供统一访问。

数据平台之于数据，好比炼油厂之于原油——数据需"提炼"才释放价值。

## 演进：数仓 → 数据湖 → 湖仓一体

### 数据仓库（Data Warehouse）

- 写入前必须定好表结构（Schema-on-write），结构规整、事务可靠、查询快。
- **痛点**：贵、封闭；数据进来前定死结构，换工具查询几乎不可能；非结构化数据支持弱。

### 数据湖（Data Lake）

- 2010 年前后：先把数据原样扔进便宜存储（HDFS/S3/OSS），想查再说（Schema-on-read）。
- **优点**：便宜、开放、灵活。
- **致命缺陷**：只是一堆散装文件，不是表——没法可靠更新、没法保证并发写入一致、没法回历史版本。缺乏治理易沦为"数据沼泽"。

存算一体架构的瓶颈：HDFS NameNode 单点、副本放大存储成本。

### 湖仓一体（Lakehouse）

业界开始给数据湖"加账本"——**表格式（Table Format）**。有了它，散装文件变成真正的表。湖仓一体 = 数据湖的低成本开放存储 + 数据仓库的事务/治理/查询能力，三者结合：

1. **开放直存格式** —— Iceberg/Delta/Hudi/Paimon 等表格式，数据存廉价对象存储，不被厂商锁定。
2. **一等公民的机器学习支持** —— 同一份数据服务 BI 与 ML，无需搬迁。
3. **高性能查询** —— 表格式的元数据管理（Manifest、统计信息）支持文件裁剪与高效查询。

## 三次架构迭代

来源 [[lakehouse-architecture-evolution|架构演进核心路线]] 总结过去十年大数据架构三次迭代：

| 阶段 | 核心技术 | 特征 | 问题 |
|------|---------|------|------|
| **1. 离线仓库** | Hadoop/Hive/MapReduce/Spark | T+1 批处理，数仓驱动报表 | 数据孤岛、ETL 重、链路长、资产沉淀差 |
| **2. 实时仓库补充** | Kafka/Flink/ClickHouse/Druid | 离线+实时并存，链路割裂 | 批流双链路、指标不统一、数据重复、治理分裂 |
| **3. 湖仓一体+批流一体** | Hudi/Iceberg/Delta+Paimon/Flink/Spark+OLAP | 统一湖仓、统一链路、统一资产 | — |

湖仓一体+批流一体成为主流，是因为它解决了前两代的痛点：批流统一链路消除数据不一致、湖仓统一资产减少冗余、元数据治理融入链路解决口径混乱。

## 湖仓一体 vs 批流一体

两者常并提但是不同层面的统一：

| 维度 | 湖仓一体 | 批流一体 |
|------|---------|---------|
| 层面 | **存储层**统一 | **计算层**统一 |
| 统一什么 | 数据湖+数据仓库能力（明细/全量/增量/历史 + 宽表/聚合/指标/查询） | 离线+实时计算（同一数据模型、同一口径） |
| 关键技术 | 开放表格式（[[iceberg]]/[[paimon]]/Hudi/Delta） | [[apache-flink|Flink]] 流批一体引擎 |
| 价值 | 冷数据归档/热数据计算/实时增量，统一资产治理 | 一张表既支持离线也支持实时，消除 T+1 与实时差异，减少重复 ETL |

湖仓一体是底座（存储），批流一体是上层（计算），二者结合构成现代数据架构。流批一体的引擎实现见 [[apache-flink]]（DataStream/Table/SQL 统一到 Transformation）。

## 流批一体

流批一体的核心理念是阿里提出的"**一套班子、一套系统、一个逻辑**"——批处理和流处理不再用两套系统、两条链路，而是同一引擎、同一数据模型、同一套口径。

### Lambda 与 Kappa 的背景

- **Lambda 架构**：批层（batch layer，Hadoop/Spark）+ 速度层（speed layer，Storm/Flink）+ 服务层。两套链路分别算批和流，结果合并。问题是双链路、口径不一致、维护两套代码。
- **Kappa 架构**：只保留流层，一切皆为流，批是流的特例。用 Kafka 重放历史数据实现"批"。简化为单链路，但要求消息可重放。

流批一体是 Kappa 思想的工程化——Flink 以流为原点，批是有界流，同一引擎同一 API 处理两者，无需 Lambda 的双链路。

### Flink 流批一体的落点

- **Table/SQL 层**：Legacy Planner → Blink Planner 演进，Table/SQL 统一到 DataStream Transformation（来源 Flink 流批一体的实践与探索）。
- **执行引擎层**：从 DataStream、DAG Scheduler、Shuffle 架构、容错策略四个维度落地流批一体（来源 Flink 执行引擎：流批一体的融合之路，Flink Committer 马国维）。
- **字节跳动实践**：Flink + Iceberg 做"计算同源、存储同源"的流批一体架构（来源 Flink 流批一体在字节跳动的探索与实践）。

## 表格式：湖仓一体的基石

湖仓一体的"账本"由开放表格式提供，四格式各有侧重（详见 [[iceberg]] 四格式对比）：

- [[iceberg]] —— 中立开放，行业标准
- [[paimon]] —— 流批一体，Streaming First
- Hudi —— Hadoop 生态，upsert
- Delta Lake —— Spark 生态，Databricks

选型见湖格式选型主题。Paimon 的流式湖仓（Streaming Lakehouse）是湖仓一体在实时方向的代表——分钟级延迟、CDC 入湖、changelog 驱动分层。

## 大厂落地

| 企业 | 架构关键词 | 效果 |
|------|-----------|------|
| 字节跳动 | 湖仓一体+实时数据主链路；ByteHouse 多级缓存+对冲读 | 实时+离线统一，Zero ETL，存储降 1/3 |
| 阿里巴巴 | OneData/MaxCompute/Hologres；Flink+Paimon | 统一资产治理，实时离线融合 |
| 京东 | 湖仓统一/批流一体 | 统一链路，数据标准化，质量提升 |
| 腾讯 | Lakehouse+批流一体；TC-Iceberg CDC 流式消费 | 统一治理，指标统一，降本增效 |
| 中信建投 | 湖仓一体（信创替代，ANCHOR 标准） | 金融行业选型落地 |

腾讯内部 CDC 场景选 Hudi、常规入湖选 Iceberg的真实决策见 [[lakehouse-tencent-practice|腾讯落地实践]]。

## 未来趋势

数据基础设施平台化、治理化、实时化：湖仓一体（治理/成本/事务）、批流一体（实时化/一致性）、数据服务化（API/自助）、治理平台化（血缘/元数据/生命周期/安全/质量）、云原生化（K8s+云存储）。数据平台终局：数据即资产，架构为能力，平台为底座，治理为保障。

## 待补充

- CIDR 论文逐页复核：湖仓一体学术定义、与数据仓库/数据湖的正式对比、事务实现细节（待 PDF 视觉复核）
- Hudi / Delta Lake 的独立页面（当前仅在 [[iceberg]] 对比表中涉及）

## 相关页面

- [[iceberg]] — 开放表格式（湖仓一体的存储基石，中立标准）
- [[paimon]] — 流式湖仓（湖仓一体的实时方向）
- [[apache-flink]] — 流批一体引擎（湖仓一体的计算层）
- [[flink-cdc]] — CDC 入湖（湖仓一体的数据接入）
- [[gravitino]] — 统一元数据治理（湖仓一体的治理层）
- [[ltap-architecture]] — LTAP 架构（统一 OLTP/OLAP 的另一视角）
