---
title: Apache Iceberg
description: 开放表格式标准——为大规模分析表提供 ACID 事务、schema 演进、时间旅行和跨引擎互操作能力
aliases: [Iceberg, Apache Iceberg, Iceberg 表格式]
tags: [big-data, tool, concept]
sources: [2026/05/10/lint-stub.md]
status: draft
created: 2026-06-15
updated: 2026-06-23
---

# Apache Iceberg

> 开放表格式标准，跨云 Lakehouse 的基础设施层。

## 概述

Apache Iceberg 是一种大规模分析表的开放表格式，核心能力：

- **ACID 事务**：支持并发安全读写
- **Schema 演进**：安全的列增加、重命名、类型变更
- **时间旅行**：查询历史数据快照，支持回滚和审计
- **分区演化**：分区策略随数据变化而变更，无需重写表
- **跨引擎互操作**：Spark、Flink、Trino、Hive、Dremio 等多引擎共用同一表格式

## 在 Agentic Data Cloud 中的角色

Google Cloud 通过 Iceberg REST Catalog 实现跨云 Lakehouse：
- 跨 AWS/Azure 的开放 catalog
- 双向联邦读取 Databricks Unity Catalog、Snowflake Polaris、AWS Glue Data Catalog
- 最小化出站成本

## 相关页面

- [[agentic-data-cloud]] — Google 跨云 Lakehouse 战略依赖 Iceberg 开放标准
- [[big-data/index|大数据技术]]
