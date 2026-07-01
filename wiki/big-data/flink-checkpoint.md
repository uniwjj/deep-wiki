---
title: Flink Checkpoint 与容错
description: 从 Chandy-Lamport 分布式快照到 Flink 异步 Barrier Checkpoint、对齐/非对齐机制、Checkpoint 与 Savepoint 的区别
aliases: [Flink Checkpoint, Checkpoint, Flink 容错, Barrier 对齐, Unaligned Checkpoint, 分布式快照]
tags: [big-data, concept, practice]
sources: [2026/07/01/distributed-snapshot-flink-checkpointing.html, 2026/07/01/flink-checkpoint-1-11.html, 2026/07/01/flink-checkpoint-savepoint.html]
created: 2026-07-01
updated: 2026-07-01
---

# Flink Checkpoint 与容错

Flink 的容错核心是 **Checkpoint**——周期性持久化算子状态的机制，使作业在失败后能从最近一次 Checkpoint 恢复状态、重放数据，实现"数据不丢、只算一次"。其理论基础是 1985 年 Chandy-Lamport 分布式快照算法。

## 理论源头：Chandy-Lamport 分布式快照

### 问题：如何给分布式系统拍照

分布式快照要记录系统在某一时刻的**全局状态**，但面临根本挑战：各进程、通道无法同时记录自身状态——没有全局时钟保证同步。

Chandy-Lamport 在论文 *Distributed Snapshots: Determining Global States of a Distributed System*（1985）中给出了答案。Lamport 自己回忆这个算法诞生于一次访问 Chandy 后的清晨淋浴——"大神的世界我们不懂，一言不合就写一篇论文"。

### 分布式系统模型

- 系统由**有限进程**和**有限消息通道**构成，用有向图描述：节点=进程，边=通道。
- 假设：通道 buffer 有限、消息保序、通道可靠。
- **全局状态 = 所有进程状态 + 所有通道状态的组合**。
  - 进程状态：初始状态 + 持续发生的 Events（如 Source 输入的新消息）。
  - 通道状态：通道上传输的消息。

### 算法：Marker 信使

作者证明通道状态可通过记录进程状态来恢复，提出算法：

1. 发起进程 p 记录自身状态，并向所有出射通道发出 **Marker**（标记消息）。
2. Marker 随消息流流经所有进程，通知每个进程记录自身状态和入射通道状态。
3. 所有进程执行完毕，一个分布式 Snapshot 即完成。

关键性质：**Marker 对计算过程无任何影响**——快照与计算并行运行，不干扰系统正常功能（正确性 + 并行性两点要求）。只要 Marker 在有限时间内传输、进程在有限时间内记录，算法即有限时间完成。这是 Flink Checkpoint 的数学根基。

## Flink 的工程实现：异步 Barrier Checkpoint

Flink 在论文 *Lightweight Asynchronous Snapshots for Distributed Dataflows* 中将 Chandy-Lamport 工程化为数据流上的**轻量级异步快照**。Flink 的 Marker 称为 **Barrier**（屏障）。

### 工作流程

1. **JobManager 的 CheckpointCoordinator** 定时向 Source Task 注入 Barrier（每个作业初始化一个 CheckpointCoordinator，在 ExecutionGraph 构建时由 `enableCheckpointing` 创建）。
2. Barrier 作为特殊消息事件，随数据流注入，向下游算子流动。
3. 算子收到 Barrier 后：对齐 → 将自身状态写入状态后端（见 [[flink-state-backend]]）→ 把 Barrier 转发给下游。
4. 当最终 **Sink 端算子**收到 Barrier 并确认，本次 Checkpoint 完成。

只有 Sink 确认才算成功，因此多输入算子存在 **Barrier 对齐时间**（可在 Web UI 查看各 Task 的对齐耗时）。

### Barrier 对齐与 EXACTLY_ONCE

当算子有多个输入时，Barrier 到达各输入的时间不同。处理方式决定语义：

- **Barrier 对齐（Aligned）= EXACTLY_ONCE（默认）**：算子收到某个输入的 Barrier 后，**暂停该输入**（缓冲后续数据），等所有输入的 Barrier 都到齐再做快照、转发。保证每条数据对状态"只影响一次"。
- **不对齐 = AT_LEAST_ONCE**：不等待，立即快照，可能导致部分数据被重复处理（对状态至少影响一次，可能多次）。

EXACTLY_ONCE 是针对**状态**而言——保证每条数据对 Flink 状态结果只影响一次。

## Unaligned Checkpoint（1.11+）

### 对齐的代价

Barrier 对齐在**反压**（backpressure）场景下会严重恶化：某输入 Barrier 迟迟到不了，算子一直缓冲数据，反压传导至上游，Checkpoint 超时失败。大规模状态作业常因反压导致 Checkpoint 失败、无法恢复。

### Unaligned 方案

Flink 1.11 引入 **Unaligned Checkpoint**（实验性，`enableUnalignedCheckpoints()`）：不再对齐等待，算子一收到第一个 Barrier 就触发快照，把**通道中尚未处理的 in-flight 数据**一并写入 Checkpoint。恢复时先重放这些 in-flight 数据，再接续。

代价是 Checkpoint 体积变大（多了 in-flight 数据），但换来了反压下的 Checkpoint 稳定性。这是从"对齐优先"到"可用性优先"的工程权衡。

## Checkpoint 的两个必要条件

Checkpoint 能生效的前提：

1. **数据源支持重放**：失败恢复需重放上次 Checkpoint 后消费的数据。如 Kafka 可重置 offset 重放；若数据源不支持重放，丢的数据无法找回。
2. **可持久化状态存储**：需 HDFS/本地文件等存储保存 Checkpoint 数据，供恢复时读取。

## 关键参数

```java
env.enableCheckpointing(60000);                              // 60s 触发一次
env.getCheckpointConfig().setCheckpointingMode(EXACTLY_ONCE); // 语义，默认
env.getCheckpointConfig().setMinPauseBetweenCheckpoints(1000);// 两次间隔至少 1s
env.getCheckpointConfig().setCheckpointTimeout(60000);        // 60s 内必须完成否则丢弃
env.getCheckpointConfig().setMaxConcurrentCheckpoints(1);     // 同时只允许 1 个
env.getCheckpointConfig().setTolerableCheckpointFailureNumber(3); // 允许失败 3 次
env.getCheckpointConfig().enableExternalizedCheckpoints(RETAIN_ON_CANCELLATION); // 取消时保留
env.getCheckpointConfig().enableUnalignedCheckpoints();       // 非对齐（反压场景）
```

默认 Checkpoint 并发为 1（有进行中的 Checkpoint 时不触发新的）。

## Checkpoint vs Savepoint

二者都基于分布式快照，但定位不同：

| 维度 | Checkpoint | Savepoint |
|------|-----------|-----------|
| **本质** | 自动容错机制 | 程序状态的全局镜像 |
| **目的** | 程序失败时快速恢复 | 程序升级、改并发度后继续从状态恢复 |
| **触发** | Flink 系统自动周期触发 | 用户主动触发 |
| **保留策略** | 默认程序取消时删除（可配 `DELETE_ON_CANCELLATION`/`RETAIN_ON_CANCELLATION`） | 一直保留，除非用户删除 |
| **存储** | 默认 JobManager 内存或配置外部存储 | 一般存 HDFS |

**Savepoint 的工程要点**：用 DataStream 开发时建议为每个算子定义 `uid`。Savepoint 恢复按 uid 匹配算子状态；若未定义 uid，Flink 自动生成，修改拓扑后 uid 可能变化，导致状态无法复用。定义 uid 后即使改了作业拓扑，相关算子仍能复用之前的状态。

## 局限

Checkpoint 是自动容错，但**无法挽救进程级失败**。如 Flink On YARN 模式下某 Container 发生 OOM，作业直接进入 FAILED 状态，此时 Checkpoint 不会自动拉起作业——需外部程序（如调度系统）检测失败并重新拉起。

## 相关页面

- [[apache-flink]] — Flink 总览：架构、四层图、抽象层级
- [[flink-state-backend]] — 状态后端：Checkpoint 持久化的状态如何存储
