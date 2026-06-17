---
title: OpenSpec Verify 扩展方案
description: 超越内置通用校验的三种自定义 verify 方案——artifact 格式校验、SKILL 动态校验、apply.instruction 内嵌校验，含对比与推荐组合
aliases: [verify扩展, custom verify, 自定义验证, 血缘校验]
tags: [ai-agent, practice, comparison]
sources: [2026/06/17/openspec-custom-schema.html]
created: 2026-06-17
updated: 2026-06-17
---

# OpenSpec Verify 扩展方案

内置的 `/opsx:verify` 只做通用语义校验（完整性、正确性、连贯性），无法覆盖领域特定的结构化检查——如血缘路径合法性、HQL 字段对齐、命名规范校验。本文梳理三种扩展方式。

## 方案 A：Schema Artifact 校验（格式校验）

在 schema 中新增一个 verify artifact，把校验变成 DAG 中的一个节点。利用 `requires` 依赖链，确保校验在 tasks 生成之前完成。

**适合场景**：apply 之前就能做的**静态格式校验**（无需代码已生成）。

```yaml
artifacts:
  # ... business-spec, implementation-spec ...

  - id: lineage-check
    generates: verify/lineage-check.md
    template: lineage-check.md
    instruction: |
      Read implementation-spec.md and validate:
      1. source_tables 字段格式是否为 db.table 格式
      2. field_list 中是否有重复字段名
      3. partition_fields 是否在 field_list 中有对应项
      4. target_table 名称是否符合命名规范（dws_/ads_ 前缀）
      Output a structured PASS/FAIL report.
      If any check FAIL, still write the report but mark overall: FAILED
    requires:
      - implementation-spec

  - id: tasks
    generates: tasks.md
    requires:
      - business-spec
      - implementation-spec
      - lineage-check   # 校验通过才能生成 tasks
```

**关键设计**：FAIL 时不阻断文件生成——report 仍然写入（标记 FAILED），由人决定是否继续。这保留了检查记录，避免信息丢失。

## 方案 B：独立 Verify SKILL（动态内容校验）

写一个独立的 SKILL 文件，在 apply 完成后手动触发。不依赖 schema 结构，可以做代码与 spec 的交叉校验。

**适合场景**：apply 完成后需要做的**动态校验**（代码已生成，验证代码与 spec 的一致性）。

```markdown
# SKILL: Data Warehouse Verify

## When to use
After /opsx:apply completes, before /opsx:archive.
Trigger: "verify", "run dw verify", "check lineage consistency"

## Steps

### 1. Task completion check
Read tasks.md → confirm all [ ] are now [x]

### 2. Spec vs HQL alignment
Read specs/impl/spec.md → extract:
  - source_tables, target_table, field_list, partition_fields

For each generated .hql file in sql/ directory:
  - Check source_tables appear in FROM / JOIN clauses
  - Check field_list matches SELECT column names
  - Check partition_fields match PARTITIONED BY clause

### 3. Business spec cross-check
Read specs/business/spec.md:
  - Verify partition convention matches business layer agreement
  - Verify target_table naming matches domain prefix rules

### 4. OpenSpec structural validation
Run: `openspec validate --change {change-name} --json`

### 5. Write report
Write to verify/report.md:
  - Overall: PASS / FAIL
  - Task Completion: [x] N/N tasks done
  - HQL Alignment: per-check results
  - Partition Check: result

### 6. Gate
If any check FAILED:
  - Show specific failures with file locations
  - Do NOT suggest /opsx:archive
  - Ask user to fix and re-run verify

If all PASSED:
  - "Verification passed. Ready for /opsx:archive"
```

## 方案 C：apply.instruction 内嵌验证

在 schema 的 `apply.instruction` 中直接描述验证步骤，最简单但最不灵活。

**适合场景**：简单检查、快速验证场景。

```yaml
apply:
  requires: [tasks]
  tracks: tasks.md
  instruction: |
    完成所有 tasks.md 中的任务。
    
    所有任务完成后，执行以下验证：
    1. 读 specs/impl/spec.md 中的 field_list
    2. 对每个生成的 HQL 文件，检查字段名是否匹配
    3. 在 tasks.md 末尾追加 ## Verification 章节，
       记录每项检查结果（PASS/FAIL）
    4. 如有 FAIL，输出具体位置并停止
```

## 三种方案对比

| 维度 | A · Schema Artifact | B · 独立 SKILL | C · apply.instruction |
|------|-------------------|----------------|----------------------|
| **检查时机** | propose 阶段（apply 前） | apply 完成后手动触发 | apply 结束时自动 |
| **适合场景** | 格式合规、必填校验 | 代码与 spec 一致性 | 简单检查、快速场景 |
| **可重跑** | 删文件重跑 | 随时 | 需重跑 apply |
| **复杂度** | 低 | 中 | 极低 |
| **依赖 schema** | 是（作为 artifact 节点） | 否 | 是（在 apply 块内） |
| **灵活性** | 中（受 DAG 约束） | 高（完全自定义） | 低（耦合在 apply 中） |

## 推荐组合

```
方案 A（artifact） + 方案 B（SKILL）
     ↓                      ↓
  静态格式校验            动态内容校验
  (propose 阶段)         (apply 后手动)
```

两者职责清晰，互不干扰，覆盖 **propose → apply → verify** 的完整链路：

1. **propose 阶段**：方案 A 的 lineage-check artifact 在 tasks 生成前拦截格式问题
2. **apply 阶段**：Agent 按 tasks.md 写代码
3. **verify 阶段**：方案 B 的 dw-verify SKILL 校验代码与 spec 的一致性

方案 C 仅在需求极简时使用——当检查逻辑只需 3-5 行描述时。

## 相关页面

- [[openspec-schema-mechanism]] — Schema DAG 机制，artifact 依赖链
- [[openspec-custom-schema-guide]] — 自定义 schema 创建，含完整数据仓库示例
- [[openspec-skill-mechanism]] — SKILL 文件的结构与加载机制
- [[openspec-sdd-practice]] — OpenSpec 七步工作流实战
- [[sdd-custom-workflow]] — SDD 薄编排中的 sdd-verify action
