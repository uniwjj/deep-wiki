---
title: Curvine：从存算分离到 Agent 数据底座
description: OPPO 高级架构师、Curvine Maintainer 闾泽军在 DACon 2026 北京站的演讲——对象存储成为默认数据层后，用原生缓存文件系统消除写放大与 NFS 网关瓶颈，以多级缓存、统一命名空间和端云协同支撑 Agent 工作区、训练、推理与多模态检索
aliases: [Curvine, Curvine 端云协同架构, Agent 数据底座, 闾泽军, Curvature, 缓存文件系统]
tags: [big-data, ai-agent, architecture, tool, practice]
sources: [2026/10/29/2026DACon北京站/AI Ready到Agent Ready：Data+AI平台架构升级/闾泽军-从存算分离到Agent数据底座——Curvine的端云协同架构实践.pdf]
created: 2026-10-29
updated: 2026-10-29
---

# Curvine：从存算分离到 Agent 数据底座

> 核心主张：存算分离之后，瓶颈已经从「存了多少」转移到「数据怎么流动」。缝合式缓存的写放大与中心化网关瓶颈无法靠调优解决，Curvine 的答案是在对象存储之上原生做一个缓存文件系统——同一层既做缓存、又做文件系统，把对象存储读成本地 IO，并以此作为 Agent 数据底座。

> 演讲者：闾泽军，Curvine Maintainer、OPPO 高级架构师、Curvine Team
> 场合：DACon 2026 北京站「AI Ready 到 Agent Ready：Data+AI 平台架构升级」
> 项目主页：github.com/CurvineIO/curvine（Apache 2.0，CNCF Landscape）

## 背景与问题

### 存算分离之后，瓶颈已经转移

演讲用三层递进说明问题背景：

| 判断 | 内容 |
|------|------|
| 存算分离·云原生主流 | 计算弹性伸缩、存储集中沉淀；存算分离已是云上架构的默认答案 |
| 对象存储·默认数据层 | 数据湖、训练语料、模型权重、Agent 工作区，都以 S3 兼容对象存储为家 |
| AI 负载·流动上关键路径 | 训练喂不饱 GPU、推理等 KV 复用、Agent 高频小 I/O——访问模式已从「大文件顺序」变为「高频、跨节点、小粒度」 |

结论：瓶颈不在「存了多少」，而在「数据怎么流动」。这正是「缝合式缓存方案」出现的背景，也是 Curvine 的起点。

### Agent 数据底座要回答的五类新问题

当 Agent 成为负载主体，数据底座要同时回答五个新问题：

1. **万级工作区**——创建风暴下，云盘 / EFS 供给跟不上
2. **沙箱临时文件**——高频随机读写，随用随弃
3. **记忆与检索**——向量 / 全文小 I/O，等不起延迟
4. **端云协同**——两个执行面，要同一份工作区
5. **交付产物**——持久落对象存储，不随沙箱消失

五类问题指向同一层底座，这正是 Curvine 要解决的对象。

### 缝合式缓存的两条先天代价

在对象存储前「缝」一层缓存，有两条绕不开的代价：

**代价一：写放大。** 对象不可变——修改一个块，代价是整对象重写；大文件随机写被成倍放大，且按请求计费。示意：100GB 文件改 1KB，缝合式方案触发 100GB 全对象重写与全量同步；Curvine 则是 64MB 分块下的分块写入与增量同步。

**代价二：NFS 网关瓶颈。** NFS 单连接吞吐有限，数据与元数据都经中心化网关单点；并发一上来，这里就是天花板。路径对比：

- 缝合式：客户端 → NFS → 中心网关 → 异步回源
- Curvine：客户端 ⇄ 并行直连数据面

### 与其缝合，不如原生做一个文件系统

演讲把缝合式方案的缺口归为三类，并逐条给出 Curvine 的回应：

| 缺口 | 缝合式缓存方案 | Curvine 原生缓存文件系统 |
|------|----------------|--------------------------|
| 语义缺口 | 原子重命名 / 硬链接 / xattr 不完整——git rename、node inotify、Python 打包语义不全 | 完整 POSIX，LTP 1129 项通过、0 Broken；git / node / inotify 原样运行 |
| 成本黑箱 | S3 请求按次计费 + 写放大——隐性成本随流量放大 | 分块写入 + 增量同步，读写路径并行直连，不经中心网关 |
| 结构瓶颈 | 中心化网关单点 + NFS 单连接——并发与吞吐受限 | 缓存可丢·数据可恢复，路径 1:1 映射对象存储，不做数据锁定 |

一句话概括定位：**同一层既做缓存、又做文件系统：语义完整，路径最短。**

## 方案/架构

### 三个角色：缓存、文件系统、底座

**Curvine = 对象存储之上的缓存文件系统**——数据持久在对象存储，访问在计算侧就近命中。

- **缓存角色**：把对象存储读成本地 IO。Memory → SSD → HDD 三级分层，热数据自动提升。
- **文件系统角色**：完整 POSIX 命名空间 + 多协议接入：FUSE / CSI / S3 / HDFS / SDK。
- **底座角色**：六类场景共用一层——Agent 工作区 / 训练加速 / 模型分发 / 多模态 / OLAP / 多云缓存。

### 六个场景，一层底座

架构图列出的六类 use case：

| 场景 | 图中描述 |
|------|----------|
| AI Agent Platform Storage | Isolated POSIX workspaces for tens of thousands of stateful Agent Pods on K8s |
| LLM Training Acceleration | Cache datasets & checkpoints near GPU nodes to shorten training cycles |
| LLM Model Distribution | Fast multi-region model artifact distribution through the cache |
| Multimodal Data Lake Access | POSIX data access to cloud object stores without copying data locally |
| OLAP Query Acceleration | Hot data cache for compute-storage separated OLAP engines |
| Multi-Cloud Data Caching | Unified cache layer across multi-cloud object storage backends |

底层对象存储后端包括 Amazon S3、HDFS、阿里云 OSS、MinIO、RustFS、Google GCS；接入面包括 POSIX/FUSE、CSI Driver（K8s）、S3 Gateway、HDFS API、Java/Python SDK。

### 架构：一条数据路径支撑多种负载

整体分三层：

- **接入面（Clients）**：CLI、SDK（Java/Python/C++）、FUSE（POSIX mount）、CSI（K8s PVC）、Apps（S3/HDFS API）
- **Curvine Server（Distributed）**
  - **Master Cluster（Raft HA · Metadata · Coordination）**：Master-1/2/3、Metadata（namespace/blocks）、Scheduler（worker/load balance）
  - **Worker Cluster（xN）**：Worker-1…N、Disk Management（Multi-Tier Cache：Memory / SSD / HDD）
- **UFS（Underlying File System）**：Object Storage（S3…）、HDFS、Local FS、Other UFS backends

关键设计点：

- 元数据 → Master；数据 I/O → Worker 直连；同节点短路读，零网络
- 控制面：Raft HA 元数据 + 调度，**50 亿文件规模**，元数据 **~100μs 级**
- 统一命名空间：端、云、训练、推理看到同一棵目录树（Raft 强一致）
- UFS 持久层：缓存可丢，数据可恢复

### 为什么用 Rust 写存储

演讲给出的四个理由，指向「快 & 稳，不做取舍」：

| 特性 | 说明 |
|------|------|
| NO GC · 延迟可预测 | 没有 Stop-the-World 暂停，尾延迟平稳——元数据 ~100μs 级、10 万+ QPS 稳定运行 |
| MEMORY SAFETY · 内存安全 | 所有权 + 借用检查在编译期消除悬垂指针、越界与 use-after-free——没有偶发 core dump |
| FEARLESS CONCURRENCY · 无畏并发 | Send / Sync 让数据竞争无法通过编译；Tokio 用少量线程承载海量 I/O |
| ZERO-COST · 零成本抽象与零拷贝 | 泛型与 trait 编译期单态化；bytes 引用计数缓冲 + splice，数据路径少一次搬运 |

同一套 Rust 内核支撑 FUSE / CSI / S3 / SDK 全部接入面——**18 万+ 行代码**，性能与稳定性来自同一个地基。

### 一张访问面，两种模式：FSMode 与 CacheMode

同一套内核、同一个命名空间，按工作负载选择模式，数据始终不被锁定；模式以挂载点为粒度控制，同一集群内可灵活配置、按需混用。

| 维度 | FSMode · 完整 POSIX 文件系统 | CacheMode · 弹性缓存层 |
|------|------------------------------|------------------------|
| 核心能力 | 语义完整；强一致元数据 | 就近命中；弹性可丢 |
| 具体指标 | LTP 1129 项通过·0 Broken（141 TCONF 跳过）；Raft HA·50 亿文件·rename 4.3 万 QPS | Memory→SSD→HDD 多级缓存，回源对象存储；缓存失效不影响数据，路径 1:1 映射对象存储，随时可恢复 |
| 面向负载 | Agent 工作区 / 工具链 / 需要完整文件语义的负载 | 热表加速 / KV Cache / 模型分发 / 跨云缓存 |

「不是两套系统：同一个内核、同一个命名空间——该要语义的地方给语义，该要速度的地方给速度。」

## 关键工程做法

### 案例一：大数据热表——跨云回源变本地命中

- **问题**：云端大数据集群把热表写到 OSS/S3；IDC 侧计算每次读都跨云回源——出口流量与首字节延迟双高，热表越热、回源越多。
- **方案**：IDC 部署 Curvine 缓存集群，读写都经缓存：R 读本地命中、W 经缓存汇聚回写——跨云只剩按需回源与增量同步。
- **效果**：对象存储下载流量、跨云带宽双双下降 **23%+**；业务侧改动为 **0**——读写仍是 POSIX 原路径。图中数据为 2026.09 内部实测的 24h 曲线，红框/绿线标出接入后的区间。

### 案例二：多模态数据湖——慢在首字节，不在算力

实验设置：envelope · 100 万条 1536 维向量（dbpedia-entities-openai-1M）· 每组 10,000 次查询 · LanceDB 0.34.0。

**问题**：向量 / 全文检索是几十到几百次小粒度随机读；对象存储上每次 I/O 都背一个首字节延迟——p50 11ms、p99 62ms，QPS 卡在 69。

**方案·两档接入**：

1. **零侵入·只放 `_indices` 目录**：不改一行业务代码——LanceDB 的索引目录指向 Curvine，全文检索立刻 2.3×
2. **全程 `curvine://`·收益最大化**：`curvine://` 作为 LanceDB ObjectStore 直读——数据不迁移，POSIX / SDK 双通道

数据链路：LanceDB 引擎 → `curvine://` ObjectStore → 对象存储（数据仍在这里）。多模态记忆中的图片 / 视频 / Embedding 以 Lance / Parquet 落盘，检索走缓存直读——不复制数据、不换引擎。

检索效果（单元格 = QPS · p50 · p99）：

| 查询类型 | OSS | 只缓存索引 | 全程 Curvine |
|----------|-----|------------|--------------|
| vector 向量检索 | 69.4 · 11ms · 62ms | 76.9 · 9ms · 51ms | 333.3 · 2ms · 10ms |
| fts 全文检索 | 31.4 · 26ms · 88ms | 71.4 · 12ms · 37ms | 144.9 · 5ms · 15ms |
| hybrid 混合检索 | 25.2 · 36ms · 90ms | 37.9 · 24ms · 52ms | 73.5 · 13ms · 21ms |
| vector_with_filter | 2.6 · 377ms | 2.6 · 378ms | 3.9 · 249ms（仅 1.5×） |

汇总：向量检索 p50 11→2ms（5.5×）；检索 QPS 69→333（4.8×）；p99 62→10ms（收敛 6×）；零侵入 2.3×。**如实标注**：vector_with_filter 只有 1.5×——瓶颈在过滤 + 扫描的 CPU 路径，不在 I/O。「知道技术在哪不起作用，和知道它在哪起作用同样重要。」测试环境为最小集群 1 Master + 1 Worker，是下限不是上限。

### 案例三：训推 KV Cache——跨实例复用，跳过重复 prefill

**问题**：长上下文推理中 prefill 占大头；而 KV 只活在单实例显存里——每个副本、每次会话都从头重算。上下文越长，TTFT 越贵。

**方案·三级分层**：

| 层级 | 介质 | 做法 |
|------|------|------|
| L0 | GPU HBM · Paged KV | 显存内分页缓存，推理引擎原生能力 |
| L1 | CPU DRAM · LMCache | 节点内存池，进程内外都可读 |
| L2 | 共享 NVMe · Curvine | 汇聚各节点本地盘为 RWX 共享层——`fs://` 直挂，跨 Pod 可见 |

实现组合：SageMaker HyperPod · vLLM + LMCache `fs://` · Curvine RWX PVC（Master + 多 Worker）。Curvine 侧 Master 负责 Metadata+Journal（Backend: EBS durable），Worker 提供 Data Pool（hostPath → node-local NVMe）。

**跨 Pod 复用流程**：t0 Pod1 prefill 写 L2 → t1 相同前缀请求到达 → t2 Pod2 L0/L1 未命中，FUSE 读共享 L2 → t3 直接生成（跳过 prefill）。key = hash(prompt prefix)，跨全部 vLLM Pod 复用。

实测数字：

- 外部前缀缓存命中率 **96.2%**
- L2 写吞吐 **~4.9 GB/s**（同节点）
- L2 读吞吐 **~0.59 GB/s**（跨节点，~23ms）
- KV 文件 **14.68 MB / 256 token**（fp16，28 层）

TTFT 效果（Qwen2-7B-Instruct，2× vLLM replicas 跨 GPU 节点，Pod A 写、Pod B 读，串行请求）：534 token 时 0.99×（基本无收益）；1,080 token 1.69×；1,563 token 2.16×；2,022 token 2.37×；2,608 token **2.7×**（774→287ms）；3,142 token 2.22×。结论：上下文越长，收益越大；1.5K–2.6K token 是加速带。

### 案例三附录：MLPerf Storage——存储能不能把加速器喂饱

MLPerf Storage 不测算力：DLIO 按真实 batch 读数据，计算按 H100 实测时间仿真，数据真走 POSIX / FUSE 才算一次有效供给；AU = 模拟计算 ÷ 总墙钟，门槛为 ≥90 / ≥90 / ≥70。三个负载：

| 负载 | 特征 | 门槛与规模 |
|------|------|------------|
| ResNet-50（图像分类·TensorFlow） | 样本约 115 KB；海量小文件、高并发打开；最吃元数据和 FUSE 扩展 | AU ≥90%；最大 128×H100 |
| U-Net3D（医学影像分割·PyTorch） | 样本约 140 MB；大文件顺序读、带宽型；单卡就要 ~2.9 GiB/s | AU ≥90%；最大 8×H100 |
| CosmoFlow（宇宙学参数·TensorFlow） | 样本约 2.8 MB；随机读 + 同步更密；更容易露出尾延迟 | AU ≥70%；最大 8×H100 |

对标结果：AU **6/8 组胜出**，U-Net3D 三档规模全部超过，八组全部跨过 AU 门槛；聚合读带宽 **7/8 不落后**，唯一落后为 ResNet-50·128 卡（23.83 vs 24.14 GiB/s，**-1.3%**，如实标注）。演讲明确：Curvine 为内部对标（非官方提交），Alluxio 为 MLCommons Closed 官方提交（Enterprise AI）；CosmoFlow 8 卡两侧 Client 配比不同，原文明示、不单独解释。

### 案例四：Agent FS——万级工作区的供给与共享

**为什么传统方案不行**：

| 方案 | 瓶颈 |
|------|------|
| EBS 云盘 | 每节点约 28 块盘上限——100 Pod/节点密度需 4 倍节点 |
| EFS 动态供给 | 创建风暴下 API 限流——PVC 排队、供给变慢 |
| S3 挂载 | 非 POSIX——git / node / vite 跑不起来 |
| 端云各一套存储 | 视图不一致——无法共享同一份工作区 |

一线观察：真实 Agent 工作区 = `SOUL.md` / `AGENTS.md` / `MEMORY.md` + `node_modules` + `.git`——天然的文件系统负载。

**Curvine 的做法**：

- **CSI 动态供给**：工作区 = 共享命名空间上的一次 `mkdir`——毫秒级、不调用云 API、无单节点上限
- **FUSE 完整 POSIX**：git / node / vite / inotify 原样运行（LTP 1129 项）
- **统一命名空间**：端、云、沙箱挂载同一棵目录树（Raft 强一致）
- **两类存储一份供给**：scratch 临时（随沙箱生灭）+ 工作区持久（跨重建保留）

**端云协同**：三类接入点——云端沙箱（Agent 执行环境，CSI/PVC 挂载）、用户本机 Agent（本地开发/调试，FUSE 挂载）、SDK/CLI（程序/脚本方式）——共同挂载 Curvine 统一文件空间，再异步持久化到 S3/OSS/MinIO。临时文件高频随机读写并行直连数据面就近命中；目标产物落盘交换（A2A artifact）并持久化；共享只读层（代码仓库 / skills / node_modules）一次准备、多端复用。「执行面可以在端、在云、在沙箱里随时生灭——工作区始终是同一棵树。」

**EKS 实测**：

- 10,000/10,000 PVC 全部 Bound——零 Pending / 零 Failed
- ~100 Agent Pod / 节点——零存储争抢
- 核心 Pod 仅 4 个：1 Master + 3 Worker
- 单集群供给容量 ~300TB（30Gi 最小分配 × 万级）
- 持久性：pod-0 / pod-5000 / pod-9999 三个样本写入，Pod 重启后数据仍在，跨节点视图一致

为什么只有 4 个核心 Pod：元数据走 Master、数据直连 Worker——控制面极轻；存储容量随 Worker 本地盘横向扩展，不靠堆云盘。

## 数据与效果

| 场景 | 问题 | 关键效果 |
|------|------|----------|
| 大数据热表 | OLAP 引擎跨云读热表，出口流量与首字节延迟双高 | 对象存储下载流量、跨云带宽双双下降 23%+；业务侧 0 改动 |
| 多模态数据湖 | 向量/全文检索高频小 I/O 被首字节延迟支配 | p50 5.5×、QPS 4.8×、p99 收敛 6×；零侵入 2.3× |
| 训推 KV Cache | 长 prompt 重复 prefill、跨实例 KV 无法共享 | TTFT 最多快 2.7×；外部前缀命中率 96.2% |
| MLPerf Storage | 存储能否把 H100 喂饱 | AU 6/8 胜出、八组全部过门槛；带宽 7/8 不落后，唯一 -1.3% |
| Agent FS | 万级工作区供给、工具链要 POSIX、端云同树 | EKS 10,000 PVC 零 Pending；~300TB 单集群 |

## 经验与边界

- **不是所有负载都能靠缓存加速**：vector_with_filter 仅 1.5×，因为瓶颈在过滤与扫描的 CPU 路径而非 I/O。原文把这条作为「唯一不显著的一组」主动标注，说明适用边界要按瓶颈位置判断。
- **性能结论的测试口径有限**：多模态检索测试是最小集群 1 Master + 1 Worker，「这是下限，不是上限」；MLPerf 对标为内部对标，非官方提交，不能等同于官方榜单结论。
- **缓存可丢、数据可恢复**：Curvine 路径 1:1 映射对象存储，不做数据锁定；缓存失效不影响数据本身。
- **FSMode 与 CacheMode 不是两套系统**：同一内核、同一命名空间，按挂载点粒度选择；该要语义的地方给语义，该要速度的地方给速度。
- **Agent 工作区不是云盘，而是一棵目录树**：执行是瞬态的，工作区是持久的；这决定了供给方式应是命名空间上的 `mkdir`，而不是逐 Pod 分配块设备。
- **下一步方向**：向 Agent 纵深——沙箱与工作区在万级密度下的快速供给、快照与恢复；记忆与检索（向量、全文与文件）收拢到同一命名空间；KV Cache 与模型分发共用同一个缓存层。向高性能深处——RDMA 高速网络、io_uring 异步 IO、SPDK 用户态存储、零拷贝与元数据路径。目标只有一个：让「把对象存储读成本地 IO」更快、更稳。

## 相关页面

- [[lakehouse]] — 存算分离与开放表格式是 Curvine 所处的数据底座背景
- [[iceberg]] — 湖仓表格式依赖对象存储，Curvine 加速其热数据读取
- [[apache-arrow]] — 列式内存格式，与多模态检索中的数据路径相邻
- [[ai-native-data-platform-vision]] — AI 时代数据平台把多模态数据视为 Agent 认知燃料，与 Agent 数据底座视角互补
- [[apache-parquet]] — 多模态记忆以 Lance / Parquet 落盘，检索走缓存直读
