---
title: Hologres Skills
description: Hologres 多模融合一站式 AI 数据检索分析平台——从 HSAP 1.0 到 HSAP 2.0，AI Function 一键调用百炼模型、CLI 与 Skills 体系、Mem0 长记忆方案、多 Agent AI 助手，实现广告素材智能生成与投放分析
aliases: [Hologres Skills, Hologres AI Function]
tags: [big-data, ai-agent, platform, tool]
sources: [2026/05/12/04. 基于 Hologres Skills 完成广告素材智能生成与投放效果分析.pdf, 2026/05/12/DataWorks DataAgent分享录音.md, 2026/05/28/基于 Hologres Skills 完成广告素材智能生成与投放效果分析.pdf]
created: 2026-05-12
updated: 2026-05-31
---

# Hologres Skills

Hologres 定位为**多模融合的一站式 AI 数据检索分析平台**，核心理念：“一份数据、一份计算、多模分析”。演讲人：**赵红梅（梅酱）**｜Hologres 产品经理（阿里云智能集团）。

录音现场细节（2026-05-28 杭州虾聊日）：

- **开场即跑 demo**：让 OpenClaw 在后台跑整个 Skill demo——输入抠版三国动漫人物刘备和吕布两个广告素材，任务自动执行：基于素材生成新广告素材 → 生成广告视频 → 多渠道投放 → 数据智能化分析；让 demo 在后台跑着，演讲同时进行
- **HSAP 演进口语版**：2016 年商业化、HSAP 1.0 主打 OLAP+在线服务；去年开始全面接入 AI（非结构化数据、AI Function、向量/全文/混合检索），正式升级为 **HSAP 2.0**，成为一站式 AI 数据分析平台
- **AI Function 抽象**：把大模型调用抽象成 SQL 函数 — AI Embedding、AI Gen（推理）、AI Translate（翻译）等
- **方案优势金句**：①SQL 一函数调用即可完成 AI 开发，无学习成本 ②不需要数据搬迁（以前数仓在数仓、AI 数据在别处） ③数据不出库，安全可控

## 摄取说明

2026-05-31 按 [[taobao-live-data-dev-paradigm]] 的高密度 PPT PDF 摄取标准重新复核：不以已转换 TXT 为依据，而是从 `2026/05/28/基于 Hologres Skills 完成广告素材智能生成与投放效果分析.pdf` 的 PDF 正文逐页转图片后视觉检查。该 PDF 共 16 页，页面均按 DataFun/虾聊日 PPT 高密度资料处理；代码、架构图、流程图、Demo 截图、二维码页和致谢页均纳入逐页索引。截图中无法可靠辨认的小字不做臆测。

## 逐页摄取索引

| 页码 | 主题 | 已摄取信息 |
|------|------|------------|
| P1 | 封面 | 题目“基于 Hologres Skills 完成广告素材智能生成与投放效果分析”，演讲人赵红梅（梅酱），阿里云智能集团产品专家 |
| P2 | Hologres 走向 AI 的发展之路 | 从 HSAP 1.0 到 HSAP 2.0，2016/2020/2023/2024/2025 五段演进，2025 全新升级为一站式 AI 数据分析平台 |
| P3 | 多模融合的一站式 AI 数据检索分析平台 | 统一 SQL、数据查询/加工/存储三层，多模数据、Dynamic Table、AI Function、百炼模型、Object Table、数据湖与 OSS 的完整架构 |
| P4 | AI Function 一键调用百炼模型 | 控制台一键部署、SQL 调用 AI Function、支持模型清单、适用场景、方案优势 |
| P5 | 广告素材生成挑战 | 游戏行业素材生成中的创意产能、效果验证、全链路闭环三类结构性瓶颈 |
| P6 | 智能生成广告素材链路 | 基于 Hologres AI Function 的 4 步闭环：素材采集和生成、标签生成和匹配、智能生成创意素材、投放与效果分析 |
| P7 | Step 1 原始素材采集和分析 | 内外部素材进入 OSS，通过 Object Table 映射图片/视频/文档，示例 SQL 包含建表、刷新元数据、查询 |
| P8 | Step 2 动态智能标签匹配 | Dynamic Table + Qwen3-Plus LLM 增量标签生成：Prompts 表、Object Table、10 分钟 freshness、`ai_gen` 内置 LLM 调用、结构化标签输出 |
| P9 | Step 3 智能生成创意素材 | Qwen-Max 生成分镜脚本，Wan 生成视频，游戏素材表、Prompt 模板、10 秒 3-4 shots 分镜要求和 Wan 参数 |
| P10 | Step 4 素材审核及投放效果分析 | Qwen 自动审核 SQL、审核维度与 JSON 输出、广告投放实时分析大屏、复杂分析能力与自动弹性 |
| P11 | 广告创意视频生成 Demo | 箭塔守汉中游戏广告素材 Demo，Hologres 控制台部署百炼模型，模型列表含 qwen3_5_plus、qwen_image_2、wan_26_r2v_flash |
| P12 | Hologres CLI & Skills | Agent Ready 的一站式实时数仓，Hologres CLI 能力、Hologres Skills 三类能力、GitHub 开源仓库 |
| P13 | Hologres Mem0 | AI Agent 长记忆方案，写入路径和检索路径，OpenClaw 插件安装命令，示例配置含 Hologres、text-embedding-v4、qwen-plus |
| P14 | Hologres AI 助手 | 数仓从入门到精通，Holo Agent 框架、Qwen Code Agent、Holo Skills/元仓/知识库、百炼模型服务，多 Agent 场景覆盖 |
| P15 | 资料参考 | 四个二维码：基于 AI Function 的游戏广告视频生成、Hologres + Mem0 最佳实践、Hologres CLI & Skills 开源仓库、阿里云大数据智能体养虾交流群 |
| P16 | 结束页 | THANKS |

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

## PDF 逐页复核补充

### P2：Hologres 走向 AI 的发展之路

PPT 将 Hologres 的 AI 化主线表述为：从分析服务一体化（Hybrid Serving/Analytics Processing, HSAP 1.0）到分析检索一体化（Hybrid Search/Analytics Processing, HSAP 2.0），目标是打造一站式 AI 数据分析平台。

- 2016：Hologres 诞生，新型 KV 存储引擎 For Blink 状态，替换搜索 HBase；2018.10 Hologres 成立；交互式分析 For MaxCompute。
- 2020：Hologres 1.0-HSAP，阿里云正式商业化，架构论文首次提出分析服务一体化理念（HSAP），发布顶会 VLDB2020。
- 2023：Hologres 2.0-Serverless，计算组实例发布，隔离与弹性；湖仓升级加速，MaxCompute 与 OSS 原生支持半结构化 JSONB 列式存储。
- 2024：Hologres 3.0 一体化实时湖仓，湖仓一体，原生支持数据湖；Dynamic Table 近实时增量加工分析服务一体，做深资源隔离与弹性 Data+AI 一体，发布 Copilot 等能力。
- 2025 全新升级：Hologres 4.0-HSAP 2.0，分析检索一体化 HSAP 2.0；全面支持非结构化数据、AI Function、ChatBI；具备领先的向量、全文、混合检索能力；成为一站式 AI 数据分析平台。

### P3：多模融合架构展开

架构从上到下分为多模融合入口、一份计算（Data+AI）与多模数据三层，统一 SQL 横跨在线推荐、BI 报表、检索分析、RAG、向量化、内容生成六类上层场景。执行能力对应点查、OLAP 分析、全文检索、向量检索、混合检索、AI 推理。

一份计算层包含数仓分层加工（ODS、DWD、DWS、ADS，由 Dynamic Table 做增量计算）、AI Function（Rerank、Embedding、chunking、parse_document 等）、百炼模型（Embedding、Qwen-image、Wan、Qwen-vl）和 AI 节点模型（独享 GPU，Embedding、DeepSeek）。多模数据层包含内部存储（向量、文本、JSON）、数据湖（MaxCompute、Paimon、Iceberg）和 OSS（图片、PDF、PPT、视频）。右侧总结为：一份多模态数据存储、一键多模式数据计算、一键部署百炼模型、一站式 AI 数据分析。

### P4：AI Function 细节

AI Function 将大模型调用抽象成 AI 函数，例如 `ai_embed`、`ai_gen`、`ai_translate`，可在 Hologres 控制台一键部署百炼模型，并用标准 SQL 在数仓内调用。截图中模型部署页选择了 `qwen3-max`；SQL 示例创建 `questions` 表，插入“什么是人工智能”“如何提高英语口语水平”“健康饮食有哪些注意事项”等问题，并用 `ai_gen('请用 20 个字回答如下问题：' || question)` 生成回答。

适用场景在 PPT 中列为：智能搜索与推荐（商品语义检索、相似商品推荐）、实时风控与异常检测（违规内容识别、IoT 设备故障报警）、智能内容生成（营销文案生成、广告视频素材创作、语音合成）、语音处理与交互（语音转写、会议纪要生成等）。方案优势为零代码集成、实时高性能、数据零搬迁、灵活和低成本。

### P5-P6：广告素材闭环的业务问题与链路

P5 明确以游戏行业为例，将广告素材生成的结构性瓶颈拆为：

- 创意产能瓶颈，特点是“做的慢”：传统素材生成靠堆人，创意风格不一致，创意落地偏差大，沟通损耗严重，人工扩展边界成本高；流水线成本高、周期长，素材迭代速度赶不上投放节奏。
- 效果验证瓶颈，特点是“测得慢”：依赖人工经验判断素材潜力，缺乏创新筛选机制，凭感觉堵爆款，试错成本高；测试素材池小，全链路周期 10-15 天，ROI 难以提升，错过红利期。
- 全链路闭环瓶颈，特点是“转不动”：素材生产与效果数据割裂，缺乏有效的下一轮创意指导；投放优化依赖人工，全链路难以规模化、自动化迭代。

P6 给出的链路图为：OSS 中的图片、视频等营销原始物料进入 Hologres，形成物料表与 Object Table；经 `ai_gen` 生成新图片素材，再经 `ai_gen` 生成分镜脚本，调用 Wan 模型生成视频；视频进入营销平台投放，投放效果分析结果再回流形成闭环。四个步骤分别是素材采集和生成、标签生成和匹配、智能生成创意素材、投放与效果分析。

### P7-P8：Object Table 与动态智能标签

Object Table 自动将 OSS 上每个文件映射成一条记录，示例表字段可见 `object_uri`、`etag`、`file`、`metadata`，用于无缝对接 AI Function 和 Dynamic Table 增量加工。P7 的示例 SQL 除 `CREATE OBJECT TABLE` 外，还包含 `REFRESH OBJECT TABLE video_object_table;` 更新元数据，以及 `SELECT * FROM video_object_table;` 查看结果。

动态标签架构的左侧输入由 Prompts 表（用户自定义 Prompt，定义分析规则与输出格式）和 Hologres Object Table（原始素材数据，字段含 `object_uri` / `file`）组成；中间通过 Dynamic Table 增量智能计算，`LEFT JOIN object_table x prompts`，`auto_refresh_mode: full`，`freshness: 10 minutes`，调用 `ai_gen('qwen3_5_plus_3', prompt, file)`；右侧输出结构化标签，包含一级分类 `catalog_l1`、二级分类 `catalog_l2`、精准标签 `tag`，并匹配 Object Table 原始数据。

```sql
CREATE DYNAMIC TABLE video_tags
WITH (auto_refresh_mode='full', freshness='10 min') AS
SELECT ai_gen('qwen3_5_plus_3', prompt, file)::jsonb --> tag_obj->>'一级分类' / '二级分类' / '标签'
```

### P9：智能生成创意素材细节

游戏素材表示例：

| 游戏名称 | 玩法介绍 | 图片 OSS 地址 | 风格 |
|----------|----------|---------------|------|
| 原神 | 开放世界冒险 RPG 元素反应战斗系统 | `oss://ai-demo/genshin_cover.png` | 二次元 |
| 王者荣耀 | 5v5 MOBA 竞技多英雄策略对战 | `oss://ai-demo/wangzhe_cover.png` | 国风 |
| 和平精英 | 百人战术竞技真实射击体验 | `oss://ai-demo/peace_cover.png` | 写实 |
| 蛋仔派对 | 多人派对竞技休闲社交玩法 | `oss://ai-demo/danzi_cover.png` | 卡通 |

分镜 Prompt 模板面向游戏类短视频，要求按 `{{游戏名}}` 生成一段 10 秒宣传视频分镜脚本，包含 3-4 个分镜。可见分镜约束为：Shot 1（0-3s）开场，游戏 Logo + 标志性场景全景；Shot 2（3-6s）玩法展示，核心战斗/操作画面；Shot 3（6-8s）高光时刻，特效/大招/胜利瞬间；Shot 4（8-10s）结尾，Slogan + 下载引导。

Wan 生成视频参数包含：模型 `'wan'`，`prompt` 为分镜脚本内容，`reference_urls` 为游戏素材图片，`size: '1280*720'`，`duration: 10`，`shot_type: 'multi'`，`audio: false`，`watermark: true`，`output_url` 输出到 OSS，示例路径 `oss://ai-demo-dataset/xxx/output/`，参考图示例为 `oss://ai-demo-dataset/xxx/man.png`，Endpoint 为 `oss-cn-hangzhou-internal.aliyuncs.com`。

### P10：素材审核与投放效果分析

素材审核 SQL 截图展示了 `SELECT material_id, oss_path, ai_gen('qwen_image_2', json_build_object(...), NULL) AS review_result FROM creative_materials WHERE status = 'pending'` 的模式。Prompt 要求从合规性、平台规范、品牌质量等维度评估素材并返回 JSON，字段包括 `score: 0-100`、`status: "approved/rejected"`、`issues: []`。温度参数可见为 `temperature: 0.1`。

投放实时分析页面展示“广告核心指标概览”，可见指标包括 Impressions、Clicks、Spend、Orders、Sales、CTR、CVR、CPC、ACOS、ROI，并配有广告渠道成本/点击占比、广告渠道花费/点击占比、漏斗类图表。PPT 强调实时性（广告数据高并发实时写入 Hologres，写入即可见）、复杂分析（内置窗口、漏斗、留存、用户路径、触点归因等复杂计算，秒级响应）、弹性扩展（根据流量负载自动弹性扩缩容）。

### P11：广告创意视频生成 Demo

Demo 标题为“箭塔守汉中游戏广告素材的 demo”。截图展示在 Hologres 控制台的 AI 模型页部署百炼模型，实例编号可见为 `1234` 且运行正常；模型列表中可见 `qwen3_5_plus`、`qwen_image_2`、`wan_26_r2v_flash`，部署状态均为“部署成功”，模型提供方为阿里云百炼（北京），Hologres 实例 region 为华东 1（杭州）。PPT 右侧给出官网文档路径：`help.aliyun.com/zh/hologres/user-guide/generate-game-promo-video-using-ai-functions`。

### P12：Hologres CLI & Skills

P12 将 Hologres 定义为 Agent Ready 的一站式实时数仓，强调“丰富的 AI 智能体友好的工具与技能集”。Hologres CLI 内置安全防护措施，支持结构化输出，覆盖实例管理、计算组管理、数据库管理、用户授权、表/视图等元数据管理、DQL 执行与结果导出、动态表管理、备份与安全。

Hologres Skills 分为三组：智能运维（慢 Query 分析、SQL 优化）、最佳实践（Flink+Hologres 搭建实时数仓、Hologres 专家权限模型授权、RoaringBitmap 长周期 UV 计算与实时 UV 精确去重、Bit-sliced Index 行为标签计算、AI Function 广告素材智能生成、多模态数据分析与检索）、智能开发（自然语言转 DDL、智能化建表）。页脚给出开源仓库：`github.com/aliyun/hologres-ai-plugins/tree/main`。

### P13：Hologres Mem0 长记忆方案

P13 目标是解决“养虾”人的痛点：历史记忆遗忘、跨设备从 0 开始。写入路径为：用户经过对话输入进入记忆提取器，提取记忆后进入向量化引擎并写入向量。检索路径为：大语言模型通过增强 Prompt 连接上下文融合器，上下文融合器接收记忆检索器返回的相关记忆，形成增强回答。

右侧 OpenClaw 示例展示 Mem0 向量存储后端使用 Hologres，嵌入模型通过 `text-embedding-v4`（1024 维）走 DashScope API，LLM 通过 `qwen-plus` 走 DashScope，模式为 open-source。截图中的“当前记住的事情”示例包括日历提醒、日历无改动、近期日程、基本信息等。部分实例 ID 与用户信息被打码，未摄取。

### P14：Hologres AI 助手

Hologres AI 助手围绕 Hologres 实例，基于资深数仓专家沉淀的多个 Skills，提供多 Agent 协同支持数仓全流程使用。功能包含知识问答、实例诊断、SQL 调优、数仓开发等数仓场景；当前公测阶段免费，后续会按 Token 调用量收费。

架构图从下到上为：模型服务层是百炼大模型（qwen 系列）；Holo 大账号调用上层 Holo Agent 框架；框架内部以 Qwen Code Agent 为核心，连接 Holo Skills、Holo 元仓、Holo 知识库。右侧总结三点：多场景 Agent 覆盖、零代码开箱即用、内置专业 Skills。四类 Agent 分别是知识问答（售前技术方案咨询、产品规格推荐、功能使用答疑）、数仓开发（数仓搭建教程、SQL 调优建议、功能开发最佳实践）、数仓运维（实例资源诊断、慢/错 SQL 诊断、成本治理）、数据分析（自然语言访问数据、报表可视化生成、数据解读与趋势预测）。

### P15-P16：资料参考与结尾

P15 为资料参考页，包含四个二维码：最佳实践“基于 AI Function 的游戏广告视频生成”、最佳实践“Hologres + Mem0 实现 OpenClaw 长记忆增强”、Hologres CLI & Skills 开源仓库、阿里云大数据智能体养虾交流群。P16 为 THANKS 结束页。

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 产品
- [[dataworks-2026-0528-xialiaori]] — 2026-05-28 虾聊日全部分享内容汇总
- [[maxcompute-skills]] — MaxCompute 同类 Skills 体系
- [[flink-skills]] — Flink Skills 联动方案
- [[emr-skills]] — EMR Skills
- [[maxcompute-data-ai]] — MaxCompute 多模态存储参考
- [[openclaw]] — OpenClaw Agent 框架
