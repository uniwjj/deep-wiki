---
title: OpenSpec Schema 机制深度解析
description: OpenSpec 工作流引擎的核心——Schema 的 DAG 模型、三层定制体系、schema.yaml 字段详解、命令映射与状态判断机制
aliases: [OpenSpec Schema, Schema 机制, schema.yaml, artifact DAG]
tags: [ai-agent, concept, tool]
sources: [2026/06/17/openspec-custom-schema.html]
created: 2026-06-17
updated: 2026-06-17
---

# OpenSpec Schema 机制深度解析

Schema 是 OpenSpec 的**工作流引擎**——定义从一个想法到最终代码合并之间，AI 需要生成哪些文档、按什么顺序生成、依据什么规则生成。Schema 决定了整个变更周期的形状。

## 核心心智模型

Schema 不是流程图，而是一个**有向无环图（DAG）的声明**。你声明节点（artifact）和边（`requires` 依赖），OpenSpec CLI 负责遍历这个图，Agent 负责在每个节点填充内容。

### 内置默认 Schema

OpenSpec 内置 `spec-driven` schema，产出四个 artifact：

```
proposal.md → specs.md → design.md → tasks.md
  (意图与范围)   (行为规约)   (技术方案)   (实现清单)
```

但这个默认流程并不适合所有团队——数据仓库开发需要血缘校验，事件驱动系统需要 AsyncAPI 规约，快速迭代项目可能不需要 design 步骤。**Custom Schema 就是把这个流程变成你自己的**。

## 三层定制体系

OpenSpec 提供三个颗粒度不同的定制层次：

```
优先级 HIGH ──────────────────────────────────────────►
┌──────────────────────────────────────────────────┐
│ Layer 1 · Project Config (config.yaml)           │ ← 最高优先级
│ context 注入 + rules 约束，不修改工作流结构        │
├──────────────────────────────────────────────────┤
│ Layer 2 · Custom Schema (核心，推荐)              │
│ openspec/schemas/<name>/  ·  版本控制  ·  团队专属  │
├──────────────────────────────────────────────────┤
│ Layer 3 · Global Override                        │ ← 最低优先级
│ ~/.openspec/schemas/  ·  跨项目共享  ·  个人习惯   │
└──────────────────────────────────────────────────┘
```

### Layer 1：Project Config

最轻量的定制，只向 AI 注入项目上下文和规则约束：

```yaml
# openspec/config.yaml
schema: spec-driven

context: |
  Tech stack: Hive/Spark, Java, TypeScript
  所有 DDL 必须包含 COMMENT 字段描述

rules:
  proposal:
    - 明确上下游血缘影响范围
    - 包含回滚方案
  tasks:
    - 每个任务必须标注对应的 Spec 层级
```

生成任意 artifact 时，OpenSpec 按此结构组装 Prompt：`<context>...</context>` + `<rules>...</rules>`（仅匹配对应 artifact）+ `<template>模板内容</template>`。context 全局有效，rules 按 artifact id 精准注入。

### Layer 2：Custom Schema（核心）

定义 artifact 的类型、顺序、依赖关系和 AI 生成指令。存放于 `openspec/schemas/<name>/`，与代码一起版本控制。这是**最有价值的定制层**。

### Layer 3：Global Override

放在 `~/.openspec/schemas/`，在本机所有项目中共享。适合个人习惯性工作流，但因不在仓库中，团队协作时每人都需单独配置，通常不推荐作为团队方案。

### Schema 解析优先级

| 优先级 | 来源 | 典型场景 |
|--------|------|---------|
| **1（最高）** | CLI flag `--schema <name>` | 临时切换 schema 测试 |
| **2** | 变更文件夹 `.openspec.yaml` | 单次变更使用特定 schema |
| **3** | `openspec/config.yaml` 的 `schema:` | 项目默认 schema |
| **4（最低）** | 内置默认 `spec-driven` | 无任何配置时的兜底 |

## schema.yaml 完整字段

一个完整的 `schema.yaml` 由三部分组成：元信息、artifacts 列表、apply 块。

```yaml
name: dw-spec-driven          # 唯一标识符，用于 --schema 参数
version: 1                    # 整数版本号
description: 数据仓库 SDD 工作流  # 人读描述

artifacts:
  - id: business-spec           # artifact 唯一 ID（用于 requires 引用）
    generates: specs/business/spec.md  # 输出路径，支持 glob
    description: 业务层规约      # CLI status 显示用
    template: business-spec.md   # templates/ 下的模板文件
    instruction: |              # 注入 AI Prompt 的生成指令
      按四层 YAML Spec 的 Business Layer 格式，
      描述业务需求、数据血缘和验收标准。
    requires: []                # 依赖的前置 artifact id 列表

  - id: implementation-spec
    generates: specs/impl/spec.md
    template: impl-spec.md
    instruction: |
      根据 business-spec，生成 Implementation Layer，
      包含 HQL 模板、分区策略、调度依赖。
    requires:
      - business-spec           # 必须先有 business-spec

  - id: tasks
    generates: tasks.md
    template: tasks.md
    instruction: |
      根据 business-spec 和 implementation-spec 生成任务列表。
      最后一组任务必须包含写入 verify/.apply-done 的步骤。
    requires:
      - business-spec
      - implementation-spec

apply:
  requires:                     # /opsx:apply 前必须存在的 artifact
    - tasks
  tracks: tasks.md              # Agent 读哪个文件追踪任务进度
```

### artifacts 子字段

| 字段 | 类型 | 说明 |
|------|------|------|
| `id` | string · 必填 | 唯一标识符。用于 `requires` 引用、`openspec status` 输出、`config.yaml` 的 `rules` 键名 |
| `generates` | string · 必填 · 支持 glob | 输出文件路径，相对于变更文件夹。支持 `specs/**/*.md` 的 glob，只要目录下有任意匹配文件即视为完成 |
| `template` | string · 必填 | `templates/` 目录下的 Markdown 文件路径。内容注入 AI Prompt 作为结构骨架 |
| `instruction` | string · 推荐 | 生成该 artifact 时注入 AI 的核心指令。这是定制 AI 行为最重要的字段 |
| `requires` | string[] · 可选 | 依赖的前置 artifact id 列表。只有所有依赖文件都存在，该 artifact 才进入 ready 状态 |
| `description` | string · 可选 | `openspec status` 输出中显示的描述文字，不影响 AI 行为 |

### apply 块字段

| 字段 | 类型 | 说明 |
|------|------|------|
| `apply.requires` | string[] · 必填 | 执行 `/opsx:apply` 前必须已存在的 artifact 列表——"实现门控" |
| `apply.tracks` | string · 必填 | Agent 执行 apply 时读取和更新的文件，通常是 `tasks.md`。Agent 逐条读任务，完成后将 `[ ]` 改为 `[x]` |

> **重要区分**：`context`、`rules`、默认 `schema` 名在 `openspec/config.yaml` 里，不在 `schema.yaml` 里。schema.yaml 定义工作流结构，config.yaml 注入项目上下文——两者职责分离。

## 命令与 Schema 的映射

| 命令 | 对应 Schema 部分 | 行为 |
|------|-----------------|------|
| `/opsx:propose` | `schema.artifacts` | 循环调用 `openspec status`，按 DAG 顺序依次生成所有 ready 状态的 artifact，直到 `apply.requires` 全部满足 |
| `/opsx:apply` | `schema.apply` | 使用 `apply.tracks` 指定文件逐条执行任务，写代码、创建文件、标记完成 |
| `/opsx:verify` | **独立于 schema** | Agent 读取变更文件夹内所有 artifact + 搜索代码库，做语义层面的完整性/正确性/连贯性校验 |
| `/opsx:archive` | **独立于 schema** | 将变更文件夹中的 delta specs 合并到 `openspec/specs/` 主目录，再移入 `archive/` |

### propose 内部循环

```
openspec status → 检查哪些 artifact ready
       ↓
openspec instructions → 拿到 template + context + rules + 依赖内容
       ↓
Agent 生成内容 → 按 instruction 写文件
       ↓
循环直到 apply.requires 满足 → 停止，提示用户
```

`openspec instructions` 一次返回 template + context + rules + 所有依赖 artifact 的内容，Agent 不需要多次读文件——这是 propose 循环内层的核心，相当于 Agent 的"配料包"。

## 状态判断机制

`openspec status` 的判断逻辑完全基于**文件是否存在**，不做任何内容语义解析。

### 三种状态

```
BLOCKED ──(依赖全部 done)──► READY ──(Agent 写入文件)──► DONE
  ↑ missingDeps 非空           ↑ 依赖满足，文件不存在       ↑ generates 路径文件存在
```

- **BLOCKED**：`requires` 中有未完成的 artifact
- **READY**：所有依赖已完成，但 `generates` 路径文件尚不存在 → Agent 的下一个目标
- **DONE**：`generates` 指定的文件（glob 或精确路径）已存在

### JSON 输出示例

```json
{
  "changeName": "add-lineage",
  "schemaName": "dw-spec-driven",
  "isComplete": false,
  "applyRequires": ["tasks"],
  "artifacts": [
    { "id": "business-spec", "outputPath": "specs/business/spec.md", "status": "done" },
    { "id": "implementation-spec", "outputPath": "specs/impl/spec.md", "status": "ready" },
    { "id": "tasks", "outputPath": "tasks.md", "status": "blocked", "missingDeps": ["implementation-spec"] }
  ]
}
```

状态完全由文件系统决定，与文件内容无关。这意味着：Agent 可以删除文件来触发重新生成，状态机会自动将其退回 READY。

## 相关页面

- [[openspec-custom-schema-guide]] — 自定义 schema 创建、模板设计与完整实战
- [[openspec-skill-mechanism]] — SKILL 文件：Agent 操作手册的生成与加载机制
- [[openspec-verify-extension]] — 扩展自定义 verify 能力的三种方案
- [[openspec-sdd-practice]] — OpenSpec 七步工作流实战
- [[sdd-custom-workflow]] — SDD 薄编排架构与 schema 配合
- [[sdd-openspec-superpowers]] — OpenSpec + Superpowers 双框架对比
