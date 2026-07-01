---
title: 小米 DiMi Data Agent——Text2SQL 全球第三的语义层 Harness 实践
description: 小米 Data Agent DiMi 在 BIRD 榜单获得全球第三，核心差异化在语义层 Harness（NL2Semantic2SQL），而非基座模型。拆解大模型直连数据库的五大生产级瓶颈：查数幻觉、根因分析陷阱、多表关联混乱、无评测闭环、权限合规。
aliases: [DiMi Data Agent, Xiaomi Data Agent, 小米数据智能体, NL2Semantic2SQL]
tags: [big-data, ai-agent, practice]
sources: [2026/06/26/小米 Data Agent 获 Text2SQL 全球榜单第三名.html]
created: 2026-06-26
updated: 2026-06-26
---

# 小米 DiMi Data Agent

> 核心主张：BIRD 榜单排名差异不在模型，在语义层的 Harness。Claude Opus 4.6 / GPT-5.5 裸打榜 30-40 名，加 Harness（腾讯、小米）冲进前五——Data Agent 决胜关键是 **NL2Semantic2SQL** 语义增强体系。

## BIRD 榜单：Text2SQL 领域"奥林匹克"

**BIRD**（Big Bench for Large-scale Database Grounded Text-to-SQL Evaluation）是目前 Text2SQL 领域公认最难、最权威的评测基准。

| 维度 | BIRD 特点 |
|------|----------|
| 数据库数量 | 95 个真实数据库 |
| 覆盖领域 | 37 个垂直领域 |
| 数据质量 | 含脏数据、缺失值 |
| 外部知识 | 需要业务知识（evidence）辅助回答 |
| 评判标准 | EX（执行正确率），答案对才算；VES 评估执行性能 |

核心原则：**SQL 写法不重要，执行结果必须和标准答案一致**。摒弃语句格式约束，评测规则与企业落地场景高度对齐。

## Harness > Model：BIRD 给出的关键证据

2026 年，OpenAI、Claude、腾讯、快手等主流厂商积极参与打榜：

| 参赛方 | 方式 | 排名 |
|------|------|------|
| Claude Opus 4.6 | 裸模型 | 30-40 名 |
| GPT-5.5 | 裸模型 | 30-40 名 |
| 腾讯 Data Agent | Model + Harness | 全球第五 |
| **小米 DiMi** | **Model + Harness** | **全球第三** |

最顶级基座模型裸打榜只在 30-40 名，排名差异不在模型，在语义层 Harness。这与 [[code-agent-vs-data-agent|Code Agent vs Data Agent]] 的论调一致：企业数据分析需要系统设计补足模型能力缺口。

## 五大生产级瓶颈

### 瓶颈一：查数幻觉——模型不认识你的业务世界

大模型训练数据里没有企业的业务规则和字段命名，导致系统性失败：

**缺失背景知识**
- "小米集团有多少人？" → 模型不知道表存的是在职数据，用通用知识瞎猜
- "ITP 这个部门今年的预算有多少？" → 模型不知道 ITP = 基础技术平台部，直接 `WHERE department = 'ITP'`，查不到结果

**缺失业务规则**
- "上周的漏损订单量是多少？" → 严格规则：用放款完成日期筛时间、交付日期 IS NOT NULL、当前机构是否自营 = 0。规则只在业务文档里，模型每次猜的不一样

**枚举值陷阱（隐蔽杀手）**

| 场景 | 字段实际存储 | 模型猜测 | 结果 |
|------|------------|---------|------|
| 查三星员工 | `Samsung` | `三星` 或 `三星集团` | 0 条，静默失败 |
| 查已完成订单 | `status = 1`（1=完成，0=开始，2=异常） | `status = 'completed'` | 报错或空结果 |
| 查手机品类 | `PHONE` 和 `phone` 混存 | `'phone'` | 漏掉一半数据 |

> 这不是模型不够聪明，是业务知识无法靠训练学到。

### 瓶颈二：深度分析不是代价高，就是完全错误

"有了 LLM + Text2SQL 就能做根因分析"是 Data Agent 领域最常见的误区。

**加法类指标：能做，但代价极高**
- "导致 12 月净收入波动的四级团队有哪些？"
- 无专用工具：模型拆成 N 个 SQL → 查 12 月 → 查 11 月 → 逐行心算差值 → 排序。调用次数从 1 次骤增到 N 次，响应时间 3 倍+，累积错误率急剧上升
- 有根因分析工具：1 次调用，底层引擎完成全部计算

**除法类指标：完全无法正确完成**
- "导致 12 月利润率（利润/收入）波动的四级团队有哪些，贡献度是多少？"
- ⚠ 实测：针对除法逻辑指标，大模型**完全无法完成任务**，且会输出看起来合理实则严重错误的答案
- 模型做法：分别查各团队自身利润率变化 → 用减法算差值 → 把"自身变化幅度最大"的团队认定为主因
- 这是根本性逻辑错误：极小规模团队利润率从 -97% 变到 +129%，对整体贡献微乎其微。**"自身变化幅度"和"对整体的贡献度"是两个不同概念**
- 正确答案需要替代法/杜邦分解算法——大模型靠 Text2SQL 凑不出来

**海量维度下钻：上下文溢出**
- "分析 11 月整体毛利率下降，具体下钻到哪些 SKU 导致的（10 万+ SKU）"
- 10 万行数据不可能全塞给大模型，模型强加 `LIMIT 50`——分析冰山一角，遗漏长尾根因

### 瓶颈三：表越多，关联越乱

真实业务场景：几百张表，字段命名各异，无物理外键，无 ER 图，关系在 DBA 脑子里。

| 问题类型 | 示例 | 后果 |
|---------|------|------|
| 选表错误 | 两张表都与"离职"相关，只有 data owner 知道哪张准确 | 基于错表做分析 |
| 关联错误 | A 表 `user_id` = `ID_123`，B 表 `user_id` = `123`（无前缀），直接 JOIN | 0 条数据，无报错，静默失败 |
| 数据膨胀 | 一对多关系未去重 | 指标数值翻倍，营销预算基于错误数据做决策 |

这类问题靠优化提示词无法根治，需要**结构化的关系模型**约束 SQL 生成边界。

### 瓶颈四：效果不稳，全靠人盯，成本高

SQL 执行成功 ≠ 答案正确。语义是否正确依赖业务知识，只有人能判断：

| 问题 | 现实 |
|------|------|
| 线上准确率怎么量化？ | 只能人工标注，成本大，无法规模化 |
| 模型升级后会不会退化？ | 无自动回归测试，只能人工逐一验证 |
| 发现 badcase 怎么修？ | 依赖研发手工调优 Prompt，迭代极慢 |

> 没有评测-回归-迭代的闭环，上了生产就是玄学驱动开发——今天跑通的 query，明天换个措辞可能就失败，没有人知道为什么。

### 瓶颈五：权限是合规红线

[[claude-code|Claude Code]] 和 Codex 的定位是个人效率工具。企业数据分析面对：

- **表级隔离**：普通员工不能查高管专属财务表
- **行级隔离**：A 部门员工不能看 B 部门薪资数据——同一张表，每行可见范围不同
- **Prompt 注入防护**：恶意用户通过构造特殊 query 绕过权限限制

这些需求靠提示词无法可靠保证，需要系统级权限管控，且要通过合规验证。企业数据安全不能建立在"大模型理解了权限要求"这个假设上。

## NL2Semantic2SQL：语义层是 Harness 核心

业界共识路径是 **NL2Semantic2SQL**，包括 OpenAI、Snowflake 等官方博文在讨论 Data Agent 时，重点都是语义层建设。

语义层的核心价值：
- 将业务知识（字段含义、枚举映射、数据质量规则、表关系）结构化为可被 Agent 调用的知识资产
- 在 NL→SQL 过程中注入业务上下文，消除幻觉
- 提供结构化关系模型约束 SQL 生成边界

这与 [[ontological-semantic-layer|本体化语义层]] 和 [[dataagent-semantic-layer|DataAgent 语义层]] 的理念一致，但 Xiaomi DiMi 的实践更强调语义层在 **Harness 工程化**中的位置——语义层不是静态知识库，而是可评测、可回归、可自进化的基础设施。

## 未来演进：评测闭环 + 自进化框架

Data + AI 的终局核心在两点：
1. **低成本的"评测-回归-迭代"闭环**
2. **自进化的、越用越懂业务的 Agent 框架**

全球第三是阶段性验证，Data + AI 真正的价值是以此为基础对业务流程的重构与变革。

## 相关页面

- [[code-agent-vs-data-agent]] — BIRD 证据直接支撑"系统设计 > 模型能力"论点
- [[ontological-semantic-layer]] — 语义层的上层概念框架
- [[dataagent-semantic-layer]] — DataWorks 语义层实物层面内容，对照 NL2Semantic2SQL
- [[data-agent-enterprise-practice]] — 蚂蚁企信 Data Agent 四层架构实践
- [[openai-data-agent]] — OpenAI 六层上下文体系解决语义正确性
- [[cloud-village-data-agent-platform]] — 网易云村 DAS Agent Platform 五层架构
- [[agent-harness-overview]] — Harness Engineering 总览
- [[infinisynapse]] — InfiniSQL + InfiniRAG Data Agent 基础设施
- [[specialized-knowledge-search]] — 语义搜索为 Data Agent 提供资产发现能力
- [[databricks-genie]] — Databricks Genie 的企业数据挑战
