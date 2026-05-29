---
title: 淘宝直播：AI 驱动的数据研发范式升级
description: 淘宝直播联合 DataWorks Data Agent 实践 R2C（Requirements To Consign）数据研发范式，三层架构（应用层/AI能力层/基建层），NL2DSL2SQL 技术决策，DSL 全自动生码 100% 准确率，Multi-Agent 工厂流水线协同
aliases: [淘宝直播数据研发, R2C, Requirements To Consign, NL2DSL2SQL]
tags: [big-data, ai-agent, practice]
sources: [2026/05/28/淘宝直播：AI 驱动的数据研发范式升级.pdf]
created: 2026-05-29
updated: 2026-05-29
---

# 淘宝直播：AI 驱动的数据研发范式升级

2026-05-28 虾聊日分享，由淘宝直播数据团队分享其联合 DataWorks Data Agent 实践 AI 驱动数据研发范式升级的完整路径。

## 核心思想

> **用工程的确定性，接管（暂时）大模型的不确定性。**

AI 时代数据研发不只是工具和效率的升级，而是**生产力的质变和生产模式的突破**。最终目标不是用 AI 替代人，而是让 **AI 产生的数据和经验，能被 AI 持续消费**。

## 三层架构

```
应用层：多 Agent 协同 + 中心化 AI Native + 本地化 AI Native    （用得好 AI）
   ↓
AI 能力层：Skill + SDD + AI Coding                            （管得住 AI）
   ↓
基建层：CDM + 知识库 + 工程平台                                （喂得饱 AI）
```

### AI 角色演进

| 阶段 | 角色 |
|------|------|
| 早期 | 经验执行者 |
| 当下 | 经验生产者 |
| 未来 | SuperAgent / HarnessAgent |

## AI 时代的五大挑战

| 挑战 | 应对 |
|------|------|
| **准确率是红线** | 100% 准确才能上线，人 review 跟不上 AI 输出速度，红线必须硬约束 |
| **知识库是标配** | 传统经验在员工脑子里，AI 时代必须全部线上化，领域模型体系化构建 |
| **消费场景复杂** | 不只给下游 ADS 同学用，AI 也要吃数据——命名/枚举/口径无二义性成硬约束 |
| **协同模式重构** | 人和 Agent 需求并行，多 Agent 并行，人退出执行环节、只守关键决策点 |
| **生产关系重塑** | 生产力质变带来生产模式和生产关系变化 |

## R2C：从需求到交付的完整闭环

**R2C = Requirements To Consign**，不是 NL2SQL，而是覆盖技术方案/代码生成/数据验证/数据回刷/基线挂载的完整交付闭环。

### NL2DSL2SQL：最核心的技术决策

| 对比 | NL2SQL | NL2DSL2SQL（R2C 选择） |
|------|--------|----------------------|
| 天花板 | 低 | 高 |
| 出错处理 | 整段重写 | 逐段修复 |
| 用户干预 | 难 | 可干预 |
| 审计追溯 | 无 | 全过程可审计 |
| 准确性 | 不稳定 | **100%** |

JSON DSL 作为结构化中间产物，是压住 AI 幻觉的"符号锚点"。四步走策略：**先地基后建筑**——数据层→规范化 Pipeline→服务化平台→智能层。

## 演进时间线

| 阶段 | 时间 | 里程碑 |
|------|------|--------|
| 基建期 | 24Q3-25H1 | 平台+AI 筹划、CDM 标准化、服务化平台启动、研发平台原型规范履约 |
| 流程期 | 25H2-25Q4 | R2C 首个 Demo、LightRag 构建、RAG 落地、多租户 |
| 应用期 | 26Q1-26Q2 | OpenClaw 数字员工、中心化 vs 本地化 AI Native、Skill 化 |

三大原则：**长期主义**（基建半年+流程半年，不抄近路）、**持续保鲜**（数据资产/知识库/Memory 持续迭代）、**紧跟前沿**（Skill/Agent/LightRag/SDD 第一时间引入）。

## R2C 架构图（以 DataWorks Data Agent 为基座）

### 应用层：中心化 vs 本地化

| 维度 | 中心化 AI Native | 本地化 AI Native |
|------|-----------------|-----------------|
| 记忆 | 每天清空 | 持续累积 |
| 经验 | 每次重学 | 可调用 |
| 错误 | 重复犯 | 不再犯 |
| 协同 | 从头交接 | 继承上下文 |

淘宝直播选择走本地化路线，缺点也明确：Agent 依赖的 DSL 迭代速度成为单点、Memory 无法轻易反哺到 Agent 进化。但 Skill 的出现让 AI Agent 能力逐步平权。

本地化实践：OpenClaw 三 Agent 协作（需求分析→建模 DDL→SQL & 自检），通过 SDD.md 文件协议沟通。效果：**ETL 3 天 → 3 小时**。

### AI 能力层

- **Skill**：能力封装，通往 Agentic 的基础
- **SDD**：共享方法论，所有 Skill 和 Agent 基于同一套规约工作
- **AI Coding**：输出协议，DSL 全自动 + Data Agent 半自动双路径

### 基建层（三大基础设施）

| 基础设施 | 组成 | 职责 |
|---------|------|------|
| **CDM（双产物+引擎）** | OneData 体系 + AI Friendly 交付 | 30+ DWD、35+ DIM、50+ DWS、100+ 资产、800+ 维度、900+ 指标 |
| **知识库（双引擎召回）** | LightRag 结构化召回 + 业务 WIKI | 表/字段/指标/维度实体关系图谱 + 黑话翻译/口径文档/枚举字典 |
| **工程平台（4 模块编排）** | 语义层管理/DSL 生码/研发后链路/中间件 | MCP 代理打通 DataWorks/Holo/Lindorm/ADB/Dataphin |

CDM 不只是数仓，是"**双产物 + 一个引擎**"的组合：
- OneData 体系：引用/聚合/组合/拓展四种建模模式
- AI Friendly 交付：资产平台 + 高质量 RAG 语料，命名/枚举/口径无二义性

## Multi-Agent 协同

有明确角色分工的工厂流水线，不是 Agent 互相聊天：

| 阶段 | 角色 | 职责 |
|------|------|------|
| 澄清阶段 | 需求分析 Agent | 不写代码，只用元数据反复校验需求：元数据校准、口径确认+黑话翻译、场景模板沉淀 |
| 规划阶段 | Coordinator + Planner | 基于澄清后的技术方案，生成执行计划 |
| 执行阶段 | Research Team（多 Agent 并行） | 技术方案→DSL→SQL→任务发布+验数+上线，全程 AI 驱动 |
| 归档阶段 | Reporter | 总结归档 |

**人保留在关键决策点，其他全部交给 Agent。**

## Skill 化实践

三种方式，统一范式：

| 方式 | 示例 | 说明 |
|------|------|------|
| **新 Skill** | 语义层 Skill | 从具体场景痛点出发，先跑通最小闭环：痛点定义→流程拆解→SKILL.md→迭代沉淀 |
| **原有 Agent → Skill** | `verify-data` | 独立验数 Agent 沉淀为通用 Skill，覆盖新表/迭代/监控/业务质疑/口径迁移 5 类场景，效率 2-4h→30min |
| **原有范式 → Skill** | `copilot_req2sql` | R2C 流程中需求澄清范式剥离为可复用 Skill，24h 交付 |

关键设计原则：
- SKILL.md 仅路由 + references 按需加载，避免上下文爆炸
- 关键节点强制人工确认，红线机制兜底
- SKILL.md 200 行只放路由，references/p1-p4.md 按阶段加载

## 四阶段 Spec 链（审计前置）

```
需求澄清 → 资源梳理 → 模型设计 → Data Agent 输入
   p1         p2         p3          p4
```

SDD 降低沟通成本，不降低问题复杂度：

| AI 能做 | AI 不能做 |
|---------|----------|
| 上下文管理——跨长流程不丢线索 | 数据建模决策——需要人的全局判断 |
| 过程追溯——每一步都有 Spec 留痕 | 指标口径判断——业务语义的最终拍板 |
| Review 前移——写之前先对齐 | 复杂业务翻译——隐性规则没法穷举 |
| 模板化执行——标准化 SQL/DSL 生成 | SQL 正确性保证——最终责任在人 |

## 双路径生码

| 路径 | 方式 | 准确率 | 适用场景 |
|------|------|--------|---------|
| 路径 A：DSL 全自动 | JSON DSL → 服务化平台 → DSL2SQL | **100%** | R2C 主流程，标准化需求 |
| 路径 B：DataWorks Data Agent 半自动 | copilot_req2sql Skill → 结构化 Prompt → Data Agent | **50-80%** | 次抛/临时取数/历史任务/CDM 层 |

两种路径都从 SDD 取上游，业务语义对齐一致。

## 关键成果

| 指标 | 数据 |
|------|------|
| AI 代码占比 | 75% |
| 数据准确率 | 100% |
| Data Agent 生码准确率 | 50-80% |
| 资产答疑准确率 | ~80-90% |
| R2C 召回准确率 | ~90-95% |
| 资产助手准确率 | ~90%+ |
| UV 用户 | 150+ |
| 代码采纳率 | 72.9% |
| 代码渗透率 | 24.6% |
| 平均对话轮次 | 5 轮 |
| 月均使用团队占比 | 52% |

## 未来规划

- R2C 全流程已跑通，下一站是 **ChatBI**：从数据研发走向数据消费
- 闭环只是起点，ChatBI 是从 0 到 1 的下一个目标

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 核心产品
- [[dataworks-2026-0528-xialiaori]] — 2026-05-28 虾聊日全部分享内容汇总
- [[maxcompute-skills]] — MaxCompute Skills 体系
- [[hologres-skills]] — Hologres Skills
- [[flink-skills]] — Flink Skills
- [[emr-skills]] — EMR Skills
- [[openclaw-agentic-search-memory]] — OpenClaw 认知引擎
- [[superetl]] — 菜鸟 SuperETL 实践（同属 Data Agent 驱动的数仓开发范式）
- [[data-dev-skills-automation]] — 数仓开发 Skills 自动化
- [[dataworks-stock-case]] — 上次虾聊日的个人量化案例
