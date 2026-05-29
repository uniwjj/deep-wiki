---
title: Hologres Skills
description: Hologres 多模融合一站式 AI 数据检索分析平台——从 HSAP 1.0 到 HSAP 2.0，AI Function 一键调用百炼模型、CLI 与 Skills 体系、Mem0 长记忆方案、多 Agent AI 助手，实现广告素材智能生成与投放分析
aliases: [Hologres Skills, Hologres AI Function]
tags: [big-data, ai-agent, platform, tool]
sources: [2026/05/12/04. 基于 Hologres Skills 完成广告素材智能生成与投放效果分析.pdf, 2026/05/12/DataWorks DataAgent分享录音.md, 2026/05/28/基于 Hologres Skills 完成广告素材智能生成与投放效果分析.pdf]
created: 2026-05-12
updated: 2026-05-29
---

# Hologres Skills

Hologres 定位为**多模融合的一站式 AI 数据检索分析平台**，核心理念：“一份数据、一份计算、多模分析”。演讲人：骆撷冬（阿里云智能集团产品专家）。

## Hologres 演进之路

从 2016 年诞生到 2025 年 HSAP 2.0 的四次架构跃迁：

| 版本 | 年份 | 里程碑 |
|------|------|--------|
| 诞生 | 2016 | 新型 KV 存储引擎 For Blink 状态，替换搜索 HBase |
| Hologres 1.0 HSAP | 2020 | 阿里云正式商业化，VLDB2020 发表分析服务一体化（HSAP）论文 |
| Hologres 2.0 Serverless | 2023 | 计算组实例发布，湖仓加速（MaxCompute/OSS 原生存储） |
| Hologres 3.0 实时湖仓 | 2024 | 湖仓一体 + Dynamic Table 近实时增量加工 + Copilot |
| Hologres 4.0 HSAP 2.0 | 2025 | 分析检索一体化 + 非结构化数据 + AI Function + ChatBI |

## 多模融合架构

```
统一 SQL 查询入口
├── 在线推荐 / BI 报表 / 检索分析 / RAG / 向量化 / 内容生成
├── 点查 / OLAP 分析 / 全文检索 / 向量检索 / 混合检索 / AI 推理
├── Dynamic Table 增量计算（ODS → DWD → DWS → ADS）
├── AI Function（Rerank / Embedding / chunking / parse_document / ...）
└── 百炼模型（Embedding / Qwen 系列 / DeepSeek / Wan 模型 / Qwen-VL）
```

### 多模数据存储

| 存储层 | 数据类型 |
|--------|---------|
| 内部存储 | 向量、文本、JSON、MaxCompute、Paimon、Iceberg |
| 数据湖 | 图片、PDF、PPT、视频 |
| 外部存储 | OSS（非结构化数据） |

## AI Function：一键调用百炼模型

在 Hologres 控制台通过 API_KEY 一键部署百炼模型，通过标准 SQL 调用：

```sql
-- 示例：AI Function 调用
SELECT ai_gen_image(prompt, style) FROM materials;
```

### 支持的百炼模型

| 类型 | 模型 |
|------|------|
| 文本推理 | Qwen 系列、Qwen3 系列、DeepSeek 系列、kimi |
| 多模态推理 | Qwen3-VL 系列、Qwen-VL 系列 |
| 向量化 | Text-embedding 系列、Tongyi-embedding 系列 |
| 内容生成 | Qwen-image 系列、Wan 模型 |
| 翻译 | Qwen-mt 系列 |

### 方案优势

- **零代码集成**：仅需 SQL 即可调用 AI 函数，分析师无需学 Python
- **实时高性能**：内核级函数调用，百亿级向量亚秒级响应
- **数据零搬迁**：直接在数仓中完成全流程，数据不出库
- **灵活低成本**：内置多种百炼模型一键部署，按 Token 调用按需使用

### 适用场景

智能搜索推荐、实时风控异常检测、智能内容生成（营销文案/广告视频/语音合成）、语音处理与交互。

## 智能营销内容生成场景

打通素材 → 生成 → 投放分析全链路，以游戏行业为例解决三大瓶颈：

| 瓶颈 | 问题 |
|------|------|
| 创意产能瓶颈 | 素材生成靠堆人，创意风格不统一，流水线成本高、周期长 |
| 效果验证瓶颈 | 依赖人工经验判断素材潜力，试错成本高，测试池小 |
| 全链路闭环瓶颈 | 素材生产与效果数据割裂，缺乏有效创意指导，全链路难以规模化 |

### 四步全链路闭环

**Step 1：原始素材采集和分析**

站外素材 + 内部素材（文档/图片/视频）→ OSS Object Table 自动映射，每个文件映射成一条记录，无缝对接 AI Function 和 Dynamic Table 增量加工。

```sql
CREATE OBJECT TABLE video_object_table
WITH (
  path='oss://ai-demo-datasets/unsplash-25k/part1/',
  "oss_endpoint" = 'oss-cn-beijing-internal.aliyuncs.com',
  "role_arn" = '***'
);
```

**Step 2：动态智能标签匹配**

利用 Dynamic Table 和 AI Function 自动增量分析素材，匹配多维度结构化标签体系。

**Step 3：智能生成创意素材**

基于 Qwen-Max 生成分镜脚本 → 通义万象（Wan）模型生成视频。全流程 SQL 驱动：

```sql
-- 生成分镜脚本
SELECT ai_gen('qwen-max', prompt) AS script FROM game_materials;

-- 通义万象生成视频
SELECT ai_gen('wan', json_build_object(
  'prompt', script,
  'reference_urls', material_list,
  'parameters', json_build_object('size', '1280*720', 'duration', 10)
)) FROM story_script;
```

**Step 4：素材审核与投放效果分析**

- **素材自动审核**：基于 Qwen 模型自动化分析素材，从合规性、平台规范、质量、热点、爆款等多维度打分，输出 `{score, status, issues}` JSON
- **广告投放实时分析**：高并发实时写入 Hologres，内置窗口/漏斗/留存/路径/触点归因等复杂计算，自动弹性扩缩容

## Hologres AI 助手：多 Agent 协作

围绕 Hologres 实例，基于资深数仓专家沉淀的多个 Skills，提供多 Agent 协同支撑数仓全流程：

| Agent | 职责 |
|-------|------|
| **QA Agent** | 售前技术方案咨询、产品规格推荐、功能使用答疑 |
| **Develop/Optimize Agent** | 数仓搭建教程、SQL 调优建议、功能开发最佳实践 |
| **OPS Agent** | 实例资源诊断、慢/错 SQL 诊断、成本治理 |
| **Data Analyze Agent** | 自然语言访问数据、报表可视化生成、数据解读与趋势预测 |

零代码开箱即用：自然语言对话，无需懂 SQL，无需写代码。内置多名资深数仓专家沉淀的专业 Skills，助力数仓从入门到精通。

## Hologres AI Function：一键调用百炼模型

Agent Ready 的一站式实时数仓工具链：

### Hologres CLI

内置安全防护措施，支持结构化输出：
- 实例管理、计算组管理
- 数据库管理、用户授权
- 表/视图等元数据管理
- DQL 执行与结果导出
- 动态表管理
- 备份与安全

### Hologres Skills

| 类别 | 能力 |
|------|------|
| **智能运维** | 慢 Query 分析、SQL 优化 |
| **最佳实践** | Flink+Hologres 搭建实时数仓、专家权限模型授权、RoaringBitmap 长周期 UV 计算、Bit-sliced Index 行为标签计算 |
| **智能开发** | 自然语言转 DDL、智能化建表、AI Function 广告素材生成、多模态数据分析与检索 |

## Hologres Mem0：Agent 长记忆方案

解决 Agent “历史记忆遗忘、跨设备从零开始”的痛点：

```bash
openclaw plugins install @hologres/openclaw-mem0
```

基于 Hologres 实现 [[openclaw]] 的长记忆增强，让 Agent 记住用户偏好和历史上下文。

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 产品
- [[dataworks-2026-0528-xialiaori]] — 2026-05-28 虾聊日全部分享内容汇总
- [[maxcompute-skills]] — MaxCompute 同类 Skills 体系
- [[flink-skills]] — Flink Skills 联动方案
- [[emr-skills]] — EMR Skills
- [[maxcompute-data-ai]] — MaxCompute 多模态存储参考
- [[openclaw]] — OpenClaw Agent 框架
