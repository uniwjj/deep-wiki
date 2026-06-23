---
title: 菜鸟数仓未来架构
description: 菜鸟物流数仓未来预测架构——Data Mesh 数据网格三层架构，包含交易/物流/LLM 三大数据域的 CDM 公共数据模型、WIKI 知识库与 ODS 源数据层
aliases: [菜鸟数仓未来预测, 菜鸟Data Mesh, Cainiao Data Warehouse Architecture]
tags: [big-data, architecture, ai-agent]
sources: [2026/06/23/菜鸟数仓未来预测.svg]
created: 2026-06-23
updated: 2026-06-23
---

# 菜鸟数仓未来架构

菜鸟物流数仓的未来预测架构，采用 **Data Mesh（数据网格）** 设计理念，按业务域组织数据，自下而上分为三层：源系统层、Data Mesh 层、应用层。

## 架构总览

```
Applications（BI Dashboards / AI Skills / AI Reports / System Apps）
        ↑
Data Mesh（ADM 应用数据集市 + 交易/物流/LLM 三大数据域）
        ↑
Source System（业务数据库 / 日志埋点 / 第三方API / 文件CSV / 消息队列）
```

## 应用层（Applications）

顶层的四大应用出口：

| 应用 | 说明 |
|------|------|
| **BI Dashboards** | 传统 BI 看板和大屏，面向管理层和运营 |
| **AI Skills** | OSI/WIKI 知识库驱动的 AI 技能，覆盖分析思路、可视化、鉴权 |
| **AI Reports** | 经营决策性分析报告，智能解读 + 定期推送 |
| **System Apps** | 系统级应用，如营销圈人发券、异常波动智能告警 |

应用层体现了从"人看报表"到"AI 消费数据"的演进——BI Dashboards 服务人类决策者，AI Skills 和 AI Reports 则直接由 AI Agent 驱动。

## Data Mesh 层

Data Mesh 层是架构核心，包含 ADM 应用数据集市和按业务域划分的数据域。

### ADM（应用数据集市）

四个 ADM 模块直接支撑应用层：

| ADM 模块 | 覆盖内容 |
|----------|---------|
| ADM 供应链履约效率分析 | OKR/KPI、过程指标计算、看板、预警分析 |
| ADM 财务经营损益分析 | OKR/KPI、过程指标计算、实时看板、预警分析 |
| ADM 销售转化分析 | OKR/KPI 指标计算、实时看板、预警分析 |
| ADM 公司级 ORK 指标分析 | OKR/KPI 指标计算、实时看板、预警分析 |

### 三大数据域

每个数据域内部采用统一的三层结构：CDM（公共数据模型）→ WIKI（知识库）→ ODS（源数据层）。

#### 交易数据域

覆盖交易、订单、支付、退款等核心业务流程。

- **CDM 公共数据模型**：DWD 明细事实表、DWS 轻度聚合汇总、DIM 维度表
- **WIKI**：表知识定义、概念+实体、指标层次关系、文档、知识图谱构建
- **ODS 源数据层**：数据镜像/原始数据、CDC 同步与增量捕获

#### 物流数据域

覆盖仓储、配送、路由、时效等物流全链路。

- **CDM 公共数据模型**：DWD 明细事实表、DWS 轻度聚合汇总、DIM 维度表
- **WIKI**：表知识定义、概念+实体、指标层次关系、文档、知识图谱构建
- **ODS 源数据层**：数据镜像/原始数据、CDC 同步与增量捕获

#### LLM 数据域

覆盖订阅、tokens、agents、时效等 AI 相关数据，是面向 AI 时代新增的数据域。

- **CDM 公共数据模型**：DWD 明细事实表、DWS 轻度聚合汇总、DIM 维度表
- **WIKI**：表知识定义、概念+实体、指标层次关系、文档、知识图谱构建
- **ODS 源数据层**：数据镜像/原始数据、CDC 同步与增量捕获

### 设计要点

- **统一域内结构**：三大数据域均采用 CDM → WIKI → ODS 的标准化三层，保证跨域一致性
- **WIKI 即基础设施**：每个数据域都内置 WIKI 知识库（表定义、概念实体、指标关系、知识图谱），让 AI 能"听懂"数据
- **LLM 数据域**：将 AI 使用数据（订阅、tokens、agents）提升为独立数据域，体现 AI Native 的数据架构思维

## 源系统层（Source System）

五种数据源接入方式：

| 数据源 | 技术栈 |
|--------|--------|
| 业务数据库 | MySQL / PostgreSQL / Oracle |
| 日志/埋点 | Flume / LogAgent |
| 第三方 API | HTTP / RPC 拉取 |
| 文件/CSV | SFTP / OSS 导入 |
| 消息队列 | Kafka / RocketMQ |

## 与现有实践的关系

菜鸟当前的 [[superetl|SuperETL]] 体系基于 [[dataworks-data-agent|DataWorks Data Agent]] 实现 Skills 编排 + Hooks 机制。本架构图展示的是更长周期内的数仓架构演进方向——从工具驱动的 ETL 开发走向 Data Mesh 化的数据网格，将 WIKI 知识库内嵌为每个数据域的基础设施。

## 关键洞察

- **Data Mesh 落地**：不是按技术分层（ODS→DWD→DWS→ADS），而是按业务域划分数据所有权，每个域内再分层
- **WIKI 无处不在**：每个数据域都内建 WIKI 层，表明"让 AI 理解数据"是未来数仓的核心需求
- **LLM 作为一等数据域**：将 AI 使用数据与传统业务数据并列，说明 AI 消费数据本身也产生了需要治理的数据资产

## 相关页面

- [[superetl]] — 菜鸟当前基于 DataWorks Data Agent 的 SuperETL 实践
- [[dataworks-data-agent]] — DataWorks Data Agent 产品架构
- [[taobao-live-data-dev-paradigm]] — 淘宝直播 R2C 架构（类似的 Data Mesh + CDM 实践）
- [[ontological-semantic-layer]] — 本体化语义层概念
