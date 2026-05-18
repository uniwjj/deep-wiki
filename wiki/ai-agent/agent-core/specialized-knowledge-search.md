---
title: 专用知识搜索
description: 利用企业已有数据资产构建语义搜索索引，为 Data Agent 提供高质量资产发现能力的搜索技术，在 Databricks Genie 中将表搜索性能提升 40%
aliases: [Specialized Knowledge Search, 专用知识搜索, 专用语义搜索]
tags: [ai-agent, big-data, concept]
sources: [2026/05/18/Pushing the Frontier for Data Agents with Genie.html]
created: 2026-05-18
updated: 2026-05-18
---

# 专用知识搜索

Specialized Knowledge Search 是 [[databricks-genie|Databricks Genie]] 的核心技术突破之一。它的核心思想是：**利用企业已有的数据资产，提取丰富的语义上下文，构建专用搜索索引**，而不是依赖通用关键词搜索或通用 embedding。

## 工作原理

Genie 从以下企业资产中提取语义上下文：

- workspace 表（schema、字段名、注释、历史查询）
- notebook（代码逻辑、markdown 说明）
- dashboard（指标定义、图表标题、数据源）
- 文档（数据字典、业务定义、口径说明）
- 文件（临时报表、Excel、历史分析）

然后使用**多个搜索索引并行工作**，结合丰富的元数据信号，高效发现与用户提问最相关的资产。

## 为什么通用搜索不够

企业数据资产的特点导致常规搜索方法失效：

| 特性 | 问题 |
|------|------|
| **命名不一致** | 用户问"收入"，表名是 `rev_recognition_fact`，dashboard 写 `ARR`，文档写"确认收入" |
| **结构不统一** | 表、dashboard、文档、notebook 混合存在，无统一索引 |
| **质量参差不齐** | 同一业务概念可能对应多个含义完全不同的资产 |
| **规模巨大** | 企业级客户拥有百万级资产 |

## 性能数据

在 Databricks 的表发现 benchmark 上，引入专用知识搜索后，**表搜索性能提升达 40%**。这是 Genie 整体准确率从 32% 跃升至 90%+ 的基础。

## 与通用 RAG 的关系

专用知识搜索可以理解为"企业语义层的 RAG"，但它与通用 RAG 的关键区别在于：

- 不是简单把文档切片灌入向量库
- 需要理解企业资产的**语义关系**（表与表的关联、字段的业务含义、口径的权威性）
- 需要结合**元数据信号**（更新时间、使用频率、来源权威性）进行排序

这与 [[dataagent-semantic-layer|DataAgent 语义层]] 的概念一致——数据字典、术语映射、业务上下文不光是知识，更是搜索基础设施。

## 相关页面

- [[databricks-genie]] — 该技术的应用产品
- [[parallel-thinking]] — Genie 的另一技术突破
- [[multi-llm-design]] — Genie 的 Multi-LLM 架构
- [[code-agent-vs-data-agent]] — 数据发现的规模挑战
- [[dataagent-semantic-layer]] — 语义层作为基础设施
- [[rag-vs-llm-wiki]] — 通用 RAG 的局限性
