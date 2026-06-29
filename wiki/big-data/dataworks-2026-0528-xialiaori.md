---
title: 阿里 DataWorks 2026-05-28 虾聊日
description: 2026年5月28日杭州虾聊日分享内容汇总——主题"以 Agent 之力赋能 OPC 数据经营新范式"，开场王子军（I have a demo）讲 OPC 引主机制，DataWorks Data Agent 正式发布，以及 MaxCompute、Hologres、Flink、EMR、OpenClaw、淘宝直播七场分享
aliases: [虾聊日 2026-05-28, 2026-05-28 虾聊日, DataWorks 虾聊日]
tags: [big-data, ai-agent, summary]
sources: [2026/05/28/DataWorks Data Agent：新一代数据智能体，企业大数据的数字员工.pdf, 2026/05/28/MaxCompute × Agent：从多模态存储到混合计算，突破数字员工的算力边界 20260528.pdf, 2026/05/28/基于 Hologres Skills 完成广告素材智能生成与投放效果分析.pdf, 2026/05/28/Flink Skills—重塑实时计算开发与运维体验.pdf, 2026/05/28/EMR Skills：从 Lakehouse 到 Agentic Lakehouse.pdf, 2026/05/28/OpenClaw_AgenticSearch_Memory.pdf, 2026/05/28/淘宝直播：AI 驱动的数据研发范式升级.pdf, 2026/05/28/DataWorks虾聊0528录音-修正版.md]
created: 2026-05-29
updated: 2026-05-31
---

# 阿里 DataWorks 2026-05-28 虾聊日

2026 年 5 月 28 日下午，阿里云大数据智能体虾聊日杭州站，主题：**以 Agent 之力赋能 OPC 数据经营新范式——一人创业不费力，DataWorks + Agent 助力经营提效**。主持人：DataFun 社区 红飞。受众：数据工程师、算法工程师、高校师生、中小企业技术负责人，以及正在探索 AI 创业的个人开发者。

## 摄取说明

2026-05-31 重新摄取 `2026/05/28/DataWorks虾聊0528录音-修正版.md`（444 行，整场录音转写文字稿），以"录音 vs PDF 视觉复核"双源对照方式补充各分享口语化新增信息，并新增本主页缺失的开场 OPC 引主机制环节。本次纠正：补全开场王子军 OPC 分享（PDF 集合中无对应 deck，仅录音保留）、补充各场演讲人原话与现场 demo 细节、修正分享目录顺序（淘宝直播在 OpenClaw 之前实际是丁源，开场为 OPC）。

## 完整议程顺序

| # | 主题 | 演讲人 | 来源 |
|---|------|--------|------|
| 0 | **OPC 引主机制** — 一人加 AI 的创业路径与杭州 OPC 政策 | 王子军｜I have a demo 创始人、未来数字港 OPC 入驻者 | 仅录音 |
| 1 | [[dataworks-data-agent\|DataWorks Data Agent 揭开面纱：新一代数据智能体，企业大数据的数字员工]] | 孙文强｜阿里云智能集团高级技术专家、DataWorks 团队 | PDF + 录音 |
| 2 | [[taobao-live-data-dev-paradigm\|淘宝直播 × DataWorks DataAgent：AI 驱动的数据研发新范式]] | 丁源｜阿里巴巴淘天集团高级数据专家、淘宝直播数据研发负责人 | PDF + 录音 |
| 3 | [[maxcompute-skills\|MaxCompute × Agent：从多模态存储到混合计算]] | 孙戴博（六稻）｜阿里云智能集团产品专家、MaxCompute PD | PDF + 录音 |
| 4 | [[hologres-skills\|基于 Hologres Skills 完成广告素材智能生成与投放效果分析]] | 赵红梅（梅酱）｜Hologres 产品经理 | PDF + 录音 |
| 5 | [[flink-skills\|Flink Skills：重塑实时计算开发与运维体验]] | 李昊哲 | PDF + 录音 |
| 6 | [[emr-skills\|EMR Skills：从 Lakehouse 到 Agentic Lakehouse]] | 畅含｜EMR 产品经理、计算平台事业部 | PDF + 录音 |
| 7 | [[openclaw-agentic-search-memory\|Agentic Search + Memory，OpenClaw 企业研究更智能]] | 陈梦成｜阿里云 AI 搜索团队 | PDF + 录音 |

## 开场：OPC 引主机制（王子军）

阿里云 PDF 集合中没有这场分享的演讲材料，仅录音保留。议题以**杭州市 5 月 13 日发布的"AI + OPC"政策**为底本展开。

### OPC 与 OPC 社区

- **OPC**（One-Person Creator）：具备"单人 + AI"核心特征的创业团队/初创企业，一般规模 ≤10 人
- **OPC 社区**：以平台/社区角色为 OPC 提供入驻、孵化、政务、营销、资本等全链路服务
- 入驻流程：线上提交 BP → 平台初筛 → 每周三下午在数字港路演评审会（产品/技术/商业三维度）→ 成功入驻才算获得官方 OPC 创业者身份，再用此身份对接政府政策

### I have a demo 平台

国内首家 **OPC 全链路孵化平台**，2026 年初开始运营。

- 800+ BP 收件，入选率 <10%
- 17 场路演评审会，最终入驻 40 家 OPC
- 三位创始人：王子军（产品 + 创业公司创始人 IP 背景）、马超（得到 AI 学习圈特邀导师，国内第一批 AI 企业化升级工程师）、汪诺（22 年投资并购经验、科大讯飞商业顾问；目前在筹一只千万级 OPC 专项基金）

### OPC 三大共同困境

| 困境 | 表现 |
|------|------|
| **语言鸿沟** | OPC 用技术语言，客户用产品语言，沟通有障碍 |
| **信任鸿沟** | 早期阶段无平台背书、无大公司资源，对接客户处于劣势 |
| **规则鸿沟** | 多数 OPC 没有招投标资质，有交付能力也接不到大单 |

### 引主机制（5 月 13 日发布的政策核心）

四角色协同：

1. **企业出题** — 场景方提出真实需求，汇总成场景清单
2. **政府搭台** — 把场景方与有交付能力的服务方撮合起来
3. **引主** — 具备招投标资质的更大 OPC 作为前端对接客户接单，把大单子拆成小任务分发给其他 OPC 合力交付
4. **OPC 合力交付** — 大家做自己擅长的事，共同完成同一个订单

录音例：拿到的场景订单有 50 万级别，单个 OPC 吃不下来，通过引主机制可由多家共同交付。

### I have a demo 提供的支持

| 维度 | 内容 |
|------|------|
| MVP 打磨 | 自研 AI 硬件 + 智能体平台，支持 MVP 初步打磨 |
| 商机对接 | 通过引主机制对接订单 |
| 政务 | 入口对接、政策材料指引 |
| 社群 | 入驻 OPC 经验/生意经互助社群 |
| 营销 | 央媒/地方媒体每周关注、IP 专家辅导线上矩阵号 |
| 资本 | 汪诺老师筹的千万级 OPC 专项基金 |

### OPC 三阶段成长路径

预苗期（打磨 MVP，找第一个商单）→ 成长期（有商业收益，向下一阶段进发）→ 精英期（可作为新引主，帮其他 OPC 接单或拆分任务）

### 王子军本人项目：Airtouch

NFC 碰一碰 + AI 整体营销方案：手环、车挂、酒店门卡、奶茶杯包装等都嵌入 NFC 芯片，碰一下打开 AI 定制页面（人格测试、本地生活推荐、剧本杀游戏系统等），替代企业传统物料。复杂订单可作为引主拆给多家 OPC 共建。

## 1. DataWorks Data Agent（孙文强）

录音补充 PDF 之外的细节：

- **现场两个具体故事**：①周五下班前业务方临时要求"周一上线一张用户活跃度表"——传统流程涉及 7-8 个环节、3-4 个系统、以天计算；DataAgent 可一次性接管目标→生成代码→调试→交付。②凌晨周期调度任务故障——传统值班同学打电话上线翻日志；DataAgent 主动把错误信息+原因+解决方案推送到钉钉/飞书/企微 IM 群，自然语言交互即可解决
- **从 Copilot 到 Agent 的能力跃迁**：以前是"会看"+"会写"（交互+写代码），现在是"会做"——真正的智能体可自主规划与执行
- **CLI 模式 vs Claw 模式的对比关键句**：录音原话——"两个 Agent、两类场景，共用一颗大脑"。CLI 服务开发态、人快速迭代；Claw 服务运行态、AI 在 IM 群独立解决问题；并存而非替代
- **共享数据语义层**：录音原话——"真正决定它们与众不同的，不是左边能写代码、右边能调度，而是下边这层共享的数据语义层。它们面对的是相同的数据世界。"
- **Demo 三大看点**：①输入是业务目标而非指令 ②收到后先理解上下文/制定计划/读取相关文件，再执行 ③执行过程中检查、修复、验证，直到完成

## 2. 淘宝直播（丁源）

录音补充：

- 自我介绍："淘宝直播团队，主要负责数据研发"——丁源是淘宝直播数据研发**负责人**
- **NL2DSL2SQL 的口语化解释**：录音原话——"我们不是做完整的 NL2SQL 过程，而是中间加了一层——NL to DSL to SQL……所有代码生成过程都是白盒的，每个过程可以分段调试和审计"
- **R2C 全流程的人话定义**：录音原话——"R2C 流程，不是拿到一句话给个 SQL 就完了，而是从需求沟通开始到最终需求交付的全流程：需求沟通、技术方案设计、代码生成、运维挂基线、数据回刷、数据验证、最后上线"
- **演进时间线（口语版）**：24Q3 基建期 → 25H1 跑通 R2C → 25Q4 中心化 AI Native → 今年（小龙虾爆火后）开始本地化 AI Native → 把经验全部变成 Skill（包括 verify-data Skill 已在阿里云公众号发布）
- **三条铁律口语版**：①坚持长期主义——代码生成不能大力出奇迹，必须扎实做基建；②资产持续保鲜——数据资产/知识库/Memory 要持续迭代；③紧跟前沿
- **从中心化转向本地化的 4 个原因**：①Agent 迭代速度成单点能力 ②Memory 记忆冲突无法像代码合并 ③Skill 出现后让 Agent 变成 Skill 实现 AI 平权 ④中心化召回是全量的，无法个性化（淘宝直播 RAG 量极大）
- **本地化 AI Native = 4 个 Agent 在本地**：主 Agent 协调；标准三趴需求流程：①需求分析 ②建模 ③写代码；OpenClaw 干活时先拉取记忆和 Spec 文件再决定怎么干
- **未来方向**：以 ChatBI 为切入点，做数据消费分析探索——不只是写代码、交付报表，而是绑定业务目标（GMV、指标变化原因等）

## 3. MaxCompute × Agent（孙戴博）

录音补充：

- **演讲目标元梗**：录音原话——"现在的演讲目标不只是让大家听懂，更是：我讲的内容，让你的 Agent 转化成文字来听，作为你 Agent 的上下文来使用 MaxCompute"
- **三部分口语版**：①丰富 Agent 接入方式（云端/本地 Claw 全覆盖）②认知对齐（财务/运营口径一致）③拓展存储和计算（高并发解决单点算力不足）
- **为什么提供官方 MaxCompute 客户端**：录音原话——"就像看银行账户，Agent 可以读，但一定要有自己的网银去看真实数据"。MaxCompute 客户端类似 Navicat，帮客户取最准确的数据，与 Agent 取数交叉校验
- **Time Travel + AutoMV 的双层保障**：①Time Travel 防误删/误覆盖，最近 7 天任意时刻可回滚，上月版本可打快照（增量存储不膨胀）；②AutoMV 解决多 Agent 同时查同一数据的算力消耗——第二、第三个 Agent 来查直接命中缓存，用户无感知
- **MaxAgent 移动端典型场景（口语版）**：数据膨胀时手机端临时弹性资源；失败作业手机端触发重跑；作业诊断、Quota 合理分配、多仓 Hive→MaxCompute 数据迁移都可自然语言交互

## 4. Hologres Skills（赵红梅/梅酱）

录音补充：

- **现场顺序**：开场先让 OpenClaw 在后台跑整个 Skill demo（输入：抠版三国动漫人物刘备和吕布两个广告素材；任务：基于素材生成新广告素材→生成广告视频→投放各渠道→数据智能化分析）；让 demo 在后台跑着，演讲同时进行
- **HSAP 演进口语版**：从 2016 年商业化、HSAP 1.0 主打 OLAP+在线服务，到去年开始全面接入 AI（非结构化数据、AI Function、向量/全文/混合检索），正式升级为 **HSAP 2.0**，成为一站式 AI 数据分析平台
- **AI Function 的核心抽象**：把大模型调用抽象成 SQL 函数——AI Embedding、AI Gen（推理）、AI Translate（翻译）等
- **方案优势口语版**：①SQL 一函数调用即可完成 AI 开发，无学习成本 ②不需要数据搬迁（以前数仓在数仓、AI 数据在别处） ③数据不出库，安全可控

## 5. Flink Skills（李昊哲）

录音补充 4 个现场 demo 场景：

1. **作业调优 demo**：原本需翻 3 个页面对比监控指标查不同文档；现在一句话搞定 → Agent 自动调好整体作业的并发度和 CPU；再问"验证一下作业反压有无缓解"，Agent 实时反馈"反压状态已消除"
2. **批量巡检 demo**：成百上千 Flink 实例的大促前全面巡检——"帮我巡检一下所有的 Flink 实例"，Flink Instance Manage Skill 自动发现全部地域所有实例（杭州、上海等），用户无需指定区域或实例列表，自动遍历所有可用区，输出详细产出报告（运行状态 + 大促风险 + 建议动作）
3. **跨产品组合 demo（实时数仓搭建）**：8 条对话搭通——Flink Instance Manage 创建实例 → MySQL Skill（RDS 数据库）→ Hologres Skill 建表 → DMS Skill 校验 schema → CMS 配置告警，全在 OpenClaw 终端完成，期间未登录任何控制台
4. **舆情分析 demo（市场部）**：一句话"帮我部署一个实时舆情分析的 AI 作业"——DataAgent 模拟实时社交媒体评论、AI Sentiment 函数做实时情感分析、结果写入 Hologres、搭建实时看板（正负面评分、点赞、评论实时滚动）

录音原话补充：

- 阿里云已开放 **69 个官方 Skill**，覆盖 6 大云领域；Flink 控制台原生内置 Skill 技能包，开箱即用、零配置启动
- Create Skill 能力：用户可基于现有原子能力像搭积木一样搭自己的定制 Skill（举例 Gartement 大促分析 Skill）

## 6. EMR Skills（畅含）

录音章节较短，与 PDF 内容高度一致（详见 [[emr-skills]]）。

## 7. OpenClaw Agentic Search & Memory（陈梦成）

录音补充：

- **自我介绍**："来自阿里云 AI 搜索团队"——本场主题"Agentic Search + Memory 让 OpenClaw 企业研究更智能"，搜索不只是工具，更是基础能力和基础设施
- **核心矛盾原话**："信息获取已经达到机器速度，但信息消化仍然是人工速度"
- **两大引擎职责对照（口语版）**：Agentic Search 解决研究质量问题；Agentic Memory 解决知识复用问题——首先保证这一次研究 OK 让客户满意，再通过把每次研究沉淀下来达到知识服用效果
- **三个 demo 现场**：①4 个中考数学试卷文件（PDF/Docx）混合输入，3 分钟内完成解析+问答 ②169 页 NVIDIA 2023 年度报告，1 分多钟定位到任意一页表格的高管薪酬数据并精准解析 ③Rick and Morty 第一季全季视频上传知识库，按问题（如"为什么搭档"）图文并茂召回，结合字幕信息和关键帧——奥运会相关多模态视频检测能力即基于此
- **Self-evolution 出处**：录音原话——"做一个自进化的这么一套机制……最近比较火的那个 Hermes Agent 的核心输出论点……让它沉淀的记忆机制越用越聪明，越用越贴近一个人的真实感受"

## 关键趋势

1. **从 Copilot 到 Agent**：各产品线全面从"辅助人"升级为"自主完成"的能力范式
2. **Skill 化是标准配置**：六大产品线均将底层能力封装为 MCP Skill，Agent 即插即用；阿里云已开放 69 个官方 Skill
3. **多模态成为标配**：MaxCompute Blob、Hologres AI Function、EMR 多模态引擎、OpenClaw Agentic Search 均已支持图片/音频/视频处理
4. **记忆系统成为差异化能力**：OpenClaw Agentic Memory 与 Hologres Mem0 分别从认知引擎和数据库层面解决 Agent 记忆问题
5. **从单产品到多 Skill 协同**：Flink + Hologres + RDS + DMS + CMS 5 Skill 联动搭建实时数仓、MaxCompute 支持多 Agent 接入是典型模式
6. **OPC 创业生态正式纳入议程**：开场以杭州 OPC 政策与引主机制开题，呼应"以 Agent 之力赋能 OPC 数据经营新范式"的会议主题——大数据 + Agent 不只是工具升级，更是单人创业者的杠杆

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 核心产品页
- [[maxcompute-skills]] — MaxCompute Skills 与 MCMCP
- [[hologres-skills]] — Hologres Skills 与 AI Function
- [[flink-skills]] — Flink Agent Skills
- [[emr-skills]] — EMR Skills 与 Agentic Lakehouse
- [[taobao-live-data-dev-paradigm]] — 淘宝直播 R2C 数据研发范式
- [[openclaw-agentic-search-memory]] — OpenClaw 认知引擎
- [[openclaw]] — OpenClaw 自托管网关
- [[dataworks-stock-case]] — 上次虾聊日的个人量化案例
- [[data-agent-practice-guide]] — 火山引擎数据智能体实践指南
- [[cainiao-superetl-dataworks-agent]] — 菜鸟 SuperETL：基于 DataWorks Data Agent 的九大 Skill 编排实践
