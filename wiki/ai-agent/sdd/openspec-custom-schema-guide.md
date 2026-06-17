---
title: OpenSpec 自定义 Schema 实战指南
description: 创建自定义 schema 的三种方式（fork/init/社区安装）、模板文件设计原则、config.yaml 与 schema.yaml 分工、数据仓库完整实战
aliases: [custom schema, 自定义Schema, schema创建, template设计]
tags: [ai-agent, practice, tool]
sources: [2026/06/17/openspec-custom-schema.html]
created: 2026-06-17
updated: 2026-06-17
---

# OpenSpec 自定义 Schema 实战指南

在理解 [[openspec-schema-mechanism]] 的核心机制后，本文聚焦如何创建和定制自己的 schema。

## 创建方式

### 方式一：Fork 现有 Schema（推荐）

最快的方式，把内置 schema 复制到项目目录，再按需修改：

```bash
# Fork 内置 spec-driven
openspec schema fork spec-driven my-workflow

# 指定目标名称
openspec schema fork spec-driven dw-spec-driven
```

执行后在 `openspec/schemas/dw-spec-driven/` 创建完整结构：

```
openspec/schemas/dw-spec-driven/
├── schema.yaml          # 工作流定义，可自由编辑
└── templates/
    ├── proposal.md      # 各 artifact 的模板骨架
    ├── specs.md
    ├── design.md
    └── tasks.md
```

### 方式二：从零创建

```bash
# 交互式创建
openspec schema init dw-spec-driven

# 非交互式，指定 artifacts 列表
openspec schema init dw-spec-driven \
  --description "数据仓库 SDD 工作流" \
  --artifacts "business-spec,implementation-spec,tasks" \
  --default
```

### 方式三：从社区仓库安装

社区维护了一个 schema 集合仓库 [openspec-schemas](https://github.com/intent-driven-dev/openspec-schemas)，可以让 Agent 直接安装：

```bash
# 告诉 Agent 安装指定 schema
Read this file:
https://raw.githubusercontent.com/intent-driven-dev/openspec-schemas/refs/heads/main/README.md
and install schema event-driven
```

### 验证与调试

```bash
# 验证 schema 合法性（语法、模板存在、无循环依赖）
openspec schema validate dw-spec-driven

# 查看 schema 解析来源
openspec schema which dw-spec-driven

# 列出所有可用 schema
openspec schemas
```

### 设为项目默认

```yaml
# openspec/config.yaml
schema: dw-spec-driven   # 此后 /opsx:propose 自动使用这个 schema
```

## Template 文件设计

Template 是 Markdown 文件，注入 AI Prompt 作为结构骨架。HTML 注释 `<!-- ... -->` 用于向 AI 提供填写指导，不会出现在最终输出中。

### 设计原则

1. Section 标题明确——AI 知道这里写什么
2. HTML 注释提供格式样例或约束说明
3. 不在 template 里硬编码具体内容——那是 `instruction` 的工作

### template vs instruction 的分工

| 维度 | template | instruction |
|------|----------|-------------|
| 职责 | 决定**结构形状**（有哪些章节、每节放什么） | 决定**生成策略**（关注什么、遵循什么规范、不要做什么） |
| 形式 | Markdown 骨架 + HTML 注释指导 | YAML 字符串，自然语言指令 |
| 注入方式 | `<template>...</template>` 包裹 | 直接注入 Prompt |

两者都注入 Prompt，合起来才是 AI 的完整指导。

### 数据仓库场景示例

```markdown
# Implementation Spec: {change-name}

## Target Table
<!-- 目标表名，格式：database.table_name -->

## Source Tables
<!--
列出所有上游来源表，格式：
- database.table_name: 字段描述
每个来源表都必须在 Hive 中实际存在
-->

## Field Mapping
<!--
字段映射表，格式：
| 目标字段 | 来源表.字段 | 转换逻辑 | 类型 |
每行对应一个输出字段
-->

## Partition Strategy
<!-- 分区字段、分区粒度、动态/静态分区 -->

## Schedule Dependencies
<!-- 调度依赖的上游任务，格式：task_id: 描述 -->

## HQL Template
<!-- 提供关键 HQL 逻辑骨架，不需要完整可运行的 HQL -->
```

## 完整实战：数据仓库 SDD 场景

以下是为数据仓库开发定制的完整 schema 设计，对应四层 YAML Spec 架构。

### DAG 设计

```
business-spec ──► implementation-spec ──► lineage-check
       │                    │                   │
       └────────────────────┴───────────────────▼
                                                tasks
```

四个 artifact + 一个校验节点：
- **business-spec**：业务需求、血缘来源、核心指标（无前置依赖）
- **implementation-spec**：HQL 规约、字段映射、分区策略（依赖 business-spec）
- **lineage-check**：血缘格式静态校验（依赖 implementation-spec）
- **tasks**：实现任务清单（依赖全部三个前置）

### 完整 schema.yaml

```yaml
name: dw-spec-driven
version: 1
description: 数据仓库 Spec 驱动开发工作流

artifacts:
  - id: business-spec
    generates: specs/business/spec.md
    description: 业务需求与血缘来源
    template: business-spec.md
    instruction: |
      按 Business Layer 格式描述：
      - 业务背景与数据需求
      - 上游血缘来源（source system / source table）
      - 核心指标定义与计算口径
      - 数据质量要求与验收标准
    requires: []

  - id: implementation-spec
    generates: specs/impl/spec.md
    description: Implementation Layer：HQL 规约
    template: impl-spec.md
    instruction: |
      根据 business-spec 生成实现规约：
      - target_table / source_tables / field_list
      - partition_fields / schedule_deps / hql_skeleton
    requires:
      - business-spec

  - id: lineage-check
    generates: verify/lineage-check.md
    description: 血缘格式静态校验报告
    template: lineage-check.md
    instruction: |
      Read implementation-spec.md and validate:
      1. source_tables 格式：每项必须是 db.table_name
      2. field_list 无重复字段名
      3. partition_fields 的每个字段必须在 field_list 中
      4. target_table 必须有 dws_/ads_/dim_ 前缀
      5. 命名必须是 snake_case
      Write structured PASS/FAIL report.
      Mark overall FAILED if any check fails.
    requires:
      - implementation-spec

  - id: tasks
    generates: tasks.md
    description: 实现任务清单
    template: tasks.md
    instruction: |
      根据 implementation-spec 和 lineage-check 生成任务：
      1. DDL 创建任务（建表 SQL）
      2. HQL 开发任务（按字段组分组）
      3. 数据质量检查任务
      4. 调度配置任务
      最后添加任务：写入 verify/.apply-done 标记文件
    requires:
      - business-spec
      - implementation-spec
      - lineage-check

apply:
  requires: [tasks]
  tracks: tasks.md
```

### config.yaml 配套

```yaml
# openspec/config.yaml
schema: dw-spec-driven

context: |
  数据仓库技术栈：Hive 3.x / Spark 3.x / YARN
  命名规范：snake_case，层级前缀 ods_/dwd_/dws_/ads_/dim_
  分区约定：日期分区字段统一为 dt，格式 yyyyMMdd
  所有 DDL 必须包含 COMMENT 字段描述
  HQL 文件统一放置在 sql/ 目录，文件名与 target_table 一致

rules:
  business-spec:
    - 必须明确指定上游血缘来源的系统名和表名
    - 核心指标需注明计算口径和去重逻辑
  implementation-spec:
    - source_tables 必须全部是现有 Hive 表
    - partition_fields 必须包含 dt 字段
  tasks:
    - 每个任务标注对应的 Spec 层级（Business/Impl）
    - DDL 任务在 HQL 任务之前
```

### 关键设计决策

| 决策 | 做法 | 理由 |
|------|------|------|
| lineage-check 位置 | 放在 tasks 之前 | 格式问题在生成任务前暴露，避免无效任务 |
| FAIL 不阻断 | report 生成但标记 FAILED | 保留检查记录，由人决定是否继续 |
| apply.instruction | 不内嵌验证 | 复杂验证交给独立 SKILL（见 [[openspec-verify-extension]]） |

## 相关页面

- [[openspec-schema-mechanism]] — Schema 机制核心概念与状态机
- [[openspec-skill-mechanism]] — SKILL 文件机制
- [[openspec-verify-extension]] — 扩展 verify 的三种方案
- [[openspec-sdd-practice]] — OpenSpec 七步工作流实战
- [[sdd-custom-workflow]] — SDD 薄编排架构
