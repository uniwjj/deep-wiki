---
ingested: 2026-11-04
wiki_pages: [databuddy-semql-metarag]
source_file: 拆解腾讯 DataBuddy 的 Text-to-Semantic-to-SQL 与 MetaRAG 技术实现.html
source_type: article-html
url: https://mp.weixin.qq.com/s/Dx4I4uawAHhZoXUwhcl6lw
title: 拆解腾讯 DataBuddy 的 Text-to-Semantic-to-SQL 与 MetaRAG技术实现
author: 噪声之下（微信公众号）
published: 2026-09-21 07:00
---

本文件是 sidecar，仅用于 `llm-wiki` CLI 追踪摄取状态；原始正文在同名 `.html`（可双击离线阅读）。

正文结构为 7 节：01 Text-to-SQL 缺的是生产资格（三类翻车现场）→ 02 统一语义层 Unity Semantics（四要素 / 三层架构 / 语义冷启动）→ 03 SemQL 确定性编译 → 04 MetaRAG 双路召回（F-RAG / S-RAG）与召回漏斗 → 05 覆盖度三级路由与分层记忆 → 06 指标 MCP（三个工具）→ 07 一点判断；末尾附 6 条参考资料。

该文为第三方技术拆解（非腾讯云官方发布），其中"官方材料 / 官方文档"系作者转述腾讯云材料。
离线副本剔除的内容：文中唯一的图片（文末"加好友进群"微信群二维码，属页面推广）。
