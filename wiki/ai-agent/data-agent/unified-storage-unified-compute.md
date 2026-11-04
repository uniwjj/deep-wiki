---
title: 统一存储与统一计算——让多模态数据成为一等公民
description: 数帆 AI Native 底座的两个公式：Unified Storage = LakeHouse（Iceberg/Paimon + Lance）+ Unified Catalog，Unified Compute = Spark/Flink/OLAP + Ray/Daft + RAG；三类计算共享存储与调度，让 Agent 能"看见"并串联数据加工、AI 特征处理与知识检索
aliases: [Unified Storage, Unified Compute, 统一存储, 统一计算, LakeHouse + Unified Catalog, 多模态数据底座]
tags: [big-data, ai-agent, architecture, concept]
sources: ["2026/11/04/为 Agent 重建数据层：网易智企产品 AI Native 演进路径/index.html"]
created: 2026-11-04
updated: 2026-11-04
---

# 统一存储与统一计算

> 来源：DataFunSummit《为 Agent 重建数据层：网易智企产品 AI Native 演进路径》（蒋鸿翔，2026-11-03）第 2 节"让 Agent 用好多模态数据，先统一存储与计算"。

## 问题：数据是否可见

原文把底层的第一个问题定义为"**数据是否可见**"。传统数仓资产体系之外的文档、图片、音视频，对 Agent 而言等同于不存在；而 Agent 要自主规划任务，前提是知道**有哪些数据、含义是什么、是否有权访问**。

## Unified Storage = LakeHouse + Unified Catalog

| 组成 | 结构化数据 | 非结构化数据 | 统一管理 |
|---|---|---|---|
| 载体 | Iceberg、Paimon（业务库表、指标、明细） | Lance 等（文档、图片、音视频、向量） | Unified Catalog |
| 能力 | 一套湖仓同时容纳两类数据 | 非结构化数据的原生格式 | 结构化与非结构化**统一元数据目录**、统一注册、统一权限、统一血缘 |

原文对为什么它是 AI Native 前提的解释：

- **没有统一 Catalog，Agent 的每一次探索都是盲猜**——它不知道有哪些数据、什么含义、谁能访问
- **没有 Lance 这类非结构化原生格式**，文档 / 图片 / 音视频就永远在数据体系之外
- 过去游离在数仓资产体系外的材料需要**进入统一目录**，使 AI 消费数据具备前提

## Unified Compute = Spark / Flink / OLAP + Ray / Daft + RAG

| 计算类别 | 组件（原文） | 承担的工作 |
|---|---|---|
| 数据计算 | Spark / Flink / OLAP | 批处理与流处理统一、OLAP 支撑交互式分析、成熟稳定并承载现有全部数据负载 |
| AI 特征计算 | Ray / Daft | 分布式 Python 计算框架、支撑多模态数据处理算子、承担 OCR / ASR / 抽帧 / 向量化等任务 |
| 知识计算 | RAG | 检索增强生成、把企业知识接入模型上下文、支撑语义召回与知识问答 |

**关键点（原文）**：三类计算**共享同一套存储与调度**，Agent 无需感知底层差异。数据加工、AI 处理、知识检索可以在同一条流水线里编排——图片 OCR、语音 ASR、视频抽帧和向量化等处理，可以同结构化明细加工和知识检索连接起来，**减少 Agent 跨不同系统编排时的衔接环节**。

## 与"多模态 + 库表联合"判断的对应

原文第 1 节的判断之一是多模态数据会更多地与库表联合处理（OCR、语音识别、视觉质检不再只是孤立应用）。统一存储与统一计算正是这条判断的工程落地：

```
非结构化原件（文档/图片/音视频）          结构化数据（业务库表/指标/明细）
        │ Lance                                      │ Iceberg / Paimon
        └──────────────┬─────────────────────────────┘
              Unified Catalog（元数据/注册/权限/血缘）
                       │
        ┌──────────────┼──────────────┐
   数据计算           AI 特征计算        知识计算
   Spark/Flink/OLAP   Ray/Daft         RAG
        └──────────────┴──────────────┘
              共享存储与调度 —— Agent 无感知
```

> 上图是按原文文字与配图整理的示意，并非原文原图。

## 原文未说明

- 原文未给出统一 Catalog 的具体实现方式（是否复用既有 Catalog 服务）、Lance 的版本与选型对比
- 原文未给出统一计算后跨引擎调度、数据搬迁、资源隔离的实际开销数据
- 原文未说明"大几百家客户"中已有多少迁移到该底座

## 相关页面

- [[shufan-ai-native-data-platform]] — 总览：数帆 AI Native 改造的三条主线
- [[table-knowledge-graph]] — 建立在统一 Catalog 与血缘之上的语义层
- [[lakehouse]] — 湖仓一体的定义与演进（统一存储的结构化侧背景）
- [[iceberg]] — Iceberg 表格式（统一存储的结构化侧）
- [[paimon]] — Paimon 流式湖仓（统一存储的另一结构化载体）
- [[bosch-lance-ray-data-production]] — 博世用 Lance + Ray 组织多模态数据生产：与"Lance 承载非结构化 + Ray 做特征计算"同一技术组合的另一个视角
- [[table-format-selection]] — 表格式选型：LakeHouse 结构化数据的另一参考
- [[ontological-semantic-layer]] — 数据底座之上的语义控制面
- [[databricks-2026-summit]] — 同为"数据 + AI 统一底座"的行业方案
- [[agent-ready-multi-cloud-luxury]] — Agent-Ready 数据底座的多云视角
