---
title: OpenClaw Agentic Search & Memory
description: OpenClaw 两大认知引擎——Agentic Search 自主驱动的研究闭环（理解→规划→执行→反馈）与 Agentic Memory 工业级记忆系统（Extract→Embed→Store→Retrieve→Fuse），双引擎协同形成越用越强的研究飞轮，使 OpenClaw 从搜索工具跃迁为企业认知引擎
aliases: [Agentic Search, Agentic Memory, OpenClaw Search, OpenClaw Memory]
tags: [ai-agent, big-data, concept]
sources: [2026/05/28/OpenClaw_AgenticSearch_Memory.pdf]
created: 2026-05-29
updated: 2026-06-15
---

# OpenClaw Agentic Search & Memory

阿里云搜索团队在 2026-05-28 虾聊日上分享了 OpenClaw 的两大核心认知引擎：Agentic Search（自主驱动的研究闭环）与 Agentic Memory（让 Agent 越用越聪明）。

- 议题主标题：**Agentic Search + Memory，OpenClaw 企业研究更智能**
- 议题副标题：**从搜索工具到企业认知引擎**
- 演讲人：**陈梦成**｜阿里云搜索团队（阿里云智能集团）
- 议题脉络（4 章）：①从"找到文档"到"得出结论"——搜索范式的代际跃迁、困境与解法 → ②Agentic Search：自主驱动的研究闭环（Agent 驱动的"理解—规划—执行—反馈"研究闭环）→ ③Agentic Memory：让 Agent 越用越聪明（工业级记忆系统的工作原理与 OpenClaw 的双层落地）→ ④搜索越深、记忆越厚、研究越精（从研究飞轮到全栈协作）

录音补充（2026-05-28 杭州虾聊日）：

- **核心矛盾原话**："信息获取已经达到机器速度，但信息消化仍然是人工速度"
- **两大引擎职责对照**：Agentic Search 解决研究质量问题；Agentic Memory 解决知识复用问题——首先保证这一次研究 OK 让客户满意，再通过把每次研究沉淀下来达到知识服用效果
- **三个 demo 现场**：①4 个中考数学试卷文件（PDF/Docx）混合输入，3 分钟内完成解析+问答 ②169 页 NVIDIA 2023 年度报告，1 分多钟定位到任意一页表格的高管薪酬数据并精准解析 ③Rick and Morty 第一季全季视频上传知识库，按问题（如"为什么搭档"）图文并茂召回，结合字幕信息和关键帧——奥运会相关多模态视频检测能力即基于此
- **Self-evolution 出处**：现场原话——"做一个自进化的这么一套机制……最近比较火的那个 Hermes Agent 的核心输出论点……让它沉淀的记忆机制越用越聪明，越用越贴近一个人的真实感受"

## 摄取说明

2026-05-31 按 CLAUDE.md 多模态资料首次摄取规则，对 `2026/05/28/OpenClaw_AgenticSearch_Memory.pdf`（共 24 页）使用 `pdftoppm -r 150 -png` 转出页面图片逐页视觉复核，不再以派生 TXT/OCR 为唯一可信来源。本次复核修正：演讲人（陈梦成）、4 章议题脉络、Agentic Search 5 项能力（补"标准交付模式"）、两引擎角色对照、双层记忆体系架构图（User Interaction Layer + Agentic Memory Layer，含 Extractor/Embedding Engine/ElasticSearch VectorStore 与 Generate Vectors for Facts and Skills 双向回路）、AgenticMemory 三模式（In-session / Cross-session / Self-evolution）、5 阶段价值跃迁时间线（搜索工具→Agentic Search→Agentic Memory→协同飞轮→认知引擎）。

## 逐页摄取索引

| 页码 | 主题 | 已摄取信息 |
|------|------|------------|
| P1 | 封面 | Agentic Search + Memory，OpenClaw 企业研究更智能；副标题"从搜索工具到企业认知引擎"；陈梦成｜阿里云搜索团队；阿里云智能集团 logo |
| P2 | 目录 | 四部分：①从"找到文档"到"得出结论"——搜索范式的代际跃迁，困境与解法 ②Agentic Search：自主驱动的研究闭环——Agent 驱动的"理解—规划—执行—反馈"研究闭环 ③Agentic Memory：让 Agent 越用越聪明——工业级记忆系统的工作原理与 OpenClaw 的双层落地 ④搜索越深、记忆越厚、研究越精——从研究飞轮到全栈协作 |
| P3 | 章节页 | 01 / 从"找到文档"到"得出结论"；副标题"搜索范式的代际跃迁——困境与解法" |
| P4 | 三个问题正在消耗生产力 | 顶部建筑/园区配图。**搜索效率低**：关键词匹配 + 链接列表的被动响应，无法满足"得出结论 / 严出交付物"。**任务难规划**：复杂研究包含相互依赖的子问题，人工拆解、追踪、交叉验证耗费大量精力。**知识不积累**：LLM 无状态，每次会话从零开始；用户偏好、历史结论都随上下文截断而消失。底部小字："效率瓶颈不是'信息不够'，而是'信息泛滥下的认知负荷过载'" |
| P5 | 企业研究需要"主动推理"，而不是"被动响应" | 左圆"传统链路"：输入关键词→文档列表→人工阅读→人工综合；右圆"真实链路"：理解需求→拆解子问题→多源获取→交叉分析→结构化输出。底部"核心矛盾"标注：信息获取是机器速度，信息消化依然是人工速度 |
| P6 | OpenClaw：用 Agentic 范式重新定义企业研究 | 底部时间线串起 5 项核心能力：①深度意图理解（拆解复杂研究需求，识别隐含约束与优先级）②多步推理与动态规划（构建 DAG 任务树，处理相互依赖的子问题）③多工具协同（联网搜索、爬虫抓取、企业知识库、代码执行智能切换）④多模态检索（PDF、图表、视频等多格式内容感知与整合）⑤标准交付模式（Skills 透出 / 开源 ACP 协议透出） |
| P7 | 两大引擎的角色定位 | 中央 OpenClaw 圆，左侧 **Agentic Search 注重"研究质量"**：把静态检索升级为 Agent 自主规划的研究闭环——联网搜索+爬虫+BrowserUse+多模态知识库；右侧 **Agentic Memory 解决"知识复用"**：跨会话的持久化记忆与技能库——LLM 驱动事实提取 + 向量检索 + 冲突判别 |
| P8 | 两大引擎与 OpenClaw 的集成 | 左截图 Agentic Search：执行计划、深度分析、表格类目对比与样例返回；右截图 Agentic Memory：调用 Data Agent 分析 game_data 数据集（1995-2016 风云游戏数据），分析结果列表（Diablo II、Call of Duty Modern Warfare 2、FIFA 16、NBA 2K14、Tiger Woods PGA Tour 2005、Call of Duty Modern Warfare 2 等），核心启示总结 |
| P9 | 章节页 | 02 / Agentic Search：自主驱动的研究闭环；副标题"Agent 驱动的'理解—规划—执行—反馈'研究闭环" |
| P10 | 理解→规划→执行→反馈：完整的研究闭环 | 四节点环形图。**理解（深度意图理解）**：拆解隐式约束（领域/粒度/时效/受众）、模糊需求自动触发 HITL 反问；把"用户一句话"转译为可执行的研究目标。**规划（多步推理与动态规划）**：目标拆为依赖明确的子任务 DAG，无依赖节点天然并行；执行结果反向触发计划重排——不是一次性规划，而是边做边推理的动态闭环。**执行（多工具协同）**：在联网搜索 / 企业知识库 / 浏览器代理 / IPython 沙箱 / DataAgent 之间智能切换，多工具并行，同一子任务可多源交叉验证；每个子问题用最合适的工具，而不是塞给一个万能工具。**反馈（多模态感知）**：对返回的多模态结果（PDF 章节、图表数值、视频片段、表格数据）做结构化感知，提炼有效信息回写上下文，决定继续/修正/终止——不止"再调一次工具"，而是"看懂了、判断了、再行动" |
| P11 | 工程架构：三层协作 + 八项生产级能力 | 左侧 **三层架构**：服务编排层 AgentManager（橙色顶层）、执行引擎层 AgentScope（橙色中层）、运行环境层 Sandbox（橙色底层）。右侧 **八项生产级能力**（4×2 矩阵）：开源 ACP 协议 / OpenTelemetry 全链路可视化 / 断点重连与级联中断 / 数据与内容安全 / 多级并发 / 降级与限流机制 / 生产级 HITL / 无 ak 鉴权机制 |
| P12 | 多模态信息处理：PDF/Image/Video 等多模态数据处理 | Agentic Search 入口截图："你的智能助手，随时待命；探索、编码、分析，让 AI 成为你的得力伙伴"。底部上传 4 个文件：北京市中考数学试卷.pptx / 河北省中考数学试卷.docx / 河南省中考数学试卷.pptx / 上海市中考数学试卷.pdf；输入框示例："解释认证是如何工作的"；底部"内容由 AI 生成，仅供参考"，含极速模式与 AI 助理浮窗 |
| P13 | 多模态信息处理：PDF 长文件问答 | Agentic Search 入口截图，上传一个 `2023-NVIDIA-Annual-Re...` 文件；输入框提示"生成 API 文档"；体现支持长文件问答 |
| P14 | 多模态信息处理（结果展示） | 上下两张产品截图：上方"根据上述文档回答以下问题：1. 统计各个省市中考数学试卷的题型分布，并综合分析下试卷整体难度 2"，附有四个上传文件，模型生成 self-sentence-and-documents 与 server_process_pdf_analyze；下方表格"各省市中考数学试卷题型与难度统计"——北京市（2021 年中考数学试卷）含选择题 1-8、填空题 9-16、计算题 17-24、解答题 25-29 等条目。右侧三项要点：支持多种数据格式输入（pdf、doc、docx、ppt、video）；文件解析服务性能好、精度高、支持大文件解析；文件内的数据定位精准，报告回答质量高 |
| P15 | 多模态知识库检索：Text/Image/Video 等多模态知识检索 | Agentic Search 入口截图，输入框示例"在整个代码库中添加日志"；体现多模态知识检索入口 |
| P16 | 多模态知识库检索（结果展示） | 上下两张产品截图：上方"知识库中关于 rick and morty 的全部视频中（列出全部相关图集），每一集 Rick 装格的分别"，输出引用图谱与多个段落；下方人物表格（Morty/Summer/无相关视频集 等）+ 精彩图集图（Morty 卡通画面）。右侧要点：支持多种模态知识的索引与召回（text、image、video）；从离线到在线的全链路工程方案；视频数据 -> 可精准定位字幕信息与关键帧 |
| P17 | 多路召回与合并：知识库/用户文件/数据库/公域信息 | 上下两张截图：上方调用 dataagent2/Best_data 数据集 game_data 表，范围 1995-2016 风云岁月；下方表格 1995-2016 年"专业难分、用户偏爱：游戏深度分析报告"，列出多款游戏（Tiger Woods PGA Tour 2005、Call of Duty Modern Warfare 3 (2011)、Call of Duty Modern Warfare 2 (2009)、Diablo II、NBA 2K14、FIFA 16、Call of Duty Modern Warfare 2 (2017) 等），含 Activity 列。右侧要点：支持多种数据源信息召回与处理；私域数据：知识库/用户文件/数据库；公域数据：websearch/crawler/browserUse；多源信息的提取、整合与加载 |
| P18 | 章节页 | 03 / Agentic Memory：让 Agent 越用越聪明；副标题"工业级记忆系统的工作原理与 OpenClaw 的双层落地" |
| P19 | 无记忆 vs 有记忆：Agentic Memory 工作原理 | **无记忆的代价**：用户每次重复背景 / 历史结论无法关联新任务 / 经验无法沉淀为复用资产。**生产级记忆**：通过 Extract → Embed → Store → Retrieve → Fuse 五步核心流程，把零散对话沉淀为高质量、可检索、会自更新的认知资产。**Extract（提取）**：Extractor 组件从对话中抽取关键事实，LLM 驱动识别实体、偏好、结论等高价值信息。**Embed（向量化）**：Embedding Engine 把提取结果统一生成向量表示，同一套 Embedding 兼顾事实记忆与技能库。**Store（存储）**：ElasticSearch VectorStore 持久化向量与原始文本，支持增量写入 + LLM 冲突判别，避免矛盾事实并存。**Retrieve（检索）**：用户 query 经向量化后，在 ElasticSearch 中执行"BM25 + KNN"的多路融合召回（Recall Memory & Skill & Kb）。**Fuse（融合）**：召回结果经可选 Reranker 排序后注入 Agent 上下文，与当前任务信息融合为增强 Prompt |
| P20 | OpenClaw 双层记忆体系：从短期到长期的认知架构 | 标题图 **OpenClaw + Agentic Memory Architecture（Long-term Memory Enhancement Solution）**。两大层级：**User Interaction Layer**——OpenClaw Client（Personal AI Assistant Platform）。**Agentic Memory Layer - Agentic Memory**——Core Flow: Extract → Embed → Store → Retrieve → Fuse；左 Extractor（Key Info Extraction）→ 中 Embedding Engine（Vector Generation，接收 User Input，对外输出 Generate Vectors for Facts and Skills 双向）→ 右 ElasticSearch（VectorStore，回路 Recall Memory & Skill） |
| P21 | AgenticMemory：跨会话 / 跨设备 / 自进化 | 左侧 OpenClaw Client 截图，右侧蓝色 Agentic Memory 卡片副标题"突破记忆边界，实现智能进化"，含三种模式：**In-session**（会话内上下文记忆，支持复杂多轮交互、长链路任务的连续推理）、**Cross-session**（跨会话记忆，支持基于本地内存、云端、长期等多档存储）、**Self-evolution**（自我进化，基于使用频率与价值评估等机制持续优化记忆质量）。底部要点：支持跨会话，突破上下文限制；支持跨设备，突破 local memory 对设备的依赖；支持自进化，越用越聪明 |
| P22 | 章节页 | 04 / 搜索越深、记忆越厚、研究越精；副标题"从研究飞轮到全栈协作" |
| P23 | Search × Memory：越用越强的研究飞轮 | 副标题"随时间自我增强——每次研究都在抬高下一次的起点"。两圆从左"研究飞轮"指向右"战略资产"。底部三层：**飞轮机制**——研究任务 → 自主规划执行 → 深度洞察 → 写入 Memory → 下次自动召回 → 起点更高 → 积累更多高质量洞察。**阶段提升**——初期（节省上下文重建）；中期（一致性与深度持续提升）；长期（形成企业专有的认知资产）。**战略价值**——每一次使用都在强化 AI 的专业背景，知识资产随业务积累，难以被竞争者复制 |
| P24 | OpenClaw 价值总结：研究范式升级 | 副标题"让每一次搜索都更有价值！"。底部时间线 5 阶段（搜索工具 → Agentic Search → Agentic Memory → 协同飞轮 → 认知引擎），每段对应一项升级：**被动→主动**（关键词检索升级为 Agent 自主规划研究闭环）；**无状态→有记忆**（跨会话持久化记忆，越用越聪明）；**独立积累→双向增强**（每次新研究自动召回历史洞察作为起点，研究结论又反哺记忆库更新）；**个人助手→团队资产**（飞轮效应叠加业务增长，形成难以被复制的战略壁垒）；**自进化的认知引擎**（企业专有的、持续生长的、越用越聪明的智能研究基础设施） |

## 搜索范式代际跃迁

### 三个核心问题

| 问题 | 表现 |
|------|------|
| **搜索效率低** | 关键词匹配 + 链接列表的被动响应，无法满足"得出结论 / 产出交付物" |
| **任务难规划** | 复杂研究包含相互依赖的子问题，人工拆解、追踪、交叉验证耗费大量精力 |
| **知识不积累** | LLM 无状态，每次会话从零开始；用户偏好、历史结论随上下文截断而消失 |

> 效率瓶颈不是"信息不够"，而是"信息泛滥下的认知负荷过载"。

### 传统链路 vs 真实链路

| 传统链路 | 真实链路 |
|---------|---------|
| 输入关键词 → 文档列表 → 人工阅读 → 人工综合 | 理解需求 → 拆解子问题 → 多源获取 → 交叉分析 → 结构化输出 |

核心矛盾：信息获取是机器速度，信息消化依然是人工速度。

## OpenClaw Agentic 范式：5 项核心能力

P6 时间线列出 OpenClaw 用 Agentic 范式重新定义企业研究的 5 项能力：

1. **深度意图理解** — 拆解复杂研究需求，识别隐含约束与优先级
2. **多步推理与动态规划** — 构建 DAG 任务树，处理相互依赖的子问题
3. **多工具协同** — 联网搜索、爬虫抓取、企业知识库、代码执行智能切换
4. **多模态检索** — PDF、图表、视频等多格式内容感知与整合
5. **标准交付模式** — Skills 透出 / 开源 ACP 协议透出

## 两大引擎的角色定位

| 维度 | Agentic Search | Agentic Memory |
|------|---------------|----------------|
| 职责 | 注重"研究质量" | 解决"知识复用" |
| 实现 | 把静态检索升级为 Agent 自主规划的研究闭环 | 跨会话的持久化记忆与技能库 |
| 技术栈 | 联网搜索 + 爬虫 + BrowserUse + 多模态知识库 | LLM 驱动事实提取 + 向量检索 + 冲突判别 |

## Agentic Search：自主驱动的研究闭环

**理解 → 规划 → 执行 → 反馈** 的四阶段完整研究闭环：

### 理解（深度意图理解）

拆解复杂研究需求，识别隐式约束（领域/粒度/时效/受众），模糊需求自动触发 HITL 反问，把"用户一句话"转译为可执行的研究目标。

### 规划（多步推理与动态规划）

目标拆为依赖明确的子任务 DAG，无依赖节点天然并行；执行结果反向触发计划重排——不是一次性规划，而是边做边推理的动态闭环。

### 执行（多工具协同）

在联网搜索 / 企业知识库 / 浏览器代理 / IPython 沙箱 / DataAgent 之间智能切换，多工具并行，同一子任务可多源交叉验证；每个子问题用最合适的工具，而不是塞给一个万能工具。

### 反馈（多模态感知）

对返回的多模态结果（PDF 章节、图表数值、视频片段、表格数据）做结构化感知，提炼有效信息回写上下文，决定继续/修正/终止——不止"再调一次工具"，而是"看懂了、判断了、再行动"。

### 工程架构：三层协作 + 八项生产级能力

| 架构层 | 组件 | 职责 |
|--------|------|------|
| 服务编排层 | AgentManager | 任务调度与编排 |
| 执行引擎层 | AgentScope | 多 Agent 执行引擎 |
| 运行环境层 | Sandbox | 安全隔离执行 |

**八项生产级能力**：开源 ACP 协议 / OpenTelemetry 全链路可视化 / 断点重连与级联中断 / 数据与内容安全 / 多级并发 / 降级与限流机制 / 生产级 HITL / 无 ak 鉴权机制

### 多模态信息处理（P12-P14）

- 支持多种数据格式输入（pdf、doc、docx、ppt、video）
- 文件解析服务性能好、精度高、支持大文件解析
- 文件内的数据定位精准，报告回答质量高（演示场景：四省市中考数学试卷题型与难度统计、NVIDIA Annual Report 长 PDF 问答）

### 多模态知识库检索（P15-P16）

- 支持多种模态知识的索引与召回（text、image、video）
- 从离线到在线的全链路工程方案
- 视频数据 → 可精准定位字幕信息与关键帧（演示场景：Rick and Morty 视频集中按角色定位关键帧）

### 多路召回与合并（P17）

- **私域数据**：知识库 / 用户文件 / 数据库
- **公域数据**：websearch / crawler / browserUse
- 多源信息的提取、整合与加载（演示场景：1995-2016 风云岁月游戏深度分析）

## Agentic Memory：让 Agent 越用越聪明

### 无记忆 vs 有记忆

| 无记忆的代价 | 生产级记忆 |
|-------------|-----------|
| 用户每次重复背景 | 零散对话沉淀为高质量、可检索、会自更新的认知资产 |
| 历史结论无法关联新任务 | 跨会话、跨设备、自进化 |
| 经验无法沉淀为复用资产 | 每次使用都在强化 AI 的专业背景 |

### 五步核心流程

```
Extract → Embed → Store → Retrieve → Fuse
```

| 阶段 | 说明 |
|------|------|
| **Extract（提取）** | Extractor 组件从对话中抽取关键事实，LLM 驱动识别实体、偏好、结论等高价值信息 |
| **Embed（向量化）** | Embedding Engine 统一生成向量表示，同一套 Embedding 兼顾事实记忆与技能库 |
| **Store（存储）** | ElasticSearch VectorStore 持久化向量与原始文本，支持增量写入 + LLM 冲突判别，避免矛盾事实并存 |
| **Retrieve（检索）** | 用户 query 经向量化后，在 ES 中执行"BM25 + KNN"多路融合召回（Recall Memory & Skill & Kb） |
| **Fuse（融合）** | 召回结果经可选 Reranker 排序后注入 Agent 上下文，与当前任务信息融合为增强 Prompt |

### OpenClaw 双层记忆体系架构

P20 给出官方架构图 **OpenClaw + Agentic Memory Architecture（Long-term Memory Enhancement Solution）**：

```
┌─────────── User Interaction Layer ───────────┐
│      OpenClaw Client                          │
│      Personal AI Assistant Platform           │
└───────────────────────────────────────────────┘
                    ↓ User Input
┌─────────── Agentic Memory Layer - Agentic Memory ────────────┐
│  Core Flow: Extract → Embed → Store → Retrieve → Fuse        │
│                                                               │
│  ┌──────────┐   Extract    ┌─────────────────┐               │
│  │ Extractor│ ───Key────► │ Embedding Engine│               │
│  │ Key Info │   Info       │ Vector Generation│              │
│  │Extraction│               └────────┬────────┘               │
│  └──────────┘                        │                        │
│                                       │ Generate Vectors for  │
│                                       │ Facts and Skills      │
│                                       ↓                        │
│                              ┌────────────────┐                │
│                              │ ElasticSearch  │                │
│                              │   VectorStore  │← Recall        │
│                              └────────────────┘  Memory&Skill  │
└──────────────────────────────────────────────────────────────┘
```

### AgenticMemory 三种模式（P21）

| 模式 | 能力 |
|------|------|
| **In-session** | 会话内上下文记忆，支持复杂多轮交互、长链路任务的连续推理 |
| **Cross-session** | 跨会话记忆，支持本地内存 / 云端 / 长期等多档存储；突破上下文限制；突破 local memory 对设备的依赖 |
| **Self-evolution** | 自我进化，基于使用频率与价值评估等机制持续优化记忆质量；越用越聪明 |

## Search × Memory：研究飞轮

### 飞轮机制

```
研究任务 → 自主规划执行 → 深度洞察 → 写入 Memory
    → 下次自动召回 → 起点更高 → 积累更多高质量洞察
```

### 三阶段提升

| 阶段 | 效果 |
|------|------|
| 初期 | 节省上下文重建 |
| 中期 | 一致性与深度持续提升 |
| 长期 | 形成企业专有的认知资产 |

### 战略价值

每一次使用都在强化 AI 的专业背景，知识资产随业务积累，难以被竞争者复制。

## OpenClaw 价值总结：5 阶段研究范式升级

P24 用一条时间线串起从搜索工具到认知引擎的 5 阶段跃迁：

```
搜索工具 → Agentic Search → Agentic Memory → 协同飞轮 → 认知引擎
```

| 跃迁 | 内涵 |
|------|------|
| **被动 → 主动** | 关键词检索升级为 Agent 自主规划研究闭环 |
| **无状态 → 有记忆** | 跨会话持久化记忆，越用越聪明 |
| **独立积累 → 双向增强** | 每次新研究自动召回历史洞察作为起点，研究结论又反哺记忆库更新 |
| **个人助手 → 团队资产** | 飞轮效应叠加业务增长，形成难以被复制的战略壁垒 |
| **自进化的认知引擎** | 企业专有的、持续生长的、越用越聪明的智能研究基础设施 |

最终目标：让每一次搜索都更有价值。

## 相关页面

- [[loop-engineering]] — Loop Engineering 概念页，含与 OpenClaw 的六组件逐项对照
- [[openclaw]] — OpenClaw 数字员工网关架构
- [[dataworks-data-agent]] — DataWorks Data Agent
- [[dataworks-2026-0528-xialiaori]] — 2026-05-28 虾聊日全部分享内容
- [[google-context-engineering]] — Google 上下文工程白皮书（记忆系统对照）
- [[agent-memory-system]] — Agent 长短期记忆系统概念
- [[agent-mcp-protocol]] — MCP/ACP 等 Agent 协议
