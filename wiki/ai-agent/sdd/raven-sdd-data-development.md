---
title: Raven SDD 数据开发——通用 SDD 的数据管道特化
description: 网易音乐安全数据团队把 Raven SDD 落到数据开发的完整实践——6 项 gap 补充（平台操作自动化/5维度数据验证/3000 SQL 代码知识库/性能基线/data-pipeline schema/安全门控）、8 步法与三层工具链（mammut-api / spark-sql-review / mammut-task-analysis）、easy-data 双目录体系与 /sdd-explore 唯一入口
aliases: [Raven SDD, SDD 数据开发, 数据管道 SDD, data-pipeline schema, Raven SDD 能力补充, 猛犸 SDD]
tags: [ai-agent, practice]
sources: ["2026/10/10/SDD 数据开发流程 - Raven SDD 能力补充.md"]
created: 2026-10-10
updated: 2026-10-10
---

# Raven SDD 数据开发——通用 SDD 的数据管道特化

> 来源：KM 文章《SDD 数据开发流程 - Raven SDD 能力补充》（作者罗霄，音乐安全数据团队，2026-05），基于 Raven SDD + Claude Code 的数据管道开发实践。

## SDD 是什么 & 为什么用它做数据开发

SDD（Spec-Driven Development）是 **Raven 平台提出的规格驱动开发方法论**：

> 先把"要做什么"写清楚（Spec），再去写代码（Code）。

AI 既是规格的产出者，也是规格的执行者；开发过程被拆为标准化阶段，每阶段有明确的输入、产出和退出条件。

### 自动化能力（传统 vs SDD）

| 能力 | 传统方式 | SDD 自动化 |
|------|---------|-----------|
| **指标口径识别** | 人工阅读线上 SQL 理解逻辑（30-60min） | AI 以项目全部代码（~3000 个 SQL）为知识库，自动推测需求中指标的口径定义、来源表、JOIN 关系 |
| **代码生成** | 手动写 SQL，列对齐易出错 | AI 自动生成 test/production 双版本，87 列精确对齐零错误 |
| **测试表创建** | 登录猛犸 UI 手动写 DDL | AI 自动构造 DDL → mammut-api CLI 执行建表 |
| **开发模式上传** | 手动打 zip → UI 上传 → UI 执行 | AI 打包 → NeiAPI 上传 → API 提交执行 → 自动轮询结果 |
| **数据验证** | 手动写验证 SQL → 手动对比 → 手动定位差异 | AI 自动构造 5 维度验证 SQL → 提交 → 分析差异 → 下钻根因 |
| **性能测试** | 登录 Spark History 手动看指标 | mammut-task-analysis 自动拉取 SHS 数据，对比基线 |
| **文档输出** | 事后人工补写（常遗漏） | 全流程自动生成，数据从真实执行结果提取 |

### 规范化能力

| 维度 | 传统方式 | SDD 规范化 |
|------|---------|-----------|
| **需求管理** | 口头/IM/邮件，易遗忘 | PRD.md 文件化，范围/验收标准/发布形式结构化 |
| **方案设计** | 脑中构思或随手记 | design.md 标准模板，可 review、可迭代 |
| **流程一致性** | 因人而异，质量参差 | 8 步法强制统一，任何人产出格式一致 |
| **决策可追溯** | 不可追溯 | ravenspec/ 记录完整决策链（PRD→Design→Task→Code） |
| **知识沉淀** | 靠个人记忆，人走知识丢 | 踩坑→自动更新 Skill 文档→下次自动规避 |
| **协作交接** | 口头交接，信息丢失 | artifact + 变更目录，任何人可接手继续 |
| **上线安全** | 靠自觉 | 安全门控强制：禁止误操作正式表、禁止 AI 发布 |

### 效率数据（3 个需求实测）

| 工作性质 | 传统耗时 | SDD + AI | 提效 |
|----------|---------|----------|------|
| 写 SQL | 6h | 40min | **9x** |
| 平台操作 | 4h | 20min | **12x** |
| 数据验证 | 6h | 2h | **3x** |
| 写文档 | 8h | 25min | **19x** |
| 等执行（不可压缩） | 8h | 8h | 1x |
| **总体** | **~5 天** | **~2 天** | **2.5x** |

**开发者角色转变**：从"执行者"变为"决策者"——传统模式 80% 执行 + 20% 思考，SDD + AI 模式 20% 决策 + 80% AI 执行，人只需在关键节点拍板（方案确认、测试结论、发布）。

## Raven SDD 通用流程（6 阶段）

三个阶段群：**想清楚、写清楚**（1-3）→ **按清单执行**（4）→ **收敛、安全上线**（5-6）。

| 阶段 | 产出 | 一句话标准 | 相关命令 |
|------|------|-----------|---------|
| 1. 需求澄清 | PRD.md | 范围、验收标准、发布形式写清楚 | `/sdd-new-change` `/sdd-explore` `/sdd-insight` |
| 2. 技术方案 | design.md | 关键决策、异常处理、测试点明确 | `/sdd-continue-change` `/sdd-plan-review` `/sdd-challenge` |
| 3. 任务拆分 | tasks.md | 每个 task 可执行、可验证、可追踪 | `/sdd-continue-change` `/sdd-ff-change` |
| 4. 实现与测试 | 代码+测试 | tasks 全完成、功能已验证、事实同步到 spec | `/sdd-apply-change` |
| 5. Bug 修复 | 修复+回归 | 根因明确、最小修复 | `/sdd-apply-change` |
| 6. 验收发布 | 上线+归档 | 验收通过、spec 同步、change 归档 | `/sdd-verify-change` `/sdd-sync-specs` `/sdd-archive-change` |

**核心原则**：发布 ≠ 结束，必须 verify + sync + archive 才算闭环；代码、测试、文档同步推进；每阶段有退出条件，未满足不进下一步。

## 数据开发 SDD 的 6 项能力补充

Raven SDD 为通用软件开发设计，直接用于数据管道开发存在以下 gap 及对应补充：

| Gap | 通用 SDD 的问题 | 补充方案 |
|-----|----------------|---------|
| **没有"平台操作"概念** | 通用 SDD 假设代码写完 → 跑测试 → 部署；数据开发中"建表、上传任务包、提交 Spark 执行"是重度平台交互 | mammut-api Skill + TASK.md 工具标注 |
| **验证方式不匹配** | 通用 SDD 用单元测试/集成测试；数据开发的"正确"= 输出数据与线上一致，需行数对比、字段 diff、空值率等多维度验证 | 5 维度数据验证体系 |
| **缺少代码知识库** | 需求中指标口径散落在已有 SQL 代码中，通用 SDD 无此推测能力 | 项目全部代码（~3000 SQL）作为知识库 |
| **缺少性能基线对比** | 数据开发核心价值往往是性能优化（如全量→增量），通用 SDD 没有"对比线上执行耗时"的阶段 | mammut-task-analysis 性能验证（对接 SHS） |
| **Artifact 语义不贴合** | 通用 SDD 的 PRD 面向产品需求、SPEC 面向 UI/API | data-pipeline schema 特化 |
| **无安全隔离保护** | AI 直接操作正式表（INSERT OVERWRITE）一次误操作就是生产事故 | security_tmp 隔离 + 双版本 + 禁止发布 |

### data-pipeline schema 特化

| Artifact | Raven SDD（通用） | data-pipeline（数据开发） |
|----------|------------------|--------------------------|
| PRD | 产品需求 | 数据需求（表清单、字段口径、下游影响） |
| SPEC | UI/API 行为规范 | 数据逻辑规格（ETL 规则、WHEN/THEN 场景） |
| Design | 技术方案 | 数据方案（SQL 方案、Spark 配置、倾斜策略） |
| Task | 通用任务拆分 | **8 步数据开发任务**（含工具标注） |

### 8 步法（细化实现阶段）

1. **方案设计**（DESIGN.md + METRIC-SPEC.md）
2. **代码开发**（test/ + production/）
3. **测试任务开发**（建表 + 任务 + 上传）
4. **提交测试执行**
5. **数据验证**（5 维度）
6. **性能验证**
7. **上线文档**（README.md）
8. **上线准备**（git + 猛犸上传）

### 6 项核心能力

| # | 补充能力 | 说明 |
|---|---------|------|
| 1 | **代码知识库 + 口径推测** | 以项目 ~3000 个 SQL 为知识库，AI 自动推测指标口径、来源表、JOIN 关系 |
| 2 | **平台 API 自动化** | mammut-api Skill 封装猛犸全部操作，TASK.md 用 `[mammut-api]` `[NeiAPI]` `[Impala]` 标注，apply-change 自动调用 |
| 3 | **5 维度数据验证** | 行数对比、分区完整性、唯一性、空值率、字段级 diff，AI 自动构造验证 SQL + 分析差异 + 定位根因 |
| 4 | **性能验证** | mammut-task-analysis 通过 Spark History Server 自动对比执行耗时、资源、扫描量 |
| 5 | **双版本代码** | test/（→ security_tmp）+ production/（→ 正式表），test 版只改输出表名 |
| 6 | **安全门控** | 只对 security_tmp 删改；禁止 AI 发布；同步轮询结果；认证信息不入 git |

### 工具链架构（三层）

1. **SDD Skills Layer（流程编排）**：`/sdd-new-change` → `/sdd-apply-change` → `/sdd-verify-change` → `/sdd-archive-change`；负责 Artifact 生成（data-pipeline schema）+ Task 执行引擎（按工具标注自动调用）
2. **Domain Skills Layer（领域能力）**：**mammut-api**（easyolap / easydev / NeiAPI）、**spark-sql-review**（SQL 质量审查）、**mammut-task-analysis**（Spark 性能诊断）
3. **Platform Layer（猛犸平台）**：自助分析（Spark/Impala）、离线开发（Azkaban）、任务运维

## 项目目录结构

**仓库**：`g.hz.netease.com/cloudmusic/security-anti/music-gorilla-da-mammut`（Azkaban 数据工作流仓库，含 ETL 管道、特征工程、定时数据处理任务）。

### easy-data/（线上代码，猛犸 Git 自动同步）

猛犸平台配置了 **Push Git 地址**，任务上线时自动将任务包、调度配置、告警配置推送到本仓库。`easy-data/` 是线上任务的**只读镜像**，开发时不直接修改。

存储路径规则：`easy-data/task/{项目名}/{集群标识}/{UUID首字母}/{FLOW_NAME$$UUID}/`，内含 `flow.info`（Owner、releaser、发布时间）、`schedule.info`（调度周期、YARN 队列、依赖）、`alert.info`（告警规则）、`job/`（`{name}$${job_id}.job/.sql/.py`）。文件统计：~2,945 .sql | ~4,419 .job | ~3,705 .info | ~48 .py。

### easy-data-develop/（开发工作区）

每个需求一个子目录（如 `OMMUSIC-3454592/`），遵循 SDD 8 步法：

- `docs/`：DESIGN.md（步骤1）、METRIC-SPEC.md（步骤1，指标口径）、PROGRESS.md（全流程进度）、TEST-REPORT.md（步骤5）、README.md（步骤7）
- `source-mapping.yaml`：开发目录 ↔ 线上路径映射
- `flows/{flow名}/`：`original/`（线上基线，只读）、`test/`（测试版 → security_tmp）、`production/`（上线版 → 正式表）

### 两个目录的关系

`easy-data/`（线上镜像，猛犸自动 push）→ 拷贝基线到 `original/` → 基于基线开发出 `test/` → `production/` → 上线后猛犸重新 push 回 `easy-data/`。

## 如何使用

- **开始前**：先 `git pull` 拉取最新代码，确保基线是最新线上版本
- **唯一入口命令**：`/sdd-explore`——执行后 AI 引导完成后续所有阶段（需求澄清→方案设计→实现→验证→归档），无需手动输入其他 sdd 命令
- **核心注意事项**：
  1. **不要跳步**——步骤 1-8 严格顺序，未确认不进下一步
  2. **人工门控**——方案确认、测试结论、发布上线必须人工拍板
  3. **verify + archive 闭环**——不 archive = 知识没沉淀
  4. **禁止 AI 发布**——步骤 8 必须人工在猛犸 UI 操作

## 相关页面

- [[data-task-increment-optimization]] — 同团队配套实践：反垃圾大宽表全量→增量优化的完整案例（本流程的实战数据）
- [[easydata-ops-diagnosis]] — 同团队运维侧工具：mammut-task-analysis / mammut-baseline-analysis 诊断 Skill（性能验证环节的底层工具）
- [[openspec-sdd-practice]] — OpenSpec 实战：另一种 SDD 框架的落地方式对照
- [[sdd-custom-workflow]] — 自定义 SDD 工作流：schema 与命令定制机制
- [[vibops-spec-first-toolchain]] — VibOps spec-first 企业级 AI Coding 工具链（企业内 SDD 平台对照）
- [[data-dev-skills-automation]] — 数仓开发 Skills 自动化：同为 Claude Code + 平台 API 的开发自动化
- [[agent-skills-system]] — Skills 体系机制（mammut-api 等 Domain Skills 的基础）
