---
ingested: 2026-11-03
wiki_pages: [super-agent, super-agent-dual-layer-architecture, agent-event-driven-triggering, inline-and-spill, async-react-loop, speculative-execution-agent, agent-compute-state-separation, agent-watchdog]
source_file: Super-agent 设计探索：迈向更自主的 Agent 系统.html
source_type: article-html
url: https://mp.weixin.qq.com/s/zoS7GlReWPhfCjENGxXCdg
title: Super-agent 设计探索：迈向更自主的 Agent 系统
author: DeepSeek 产品部（DeepSeek Harness 团队，Yujia Qiao 执笔）
published: 2026-10-31 20:29
---

本文件是 sidecar，仅用于 `llm-wiki` CLI 追踪摄取状态；原始正文（含 14 幅正文配图）在同名 `.html` 文件中，双击可在浏览器阅读。

正文结构为 9 节：1. Super-agent（定义与四个特征）→ 2. 双层架构（Router/Worker）→ 3. 事件驱动（过滤/路由/上下文/投机执行）→ 4. 面向人的交互（全双工/多信道）→ 5. 计算、状态与环境的分离 → 6. Watchdog → 7. 初步实验（消融 + Router 批处理）→ 8. 开放问题（扩展/A2A/安全）→ 9. 结语。

原页面末尾另有一张 DeepSeek 招聘海报（第 15 幅图），属页面广告，未纳入离线副本与 wiki。
