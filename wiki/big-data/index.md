---
title: 大数据技术
description: 大数据技术领域索引——数据湖、湖仓一体、流批一体、表格式、流计算引擎、实时数仓
aliases: [big-data, 大数据]
tags: [big-data, meta, summary]
sources: [2026/05/10/lint-stub.md]
created: 2026-05-10
updated: 2026-07-01
---

# 大数据技术

大数据技术栈——存储格式、计算引擎、架构范式。聚焦纯技术本身，Data Agent 与 AI 数据平台相关内容见 [[ai-agent/data-agent/index|Data Agent 与 AI 数据平台]]。

## 架构范式

- [[ltap-architecture]] — Databricks LTAP 架构：存储层统一 OLTP/OLAP 的"第三条路"

## 表格式与数据湖

- [[iceberg]] — Apache Iceberg 开放表格式标准（待扩充）

## 平台与产品

- [[maxcompute-data-ai]] — MaxCompute Data+AI 演进

## 待建设（学习规划中）

以下主题为近期学习规划，页面随文章摄入逐步建立：

- 湖仓一体 / 流批一体 / 实时数仓 — 架构范式总论
- Apache Flink — 流处理引擎核心（窗口/状态/时间/Checkpoint）
- CDC (Change Data Capture) — 变更捕获与 Flink CDC
- Apache Paimon — 流批一体表格式
- Apache Gravitino — 统一元数据治理
- Iceberg vs Paimon vs Hudi — 表格式选型对比
- 实时计算落地实践

## 相关页面

- [[ai-agent/data-agent/index]] — Data Agent 与 AI 数据平台（原 big-data/ 下 Data Agent 内容已迁此）
- [[homepage]]
