---
title: Flink CDC
description: 变更数据捕获——CDC 概念、Flink CDC 1.0 痛点、2.0 增量快照(DBLog+FLIP-27)、与 Debezium 协同、3.0 YAML pipeline 演进
aliases: [Flink CDC, CDC, Change Data Capture, 变更数据捕获, 增量快照, DBLog]
tags: [big-data, tool, concept]
sources: [2026/07/01/flink-cdc-2-0-principle.html, 2026/07/01/flink-cdc-incremental-snapshot-framework.html, 2026/07/01/flink-cdc-streaming-integration.html, 2026/07/01/flink-cdc-debezium-collaboration.html, 2026/07/01/flink-cdc-incremental-snapshot-algorithm.html, 2026/07/01/flink-cdc-incremental-snapshot-mechanism.html, 2026/07/01/flink-cdc-practice-optimization.html, 2026/07/01/flink-cdc-enterprise-integration.html, 2026/07/01/flink-cdc-2-2-release.html]
created: 2026-07-01
updated: 2026-07-01
---

# Flink CDC

**CDC（Change Data Capture，变更数据捕获）** 是实时监控数据库变更、将变更记录写入数据流的技术。**Flink CDC** 是 Apache Flink 的一组 Source 连接器，基于数据库日志 CDC 实现**全量 + 增量一体化读取**——用户在 Flink 中看到的就是该表的最新一致性快照，无需自行处理全量与增量的衔接合并。

## CDC 的两种策略

| 策略 | 原理 | 特点 |
|------|------|------|
| **Query-based** | 周期性查询数据库探测变化 | 简单，但受查询频率限制，延迟高、压数据库，不适合实时 |
| **Log-based** | 监听数据库日志（MySQL Binlog / Oracle Redo Log / MongoDB OpLog） | 近乎零延迟捕获，不压数据库；日志顺序完备，可实现 exactly-once |

Flink CDC 从一开始就采用 **Log-based** 策略，结合 Flink 的 [[flink-checkpoint|Checkpoint]] 机制保证一致性与容错。

## 链路简化：Flink CDC 的核心价值

传统实时同步链路冗长：

```
MySQL → Canal/Debezium → Kafka → Flink → 下游
```

四个组件，每个都要部署/监控/维护，故障排查需逐环节定位。Flink CDC 把中间环节"吞掉"：

```
MySQL → Flink → 下游
```

只需一个 `CREATE TABLE` 指定连接信息，即可像查普通表一样实时读取全量 + 增量。核心优势是**简化架构、降低运维成本**。这背后的关键是将 Debezium 作为**嵌入式引擎**深度整合进 Flink 运行时（详见下文"与 Debezium 协同"）。

## Flink CDC 1.0 的三大痛点

1.0 集成 Debezium Engine 采集数据，支持全量 + 增量，但有三个痛点：

1. **一致性靠加锁保证** —— 全量与增量衔接时需对库/表加锁（MySQL 用 `FLUSH TABLES WITH READ LOCK` 全局锁）。加锁时长不确定，极端情况会**把数据库 hang 住**，影响线上业务。
2. **不支持水平扩展** —— 全量阶段只能单并发读取，大表（亿级）读取耗时可达**小时级**。
3. **全量阶段不支持 Checkpoint** —— fail 后只能从头重读，结合大表耗时，恢复代价极高。

## Flink CDC 2.0：增量快照框架

2.0 借鉴 **Netflix DBLog** 的无锁算法，基于 **FLIP-27** 新 Source 接口重写，达成三个目标：**无锁、水平扩展、全量阶段支持 Checkpoint（断点续传）**。

### FLIP-27 新 Source 接口

FLIP-27 解决旧 `SourceFunction` 的痛点（split 发现与读取耦合、批流需不同实现、checkpoint 锁被 source 占有等），引入两个核心组件：

- **SplitEnumerator**（运行在 JobManager）—— 发现并分配 split。
- **SourceReader**（运行在 TaskManager）—— 读取 split 实际数据。

两者通过 `SourceEvent` 消息通信。新架构天然批流一体：批处理产出固定有限 split 集合，流处理产出无限 split。`SourceReaderBase` 抽象类提供主线程与读取线程的同步机制，用户只需实现 `SplitReader`（取记录）和 `RecordEmitter`（解析转换）。

### DBLog 算法原理

DBLog 算法分两步：**分 chunk** 和 **读 chunk**。

**分 chunk** —— 把一张表按主键 PK 切成多个 chunk（桶/片）。通过 query 获取最大/最小 PK，按步长划分，每个 chunk 是**左闭右开**区间，实现无缝衔接。首尾 chunk 用正无穷/负无穷兜底（所有 < k1 的由第一个 chunk 读，> kN 的由最后一个读）。chunk 可分发给不同并发 task，实现**水平扩展**。

**读 chunk** —— 每个 chunk 读取时同时读两份：
1. 该 chunk 的历史数据（全量）
2. 读取期间该 chunk 发生的 Binlog 事件（增量）

然后做 **normalize**（归并）：用 Binlog 中的最新值覆盖历史数据中对应的行，得到该 chunk 的一致性快照。关键是**只锁 chunk 范围内的内存状态，不锁数据库**——这就是"无锁"的由来。

### 全量 + 增量一体化衔接

所有 chunk 读完并 normalize 后，Flink CDC 记录下当前 Binlog 位点，随后切换到纯增量模式（只消费 Binlog）。因为每个 chunk 的 normalize 都对齐到了 Binlog，整体衔接保证一致性，无需全局锁。

### 断点续传

基于 FLIP-27，每个 chunk 的读取进度可作为 split 状态保存进 [[flink-checkpoint|Checkpoint]]。fail 后从上次未完成的 chunk 继续，而非从头——解决 1.0 全量阶段无法恢复的痛点。

## 与 Debezium 协同

Flink CDC 没有重复造轮子，而是把 Debezium（成熟的基于日志的 CDC 框架）作为**嵌入式引擎**集成进连接器，对用户透明。

**分工**：
- **Debezium（数据捕手）**：连接数据库，以最小影响抓取变更事件。两种模式：全量快照（Snapshot，首次同步全量数据）+ 增量流式（Streaming，持续监听 binlog）。
- **Flink（数据加工厂）**：把 Debezium 的原始格式转成 Flink 生态的通用语言——**RowData** 对象。

**关键：数据格式转换（反序列化）**。Debezium 每条事件含丰富上下文：
- `op` —— 操作类型：`c`=create、`u`=update、`d`=delete、`r`=快照读取
- `before` / `after` —— 更新前/后的行数据
- `source` —— 库/表/时间戳等元信息

Flink CDC 的反序列化模块（如 `DebeziumJsonDeserializationSchema`）解析这些字段，按 `op` 构造 RowData 并打上 **RowKind** 标签，对应 Flink 的 changelog 语义：

| Debezium op | Flink RowKind | 含义 |
|-------------|---------------|------|
| `c` / `r` | `+I` (INSERT) | 新增数据 |
| `u` before | `-U` (UPDATE_BEFORE) | 更新前的旧数据 |
| `u` after | `+U` (UPDATE_AFTER) | 更新后的新数据 |
| `d` | `-D` (DELETE) | 删除 |

这一步让 Debezium 的变更事件流入 Flink 的 changelog 流，下游算子可按 changelog 语义处理更新/删除。

## Flink CDC 3.0：YAML pipeline 演进

3.0（2023 年 11 月发布）引入全新 **YAML pipeline 作业**，使 Flink CDC 成为独立的端到端数据集成框架：

- **无状态设计** + **DataSource/DataSink 抽象** —— 解耦 source/sink。
- **Schema Evolution** —— 源表结构变更（加列等）可自动同步到下游。
- **极简 YAML 语法**描述数据集成作业，降低使用门槛。

2024 年初 Flink CDC 作为 Flink 子项目加入 Apache 软件基金会，至 3.1.1 已有 130+ 贡献者、1000+ commits、5000+ star，生态覆盖 MySQL/PostgreSQL/Oracle/MongoDB/Db2/OceanBase 等主流数据库。

## 版本演进小结

| 版本 | 关键能力 |
|------|---------|
| 1.x | 集成 Debezium，全量+增量，但有锁/单并发/无 Checkpoint 三痛点 |
| 2.0 | 增量快照框架（DBLog + FLIP-27）：无锁、水平扩展、断点续传 |
| 2.2 | 新增四种数据源，支持动态加表 |
| 3.x | YAML pipeline 端到端集成框架、Schema Evolution、加入 Apache |

## 相关页面

- [[apache-flink]] — Flink 引擎总览：CDC 连接器作为 Flink 的 Source
- [[flink-checkpoint]] — Checkpoint 容错：CDC 2.0 增量快照的断点续传依赖 Checkpoint
- [[flink-state-backend]] — 状态后端：CDC 作业的状态存储
- [[iceberg]] — Iceberg 表格式（CDC 数据常写入 Iceberg 湖）
