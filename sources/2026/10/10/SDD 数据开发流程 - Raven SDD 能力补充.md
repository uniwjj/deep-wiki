---
ingested: 2026-10-10
wiki_pages: [raven-sdd-data-development]
source_url: https://km.netease.com/v4/detail/blog/269706
author: 罗霄
---

# SDD 数据开发流程实践分享

> 基于 Raven SDD + Claude Code 的数据管道开发实践
> 音乐安全数据团队 | 2026-05

---

## 一、SDD 是什么 & 为什么用它做数据开发

### 1.1 SDD 简介

SDD（Spec-Driven Development）是 Raven 平台提出的**规格驱动开发方法论**：

> 先把"要做什么"写清楚（Spec），再去写代码（Code）。

AI 既是规格的产出者，也是规格的执行者。开发过程被拆为标准化阶段，每阶段有明确的输入、产出和退出条件。

### 1.2 SDD 带来的自动化能力

传统数据开发中大量时间花在重复性执行工作上。SDD 模式通过 AI + Skills 实现了端到端自动化：

| 能力 | 传统方式 | SDD 自动化 |
|------|---------|-----------|
| **指标口径识别** | 人工阅读线上 SQL 理解逻辑（30-60min） | AI 以项目全部代码（~3000 个 SQL）为知识库，自动推测需求中指标的口径定义、来源表、JOIN 关系 |
| **代码生成** | 手动写 SQL，列对齐易出错 | AI 自动生成 test/production 双版本，87 列精确对齐零错误 |
| **测试表创建** | 登录猛犸 UI 手动写 DDL | AI 自动构造 DDL → mammut-api CLI 执行建表 |
| **开发模式上传** | 手动打 zip → UI 上传 → UI 执行 | AI 打包 → NeiAPI 上传 → API 提交执行 → 自动轮询结果 |
| **数据验证** | 手动写验证 SQL → 手动对比 → 手动定位差异 | AI 自动构造 5 维度验证 SQL → 提交 → 分析差异 → 下钻根因 |
| **性能测试** | 登录 Spark History 手动看指标 | mammut-task-analysis 自动拉取 SHS 数据，对比基线 |
| **文档输出** | 事后人工补写（常遗漏） | 全流程自动生成，数据从真实执行结果提取 |

### 1.3 SDD 带来的规范化能力

| 维度 | 传统方式 | SDD 规范化 |
|------|---------|-----------|
| **需求管理** | 口头/IM/邮件，易遗忘 | PRD.md 文件化，范围/验收标准/发布形式结构化 |
| **方案设计** | 脑中构思或随手记 | design.md 标准模板，可 review、可迭代 |
| **流程一致性** | 因人而异，质量参差 | 8 步法强制统一，任何人产出格式一致 |
| **决策可追溯** | 不可追溯 | ravenspec/ 记录完整决策链（PRD→Design→Task→Code） |
| **知识沉淀** | 靠个人记忆，人走知识丢 | 踩坑→自动更新 Skill 文档→下次自动规避 |
| **协作交接** | 口头交接，信息丢失 | artifact + 变更目录，任何人可接手继续 |
| **上线安全** | 靠自觉 | 安全门控强制：禁止误操作正式表、禁止 AI 发布 |

### 1.4 效率数据（3 个需求实测）

| 工作性质 | 传统耗时 | SDD + AI | 提效 |
|----------|---------|----------|------|
| 写 SQL | 6h | 40min | **9x** |
| 平台操作 | 4h | 20min | **12x** |
| 数据验证 | 6h | 2h | **3x** |
| 写文档 | 8h | 25min | **19x** |
| 等执行（不可压缩） | 8h | 8h | 1x |
| **总体** | **~5 天** | **~2 天** | **2.5x** |

### 1.5 开发者角色转变

<div style="margin:1rem 0;">
<p><strong>传统：</strong></p>
<div style="display:flex;height:24px;border-radius:4px;overflow:hidden;margin:0.5rem 0;">
<div style="width:80%;background:#ef4444;display:flex;align-items:center;padding:0 8px;color:white;font-size:12px;font-weight:600;">80% 执行</div>
<div style="width:20%;background:#2563eb;display:flex;align-items:center;padding:0 8px;color:white;font-size:12px;font-weight:600;">20% 思考</div>
</div>
<p><strong>SDD + AI：</strong></p>
<div style="display:flex;height:24px;border-radius:4px;overflow:hidden;margin:0.5rem 0;">
<div style="width:20%;background:#2563eb;display:flex;align-items:center;padding:0 8px;color:white;font-size:12px;font-weight:600;">20% 决策</div>
<div style="width:80%;background:#16a34a;display:flex;align-items:center;padding:0 8px;color:white;font-size:12px;font-weight:600;">80% AI 执行</div>
</div>
</div>

从"执行者"变为"决策者"——只需在关键节点拍板（方案确认、测试结论、发布），其余全由 AI 完成。

---

## 二、Raven SDD 通用流程

> 详细文档：https://raven.netease.com/st/sdd/docs/development/requirement/overview

### 6 阶段总览

<div style="display:flex;align-items:center;gap:0.5rem;margin:1.5rem 0;flex-wrap:wrap;">
<div style="background:#dbeafe;border:2px solid #2563eb;border-radius:10px;padding:1rem 1.2rem;text-align:center;min-width:140px;">
<div style="font-weight:700;font-size:0.85rem;color:#1e40af;">想清楚、写清楚</div>
<div style="font-size:0.8rem;color:#374151;margin-top:0.3rem;">1.需求澄清<br>2.技术方案<br>3.任务拆分</div>
</div>
<div style="font-size:1.5rem;color:#6b7280;">→</div>
<div style="background:#dcfce7;border:2px solid #16a34a;border-radius:10px;padding:1rem 1.2rem;text-align:center;min-width:140px;">
<div style="font-weight:700;font-size:0.85rem;color:#166534;">按清单执行</div>
<div style="font-size:0.8rem;color:#374151;margin-top:0.3rem;">4.实现与测试</div>
</div>
<div style="font-size:1.5rem;color:#6b7280;">→</div>
<div style="background:#fef3c7;border:2px solid #d97706;border-radius:10px;padding:1rem 1.2rem;text-align:center;min-width:140px;">
<div style="font-weight:700;font-size:0.85rem;color:#92400e;">收敛、安全上线</div>
<div style="font-size:0.8rem;color:#374151;margin-top:0.3rem;">5.Bug 修复<br>6.验收 & 发布</div>
</div>
</div>

| 阶段 | 产出 | 一句话标准 | 相关命令 |
|------|------|-----------|---------|
| 1. 需求澄清 | PRD.md | 范围、验收标准、发布形式写清楚 | `/sdd-new-change` `/sdd-explore` `/sdd-insight` |
| 2. 技术方案 | design.md | 关键决策、异常处理、测试点明确 | `/sdd-continue-change` `/sdd-plan-review` `/sdd-challenge` |
| 3. 任务拆分 | tasks.md | 每个 task 可执行、可验证、可追踪 | `/sdd-continue-change` `/sdd-ff-change` |
| 4. 实现与测试 | 代码+测试 | tasks 全完成、功能已验证、事实同步到 spec | `/sdd-apply-change` |
| 5. Bug 修复 | 修复+回归 | 根因明确、最小修复 | `/sdd-apply-change` |
| 6. 验收发布 | 上线+归档 | 验收通过、spec 同步、change 归档 | `/sdd-verify-change` `/sdd-sync-specs` `/sdd-archive-change` |

**核心原则**：
- 发布 ≠ 结束，必须 verify + sync + archive 才算闭环
- 代码、测试、文档同步推进
- 每阶段有退出条件，未满足不进下一步

---

## 三、数据开发 SDD 对 Raven SDD 的补充

### 3.1 Raven SDD 在数据开发中遇到的问题

Raven SDD 为通用软件开发设计，直接用于数据管道开发时存在以下 gap：

<div style="display:grid;grid-template-columns:repeat(3,1fr);gap:1rem;margin:1.5rem 0;">
<div style="background:#fef2f2;border:1px solid #fecaca;border-radius:8px;padding:1rem;">
<div style="font-size:1.2rem;">❌</div>
<div style="font-weight:700;color:#991b1b;margin:0.3rem 0;">没有"平台操作"概念</div>
<div style="font-size:0.85rem;color:#374151;">通用 SDD 假设代码写完 → 跑测试 → 部署。数据开发中"建表、上传任务包、提交 Spark 执行"是重度平台交互。</div>
<div style="margin-top:0.5rem;"><span style="display:inline-block;background:#dcfce7;color:#166534;font-size:0.75rem;padding:0.1rem 0.5rem;border-radius:4px;">→ mammut-api Skill + TASK.md 工具标注</span></div>
</div>
<div style="background:#fef2f2;border:1px solid #fecaca;border-radius:8px;padding:1rem;">
<div style="font-size:1.2rem;">❌</div>
<div style="font-weight:700;color:#991b1b;margin:0.3rem 0;">验证方式不匹配</div>
<div style="font-size:0.85rem;color:#374151;">通用 SDD 用单元测试/集成测试。数据开发的"正确"= 输出数据与线上一致，需行数对比、字段 diff、空值率等多维度验证。</div>
<div style="margin-top:0.5rem;"><span style="display:inline-block;background:#dcfce7;color:#166534;font-size:0.75rem;padding:0.1rem 0.5rem;border-radius:4px;">→ 5 维度数据验证体系</span></div>
</div>
<div style="background:#fef2f2;border:1px solid #fecaca;border-radius:8px;padding:1rem;">
<div style="font-size:1.2rem;">❌</div>
<div style="font-weight:700;color:#991b1b;margin:0.3rem 0;">缺少代码知识库</div>
<div style="font-size:0.85rem;color:#374151;">需求中指标口径散落在已有 SQL 代码中。通用 SDD 没有"以项目代码为知识库推测指标口径"的能力。</div>
<div style="margin-top:0.5rem;"><span style="display:inline-block;background:#dcfce7;color:#166534;font-size:0.75rem;padding:0.1rem 0.5rem;border-radius:4px;">→ 项目全部代码（~3000 SQL）作为知识库</span></div>
</div>
<div style="background:#fef2f2;border:1px solid #fecaca;border-radius:8px;padding:1rem;">
<div style="font-size:1.2rem;">❌</div>
<div style="font-weight:700;color:#991b1b;margin:0.3rem 0;">缺少性能基线对比</div>
<div style="font-size:0.85rem;color:#374151;">数据开发核心价值往往是性能优化（如全量→增量）。通用 SDD 没有"对比线上执行耗时"的阶段，也无法对接 SHS。</div>
<div style="margin-top:0.5rem;"><span style="display:inline-block;background:#dcfce7;color:#166534;font-size:0.75rem;padding:0.1rem 0.5rem;border-radius:4px;">→ mammut-task-analysis 性能验证</span></div>
</div>
<div style="background:#fef2f2;border:1px solid #fecaca;border-radius:8px;padding:1rem;">
<div style="font-size:1.2rem;">❌</div>
<div style="font-weight:700;color:#991b1b;margin:0.3rem 0;">Artifact 语义不贴合</div>
<div style="font-size:0.85rem;color:#374151;">通用 SDD 的 PRD 面向产品需求、SPEC 面向 UI/API。数据开发需要表清单、字段口径、ETL 转换规则。</div>
<div style="margin-top:0.5rem;"><span style="display:inline-block;background:#dcfce7;color:#166534;font-size:0.75rem;padding:0.1rem 0.5rem;border-radius:4px;">→ data-pipeline schema 特化</span></div>
</div>
<div style="background:#fef2f2;border:1px solid #fecaca;border-radius:8px;padding:1rem;">
<div style="font-size:1.2rem;">❌</div>
<div style="font-weight:700;color:#991b1b;margin:0.3rem 0;">无安全隔离保护</div>
<div style="font-size:0.85rem;color:#374151;">AI 如果直接操作正式表（INSERT OVERWRITE），一次误操作就是生产事故。通用 SDD 没有数据安全隔离机制。</div>
<div style="margin-top:0.5rem;"><span style="display:inline-block;background:#dcfce7;color:#166534;font-size:0.75rem;padding:0.1rem 0.5rem;border-radius:4px;">→ security_tmp 隔离 + 双版本 + 禁止发布</span></div>
</div>
</div>

### 3.2 data-pipeline schema 特化

| Artifact | Raven SDD（通用） | data-pipeline（数据开发） |
|----------|------------------|--------------------------|
| PRD | 产品需求 | 数据需求（表清单、字段口径、下游影响） |
| SPEC | UI/API 行为规范 | 数据逻辑规格（ETL 规则、WHEN/THEN 场景） |
| Design | 技术方案 | 数据方案（SQL 方案、Spark 配置、倾斜策略） |
| Task | 通用任务拆分 | **8 步数据开发任务**（含工具标注） |

### 3.3 8 步法细化实现阶段

<div style="display:grid;grid-template-columns:auto auto 1fr;gap:0.5rem;align-items:center;margin:1rem 0;">
<div style="text-align:right;padding:0.4rem 0.8rem;background:#eff6ff;border-radius:6px;font-size:0.85rem;"><strong>阶段 1-3</strong> 想清楚</div>
<div style="color:#6b7280;font-size:1.2rem;">→</div>
<div style="padding:0.4rem 0.8rem;background:#f0fdf4;border-radius:6px;font-size:0.85rem;">步骤 1: 方案设计（DESIGN.md + METRIC-SPEC.md）</div>

<div style="text-align:right;padding:0.4rem 0.8rem;background:#eff6ff;border-radius:6px;font-size:0.85rem;"><strong>阶段 4</strong> 执行</div>
<div style="color:#6b7280;font-size:1.2rem;">→</div>
<div style="padding:0.4rem 0.8rem;background:#f0fdf4;border-radius:6px;font-size:0.85rem;">步骤 2: 代码开发（test/ + production/）</div>

<div style="text-align:right;padding:0.4rem 0.8rem;font-size:0.85rem;"></div>
<div></div>
<div style="padding:0.4rem 0.8rem;background:#f0fdf4;border-radius:6px;font-size:0.85rem;">步骤 3: 测试任务开发（建表 + 任务 + 上传）</div>

<div style="text-align:right;padding:0.4rem 0.8rem;font-size:0.85rem;"></div>
<div></div>
<div style="padding:0.4rem 0.8rem;background:#f0fdf4;border-radius:6px;font-size:0.85rem;">步骤 4: 提交测试执行</div>

<div style="text-align:right;padding:0.4rem 0.8rem;font-size:0.85rem;"></div>
<div></div>
<div style="padding:0.4rem 0.8rem;background:#f0fdf4;border-radius:6px;font-size:0.85rem;">步骤 5: 数据验证（5 维度）</div>

<div style="text-align:right;padding:0.4rem 0.8rem;font-size:0.85rem;"></div>
<div></div>
<div style="padding:0.4rem 0.8rem;background:#f0fdf4;border-radius:6px;font-size:0.85rem;">步骤 6: 性能验证</div>

<div style="text-align:right;padding:0.4rem 0.8rem;background:#eff6ff;border-radius:6px;font-size:0.85rem;"><strong>阶段 5-6</strong> 上线</div>
<div style="color:#6b7280;font-size:1.2rem;">→</div>
<div style="padding:0.4rem 0.8rem;background:#f0fdf4;border-radius:6px;font-size:0.85rem;">步骤 7: 上线文档（README.md）</div>

<div style="text-align:right;padding:0.4rem 0.8rem;font-size:0.85rem;"></div>
<div></div>
<div style="padding:0.4rem 0.8rem;background:#f0fdf4;border-radius:6px;font-size:0.85rem;">步骤 8: 上线准备（git + 猛犸上传）</div>
</div>

### 3.4 补充的 6 项核心能力

| # | 补充能力 | 说明 |
|---|---------|------|
| 1 | **代码知识库 + 口径推测** | 以项目 ~3000 个 SQL 为知识库，AI 自动推测指标口径、来源表、JOIN 关系 |
| 2 | **平台 API 自动化** | mammut-api Skill 封装猛犸全部操作，TASK.md 用 `[mammut-api]` `[NeiAPI]` `[Impala]` 标注，apply-change 自动调用 |
| 3 | **5 维度数据验证** | 行数对比、分区完整性、唯一性、空值率、字段级 diff，AI 自动构造验证 SQL + 分析差异 + 定位根因 |
| 4 | **性能验证** | mammut-task-analysis 通过 Spark History Server 自动对比执行耗时、资源、扫描量 |
| 5 | **双版本代码** | test/（→ security_tmp）+ production/（→ 正式表），test 版只改输出表名 |
| 6 | **安全门控** | 只对 security_tmp 删改；禁止 AI 发布；同步轮询结果；认证信息不入 git |

### 3.5 工具链架构

<div style="margin:1rem 0;">
<div style="border:2px solid #2563eb;border-radius:12px;padding:1rem 1.5rem;background:#eff6ff;margin-bottom:0.5rem;">
<div style="font-weight:700;font-size:0.85rem;color:#1e40af;margin-bottom:0.5rem;">SDD Skills Layer（流程编排）</div>
<div style="display:flex;gap:0.8rem;flex-wrap:wrap;">
<span style="background:white;border:1px solid #d1d5db;border-radius:8px;padding:0.4rem 0.8rem;font-size:0.8rem;">/sdd-new-change</span>
<span style="font-size:1rem;color:#6b7280;">→</span>
<span style="background:white;border:1px solid #d1d5db;border-radius:8px;padding:0.4rem 0.8rem;font-size:0.8rem;">/sdd-apply-change</span>
<span style="font-size:1rem;color:#6b7280;">→</span>
<span style="background:white;border:1px solid #d1d5db;border-radius:8px;padding:0.4rem 0.8rem;font-size:0.8rem;">/sdd-verify-change</span>
<span style="font-size:1rem;color:#6b7280;">→</span>
<span style="background:white;border:1px solid #d1d5db;border-radius:8px;padding:0.4rem 0.8rem;font-size:0.8rem;">/sdd-archive-change</span>
</div>
<p style="font-size:0.8rem;margin:0.5rem 0 0;color:#374151;">Artifact 生成（data-pipeline schema）+ Task 执行引擎（按工具标注自动调用）</p>
</div>
<div style="text-align:center;font-size:1.2rem;color:#6b7280;">▼</div>
<div style="border:2px solid #16a34a;border-radius:12px;padding:1rem 1.5rem;background:#f0fdf4;margin-bottom:0.5rem;">
<div style="font-weight:700;font-size:0.85rem;color:#166534;margin-bottom:0.5rem;">Domain Skills Layer（领域能力）</div>
<div style="display:flex;gap:1rem;flex-wrap:wrap;">
<span style="background:white;border:1px solid #d1d5db;border-radius:8px;padding:0.4rem 0.8rem;font-size:0.8rem;"><strong>mammut-api</strong><br><small>easyolap / easydev / NeiAPI</small></span>
<span style="background:white;border:1px solid #d1d5db;border-radius:8px;padding:0.4rem 0.8rem;font-size:0.8rem;"><strong>spark-sql-review</strong><br><small>SQL 质量审查</small></span>
<span style="background:white;border:1px solid #d1d5db;border-radius:8px;padding:0.4rem 0.8rem;font-size:0.8rem;"><strong>mammut-task-analysis</strong><br><small>Spark 性能诊断</small></span>
</div>
</div>
<div style="text-align:center;font-size:1.2rem;color:#6b7280;">▼</div>
<div style="border:2px solid #d97706;border-radius:12px;padding:1rem 1.5rem;background:#fffbeb;">
<div style="font-weight:700;font-size:0.85rem;color:#92400e;margin-bottom:0.5rem;">Platform Layer（猛犸平台）</div>
<div style="display:flex;gap:1rem;flex-wrap:wrap;">
<span style="background:white;border:1px solid #d1d5db;border-radius:8px;padding:0.4rem 0.8rem;font-size:0.8rem;">自助分析（Spark/Impala）</span>
<span style="background:white;border:1px solid #d1d5db;border-radius:8px;padding:0.4rem 0.8rem;font-size:0.8rem;">离线开发（Azkaban）</span>
<span style="background:white;border:1px solid #d1d5db;border-radius:8px;padding:0.4rem 0.8rem;font-size:0.8rem;">任务运维</span>
</div>
</div>
</div>

---

## 四、项目目录结构

### 4.1 仓库概况

**仓库**：`g.hz.netease.com/cloudmusic/security-anti/music-gorilla-da-mammut`

Azkaban 数据工作流仓库，包含 ETL 管道、特征工程、定时数据处理任务。

### 4.2 easy-data/（线上代码，猛犸 Git 自动同步）

> 参考：https://study.sf.163.com/documents/read/easydev/date20230915162917.md

猛犸平台配置了 **Push Git 地址**，任务上线时自动将任务包、调度配置、告警配置推送到本仓库。`easy-data/` 是线上任务的**只读镜像**，开发时不直接修改。

存储路径规则：`easy-data/task/{项目名}/{集群标识}/{UUID首字母}/{FLOW_NAME$$UUID}/`

<div style="background:#f9fafb;border:1px solid #e5e7eb;border-radius:8px;padding:1rem 1.5rem;font-family:monospace;font-size:0.85rem;line-height:1.8;">
easy-data/task/music_security/hz11/{0-9,a-f}/<br>
&nbsp;&nbsp;└── {FLOW_NAME}$${UUID}/<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── flow.info <span style="color:#6b7280;"># Owner、releaser、发布时间</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── schedule.info <span style="color:#6b7280;"># 调度周期、YARN 队列、依赖</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── alert.info <span style="color:#6b7280;"># 告警规则</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── job/<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── {name}$${job_id}.job <span style="color:#6b7280;"># Job 配置</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── {name}$${job_id}.sql <span style="color:#6b7280;"># SQL 逻辑</span><br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── {name}$${job_id}.py <span style="color:#6b7280;"># PySpark 脚本</span>
</div>

文件统计：~2,945 .sql | ~4,419 .job | ~3,705 .info | ~48 .py

### 4.3 easy-data-develop/（开发工作区）

每个需求一个子目录，遵循 SDD 8 步法：

<div style="background:#f9fafb;border:1px solid #e5e7eb;border-radius:8px;padding:1rem 1.5rem;font-family:monospace;font-size:0.85rem;line-height:1.8;">
easy-data-develop/<br>
├── docs/ <span style="color:#6b7280;"># 团队文档</span><br>
│&nbsp;&nbsp;&nbsp;├── SDD-SPEC.md <span style="color:#6b7280;"># 数据开发完整规范</span><br>
│&nbsp;&nbsp;&nbsp;└── TECH-SHARING*.md <span style="color:#6b7280;"># 实践分享</span><br>
├── OMMUSIC-3454592/ <span style="color:#6b7280;"># 需求：反垃圾宽表增量优化</span><br>
│&nbsp;&nbsp;&nbsp;├── docs/<br>
│&nbsp;&nbsp;&nbsp;│&nbsp;&nbsp;&nbsp;├── DESIGN.md <span style="color:#6b7280;"># 步骤1 产出</span><br>
│&nbsp;&nbsp;&nbsp;│&nbsp;&nbsp;&nbsp;├── METRIC-SPEC.md <span style="color:#6b7280;"># 步骤1 产出（指标口径）</span><br>
│&nbsp;&nbsp;&nbsp;│&nbsp;&nbsp;&nbsp;├── PROGRESS.md <span style="color:#6b7280;"># 全流程进度</span><br>
│&nbsp;&nbsp;&nbsp;│&nbsp;&nbsp;&nbsp;├── TEST-REPORT.md <span style="color:#6b7280;"># 步骤5 产出</span><br>
│&nbsp;&nbsp;&nbsp;│&nbsp;&nbsp;&nbsp;└── README.md <span style="color:#6b7280;"># 步骤7 产出</span><br>
│&nbsp;&nbsp;&nbsp;├── source-mapping.yaml <span style="color:#6b7280;"># 开发目录 ↔ 线上路径</span><br>
│&nbsp;&nbsp;&nbsp;└── flows/{flow名}/<br>
│&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── original/ <span style="color:#6b7280;"># 线上基线（只读）</span><br>
│&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;├── test/ <span style="color:#6b7280;"># 测试版（→ security_tmp）</span><br>
│&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└── production/ <span style="color:#6b7280;"># 上线版（→ 正式表）</span><br>
├── OMMUSIC-3404598/ <span style="color:#6b7280;"># 需求：冲塔资源召回</span><br>
└── OMMUSIC-3468999/ <span style="color:#6b7280;"># 需求：fake2 打标变更</span>
</div>

### 4.4 两个目录的关系

<div style="text-align:center;margin:1.5rem 0;">
<div style="display:inline-block;text-align:left;">
<div style="background:#fef3c7;border:2px solid #d97706;border-radius:8px;padding:0.8rem 1.5rem;display:inline-block;">
<strong>easy-data/</strong>（线上镜像，猛犸自动 push）
</div>
<div style="padding-left:3rem;color:#6b7280;">│<br>│ 拷贝基线到 original/<br>▼</div>
<div style="background:#eff6ff;border:2px solid #2563eb;border-radius:8px;padding:0.8rem 1.5rem;display:inline-block;">
<strong>easy-data-develop/</strong> flows/{flow}/original/
</div>
<div style="padding-left:3rem;color:#6b7280;">│<br>│ 基于基线开发<br>▼</div>
<div style="background:#dcfce7;border:2px solid #16a34a;border-radius:8px;padding:0.8rem 1.5rem;display:inline-block;">
<strong>test/</strong>（测试）→ <strong>production/</strong>（上线版）
</div>
<div style="padding-left:3rem;color:#6b7280;">│<br>│ 上线后猛犸重新 push<br>▼</div>
<div style="background:#fef3c7;border:2px solid #d97706;border-radius:8px;padding:0.8rem 1.5rem;display:inline-block;">
<strong>easy-data/</strong>（自动更新）
</div>
</div>
</div>

---

## 五、如何使用

### 5.1 开始前

<div style="background:#fef3c7;border-left:4px solid #d97706;padding:1rem 1.5rem;border-radius:4px;margin-bottom:1.5rem;">
<strong>⚠️ 记得先拉取最新代码</strong>：<code>git pull</code>，确保基线是最新线上版本。
</div>

### 5.2 唯一入口命令

只需使用一个命令即可启动整个流程：

<div style="background:#1e293b;color:#e2e8f0;padding:1.5rem 2rem;border-radius:8px;font-size:1.3rem;text-align:center;margin:1.5rem 0;">
<code style="color:#67e8f9;font-size:1.4rem;">/sdd-explore</code>
</div>

执行后 AI 会引导你完成后续所有阶段（需求澄清→方案设计→实现→验证→归档），**无需手动输入其他 sdd 命令**，跟着 AI 的引导操作即可。

### 5.3 完整流程参考

<p style="color:#6b7280;font-size:0.9rem;">以下是 AI 会自动引导你走过的完整流程（仅供了解，无需手动执行）：</p>

<div style="display:flex;align-items:center;gap:0.5rem;margin:1.5rem 0;flex-wrap:wrap;">
<div style="background:#dbeafe;border:2px solid #2563eb;border-radius:10px;padding:1rem 1.2rem;text-align:center;min-width:120px;">
<div style="font-weight:700;font-size:0.85rem;color:#1e40af;">探索</div>
<div style="font-size:0.8rem;color:#374151;margin-top:0.3rem;"><code>/sdd-explore</code></div>
</div>
<div style="font-size:1.5rem;color:#6b7280;">→</div>
<div style="background:#dbeafe;border:2px solid #2563eb;border-radius:10px;padding:1rem 1.2rem;text-align:center;min-width:120px;">
<div style="font-weight:700;font-size:0.85rem;color:#1e40af;">创建变更</div>
<div style="font-size:0.8rem;color:#374151;margin-top:0.3rem;">自动触发</div>
</div>
<div style="font-size:1.5rem;color:#6b7280;">→</div>
<div style="background:#dcfce7;border:2px solid #16a34a;border-radius:10px;padding:1rem 1.2rem;text-align:center;min-width:120px;">
<div style="font-weight:700;font-size:0.85rem;color:#166534;">实现（8步法）</div>
<div style="font-size:0.8rem;color:#374151;margin-top:0.3rem;">AI 逐步引导</div>
</div>
<div style="font-size:1.5rem;color:#6b7280;">→</div>
<div style="background:#fef3c7;border:2px solid #d97706;border-radius:10px;padding:1rem 1.2rem;text-align:center;min-width:120px;">
<div style="font-weight:700;font-size:0.85rem;color:#92400e;">验证 & 归档</div>
<div style="font-size:0.8rem;color:#374151;margin-top:0.3rem;">AI 引导完成</div>
</div>
</div>

### 5.4 核心注意事项

| # | 规则 | 说明 |
|---|------|------|
| 1 | **不要跳步** | 步骤 1-8 严格顺序，未确认不进下一步 |
| 2 | **人工门控** | 方案确认、测试结论、发布上线必须人工拍板 |
| 3 | **verify + archive 闭环** | 不 archive = 知识没沉淀 |
| 4 | **禁止 AI 发布** | 步骤 8 必须人工在猛犸 UI 操作 |

---

## 附录：相关文档

| 文档 | 位置 |
|------|------|
| Raven SDD 官方文档 | https://raven.netease.com/st/sdd/docs/development/requirement/overview |
| 猛犸 Git 同步说明 | https://study.sf.163.com/documents/read/easydev/date20230915162917.md |
| 数据开发完整规范 | `easy-data-develop/docs/SDD-SPEC.md` |
| 实践总结（3 需求） | `easy-data-develop/docs/TECH-SHARING.md` |

