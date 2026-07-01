---
title: Apache Flink
description: 流处理计算引擎——有界/无界流、有状态计算、运行时架构与四层图、与 Spark/Storm 的定位差异
aliases: [Flink, Apache Flink, flink, 流处理引擎, 流计算引擎]
tags: [big-data, tool, concept]
sources: [2026/07/01/flink-design-implementation.html, 2026/07/01/flink-architecture-principle.html, 2026/07/01/flink-stream-batch-comparison.html, 2026/07/01/flink-learning-roadmap.html, 2026/07/01/aliyun-realtime-flink-product.html]
created: 2026-07-01
updated: 2026-07-01
---

# Apache Flink

Apache Flink 是一个**有状态流处理计算引擎**（Stateful Computations over Data Streams）。它把数据视作持续流动的事件流，在流之上提供有状态计算、容错恢复与精确一次语义，是当前实时计算领域的主流引擎。

## 核心定位

Flink 官方定义自身为 "Stateful Computations over Data Streams"，三个关键词决定了它与批处理引擎的根本差异：

- **Data Streams**：数据是无界、持续到来的事件流，而非静态的有界数据集。
- **Stateful**：算子可以维护"记忆"（状态），支撑聚合、窗口、模式匹配等有状态计算。
- **Computations**：在流上实时执行转换，延迟在毫秒到秒级。

### 有界流与无界流

| 维度 | 有界流（Bounded） | 无界流（Unbounded） |
|------|------------------|---------------------|
| 数据 | 有明确起止， finite | 持续到来，无终点 |
| 处理方式 | 批式（批量） | 流式（逐条） |
| 典型场景 | 离线统计 | 实时统计 |
| 结果去向 | 供人决策 | 供机器自动化处理 |

注意一组易混概念的区分：**离线/实时** 指处理延迟，**批量/流式** 指处理方式。Flink 的设计原点是流式处理无界流，有界流被视为无界流的特例——这也是 Flink "流批一体"哲学的根基（详见 [[flink-checkpoint]] 与流批一体主题）。

## 抽象层级

Flink 提供四层 API，从低到高：

1. **底层 ProcessFunction** —— 最底层，可访问事件、时间、状态、定时器，用于复杂事件处理。
2. **DataStream API** —— 核心 API，提供 map/filter/join/keyBy/window 等流式算子。
3. **Table API & SQL** —— 声明式关系型 API，Table/SQL 在执行层统一到 DataStream Transformation，是流批一体的落点。
4. **CepLibrary / Gelly / ML** —— 顶层的领域库（复杂事件处理、图计算、机器学习）。

DataStream 编程套路可归纳为五步：获取执行环境 → Source 加载数据 → Transformation 转换 → Sink 输出结果 → 触发执行（Action）。

## 运行时架构

Flink 是 Master-Slave 架构，核心角色：

- **JobManager**（Master）—— 作业协调者，内含三个组件：
  - **JobMaster**：管理单个作业的执行，将 JobGraph 转为 ExecutionGraph。
  - **ResourceManager**：管理 TaskSlot 资源调度（非 YARN 的 RM，是 Flink 内部的）。
  - **Dispatcher**：接收作业提交，为每个作业启动一个 JobMaster。
- **TaskManager**（Slave/Worker）—— 真正执行计算的进程，每个 TM 持有若干 **TaskSlot**，Task 在 Slot 中运行。
- **Client** —— HTTP REST 客户端，负责将 JobGraph 提交到 JobManager，非运行时角色。

### 并行度与 Task

- **并行度（Parallelism）** 决定一个算子被拆成多少个并行实例。
- **Operator** 是算子，**OperatorChain** 是把多个算子链在一起（减少序列化/网络传输），类比 Spark 的 Stage。
- **Task** 是执行单元：一个 OperatorChain 按并行度展开成多个 Task，Task 是预先启动、不移动位置的，数据在 Task 之间流动。

### TaskSlot 与资源

- 每个 TaskManager 拥有若干 **TaskSlot**，Slot 是 TM 内的资源切片（内存等分）。
- **Slot 共享机制**：同一个作业的不同算子子任务可共享一个 Slot，使一个 Slot 内跑完整作业的一条并行管线。这样资源按作业最高并行度分配即可，避免资源浪费。
- Slot 只做内存隔离，不做 CPU 隔离。

### 四层执行图

Flink 作业从代码到运行经过四层图转换，这是理解 Flink 任务执行的关键：

| 层 | 名称 | 生成方 | 作用 |
|----|------|--------|------|
| 1 | **StreamGraph** | Client（用户代码） | 最初的拓扑图，算子拼接，逻辑结构 |
| 2 | **JobGraph** | Client（优化） | StreamGraph 经算子链优化后，提交给 JM 的结构 |
| 3 | **ExecutionGraph** | JobManager | JobGraph 的并行化版本，调度层核心数据结构 |
| 4 | **物理执行图** | 各 TaskManager | Task 在各节点部署后形成的实际运行关系，非数据结构 |

提交流程：Client 生成 StreamGraph → 优化为 JobGraph → REST 提交到 JM → JobMaster 构造 ExecutionGraph → 向 ResourceManager 申请 Slot → 部署 Task 到各 TM → 上下游 Task 建立连接 → 形成物理执行图。

## 内存管理

Flink 自主管理内存而非完全依赖 JVM 堆 GC，原因是 JVM 内存模型的三个缺陷：对象存储密度低（一个 boolean 对象占 16 字节）、Full GC 影响性能（大堆可达秒到分钟级停顿）、OOM 影响稳定性。Flink 在 TM 内存模型中划分出 Framework Heap、Task Heap、Managed Memory（受 Flink 自主管理的堆外内存，用于排序/状态等）等区域，以字节级方式管理数据，规避 GC 问题。这一设计与 Spark、HBase 的思路一致。

## 与 Spark / Storm 的定位差异

三代实时计算引擎的设计哲学对比（来源 flink-stream-batch-comparison）：

| 引擎 | 模型 | 特点 |
|------|------|------|
| **Storm** | 纯流式（逐条） | 早期实时计算代表，但无高级 API、无状态管理、无 Exactly-Once |
| **Spark Streaming** | 微批（micro-batch） | 把流切成小批次 RDD 处理，延迟秒级，本质仍是批 |
| **Flink** | 流批一体 | 真正的逐条流处理，毫秒级延迟，原生状态管理与 Checkpoint |

Flink 最终胜出的关键：真正的流式语义（而非微批伪装）+ 有状态计算 + Exactly-Once 容错 + 流批一体。这与阿里"一套班子、一套系统、一个逻辑"的流批一体理念契合（见流批一体主题）。

## 容错与状态

Flink 的两大核心竞争力各有专页：

- **容错机制**：基于 Chandy-Lamport 分布式快照算法的异步 Barrier Checkpoint，详见 [[flink-checkpoint]]。
- **状态管理**：Keyed/Operator State 与三类状态后端（Memory/Fs/RocksDB），详见 [[flink-state-backend]]。

## 产品化形态

阿里云实时计算 Flink 版（前身 Alibaba Blink）是 Flink 的全托管 Serverless 商业版本，与开源 Flink 的差异主要体现在：全托管免运维、按量计费、深度集成阿里云生态（如 Flink CDC 直连、Hologres/MaxComputeSink）。工程化视角的平台演进（从 Hadoop/YARN 到 K8s 云原生）见实时计算主题。

## 学习路径

按深度递进的建议顺序：DataStream API 与编程模型 → 架构与四层图 → [[flink-checkpoint|Checkpoint 容错]] → [[flink-state-backend|状态后端]] → Table/SQL 与流批一体 → Flink CDC（见 CDC 主题）→ 实时数仓分层实践。

## 相关页面

- [[flink-checkpoint]] — Checkpoint 容错机制：Chandy-Lamport → 异步快照 → Barrier 对齐
- [[flink-state-backend]] — 状态后端：Keyed/Operator State、Memory/Fs/RocksDB 选型
- [[flink-cdc]] — Flink CDC 变更数据捕获（Flink 的数据接入层）
- [[iceberg]] — Apache Iceberg 开放表格式（Flink 常作为写入引擎）
- [[paimon]] — Apache Paimon 流式湖仓（Flink 最佳集成的湖存储）
- [[lakehouse]] — 湖仓一体（Flink 流批一体是湖仓一体的计算层）
- [[realtime-data-warehouse]] — 实时数仓（Flink 是实时数仓计算底座）
- [[ltap-architecture]] — LTAP 架构（流处理在统一数据平台的角色）
