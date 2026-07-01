---
title: 湖格式选型对比（Iceberg vs Paimon vs Hudi vs Delta）
description: 四大湖仓表格式横向对比——元数据/索引/更新机制/CDC/生态绑定，腾讯真实选型决策，专家圆桌观点，按场景的选型建议
aliases: [湖格式选型, Iceberg vs Paimon vs Hudi, 表格式对比, 数据湖表格式选型]
tags: [big-data, comparison, concept]
sources: [2026/07/01/paimon-iceberg-hudi-comparison.html, 2026/07/01/iceberg-v3-watershed.html, 2026/07/01/lakehouse-tencent-practice.html, 2026/07/01/lakehouse-roundtable.html, 2026/07/01/lakehouse-tech-selection.html, 2026/07/01/paimon-four-vendors-practice.html, 2026/07/01/paimon-core-principle.html]
created: 2026-07-01
updated: 2026-07-01
---

# 湖格式选型对比（Iceberg vs Paimon vs Hudi vs Delta）

四大湖仓表格式（Iceberg / Paimon / Hudi / Delta Lake）各有定位与"娘家"。本页综合各技术页的对比视角，给出横向机制对比、真实选型决策与按场景的建议。

## 四格式定位速览

详见 [[iceberg]] 的四格式对比表。核心差异在"娘家"与侧重（来源 [[iceberg-v3-watershed|v3 分水岭]]）：

| 格式 | 娘家 | 侧重 | 一句话 |
|------|------|------|--------|
| **Hudi** | Uber 2016 | Hadoop 生态 | 老架构平滑升级、upsert |
| **Delta Lake** | Databricks | Spark 生态 | Databricks 平台一体化 |
| **Paimon** | Flink 社区 2022（阿里） | 流批一体 | Streaming First，CDC 入湖丝滑 |
| **Iceberg** | Netflix 2018 | 中立开放 | 不绑定任何引擎/云，做行业标准 |

Hudi 偏 Hadoop、Delta 偏 Spark、Paimon 偏 Flink——都有娘家。**Iceberg 没有娘家**，这是它最大的特点。围绕四格式形成三种理念：湖仓一体（Databricks）、流式湖仓（Paimon 主场）、开放湖仓（Iceberg 阵营）。

## 横向机制对比

来源 [[paimon-iceberg-hudi-comparison|三格式比较]]、[[lakehouse-tech-selection|技术选型]]、[[paimon-core-principle|Paimon 核心原理]]：

| 维度 | Iceberg | Paimon | Hudi | Delta |
|------|---------|--------|------|-------|
| **更新机制** | 快照 + delete file（pos/eq-delete） | LSM Tree（文件级 LSM） | MOR/COW，日志合并 | 事务日志 + parquet |
| **元数据存储** | 三层元数据（Catalog/metadata/manifest） | snapshot + manifest + data | .hoodie 时间线元数据 | _delta_log 事务日志 |
| **索引** | manifest 文件统计做裁剪 | 主键哈希分 Bucket | Bloom/Record Index/HBase Index | 文件统计 |
| **CDC/changelog** | v3 行级血缘（原生 CDC）；v2 需外部 | Changelog Producer 原生产出完整 changelog | 原生支持 upsert/CDC | 需 CDF（Change Data Feed） |
| **流批一体** | 偏批/分析 | **Streaming First**，流批一体主场 | 偏批/upsert | 偏批（Spark） |
| **引擎绑定** | 中立，跨引擎 | Flink 最佳，多引擎读 | Hadoop/Hive 深度绑定 | Spark 深度绑定 |
| **Schema 演进** | 强（分区+Schema 演进，无需重写） | 完整 Schema 演进 | 支持 | 支持 |
| **国内采用度** | 强（腾讯/华为/阿里/网易） | **第一梯队**（字节/阿里/小米/shopee） | 传统企业为主 | 最低 |

### 更新机制的根本差异

- **Iceberg**：用快照 + delete file 实现更新，偏批/分析场景。v2 引入 pos-delete/eq-delete 支持 upsert，v3 删除向量让行级更新快 10 倍。
- **Paimon**：用 **LSM Tree** 原生支持高频更新（文件级 LSM：Sorted Run + Compaction）。这是它与 Iceberg 的根本差异——Paimon 偏流/实时，Iceberg 偏批/分析。
- **Hudi**：MOR（Merge On Read，读时合并）适合 upsert，COW（Copy On Write，写时复制）适合读多写少。

### CDC/changelog 能力

Paimon 的 **Changelog Producer** 能为任意输入流产生完整增量数据（所有 update_after 都有 update_before），让 Paimon 表本身成为 changelog 源——这是它"CDC 入湖丝滑"的招牌。Iceberg v3 的"行级血缘"让 CDC 成为表的原生能力，正在追赶。详见 [[paimon]] 三大核心能力、[[iceberg]] v3 演进。

## 真实选型决策：腾讯

来源 [[lakehouse-tencent-practice|腾讯落地实践]] 给出腾讯内部的真实选型逻辑（比对比表更有说服力）：

- **CDC 场景选 Hudi** —— CDC 频繁 upsert，Hudi 的 MOR/upsert 能力强。
- **常规入湖选 Iceberg** —— 批处理/分析为主，Iceberg 中立开放、查询性能好。

这说明选型不是"哪个最好"，而是"按场景匹配"。腾讯云 TC-Iceberg 还做了两表拆分（change store + base store）支持 CDC 流式消费，弥补 Iceberg 在 CDC 的短板。

## 专家圆桌观点

来源 [[lakehouse-roundtable|数仓还是湖仓圆桌]] 是少见的多方视角材料——Apache Iceberg（Amoro PPMC）、Apache Hudi、Apache Paimon、StarRocks 四位社区代表同台讨论选型。核心共识：没有万能格式，按业务场景（实时性/更新频率/查询模式/生态绑定）选择，且四格式在互相借鉴融合（如 Iceberg v3 引入 CDC 能力、Paimon 借鉴 Iceberg 元数据设计）。

## 按场景选型建议

综合各来源：

| 场景 | 推荐 | 理由 |
|------|------|------|
| 流式湖仓 / 分钟级实时 / CDC 入湖 | **Paimon** | Streaming First，LSM 高频更新，changelog 驱动分层 |
| 开放湖仓 / 跨云跨引擎 / 中立标准 | **Iceberg** | 不绑定引擎/云，v3 成行业标准，REST Catalog 跨云 |
| 老旧 Hadoop 栈平滑升级 / 强 upsert | **Hudi** | Hadoop/HDFS 兼容最好，MOR upsert 强 |
| Databricks 平台 / Spark 深度集成 | **Delta** | 与 Spark/Databricks 无缝 |
| 批处理分析为主 | **Iceberg** | 查询性能好，文件裁剪强 |
| CDC 频繁 upsert 但又想用 Iceberg 生态 | Iceberg + 补丁 | 腾讯 TC-Iceberg 两表拆分方案 |

## 国内市场格局

来源 [[iceberg-v3-watershed|v3 分水岭]]：

- **Paimon 国内第一梯队** —— 阿里云 EMR Serverless StarRocks + Paimon、字节大规模 Flink CDC + Paimon、小米/shopee。实时湖仓几乎是 Paimon 主场。
- **Iceberg 强势布局** —— 腾讯云 DLC/EMR、华为云 MRS/DLI、阿里云 MaxCompute/EMR（Iceberg+Paimon 两条腿）、网易数帆 Amoro、字节 ByteHouse/LAS、小米、B 站。
- **Hudi** —— 仍以 Hadoop 为底的传统企业、早期湖仓改造的互联网公司。
- **Delta** —— 国内采用度最低（Databricks 中国区无直销）。

## 四格式互链

- [[iceberg]] — 中立开放表格式（详细元数据架构、v3 演进）
- [[paimon]] — 流式湖仓（LSM、三大核心能力、事件溯源视角）
- [[lakehouse]] — 湖仓一体（表格式是湖仓基石）
- [[realtime-data-warehouse]] — 实时数仓（Paimon 流式湖仓分层成新主流）

## 相关页面

- [[iceberg]] / [[paimon]] — 各格式独立深度页
- [[lakehouse]] — 湖仓一体架构（表格式是其存储基石）
- [[apache-flink]] / [[flink-cdc]] — Paimon 的最佳集成引擎与数据接入
- [[gravitino]] — 统一元数据治理（跨格式统一管理）
