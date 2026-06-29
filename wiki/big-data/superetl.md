---
title: 菜鸟 SuperETL
description: 菜鸟基于 DataWorks Data Agent 的物流数仓 SuperETL 体系——Skills 编排 + Hooks 机制 + 知识库 + CLI 工具，场景开发效率提升 2-3 倍
aliases: [SuperETL, 菜鸟ETL]
tags: [big-data, ai-agent, practice]
sources: [2026/05/12/02. 菜鸟SuperETL.pdf, 2026/05/12/DataWorks DataAgent分享录音.md, 2026/06/23/菜鸟数仓未来预测.svg, 2026/06/28/菜鸟借助 DataWorks Data Agent 实现 AI 数据研发 SuperETL 落地.html]
created: 2026-05-12
updated: 2026-06-29
---

# 菜鸟 SuperETL

菜鸟 AI 平台基于 [[dataworks-data-agent|DataWorks Data Agent]] 通用底座，注入物流领域“行业 Know-how”，打造的物流数仓数据研发智能体。演讲人：董晃（菜鸟 AI 平台数据技术专家）。

## 核心变化

![数仓开发耗时分布](sources/2026/05/12/DataWorks%20数仓开发耗时分布.png)

- **研发流程痛点**：传统数仓研发流程存在三个割裂——多平台割裂（DataWorks/Flink/Paimon 各自独立）、规范虚设（规范止于文档层面）、质量难控（运维重于研发，修复成本指数增长）
- **时间分布（353 分布）**：30% 需求调研、50% 数据同步+建模+开发运维、20% 数据应用

- **开发方式的变化**：从“工具辅助”到“智能体驱动”
- **业务的深度融合**：基于 Data Agent 通用底座 + 物流行业 Know-how
- **价值体现**：部分场景开发效率提升 **2-3 倍**

## 集成架构

```
入口 → 检索 → 规划 → 开发 → 验证 → 发布
  │       │      │      │      │      │
  └───────┴──────┴──────┴──────┴──────┘
         记忆与上下文 / Skills / CLI / Hooks / Wiki
              DataWorks Data Agent 核心底座
```

Skills 作为编排层连接 Data Agent 底座与菜鸟自定义能力，包含：
- **记忆与上下文**：历史对话和项目状态持久化
- **Hooks**：部署前检查、发布阻断等自定义钩子
- **物流领域数仓规范**：行业特有的数据标准和最佳实践
- **Wiki**：菜鸟内部知识库集成

## SuperETL 技能体系

### 为什么是 9 个 Skill 而非 1 个？

菜鸟的规范、checklist、运维经验形成一个庞大的上下文。如果全部塞进一个 Skill 的 MD 文件，大模型注意力机制无法精确执行每一步。因此将体系拆分为 9 个专职子技能：

| # | 技能 | 职责 | 铁律 |
|---|------|------|------|
| 1 | **元技能 (using-superetl)** | 会话启动的意图识别 + Hook 加载，不做实际操作 | 禁止直跳子技能 |
| 2 | **DeepResearch (etl-deepresearch)** | 先搜再答——先检索物流领域经验文档，注入行业 Know-how | 先搜索后回答，禁止先问用户 |
| 3 | **Debug 模式 (etl-debugging)** | 运维场景：任务出错的诊断和修复 | 无数据证据前绝不提修复方案 |
| 4 | **需求沟通 (etl-brainstorming)** | 与用户确认指标、口径、涉及表等需求细节，压制 AI 幻觉 | 设计未确认前禁止发布 |
| 5 | **计划编写 (etl-writing-plans)** | Spec 驱动的任务编排，输出 MD 格式实施计划 | 计划确认前禁止写 SQL |
| 6 | **验证式开发 (etl-validated-coding)** | 边验证边开发，包含单元测试 | 没有验证证据的 SQL 禁止发布 |
| 7 | **评审发布 (etl-review-and-release)** | 分级 review，人工与 AI 审查结合 | 未通过检查项禁止发布，无例外 |
| 8 | **并行分派 (etl-dispatch-parallel)** | 独立子任务启动并行子 Agent 执行 | 有依赖时禁止并行 |
| 9 | **子代理驱动 (etl-subagent-driven)** | 独立子代理 + 两阶段审查 | — |

### Spec 优先级

```
用户指令 > SuperETL 技能指令 > 系统默认提示
```

用户始终掌握指令的最高优先级。

### 置信度评估机制

`etl-deepresearch` 内置置信度分级，决定后续路由：
- **90%+** → 直接回答，无需进一步确认
- **30%-90%** → 精准提问 1-2 个问题后继续
- **<30%** → 进入 `etl-brainstorming` 头脑风暴，与用户交互确认

执行链路：deepresearch（检索+置信度判断）→ brainstorming（低置信度时）→ writing-plans → validated-coding → review-and-release

### 实战推演：物流单量汇总表新增字段

以向 `dws_lgt_order_1d` 新增”签收及时率”字段为例（`sign_on_time_rate = 及时签收/总单量, DECIMAL[10,4]`），完整展示 6 步 SuperETL 实战流程及 Hook 机制：

| 步骤 | Skill | 执行内容 | Hook 机制 |
|------|-------|---------|-----------|
| 1. 意图路由 | using-superetl | 读取”新增签收及时率字段”请求，匹配触发词后路由到 deepresearch | SessionStart：inject skill system |
| 2. 拉取检索 | etl-deepresearch | 检索表结构 `dws_lgt_order_1d`，读取规范 spec/02、03，通过 dataworks skills 检索下游依赖 | spec-tracker 记录规范读取，track-skill-invocation 记录调用 |
| 3. 明确逻辑 | etl-brainstorming | 确认公式、数据类型、字段命名、数据来源 `ods_logistics_order`，用户确认设计方案 | 读取 DDL template，记录技能调用 |
| 4. 生成计划 | etl-writing-plans | ALTER TABLE ADD COLUMN + ETL SQL 修改 + 回刷方案，输出到 `docs/plans/` | 推荐 checklist，计划输出到指定目录 |
| 5. 验证开发 | etl-validated-coding | DDL+ETL SQL 变更、单元测试 + 数据验证、性能优化、etl-code-reviewer Agent 审查 | PostToolUse：spec-tracker 追踪 |
| 6. 安全发布 | etl-review-and-release | 功能验证通过、准备回滚脚本、配置 DQC+SLA 监控、`CHECKLIST_VERIFIED=1` 后发布 | deploy-check：flag 判断放行 |

## Hooks 机制

### 四个触发点

在 AI 调用千问 API 的流程中，四个关键节点可插入 Hook：

1. **SessionStart（会话启动）** — 注入 using-superetl 元技能、上下文和规范文档
2. **PreToolUse（工具调用前）** — 规范阅读追踪、检测发布/写操作触发 checklist 拦截
3. **PostToolUse（工具调用后）** — 结果校验(spec-tracker 追踪)、发布阻断
4. **SessionEnd（会话结束）** — Wiki 知识库整合、数据上报

通过 `hooks.json` 路由配置，使用 matcher 正则匹配选择 hook 脚本，由 `run-hook.cmd` 执行。

### 发布阻断机制

所有部署操作天然被阻断——第一次执行 `deploy` 命令必定报错，Hook 拦截并提示："检测到发布/写操作，必须先完成发布前检查清单。"

AI 必须先逐项完成 checklist（数据测试报告、回退方案、DQC 配置），仅当逐项验证通过、命令前携带 `CHECKLIST_VERIFIED=1` 前缀时才放行。彻底杜绝"带病上线"。

```bash
# 发布阻断脚本 d2-deploy-prehooks.sh 逻辑：
# 解析 AI 传参 → 检查是否带验证标志 → 有标志放行/无标志阻断+提示 checklist
```

## cn-odpscmd CLI 工具

菜鸟自研的统一 CLI，覆盖 ODPS/DataWorks/元数据/FBI 报表等能力。**严格区分开发环境（带 `_dev` 后缀）和生产环境**，所有 SQL 查询必须在开发环境执行。

| 能力 | 命令 |
|------|------|
| SQL 查询 | `query` 执行 SQL，`query --file` 从脚本执行，`query --output` 导出 CSV |
| DataWorks 脚本管理 | `createnode` 创建、`updatenode` 更新、`deploynode` 发布 |
| 元数据查询 | `tablemeta` 查表结构、`tablelineage` 查血缘、`tasklogs` 查日志 |
| 其他 | 权限初始化与登录、FBI 报表查询、项目空间权限查询 |

## 企业知识组装（6 大类）

菜鸟物流行业如何组织自己的 SuperETL 知识库：

| 类别 | 内容 | 示例 |
|------|------|------|
| **数据规范** | 分层架构、命名规则、任务修改规则 | 数仓分层定义、表命名规范 |
| **检查清单** | 防 AI 随意修改线上的 checklist | 发布前校验项、任务修改后验证项 |
| **模板** | DDL/SQL 标准化模板 | 建表语句模板、12 个 SQL 模板 |
| **理论指南** | 建模方法论 | 维度建模、Medallion 架构 |
| **技术技巧** | SQL 优化、运维经验 | 索引优化、参数调优 |
| **领域知识** | 物流行业术语 | 自派揽派、小件员、签收及时率 |

## 知识库

菜鸟沉淀的领域经验结构化为：
- `识库.md` — 物流数据领域知识
- `层规划域.md` — 数仓分层规划规范
- `任务发布checklist.md` — 发布上线检查清单

## 展望

### AI 时代数据研发的变与不变

![AI时代数据网格](sources/2026/05/12/DataWorks%20SuperETL中AI时代数据网格.png)

**不变的核心**：
- **数据分层架构**：ODS→DWD→DWS→ADS（或 Medallion 青铜→白银→黄金），数据质量逐层提升
- **维度建模方式**：时间+地点+人物+事件的结构化描述，对 AI 同样友好

**正在变化的方向**：
- **网格化管理**：按数据域组织中间层（交易、物流、LLM 数据域），按业务场景组装 ADS 层，避免"把 3000 张表全扔给 AI"
- **LLM 数据域**：将大模型调用、成本、时效纳入数据平台治理，作为独立数据域管理
- **Wiki 即交付物**：建表的同时维护好语义 Wiki（类似 Ontology/本体论），让消费者无需理解底层表结构
- **应用层全面 AI 化**：传统 BI 看板之外，新增 AI Skills（自然语言知识检索）、AI Reports（自动生成经营分析）、System Apps（数据驱动业务动作）
- **交付物变化**：从"交付表/报表"转为交付"AI 分析 Skill + 深度分析报告"，包含分析思路沉淀和鉴权机制
- **闭环**：源系统采集 → 域化建模 → 知识化沉淀 → AI 可用 → 应用自动化

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 产品架构
- [[data-dev-skills-automation]] — 数仓开发 Skills 自动化参考
- [[data-agent-practice-guide]] — 数据智能体实践指南
- [[flink-skills]] — Flink 的类似 Skill 编排实践
- [[cainiao-data-warehouse-architecture]] — 菜鸟数仓未来架构（Data Mesh 化的演进方向）
