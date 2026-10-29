---
title: Data-Ontology-Agent 协同进化
description: 张绍磊（中国人民大学）在 DACon 2026 北京站的演讲——传统 Human-first 数据系统的局限、CoDA-Bench 揭示 SOTA Agent 在复杂数据环境中迷失（即使给对数据准确率仅约 60.5%）、Agent + Ontology + Environment 的新一代数据系统、DeepAnalyze / DeepPrep / SkillAdam / EvoOntology 四项工作与本体五步自进化闭环
aliases: [Data-Ontology-Agent, 自进化数据智能体, EvoOntology, DeepAnalyze, SkillAdam, CoDA-Bench, DeepPrep, 数据本体智能体协同进化]
tags: [ai-agent, big-data, concept, practice]
sources: [2026/10/29/2026DACon北京站/企业本体与知识工程/张绍磊-自进化数据智能体迈向+Data-Ontolo-gy-Agent+协同进化.pdf]
created: 2026-10-29
updated: 2026-10-29
---

# Data-Ontology-Agent 协同进化

> 来源：DACon 2026 北京站「企业本体与知识工程」专场，讲者张绍磊（中国人民大学），标题页日期 2026.10.24。PDF 共 29 页。

> 核心主张：传统数据系统是 Human-first——人类专家依据领域知识处理海量数据；把 Agent 放进来并不自动变好，复杂数据环境会让 SOTA Agent 系统失败。新一代数据系统应当是 Agent-first：**Agent + Ontology + Environment** 三者协同进化——Agent 完成长程任务，Ontology 提供可生长的环境语义，Environment 提供执行反馈与真实数据。

## 背景与问题

### 数据系统是释放数据价值的关键

讲者从宏观背景切入：提升数据科学系统的智能化水平与性能，才能高效释放数据价值。幻灯片列出三个方向：

- **助力数据基础设施建设**：网络设施（高速泛在连接能力）、算力设施（高效敏捷处理能力）、流通设施（打通数据共享流通堵点）、安全设施（保障数据安全）。
- **促进智慧城市精细治理**：政府、民众、企业通过"智慧政务服务"互动。
- **推动产业数智化转型**。

引用来源：《国家数据局数据技术设施体系》《数据空间发展战略蓝皮书》。

### 传统数据系统：Human-first

传统数据系统由人类专家根据领域知识处理海量数据。幻灯片把系统拆成三块：

- **Human**：阅读需求、执行、复核与返工（Manual Execution / Review & Rework）。
- **Rules**：Rules、Mapping、IF/THEN、SOP、Exceptions、Scripts、Logs。
- **Environment**：Excel、CSV、XML、SQL、PDF、DOC、IMG、TXT、LOG、JSON、Parquet、API 等异构数据。

人类专家在 Rules 与 Environment 之间手工搬运、判断、复核。这条路径依赖专家，难以规模化。

### AI 赋能的数据系统：Agent 替代人类专家？

引入 Agent 后，架构变成：LLM 处于 Harness 之中，外挂 Tools、Memory、Skill；Agent 仍然面对同样的 Rules 与 Environment，仍然要 Manual Execution 与 Review & Rework。

讲者提出的问题是：**Agent 能在海量数据中完成复杂任务么？**

列举的代表性 Agent 系统：Codex、Claude Code、Mini-swe-agent。

### CoDA-Bench：Agent 迷失在复杂数据环境

为回答这个问题，讲者团队构建了 **CoDA-Bench**（Yuxin Zhang, Ju Fan, Meihao Fan, Shaolei Zhang*, Xiaoyong Du. *CoDA-Bench: Can Code Agents Handle Data-Intensive Tasks?*. ICML 2026）。

Benchmark 的定位区分两类智能：

| 评估对象 | 任务形态 | 代表基准 |
|---------|---------|---------|
| Code Intelligence | 给代码片段/终端任务，改代码 | SWE-Bench、Terminal-Bench 等 |
| Data Intelligence | 给一个真实数据工作区，让 Agent 自己找数据、理解关系、完成分析 | DS-1000、DA-Code、DataSciBench 等 |

CoDA-Bench 的构造方式：从 Kaggle 社区和 Notebook 中构建**数据共现图**，形成海量数据社区，再把 Agent 放进真实工作区。规模数据（第 7 页）：

| 指标 | 数值 |
|------|------|
| 任务总数（full benchmark） | 1,009 |
| CODA-HARD 子集 | 119 |
| Kaggle 派生工作区（communities） | 31 |
| 每个环境平均文件数 | 980 |

示例沙箱 `community_27`：119 datasets、8,533 files、13.0 GB on disk；目录里混有 CSV、PDF、JSON、PNG、DB 等文件，题目要求 Agent 自己在真实工作区里找证据，而不是读一个干净的表。

### Agent 到底失败在哪

CODA-HARD 榜单（第 8 页，截图可见部分）：

| Rank | Agent framework | Backbone | DA（Discovery Accuracy） | EA（Execution Accuracy） | Tokens/task | Cost/task |
|------|----------------|----------|--------------------------|--------------------------|-------------|-----------|
| 1 | Mini-SWE-agent | GPT-5.5 | 67.50% | 49.60% | 101,017 | $0.39 |
| 2 | Codex CLI | GPT-5.5 | 61.40% | 47.90% | 380,558 | $1.39 |
| 3 | Claude Code | Claude-Opus-4.6 | 68.40% | 45.40% | 113,433 | $0.20 |
| 4 | Claude Code | Claude-Opus-4.7 | 61.40% | 45.40% | 123,699 | $0.22 |
| 5 | OpenHands | GPT-5.5 | 63.20% | 44.50% | 154,913 | $0.65 |
| 6 | Claude Code | Claude-Sonnet-4.6 | 61.40% | 42.90% | 81,714 | $0.11 |
| 10 | OpenHands | DeepSeek-V4-Pro | 58.80% | 36.10% | 330,161 | $0.15 |

> 第 7–9 名被页面红色标注遮挡，无法可靠辨认，故不录入；上表只保留截图可读的行。

两个关键发现：

1. **即使给对数据，准确率也只有约 60.5%。** 同一 Agent 在 Community（自己找数据）与 Oracle（直接给正确数据）两种设定下对比：GPT-5.2 + OpenHands 从 56.1% 提升到 61.0%（Δ=4.9%）；Claude Code 从 49.2% 提升到 60.0%（Δ=10.8%）。说明"找数据"只是问题的一部分，"理解数据、把数据用对"仍然是瓶颈。
2. **失败原因分布**（GPT-5.2 Failures）：Code Generation 45.5%、Data Discovery 41.6%、Task Understanding 8.1%、Execution Errors 4.8%。也就是说，代码生成与数据发现合计约 87.1%，任务理解与执行错误只占小头。

讲者的归纳是三类局限：

- **数据思维不足**：Agent 不知道怎么系统地探查、验证、清洗数据。
- **业务语义不清**：不理解数据对象、指标口径与业务关系。
- **数据质量不佳**：真实环境里的脏数据、缺失、冲突。

> 数据环境复杂导致 SOTA Agent 系统失败——这不是换一个更强的模型就能解决的问题。

## 方案/架构

### 新一代数据系统：Human-first 到 Agent-first

讲者给出的新一代数据系统公式：

```
Agentic Data System = Agent + Ontology + Environment
```

三者的分工：

| 组件 | 角色 | 图中元素 |
|------|------|---------|
| Agent | 自主完成长程任务 | LLM + Harness（Tools / Memory / Skill） |
| Ontology | 对环境的语义建模 | Objects、Properties、Links、Actions、Functions、Security |
| Environment | 真实数据与执行反馈 | DB、Excel、XML、SQL、PDF、DOC、IMG、TXT、LOG、JSON、Parquet、API |

数据流是双向的：Agent 通过 **Action Execution** 作用于 Environment；Environment 通过 **Execution Feedback** 回流到 Ontology，Ontology 再支撑 Agent 的理解与决策。讲者把目标概括为两个词：**自主性**（完成长程任务）与**自进化**（系统持续提升）。

### 四项工作与协同进化地图

第 11 页把四项工作挂到三联架构上：

| 工作 | 对应位置 | 一句话 |
|------|---------|--------|
| **DeepAnalyze** | Agent | 自主数据科学大模型，模拟数据科学家行为，从数据源自动提取 insight |
| **SkillAdam** | Agent（Skill 层） | 稳定、高效的 Skill 自进化，从用户 trajectory 自主沉淀 skill |
| **EvoOntology** | Ontology | 自进化 Ontology 层，让 Agent 持续理解不熟悉的数据 |
| **DeepPrep** | Data / 数据准备 | 自主数据准备，自然语言控制表格处理与转换 |

### DeepAnalyze：自主数据科学大模型

定位：模拟数据科学家的行为，自动从数据源中提取 insight。

给定数据源，DeepAnalyze 自主编排操作、根据反馈自适应优化：

- **全流程数据科学 Pipeline**：数据准备、分析、建模、可视化、洞察。
- **开放式数据研究**：自动研究数据，生成分析师级别报告。

论文：*DeepAnalyze: Agentic Large Language Models for Autonomous Data Science*（Shaolei Zhang, Ju Fan, Meihao Fan, Guoliang Li, Xiaoyong Du）。开源模型 DeepAnalyze-8B，数据集 DataScience-Instruct-500K。

讲者给出的社区数据（第 13 页）：

- HuggingFace 开源模型下载 30K+，开源数据集下载 20K+；
- 曾在 HuggingFace 日趋势榜论文排名第一（#1 Paper of the day）；
- 代码于 2025 年 10 月 21 日在 GitHub 正式开源；
- 幻灯片文字写"已获得 **4700K+** 个 GitHub 星标"。

> 数字存疑：同一份 PDF 第 28 页的仓库卡片显示 DeepAnalyze 为 **4.7k stars**；"4700K+" 约为 470 万，与图示和常理不符，疑为"4.7K"的排版错误。本页按原文记录并标注，不擅自采用任一数字作为确定结论。

### DeepPrep：自主数据准备

定位：自然语言控制表格数据处理、转换。

论文：Meihao Fan, Ju Fan, Yuxin Zhang, Shaolei Zhang, Xiaoyong Du, Jie Song, Peng Li, Fuxin Jiang, Tieying Zhang, Jianjun Chen. *DeepPrep: An LLM-Powered Agentic System for Autonomous Data Preparation*. VLDB 2026。

第 15 页对比了三条路线：

| 路线 | 做法 | 问题 |
|------|------|------|
| (a) Code Generation | LLM 直接生成 pandas 代码并执行 | 结果表可能为空；出错后难以定位 |
| (b) ReAct | 在表之间逐步去重、连接、分组 | 无法回滚，调试困难 |
| (c) DeepPrep | 基于算子的数据准备流水线 | 每个算子可检查、可回滚 |

DeepPrep 的算子包括：SelectColumn、SplitColumn、Pivot、Deduplicate、ValueTransform、Explode、GroupBy。图中展示了一条从 Source Tables 出发的算子 DAG：先去重 movies/id，再做 ValueTransform（movies, title）、SplitColumn（ratings, values）、Pivot、Explode（movies, genres）、Deduplicate，最后 GroupBy 输出 director/genre。关键差异是：**发现漏了算子时可以回滚补上（Rollback to add missed data preparation operators）**，而不是像 ReAct 那样只能往前试。

### SkillAdam：稳定、高效的 Skill 自进化

问题：Skill 依赖人工撰写，优化不稳定。讲者把现有 SGD-like 方法画成"随机初始化 → 按预设规则决定改什么 → 生成一个完整新 Skill"的路径，问题是改动幅度不可控、优化过程震荡。

SkillAdam 的类比是 Adam 优化器：

| Adam | SkillAdam |
|------|-----------|
| Rollout：从数据集采样 mini-batch，前向计算 loss | Rollout：采样 Case，执行当前 Skill，得到 Trajectories 与 Feedback |
| Moment Estimation：一阶动量 m、二阶动量 v | Moment Estimation：Optimization Memory（Evolving Issue Tracker，记录 constraints missed / cheaper cart missed / items mismatched）+ Volatility-Driven Edit Budget（把 case-level changes 折算成编辑预算） |
| Parameter Update：按 Adam 公式更新参数 | Skill Update：Patch Generator 生成 Skill Patch，应用后得到 Updated Skill |
| 参数 θ_t | Skill S_t |

三个设计点：**Trajectory-Informed Initialization**（用轨迹初始化，而非随机）、**Optimization Memory**（把历史问题与优化方向记下来）、**Volatility-Driven Edit Budget**（按问题波动决定改多少，而不是一次重写整个 Skill）。更新后若变好则保留，否则丢弃。

### EvoOntology：自进化 Ontology 层

问题定义：在复杂工作区完成任务，需要对环境的语义建模。没有 Ontology Layer 时，Data Agent 面对异构数据源（Tables、CSV、Docs、Databases、Charts、Logs）只能"盲探"，讲者称之为 **Blind Data Exploration with High Semantic Uncertainty**。

第 20 页右侧用一张手绘工作区语义地图说明"环境里到底有什么"：以 OD 为中心，分出 Philosophy、Calendar、Reporting、System、Resources、IT、Administration、Content、Projects、Contacts、Products & Services 等分支，再往下有 meetings、reports、accounts、procedures、servers、tools、templates、files 等节点。这张图是"需要被建模的语义结构"的示意，手写小字部分无法全部可靠辨认。

本体在幻灯片中的定义：

- 核心问题：探讨"世界上存在什么？"以及实体、属性、关系、空间和时间之间的基本规律。
- 计算机领域基本定义：对特定领域中"概念、数据和实体之间的类别、属性和关系"的形式化表达与规范化描述。

#### 三层结构

| 层 | 作用 |
|----|------|
| **Content Layer** | 类型化语义图，包含 Term、Mapping、Constraint 和 Evidence 四类节点。Semantic Relation 连接 Term；Structural Reference 连接 Term 与 Mapping，并将 Constraint 或 Evidence 挂接到其约束或支撑的对象上 |
| **Schema Layer** | 定义四类节点的字段、允许使用的 Semantic Relation 类型，以及合法的 Structural Reference 模式，从而确定 Ontology Layer 的表达边界 |
| **Tool Layer** | 通过 `browse_semantics`、`resolve_semantics` 和简洁的 `session manifest` 向 Agent 暴露 Ontology Layer。会话初始化时只注入 manifest；具体记录及其关联对象均按需检索 |

#### 构建初始本体

收集任务与数据源，探测数据并保留证据。第 22 页的例子：

- 用户问题："Why did cost rise last month?"，结果是 Cost rose 12% last month，top drivers 为 Inflation (+7pp)、FX (+3pp)、Volume (+2pp)。
- Builder Agent 从 workload queries 提取候选概念：Cost、Time、Revenue、Profit。
- 在异构数据中 ground 候选概念：Cost Table（`month_id`、`cost_type`、`cost_amount`，如 Jan-24 Labor 1,234,567；Jan-24 Freight 234,987；Feb-24 Labor 1,310,000）、Metric Definition（total cost = sum of cost_amount）、Revenue Records。
- 构建 Content Layer：Term（Cost、Revenue、Profit、Time）、Mapping（Cost Table、Revenue Records）、Evidence（Cost catalog、GAAP revenue definition、data table sample）、Constraint（Profit = Revenue - cost、Time grain = Month）。
- Tool Layer 暴露 `browse`（Find relevant terms, Query → Ranked Terms）与 `resolve`（Retrieve complete semantics, Term IDs → Mappings, Relations, Constraints, and Evidence）。

#### 五步自进化闭环

1. **Build**：从 workload 提取候选概念，在原始数据源中验证，并发布 `ontology_v0`。
2. **Use**：Data Agent 按需查询 Ontology Layer，同时记录工具交互和任务结果。
3. **Evolve**：诊断重复出现的行为，将问题归因到 Content、Tool 或 Schema，并生成局部 Candidate 补丁。
4. **Evaluate**：在相同数据、Agent、解码配置和交互预算下比较 Parent 与 Candidate。
5. **Publish or reject**：通过门控的 Candidate 发布为新 ontology；否则保留 Parent，并把结果用于下一轮。

第 24 页给了归因示例：历史轨迹中 Q3 "Cost by channel lost quarter" 失败，归因为 **Content Level** —— Missing term "Channel"；同时发现 **Tool Level** —— Missing "FX Convert"；**Schema Level** —— No schema-level issue identified。然后 patch Parent Ontology，生成 Candidate Ontology（新增 Channel 项与 Tool 3: FX Convert），再用 Data Agent 在相同条件下 Evaluate，接受则升级为 Ontology v2.0，拒绝则回退 v1.0 并进入下一轮。

## 关键工程做法

### 把"数据准备"算子化，而不是让模型自由写代码

DeepPrep 的核心工程判断是：数据准备的可调试性比"让 LLM 自由生成代码"更重要。代码生成路线一旦结果为空，很难知道是哪一步错了；ReAct 路线没有回滚机制。DeepPrep 把数据准备拆成可组合算子，每个算子有明确输入输出，出错时能定位到具体节点并回滚补算子。这是"Agent 系统"与"Agent 写代码"的差别：前者把过程变成可检查的结构。

### 把"Skill 优化"做成优化器，而不是提示词改写

SkillAdam 把 Skill 当作可优化参数：用轨迹反馈估计"哪里错了"（Optimization Memory），用波动估计"该改多少"（Edit Budget），再由 Patch Generator 生成局部补丁。它解决的是 Skill 自进化中的两个不稳定源：改错方向、改过头。讲者的实验覆盖短程（约 10 轮）与长程 Agent 任务，说明该方法不是只对单一任务有效。

### 把"本体"做成可查询、可评估、可回滚的运行时层

EvoOntology 不是一次性建模文档，而是 Agent 运行时的一部分：

- 会话初始化只注入 manifest，避免把整个本体塞进上下文；具体记录按需检索。
- 本体变更走 Candidate → Evaluate → Publish/Reject 的门控，评估条件固定为相同数据、Agent、解码配置与交互预算。
- 失败归因分三层（Content / Tool / Schema），每层对应不同的补丁类型：补概念、补工具、改 schema。
- 版本可回退（v1.0 ↔ v2.0），把本体演进变成可审计的工程过程。

### 把"评测"做成训练场与考场同源

CoDA-Bench 的构造直接从 Kaggle 社区与 Notebook 的数据共现关系出发，让任务、数据、证据在同一个真实工作区里；这与"先造一个干净题库"不同。讲者在 DeepAnalyze / DeepPrep 的评价中也强调真实数据工作区，而不是单表问答。

## 数据与效果

### CoDA-Bench

- 1,009 个任务、119 个 CODA-HARD 任务、31 个 Kaggle 派生工作区、每环境平均 980 个文件。
- CODA-HARD 榜首 Mini-SWE-agent + GPT-5.5：DA 67.50%、EA 49.60%、101,017 tokens/task、$0.39/task。
- Oracle 数据设定下，GPT-5.2 + OpenHands 56.1% → 61.0%（Δ4.9%），Claude Code 49.2% → 60.0%（Δ10.8%）；"即使给对数据，准确率也只有约 60.5%"。
- GPT-5.2 失败分布：Code Generation 45.5%、Data Discovery 41.6%、Task Understanding 8.1%、Execution Errors 4.8%。

### SkillAdam

短程任务（约 10 轮）：

| Method | SearchQA | Spreadsheet | OfficeQA | DocVQA | LiveMath |
|--------|----------|-------------|----------|--------|----------|
| NoSkill | 77.7 | 41.8 | 33.1 | 78.8 | 37.6 |
| HumanSkill | 81.8 | 72.9 | 66.9 | 90.1 | 38.4 |
| LLMSkill | 80.9 | 43.2 | 51.7 | 89.6 | 40.0 |
| Trace2Skill | 82.4 | 49.6 | 65.7 | 90.6 | 52.0 |
| TextGrad | 81.4 | 41.1 | 42.0 | 87.2 | 49.2 |
| GEPA | 84.8 | 73.6 | 63.9 | 89.1 | 43.2 |
| SkillOpt | 87.3 | 80.7 | 72.1 | 91.2 | 66.9 |
| **SkillAdam** | **87.5** | **81.1** | **72.1** | **92.3** | **67.7** |

长程任务：

| Method | ALFWorld | DP-Shopping | DP-Travel | DP-Avg |
|--------|----------|-------------|-----------|--------|
| NoSkill | 83.6 | 31.7 | 0.0 | 15.8 |
| SkillOpt | 87.3 | 41.7 | 1.7 | 21.7 |
| **SkillAdam** | **89.6** | **45.0** | **11.7** | **28.3** |

讲者的结论：Skill 在短程（约 10 轮）和长程 Agent 任务上均有显著提升；SkillAdam 在表中所有指标上不低于 SkillOpt，且在 DP-Travel 上从 1.7 提升到 11.7，是相对提升最明显的一项。

### EvoOntology

柱状图（Baseline / Initial / Evolved）：

**DDR-Bench (10-K)，Traj-Wise (%)**

| 模型 | Baseline | Initial | Evolved |
|------|----------|---------|---------|
| GPT-5.5 | 64.2 | 78.2 | 90.9 |
| GPT-5.6-sol | 68.5 | 82.9 | 93.5 |
| Claude-Sonnet-5 | 72.5 | 78.2 | 81.3 |
| Claude-Opus-4.8 | 73.0 | 87.9 | 92.3 |

**InsightBench，Insight (%)**

| 模型 | Baseline | Initial | Evolved |
|------|----------|---------|---------|
| GPT-5.5 | 52.9 | 53.4 | 53.4 |
| GPT-5.6-sol | 51.6 | 52.8 | 53.2 |
| Claude-Sonnet-5 | 53.3 | 54.2 | 54.4 |
| Claude-Opus-4.8 | 54.9 | 55.7 | 55.8 |

**BIRD，Overall EX (%)**

| 模型 | Baseline | Initial | Evolved |
|------|----------|---------|---------|
| GPT-5.5 | 61.5 | 66.1 | 68.9 |
| GPT-5.6-sol | 63.5 | 68.2 | 70.7 |
| Claude-Sonnet-5 | 61.9 | 67.4 | 71.8 |
| Claude-Opus-4.8 | 67.5 | 73.2 | 78.3 |

折线图（Evolution round）：

- DDR-Bench：四个模型随轮次 0→5 持续上升，例如 GPT-5.5 从约 78 升到约 91，Claude-Opus-4.8 从约 88 升到约 92。
- InsightBench：随轮次 0→3 小幅上升，曲线较平。
- BIRD：随轮次 0→4 上升，例如 GPT-5.5 从约 66 升至约 69，Claude-Opus-4.8 从约 73 升至约 78。

讲者的结论：**自进化 Ontology 层优于 Palantir 式本体层；Agent 在复杂任务上表现更准确；Agent 表现随着 Ontology 层的进化持续提升。**

### DeepPrep

在 Synth-Spider、Synth-Bird、Parrot 三个数据准备基准上，横轴为 Cost（$，对数刻度）、纵轴为 Accuracy，DeepPrep（Ours Pareto 虚线）位于所有对比方法（Claude-4、GPT-5、Qwen3 系列、MCTS-OP、MontePrep、PaS、ReAct、CodeGen、AutoPrep 等）的左上方，讲者称为"取得成本-质量帕累托最优的数据准备效果"。幻灯片未给出具体数值表。

### 开源与社区

- 项目主页：`deepanalyze.net`；在线系统 + 开源研究。
- EvoOntology：1000+ stars，开源插件，一键接入任意 Agent（第 26 页展示 Codex 界面）。
- 第 28 页仓库卡片：DeepAnalyze 4.7k stars、EvoOntology 1k、SkillAdam 100、CoDA-Bench 47、DataEvolver 5、DeepPrep 2。

## 经验与边界

- **不是"换个更强模型"的问题**。CoDA-Bench 显示，即使把正确数据直接给 Agent，准确率也只有约 60.5%；失败主要来自代码生成（45.5%）与数据发现（41.6%），说明数据环境的语义建模与数据操作能力是独立瓶颈。
- **本体必须可进化，而不是一次性建模**。EvoOntology 的 Build → Use → Evolve → Evaluate → Publish/Reject 五步，把本体变更变成有门控的实验：同数据、同 Agent、同解码配置、同交互预算下比较 Parent 与 Candidate，避免"改完感觉更好"。
- **本体的表达边界由 Schema Layer 决定**。允许哪些关系、哪些引用模式，直接决定本体能表达什么、不能表达什么；没有 Schema 约束的图容易退化为不可校验的知识堆。
- **Tool Layer 要按需暴露**。会话初始化只注入 manifest，具体记录按需检索，避免把整个本体塞进上下文——这与"上下文不是越多越好"的工程原则一致。
- **Skill 自进化要控制改动幅度**。SkillAdam 的 Edit Budget 与 Optimization Memory 是对"一次重写整个 Skill"的反思：没有预算约束的自我修改容易震荡，也无法判断哪次修改真正有效。
- **数据准备要可回滚**。DeepPrep 用算子图替代自由代码生成，核心收益不是"更自动"，而是"出错可定位、可回滚"；这是 Agent 工程化的共性要求。
- **研究阶段的证据边界**：本页四项工作均为学术研究/开源项目，不是企业生产系统。CoDA-Bench 是基准，DeepAnalyze 是 8B 模型，SkillAdam 与 EvoOntology 是方法/插件，DeepPrep 是 VLDB 2026 论文系统；幻灯片没有给出任何企业部署、成本、延迟、并发或长期运维数据。
- **对比口径有限**：EvoOntology 只在 DDR-Bench (10-K)、InsightBench、BIRD 三个基准上比较，"优于 Palantir 式本体层"是讲者的 slide 结论，未披露 Palantir 侧的具体配置、统计显著性与误差棒；折线图轮次有限（3–5 轮），不能外推为长期进化曲线。
- **数字不一致**：DeepAnalyze 的 GitHub 星标在正文写"4700K+"，仓库卡片写"4.7k"，二者矛盾；本页按原文记录并标注存疑。
- **"协同进化"是讲者的综合框架**，四项工作分别解决 Agent、Skill、Ontology、Data Preparation 四个侧面；幻灯片没有给出四者联合运行的端到端实验，不能把单项收益相加当作系统收益。

### 开放问题

以下问题原文未展开，按演讲内容列出，供后续研究核对（**非讲者原话，也不代表原文已给出答案**）：

- **本体进化的成本**：每次 Build/Use/Evolve/Evaluate 需要额外的 Agent 调用与环境执行，原文未给出 token、时延、算力或人工审核成本。
- **Candidate 评估的统计可靠性**：Evaluate 在"相同数据、Agent、解码配置和交互预算"下比较 Parent 与 Candidate，但未说明样本量、重复次数、显著性判定与门控阈值；小样本下如何避免把噪声当进化，原文未答。
- **Schema 变更的迁移成本**：Content/Tool/Schema 三层都可打补丁，但 Schema 变更会影响已有 Term、Mapping 与 Constraint 的兼容性；迁移策略与回滚边界未展开。
- **多人协作下的本体冲突**：企业场景中多个团队同时维护本体时，概念命名、口径冲突与权限如何合并，原文未涉及。
- **长期漂移与遗忘**：Ontology 持续进化后，早期版本支撑的案例是否需要重新验证；如何防止"越进化越偏"的漂移，原文未给出机制。
- **基准外泛化**：EvoOntology 只在 DDR-Bench (10-K)、InsightBench、BIRD 三个基准上评估；在真实企业数据、超大规模 schema、强权限约束下是否同样有效，尚未验证。
- **生产化缺口**：四项工作均为研究/开源形态；并发、权限、审计、数据安全、SLA 与运维体系未在幻灯片中讨论。

## 相关页面

- [[agent-self-evolution]] — 自进化智能体：经验沉淀、可学习更新、验证写回与持续评估的四个闭环
- [[ontology]] — 本体论基础：五要素框架与知识工程背景
- [[genie-ontology]] — Databricks Genie Ontology：人工定义 + 自动生长的企业知识图谱，可与 EvoOntology 的自进化路线对照
- [[data-agent-vs-ontology-agent]] — Data Agent 与 Ontology Agent 的推理空间与确定性来源差异
- [[business-cognition-system]] — 可演进的业务认知系统：知识构建、组织、加载与反馈回流
- [[ontology-driven-knowledge-engineering]] — 本体驱动知识工程：企业知识底座的三层架构与建模方法
- [[data-agent-enterprise-practice]] — 企业 Data Agent 落地实践：从问数到分析闭环的工程路径
- [[agent-evaluation-framework]] — Agent 评测框架：基准设计与评测门禁的方法论背景
