---
title: Agent TDD 工作流
description: AI Agent 的测试驱动开发——从 Superpowers 强制 TDD 到 Meta JiT 语义级测试生成
aliases: [agent-tdd-workflow, TDD, 测试驱动开发, RED-GREEN-REFACTOR]
tags: [ai-agent, practice]
sources: [2026/04/02/Superpowers框架.md, 2026/05/10/Claude-Code-Source-Analysis.pdf]
created: 2026-05-10
updated: 2026-05-17
---

# Agent TDD 工作流

在 AI 编程中，TDD 从"人的纪律"变成了"Agent 的自动约束"。

## 为什么 AI 需要 TDD

AI 生成代码的速度远超人类编写测试的能力。静态测试脆断、覆盖率过时、测试维护成本爆炸。TDD 让 AI **自己写的测试发现自己写的 bug**——人工修复变成了 AI 自修。

## Superpowers 强制 TDD 循环

Superpowers 将 RED-GREEN-REFACTOR 从建议变为**强制执行**：

1. **RED**：先写失败测试
2. **GREEN**：写刚好让测试通过的最少代码
3. **REFACTOR**：优化代码结构，测试保护不退化

实测效果：代码一次通过率从 58% 提升到 91%，人工修复从 2 轮降至 0.2 轮。详见 [[superpowers-framework]]。

## Meta JiT 测试

Meta 论文提出 **JIT（Just-in-Time）测试**：在代码审查阶段动态生成测试，而非静态覆盖验证。

核心创新——**Dodgy Diff**：将 diff 从文本差异重定义为**语义信号**，识别行为意图和风险区域。

流程：意图重建 + 风险建模 → 变异引擎生成可疑变体 → LLM 合成回归测试 → 噪声过滤。

已验证结果：22,000+ 测试评估，bug 检出率提升 4 倍，有意义故障检测提升 20 倍。

## 相关页面

- [[superpowers-framework]] — Superpowers 强制 TDD 实测基准
- [[meta-jit-testing]] — Meta JIT 测试机制详解
- [[agent-skill-test-case]] — Agent Skill 测试用例
- [[sdd-custom-workflow]] — SDD 工作流中的 TDD 编排
