---
ingested: 2026-11-12
wiki_pages: [agent-pattern-ladder, agent-multi-agent-collaboration, agent-design-paradigms]
source_file: AI Agent 五种经典设计模式：Multi-Agent、ReAct、Plan-and-Execute 一篇讲透.html
source_type: article-html
url: https://mp.weixin.qq.com/s/7yLkilSr6Y4inh4JtKXT6A
title: AI Agent 五种经典设计模式：Multi-Agent、ReAct、Plan-and-Execute 一篇讲透
author: 智能体AI
published: 2026-11-12 08:00
---

本文件是 sidecar，仅用于 `llm-wiki` CLI 追踪摄取状态；原始正文（含 6 幅正文配图）在同名 `.html` 文件中，双击可在浏览器阅读。

正文结构为 7 节：01 RAG（架构两段 + 适用边界）→ 02 单智能体 Agent（Memory/Planning/Tools + 执行示例 + 单 Agent 的问题）→ 03 Multi-Agent RAG（Router/Retrieval/Aggregation/LLM + 「不一定比单 Agent 更好」）→ 04 Plan-and-Execute（Planner/Executor + Observer/Reflection + 踩坑）→ 05 ReAct（Reasoning/Action/Observation + 优势与限制）→ 06 五种模式选型对照与组合方式 → 07 企业级还需 Runtime 与 Harness（Runtime / Harness / Memory / MCP-Tools / Evaluation / Security&Governance 六件套）。

配图复核（均已看过实际画面）：

- 图 1（PNG 1080×720）：五种模式总览表，含各模式的架构流程图与典型场景标签，另标注「Runtime + Harness 才能落地企业级 Agent」。
- 图 2（GIF 450×653）：RAG 检索问答流水线八步——用户提问 → Query 理解 → 问题向量化 → 向量数据库检索（Top-K）→ 召回相关文档 → 重排序与上下文增强（Prompt = Query + Chunks）→ LLM 生成答案 → 返回结果（附引用溯源）。
- 图 3（GIF 540×795）：用户提问 → Query 理解 → AI Agent → Memory / Planning / Tools 三路并行 → 外部工具（API / 搜索 / 数据库）→ 执行结果回传 → Agent 汇总 → 输出答案。
- 图 4（GIF 800×740）：Router Agent 并行检索——Query 理解 → Router 并行分发给 Agent 1..N（各连数据源 1..N）→ Aggregation Agent 多路结果去重、融合排序 → LLM 生成 → 最终答案。
- 图 5（GIF 465×720）：任务执行流程——用户任务 → Task Understanding → Planner（规划路径）→ 任务拆分 → 执行循环（Step 1..N，Observer·Reflection 回环：出口=输出、回流=调整、虚线=反馈重试）→ 整合结果 → 最终输出。
- 图 6（GIF 800×600）：ReAct 三环节循环图——Reasoning → Action → Observation → 回到 Reasoning。

原页面末尾另有一张 272×272 二维码图，属页面引流内容，未纳入离线副本与 wiki。
