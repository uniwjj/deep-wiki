---
title: Flink 状态后端
description: Keyed/Operator State 两类状态、Memory/Fs/RocksDB 三种状态后端原理与选型、增量快照与云原生演进
aliases: [Flink State Backend, 状态后端, Flink 状态管理, Keyed State, Operator State, RocksDB StateBackend]
tags: [big-data, concept, practice]
sources: [2026/07/01/flink-state-backend-deep.html, 2026/07/01/flink-embedded-to-cloudnative.html]
created: 2026-07-01
updated: 2026-07-01
---

# Flink 状态后端

状态（State）是 Flink 有状态计算的"记忆"——算子在运行中维护的中间数据，支撑实时推荐、风控监控、IoT 传感器历史等场景。**状态后端（State Backend）** 决定状态存在哪里、如何访问、如何容错恢复，是 Flink 架构中数据的"守护者"。

## 两类状态

| 类型 | 绑定对象 | 典型场景 |
|------|---------|---------|
| **Keyed State**（键控状态） | 数据的 key | 每个 key 独立维护状态，如按用户/设备维度的个性化推荐 |
| **Operator State**（算子状态） | 算子实例 | 全局计数、跨分区状态共享，如实时大屏统计全平台交易总额 |

两类状态都需要可靠的后端存储来保证持久化与故障恢复。

## 状态后端的三个职责

State Backend 决定三件事：

1. **存哪里**：JVM 堆内存、本地 SSD、还是分布式文件系统。
2. **怎么访问**：堆内直接读写对象，还是序列化后访问（如 RocksDB）。
3. **如何容错**：全量快照还是增量快照。

它配合 [[flink-checkpoint|Checkpoint]] 工作——Checkpoint 协调快照过程，State Backend 决定状态本身如何存储与序列化。

## 三种状态后端

### MemoryStateBackend

- **存储**：状态以对象形式存于 TaskManager 的 JVM 堆内存；Checkpoint 快照默认存 JobManager 堆内存（可配 HDFS/S3）。
- **优点**：无磁盘 I/O，延迟极低；实现简单，无额外依赖。适合开发测试、小流量调试。
- **缺点**：
  - 受限于 JVM 堆大小，状态一大就 OOM。
  - GC 压力随状态增长急剧上升，频繁 Full GC 拖累吞吐甚至秒到分钟级停顿。
  - Checkpoint 默认存 JM 内存，JM 故障则全丢；单次状态大小受 RPC 框架 akka frame 限制（默认 ~10MB）。
- **适用**：状态小、对延迟敏感的开发/测试场景，**严禁生产大状态**。

### FsStateBackend（文件系统状态后端）

- **存储**：状态仍存 TaskManager 堆内存（同 Memory）；但 Checkpoint 快照写入外部文件系统（HDFS/S3/本地）。
- **优点**：Checkpoint 持久化到可靠存储，JM 故障不丢；适合中等规模状态（受 TM 堆限制）。
- **缺点**：状态仍在堆内，大状态仍有 OOM 和 GC 问题——本质和 Memory 一样受堆约束。
- **适用**：中等状态量、需要可靠 Checkpoint 的生产场景。

> 注意：Memory/Fs 的命名容易误解。Fs 的"Fs"指 Checkpoint **快照**落文件系统，状态本身仍在堆内。两者状态存储方式相同（堆内对象），区别在 Checkpoint 落点。

### RocksDBStateBackend

- **存储**：状态存入 TaskManager 本地的 **RocksDB**（嵌入式 LSM 树数据库，存磁盘）；Checkpoint 支持增量快照写入 HDFS/S3。
- **优点**：
  - 状态存磁盘，**唯一支持超大状态**（TB 甚至 PB 级），不受 JVM 堆限制。
  - 支持增量 Checkpoint，只传变化的 SST 文件，容错开销大幅降低（据来源可降 70%+）。
  - 堆外存储规避 GC 问题。
- **缺点**：状态访问需序列化/反序列化经 RocksDB，延迟高于堆内方案；依赖 RocksDB 调参（block cache、write buffer 等）。
- **适用**：大状态生产作业的**事实标准**。某头部电商双 11 用 RocksDB 后端处理峰值 2 亿条/秒、PB 级状态，延迟保持毫秒级。

## 选型决策

| 场景 | 推荐 | 理由 |
|------|------|------|
| 本地开发/单元测试 | Memory | 零依赖、快 |
| 中等状态 + 需可靠 Checkpoint | Fs | 状态在堆内够用，Checkpoint 落 HDFS |
| 大状态（GB~PB）/ 长期运行生产作业 | **RocksDB** | 唯一能撑大状态、支持增量快照 |

生产环境大状态作业几乎统一选 RocksDB。其增量快照 + 堆外存储是 TB 级状态能在分钟级完成故障恢复的关键。

## 增量快照

RocksDB 的增量 Checkpoint 是相对全量快照的关键优势：RocksDB 内部是 LSM 树，数据按 Sorted Run 落盘为 SST 文件，Compaction 时合并。增量 Checkpoint 只上传**自上次 Checkpoint 以来新增/变化的 SST 文件**，而非整个状态。这使得大状态作业的 Checkpoint 体积和时间可控——容错开销据来源降低 70%+，TB 级状态也能分钟级恢复。

对比：Memory/Fs 只能全量快照（每次把整个堆内状态序列化上传），状态一大 Checkpoint 就拖垮作业。

## 云原生演进

来源 [[flink-embedded-to-cloudnative]] 梳理了 Flink 状态管理从嵌入式到云原生的演进：

- **嵌入式状态管理**：早期状态存算子内存，能力有限。
- **可插拔 StateBackend**：抽象出 StateBackend 接口，引入 RocksDB 支持大状态。
- **增量 Checkpoint**：解决大状态快照效率。
- **云原生（K8s）**：Flink 1.18+ 深度集成 Kubernetes 原生存储，状态管理在弹性扩缩容场景下更高效。多层存储架构（热数据内存、温数据 NVMe SSD、冷数据分布式文件系统）实现智能分层，毫秒级访问热数据。

新一代方向是把状态管理从"单机嵌入式"推向"云原生分层存储"，支撑弹性扩缩容下的状态高效迁移与恢复。

## 相关页面

- [[apache-flink]] — Flink 总览：架构与运行时
- [[flink-checkpoint]] — Checkpoint 容错：State Backend 配合 Checkpoint 实现状态持久化与恢复
