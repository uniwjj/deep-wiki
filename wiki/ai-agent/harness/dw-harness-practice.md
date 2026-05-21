---
title: 数仓 Harness 工程落地方案
description: 得物技术基于 Claude Code Harness 的数仓侧落地方案——五层防御体系、8步 SKILL 工作流、hooks 自动验证、subagent 上下文隔离
aliases: [DW Harness, 数仓Harness, 数仓侧Harness落地方案]
tags: [ai-agent, practice, big-data]
sources: [2026/05/21/Claude Code Harness 工程：数仓侧落地方案｜得物技术.md]
created: 2026-05-21
updated: 2026-05-21
---

# 数仓 Harness 工程落地方案

> 来源：得物技术 · 丹克 · 2026-05-20

## 背景与痛点

得物离线数仓各小组已基本完成 AI Coding 工具覆盖，主力工具为 [[claude-code|Claude Code]]。但日常使用中暴露出三类结构性痛点：

| 痛点 | 描述 | 根因 |
|------|------|------|
| **上下文失忆** | 临时口述的约束（如“金额单位是千元”）compact 后丢失，生成的 SQL 数据差 1000 倍 | [[claude-code-context-compaction|context compact]] 机制的系统性限制 |
| **规范执行不稳定** | 人工规范遵守率降至 60%~70%，AI 靠 prompt 记忆也只有 70%~80% | 规范靠“记忆”而非“强制检查” |
| **context 膨胀** | 复杂需求中血缘查询(500~3000 tokens) + 自测结果(5000~15000 tokens) + SKILL 文件(~10000 tokens) 迅速撑满 context，compact 后关键约束遗忘 | 高 token 操作与主 context 混在一起 |

核心矛盾：越是复杂的需求，越依赖 AI；但越复杂的需求，context 越容易撑满，AI 越容易“失忆”。

**核心原则**：规范执行是人的短板、AI 的长板；业务判断是 AI 的短板、人的长板。

## Harness 是什么

在 Claude Code 中，Harness = **Claude Code 的宿主运行框架**（工具链容器）。它：

- 管理 context window 生命周期
- 在 LLM 推理循环之外确定性地执行 hooks
- 协调 subagents 的生命周期
- 不依赖模型判断，直接执行配置的自动化行为

参见 [[claude-code-harness|Claude Code Harness 架构]] 和 [[agent-harness-overview|Agent Harness 综述]]。

## Compact 对 DW 的影响

当 context 接近满时 auto-compact 触发（默认 95%），整个对话历史被替换为摘要，token 缩减到约 12%。DW 场景最痛的是：

- 对话中说的“这次用 OVERWRITE 模式”、“先忽略 field_a 字段” → compact 后全忘
- SKILL 文件读了一半，compact 后前几个 step 的内容没了 → Claude 重复询问
- 自测跑出 50 行结果 + 血缘查询的几十行表结构 → context 迅速膨胀到 compact 阈值

参见 [[claude-code-context-compaction|Claude Code 上下文压缩]]。

## 五层防御体系

从简单到复杂，层层递进：

### 第一层：CLAUDE.md 持久化

**机制**：项目根目录 `.claude/CLAUDE.md` 每次 compact 后从磁盘重新注入，是最可靠的持久化位置。

格式建议：

```markdown
# 当前迭代状态（每次迭代手动更新）

## 正在开发
- 表：db_a.dws_table_a
- 版本：V1.0
- node_id：1000000001
- 状态：ETL开发阶段（Step 3/8）

## 本次迭代约束
- 禁止修改：dwd_table_b（已上线，只读）
- 分区字段：partition_dt（格式 yyyyMMdd，不是 dt）
- amount 字段单位：千元（不是元）

## 数仓全局规范
- 建表：分区字段必须是 partition_dt string
- 禁止：SELECT *，UPDATE/DELETE 无 WHERE
- 金额字段用 DECIMAL(20,4)，不用 DOUBLE
- INSERT 必须带 PARTITION 子句
```

**操作规则**：新迭代开始时更新“正在开发”和“本次迭代约束”两节；上线后清空约束节；全局规范长期保留，控制在 100 行以内。

### 第二层：Auto Memory 自动积累

**机制**：Claude 自动将跨会话发现写入 `~/.claude/projects/<project>/memory/MEMORY.md`，每次 compact 后重新注入。

主动触发时机：
- “这张表的 amount 字段单位是千元，请记住”
- “field_a 在特定场景下会为空，请记住这个踩坑”
- “本次 V1.0 的关键变更是 field_b 逻辑调整，请记住”

参见 [[claude-code-memory-system|Claude Code 记忆系统]]。

### 第三层：Hooks 自动验证（核心防御）

解决“每次写完 SQL 自动检查”的关键机制。Hook 在 Harness 层确定性触发，不依赖 Claude 是否记住规范。

**关键配置**（`.claude/settings.json`）：

| Hook 类型 | 匹配器 | 作用 |
|-----------|--------|------|
| `PostToolUse` | `Write\|Edit` | 写 .sql 文件后自动运行规范检查 |
| `PreToolUse` | `Bash` | 执行 bash 前拦截危险 DDL |
| `SessionStart` | `compact` | compact 后自动重注入数仓规范 |
| `Stop` | - | 用 haiku 检查任务完整性 |

**SQL 规范检查脚本**（`validate_sql.sh`）检查项：
1. 禁止 `SELECT *`
2. `INSERT` 必须带 `PARTITION` 子句
3. 金额字段建议用 `DECIMAL(20,4)`，不用 `DOUBLE`
4. `UPDATE`/`DELETE` 必须有 `WHERE`

**危险 DDL 拦截脚本**（`block_dangerous_ddl.sh`）：
- 拦截生产表（非 `_dev`/`_test`/`_stg` 后缀）的 `DROP TABLE`/`TRUNCATE TABLE`

**关键规则**：阻断必须用 `exit 2`，`exit 1` 不会阻止 Claude 继续执行。

### 第四层：Subagents 上下文隔离

核心原则：把“高 token 消耗但结果只需要摘要”的操作放到 subagent 的独立 context 中执行。

三种核心 subagent：

| Subagent | 用途 | 模型 | 输出限制 |
|----------|------|------|----------|
| `sql-validator` | SQL 语法验证与规范检查 | haiku | ≤50行结构化报告 |
| `dw-explorer` | 表结构/DDL/血缘探索 | haiku | ≤80行结构摘要 |
| `data-quality-checker` | 23 项标准自测执行 | - | PASS/FAIL 汇总 |

使用方式：在主对话中自然触发，如“用 dw-explorer 分析 db_a.dwd_table_a 的结构”，或强制指定 `@"sql-validator (agent)" 验证 path/to/insert.sql`。

参见 [[claude-code-subagent|Claude Code Subagent 系统]]。

### 第五层：SKILL 文件改造

**当前问题**：SKILL 文件（01~08.md）全文加载进主 context，加速 compact 触发。

**改造方向**：
- 把 SKILL 的“执行步骤”提炼成 subagent 指令，subagent 内部读完整 SKILL
- 用 path-scoped rules 替代 SKILL 中的规范章节，按需加载
- 主对话不直接触发 SKILL，而是启动 subagent 来读 SKILL 执行，主对话只接收最终产出

示例 path-scoped rule（`.claude/rules/etl-rules.md`）：
```markdown
---
paths:
  - "**/*insert*.sql"
  - "**/*_di.sql"
  - "**/*_df.sql"
---
# ETL 开发规范（按文件路径自动加载）
- 必须有 partition_dt 分区
- INSERT OVERWRITE 前检查分区是否已存在
- 不允许跨库 JOIN
```

## 数仓 Harness 架构

```
┌────────────────────────────────────┐
│        持久化层（CLAUDE.md）         │  ← 解决“失忆”问题
│  compact 后从磁盘重新注入           │
├────────────────────────────────────┤
│       Harness 层（Hooks）           │  ← 解决“规范靠记忆”问题
│  PostToolUse/PreToolUse/SessionStart│     exit 2 强制阻断
├────────────────────────────────────┤
│       Subagent 层                   │  ← 解决“context 撑满”问题
│  dw-explorer / sql-validator / ...  │     compact 频率降低 50%~70%
└────────────────────────────────────┘
         ▲                  ▲
         │                  │
    ┌────┴────┐      ┌──────┴──────┐
    │ 需求分析 │      │ SLA/DQC     │  ← 主会话直接处理（context 压力低）
    │ 技术设计 │      │ SR 导入     │
    └─────────┘      └─────────────┘
    ┌─────────────────────────────────┐
    │ 必须通过 Harness 机制            │
    │ ETL → hook 自动检查             │  ← 每次写 .sql 自动触发
    │ 自测 → subagent 隔离            │  ← 23条SQL结果不进入主context
    │ 数据比对 → subagent 隔离         │
    │ 性能优化 → subagent 隔离         │  ← 血缘查询结果不进主context
    └─────────────────────────────────┘
```

三层机制分工明确：Hooks 处理“写动作触发的规范检查”，subagent 处理“读操作的 context 隔离”，CLAUDE.md 处理“跨会话状态持久化”。

## 基于 SKILL 的 8 步工作流

| 步骤 | 执行方式 | Harness 机制 |
|------|---------|-------------|
| Step 1: 需求分析 | 主会话 + dw-explorer | `SessionStart` 注入迭代约束 |
| Step 2: 技术设计 | 主会话 | 设计完成后手动更新 CLAUDE.md |
| Step 3: ETL 开发 | 主会话 + hook 自动检查 | `PostToolUse` 每次写 .sql 自动触发 validate_sql.sh |
| Step 4: 自测 | subagent 隔离 | data-quality-checker subagent，主会话只收 PASS/FAIL 摘要 |
| Step 5: 数据比对 | subagent 隔离 | data-comparator subagent，只返回差异超过容差的字段 |
| Step 6: SR 导入 | 主会话 + SKILL | context 压力低，直接处理 |
| Step 7: 性能优化 | subagent 隔离 | dw-explorer subagent，血缘 + DDL 结果不进主 context |
| Step 8: SLA/DQC | 主会话 | 生成 DQC 配置 JSON |

### SKILL 命令体系

8 个标准命令对应 8 步流程（如 `/dw-etl`、`/dw-自测` 等），每个 SKILL 封装了：
1. **规范内容**（写死，不靠记忆）
2. **产出格式**（结构化，不走样）
3. **自动护栏**（Hook 配合，exit 2 强制修正）
4. **subagent 卸载**（防 context 膨胀）

核心理念：把每次都要重复讲的要求写进 SKILL，把每次都怕忘的检查点写进 SKILL，把每次都需要的结构化输出写进 SKILL。

## 核心问题解决

### 本质瓶颈：语义理解

数仓 AI 开发中，技术能力不是瓶颈，语义理解才是。需求理解偏差占总返工的 40%~60%，约 40%~50% 的时间花在理解需求和对口径上。AI 不准确的根本原因不是“不会写 SQL”，而是“不理解业务语义”——不知道 amount 单位是千元还是元，不知道 is_perform=1 在这个版本里意味着什么。

### 准确率公式

> 准确率 = 语义理解深度 × 数据规范覆盖度

Harness 工程的本质：把“语义”和“规范”从不可靠的 LLM 记忆中，迁移到确定性的 hooks + 持久化文件里。

### 四类问题的 Harness 解法

| 问题 | 传统方式 | Harness 解法 | 效果 |
|------|---------|-------------|------|
| 字段口径遗忘 | 口头约定 → compact 丢失 | 写入 CLAUDE.md + Auto Memory | 基本不出现 |
| 需求理解偏差 | AI 直接读 PRD 生成 | 需求分析产出写进 CLAUDE.md，Stop hook 检查完整性 | 首次通过率 50%→90% |
| SQL 规范不一致 | 靠 LLM 记忆，遵守率 70%~80% | PostToolUse hook 每次自动检查 | 遵守率 95%+ |
| context 耗尽 | 血缘+自测+SKILL 全在主 context | dw-explorer + data-quality-checker + sql-validator subagent 隔离 | compact 频率降低 50%~70% |

## 与传统方式对比

| 维度 | 传统 AI 辅助开发 | Harness + AI 流水线 |
|------|----------------|-------------------|
| 规范遵守 | 靠 LLM 记忆（70%~80%） | hooks 强制检查（95%+） |
| 上下文管理 | 依赖 compact，随机丢失 | CLAUDE.md + subagent 隔离 |
| 错误方式 | 上线后发现 | 开发中实时阻断 |
| 组织定位 | “AI 帮我写代码” | “AI 嵌入研发流水线” |

最终目标不是让 Claude 更聪明，而是让整个研发流水线更可靠：

- **Claude（LLM）**：理解需求、设计方案、生成代码 → 需要语义理解
- **Harness（hooks）**：规范检查、危险拦截、任务完整性验证 → 需要确定性执行
- **Subagents**：大量文件读取、血缘查询、自测执行 → 隔离 context 消耗
- **CLAUDE.md + Memory**：字段口径、迭代约束、踩坑经验 → 跨会话持久化

## 相关页面

- [[claude-code-harness|Claude Code Harness 架构]] — Claude Code 的 Harness 编排系统
- [[agent-harness-overview|Agent Harness 综述]] — Agent 六承重层拆解
- [[harness-engineering-practice|Harness Engineering 实践]] — Human-first → Agent-aware 工程转型
- [[claude-code-context-compaction|Claude Code 上下文压缩]] — compact 机制详解
- [[claude-code-subagent|Claude Code Subagent 系统]] — subagent 架构与机制
- [[claude-code-memory-system|Claude Code 记忆系统]] — Auto Memory 与 MEMORY.md
- [[claude-code-configuration-guide|Claude Code 配置指南]] — settings.json 与 hooks 配置
