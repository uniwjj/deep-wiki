---
title: 实时数仓
description: 数仓演进（离线大数据→Lambda→Kappa）、ODS/DWD/DWM/DWS/ADS 分层、Lambda vs Kappa 选型、实时数仓分层实践、实时计算引擎与平台化
aliases: [实时数仓, Real-time Data Warehouse, Lambda 架构, Kappa 架构, 数仓分层, 实时计算]
tags: [big-data, concept, architecture, practice]
sources: [2026/07/01/realtime-warehouse-lambda-kappa.html, 2026/07/01/realtime-warehouse-lambda-kappa-illustrated.html, 2026/07/01/realtime-warehouse-architecture-comparison.html, 2026/07/01/realtime-warehouse-layering-practice.html, 2026/07/01/realtime-warehouse-architecture-techstack.html, 2026/07/01/realtime-warehouse-18k-words.html, 2026/07/01/realtime-warehouse-hologres.html, 2026/07/01/realtime-warehouse-scenarios.html, 2026/07/01/realtime-warehouse-landing-path.html, 2026/07/01/realtime-warehouse-tech-selection.html, 2026/07/01/warehouse-layering-maxcompute.html, 2026/07/01/realtime-computing-engine-comparison.html, 2026/07/01/realtime-computing-aliyun-platform.html, 2026/07/01/realtime-computing-aliyun-productization.html]
created: 2026-07-01
updated: 2026-07-01
---

# 实时数仓

**实时数仓**是在传统离线数仓基础上引入流处理能力，支撑秒级看板、在线决策、实时风控等 T+0 业务场景的数据仓库架构。其核心演进是从离线大数据架构 → Lambda → Kappa，分层模型从离线数仓的 ODS/DW/ADS 演化为实时版的 ODS/DWD/DWS/ADS。

## 数仓概念与演进

数据仓库是 Inmon 1990 年提出的概念：**面向主题、集成、相对稳定、反映历史变化**的数据集合，用于支持管理决策。架构演进（来源 [[realtime-warehouse-lambda-kappa|Lambda 与 Kappa]]）：

1. **离线大数据架构** —— 互联网时代用 Hadoop/Spark 替代经典数仓的传统工具，但架构本质未变，仍以离线批处理为主。
2. **Lambda 架构** —— 业务实时性要求提高，在离线数仓基础上加一个实时计算加速层，对数据源做流式改造。
3. **Kappa 架构** —— 实时业务成主要部分，架构调整为以实时事件处理为核心。

## 数仓分层

数仓分层是数仓建设的核心方法论。三层精简模型与五层细分模型（来源 [[realtime-warehouse-lambda-kappa|Lambda 与 Kappa]]、[[warehouse-layering-maxcompute|MaxCompute 数仓分层]]）：

| 层 | 全称 | 职责 |
|----|------|------|
| **ODS** | Operation Data Store | 贴源层/数据准备区，直接接入源数据（业务库/埋点日志/MQ），原样保存 |
| **DWD** | Data Warehouse Details | 数据明细层，清洗规范化（去空值/脏数据/离群值），与 ODS 同粒度，补全维度 |
| **DWM** | Data Warehouse Middle | 数据中间层，在 DWD 基础上轻微聚合，算统计指标 |
| **DWS** | Data Warehouse Service | 数据服务层，按主题整合汇总为宽表，供 OLAP/数据分发 |
| **ADS** | Application Data Service | 数据应用层，存 ES/Redis/PostgreSQL，供数据分析挖掘 |

分层价值：划清层次结构、数据血缘追踪、减少重复开发、把复杂问题简单化、屏蔽原始数据异常。

## Lambda 架构

Lambda 由 Twitter 工程师 Nathan Marz 提出，在离线数仓基础上**增加实时计算链路**，对数据源做流式改造（发到消息队列），实时计算订阅 MQ 完成指标增量计算，服务层合并离线+实时结果。三层组成：

| 层 | 职责 | 技术 |
|----|------|------|
| **批处理层（Batch）** | 存储管理主数据集（不可变），预先批处理计算视图，基于完整历史数据保证准确性 | Hadoop MapReduce / Spark |
| **速度层（Speed）** | 实时处理新数据，提供最新数据的实时视图，最小化延迟（只处理最近数据，牺牲准确性换延迟） | Storm / Flink / Spark Streaming |
| **服务层（Serving）** | 响应查询，合并批层与速度层结果 | Druid / Redis / 数据库 |

**优点**：灵活可扩展，对硬件故障和人为失误容错性好。**缺点**：双链路（批+流）维护两套代码，口径不一致，逻辑复杂。

## Kappa 架构

Kappa 是对 Lambda 的简化——**只保留流层，一切皆为流，批是流的特例**。用 Kafka 等消息队列作为唯一数据源，所有计算由流处理引擎完成。历史数据重处理通过 Kafka 重放（回溯 offset）实现，无需批层。

**优点**：单链路、一套代码、口径统一。**缺点**：要求消息可重放（Kafka 保留期有限，超长期历史重放受限于 retention）；流处理引擎需足够强大处理全量。

## Lambda vs Kappa 选型

| 维度 | Lambda | Kappa |
|------|--------|-------|
| 链路 | 批+流双链路 | 纯流单链路 |
| 一致性 | 批流口径可能不一致 | 单链路口径统一 |
| 历史重处理 | 批层自然支持 | 靠 Kafka 重放（受 retention 限制） |
| 复杂度 | 高（两套代码） | 低（一套代码） |
| 适用 | 大规模报表系统，需保障数据一致性 | 小规模实时业务，实时为主 |

实际中常结合：核心实时用 Kappa，复杂历史回算仍借批层。流批一体（见 [[lakehouse]]）是 Kappa 思想的进一步工程化——Flink 同一引擎同一 API 处理批流，无需 Lambda 双链路。

## 实时数仓分层实践

实时数仓借鉴离线分层精华 + 流式计算灵活性，实现 T+0 数据驱动（来源 [[realtime-warehouse-layering-practice|ODS 到 ADS 实战]]）：

| 层 | 实时版职责 | 典型技术 |
|----|-----------|---------|
| **ODS** | 接入原样保存源变更数据，Kafka Topic 或临时存储，保留 1~3 天 | Flink CDC / Canal / Kafka |
| **DWD** | 清洗、规范化、维表 Join（异步维表/广播维表）、构建事实流，**最关键层级** | Flink SQL |
| **DWS** | 时间窗口聚合、按主题汇总为宽表，支撑实时监控看板 | Flink SQL + HBase/ClickHouse/StarRocks |
| **ADS** | 面向报表接口输出，实时 GMV/活跃用户/转化率，高并发访问 | ES / Redis / StarRocks |

完整链路：数据采集（Kafka/Canal/Flink CDC）→ 实时计算（Flink 清洗/聚合/维表补充）→ 存储（HBase/Kafka/ES/StarRocks）→ 应用（BI/实时看板/推荐）。

### 维表关联

实时数仓 DWD 的关键操作是维表关联（Lookup Join / Broadcast Join）——流数据关联静态维表补全维度字段。抖音集团用 Paimon 重构实时数仓时，维表关联性能优化是重点（见 [[paimon]] 抖音实践）。

## 实时计算引擎与平台化

实时数仓的计算底座是流处理引擎，三代演进（来源 [[realtime-computing-engine-comparison|引擎对比]]）：

- **Storm** —— 纯流式逐条，早期代表，但无高级 API/状态管理/Exactly-Once。
- **Spark Streaming** —— 微批（mini-batch），延迟秒级，本质仍批。
- **Flink** —— 流批一体，毫秒级，原生状态与 Checkpoint。**实时数仓事实标准**。

Flink 详见 [[apache-flink]]。其容错（[[flink-checkpoint]]）与状态管理（[[flink-state-backend]]）是实时数仓 Exactly-Once 与大状态支撑的基础。

### 平台化（阿里云实时计算）

来源 [[realtime-computing-aliyun-platform|阿里云平台建设]]、[[realtime-computing-aliyun-productization|产品化思考]]：实时计算平台从 Hadoop/YARN 生态迁移到 K8s 云原生的 2.0→3.0 演进，涵盖引擎内核、资源底座、平台架构、技术品牌四方面。Flink CDC 产品化简化了过去"CDC→Kafka→Flink"链路（见 [[flink-cdc]] 链路简化）。

## 技术选型

2025 主流实时数仓技术栈组合（来源 [[realtime-warehouse-tech-selection|技术选型报告]]）：

- **Flink + Kafka + Doris/StarRocks** —— 经典实时数仓，Flink 算、OLAP 查。
- **Flink + Kafka + Paimon + Doris/StarRocks** —— 流式湖仓，Paimon 做湖存储，CDC 入湖丝滑（见 [[paimon]] 流式湖仓分层）。
- **Flink + Hologres** —— 阿里云一体化方案，Binlog 驱动、计算组资源隔离（见 [[realtime-warehouse-hologres|Hologres 实时数仓]]）。

湖仓一体背景下，Paimon 流式湖仓（ODS/DWD/DWS 全在 Paimon + StarRocks 查询）正成为新主流，把传统离线数仓延时从小时/天级降到分钟级。

## 落地路径与常见坑

来源 [[realtime-warehouse-landing-path|落地路径]]：端到端链路从采集到可视化的常见坑——状态膨胀导致 Checkpoint 失败、维表关联反压、数据乱序与迟到、Exactly-Once 端到端保障（Sink 需支持幂等/事务）、小文件问题（湖存储）等。

## 相关页面

- [[lakehouse]] — 湖仓一体（实时数仓的存储底座；流批一体是 Kappa 的工程化）
- [[apache-flink]] — 实时计算引擎（实时数仓计算底座）
- [[flink-checkpoint]] — Exactly-Once 语义基础
- [[flink-cdc]] — CDC 数据接入实时数仓 ODS 层
- [[paimon]] — 流式湖仓（实时数仓分层的新主流存储）
- [[iceberg]] — 湖存储表格式
