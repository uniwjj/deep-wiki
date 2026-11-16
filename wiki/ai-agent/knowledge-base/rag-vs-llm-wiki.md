---
title: RAG vs LLM Wiki
description: Karpathy 用 LLM Wiki 终结 RAG——将原始数据视为源代码，LLM 充当编译器，Wiki 是可执行产物。三个极简组件替代复杂 RAG 架构
aliases: [RAG对比, LLM Wiki vs RAG]
tags: [ai-agent, concept, comparison]
sources: [2026/04/07/Karpathy终结RAG草莽时代.md, 2026/07/03/三代知识检索范式-RAG-GraphRAG-LLM-Wiki.html, "2026/11/16/构建 AI 时代的知识底座：直播数据 LLM Wiki 实践/index.html"]
created: 2026-05-09
updated: 2026-11-16
---

# RAG vs LLM Wiki

## 核心对比

| 传统 RAG | LLM Wiki |
|----------|----------|
| 文档分块→向量嵌入→专用DB→相似搜索 | raw/→LLM编译→结构化Wiki→模型内生理解 |
| 每次查询"重新做菜" | 持续建立"私人厨房+半成品" |
| 架构复杂 | 三个极简组件 |
| 临时理解 | 长期结构 |

## 三个极简组件

1. **Markdown 文件夹**：知识库本体
2. **统一内部格式**：标题+摘要+标签+正文
3. **Claude Code 查询界面**：终端直接提问

> "无需数据库，无需向量嵌入，无需服务器。只需文件和一个功能强大的模型。"

## 核心理念

将原始数据视为"**源代码**"，LLM 充当"**编译器**"，最终生成的 Wiki 是"**可执行产物**"。

## 三阶段工作流

数据导入 → 编译 → 主动维护（Lint 健康检查）

## 为什么能替代 RAG

传统 RAG：分块→嵌入→检索→喂给 LLM。LLM Wiki：依赖 LLM 对结构化文本的推理能力，Markdown 文件是"真理之源"，每项声明可追溯到具体 `.md` 文件。

## 补充口径：编译时产物 vs 运行时手段

Karpathy 的叙事偏"替代"，工程落地时更常见的口径是**分工**：Wiki 是编译时产物（把材料预处理成高质量页面），RAG 是运行时手段（在查询时刻做精准召回）——frontmatter 可用于硬过滤（层级/域/血缘），正文用于语义匹配，Wiki 未覆盖的长尾回退到原始材料检索。完整检索栈 = Wiki 提供语料 + RAG 提供召回，见 [[taobao-live-data-llm-wiki]]。

## 进阶方向

通过合成数据生成与微调，将结构化知识"压缩"进模型权重——从外部知识系统迈向模型内部长期记忆。

## 企业反响

- "每个企业都有一个 raw/ 目录，从来没有人整理过它。这就是产品。"
- Claudeopedia 已产品化；Secondmate 通过 OpenClaw 管理 10 个 Agent 做"群体知识库"

## 五代方案演进

| 代 | 方案 | 新增 | 新问题 |
|----|------|------|--------|
| 1 | 传统 RAG | 起点 | 碎片化、无关联 |
| 2 | LLM Wiki | Wiki层+交叉引用+Lint | 错误传播、平滑化 |
| 3 | 认知治理 | Schema+反对者规则 | 扩展瓶颈、单人 |
| 4A | GraphRAG | 知识图谱+社区摘要 | 索引昂贵 |
| 4B | Wiki v2 | 置信度+遗忘+混合搜索 | 单人 |
| 5 | WUPHF | 私有本+归档员+Git | 写冲突、复杂度高 |

## 相关页面

- [[knowledge-retrieval-paradigms]] — 三代范式对比（RAG/GraphRAG/LLM Wiki，含选型与成本）
- [[taobao-live-data-llm-wiki]] — 工程侧口径：编译时 vs 运行时，Wiki 与 RAG 分工而非替代
- [[llm-wiki-concept]] — LLM Wiki 核心概念
- [[graphrag]] — 第二代 GraphRAG / LightRAG
- [[bookrag]] — 结构感知 RAG 方案
