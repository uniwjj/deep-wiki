---
title: Superpowers 三条铁律
description: Superpowers 的三条硬性工程纪律——没有失败测试不写生产代码、不做根因调查不修 Bug、没有新鲜验证证据不做完成声明
aliases: [Superpowers Iron Laws, AI 编程铁律, Superpowers 铁律]
tags: [ai-agent, concept, practice]
sources: [2026/05/24/Superpowers 实战指南：7 步流程 + 14 个技能 + 3 条铁律，搭建让 AI 编程更稳、更守规矩的工作流.html]
created: 2026-05-24
updated: 2026-05-24
---

# Superpowers 三条铁律

Superpowers 管得很严。每条规则背后都有明确的工程逻辑，不是建议，是**硬性约束**。

三条铁律本质上是对抗软件开发中三种典型的偷懒行为：不写测试、不查根因、不验证就收工。

## 铁律一：没有失败测试就不写生产代码

关联技能：[[agent-tdd-workflow]]

**逻辑链**：如果你写不出一个能失败的测试，说明你还没想清楚要实现什么。连行为都定义不了，写出来的代码大概率是瞎猜的。

三个具体收益：

- 迫使你在写代码前思考接口设计
- 提供即时反馈，写完马上就能验证
- 形成安全网，后续改动不怕引入回归

## 铁律二：不做根因调查就不修 Bug

关联技能：systematic-debugging

**逻辑链**：没有根因分析的修复本质上是在碰运气。改了一行代码 Bug 消失了，但不确定为什么。万一根本原因在别处，"修复"只是掩盖了症状，后续排查成本翻倍。

根因调查慢在前面，但修一次彻底解决。不查根因快在前面，同一个 Bug 反复出现，总时间反而更多。

## 铁律三：没有新鲜验证证据就不做完成声明

关联技能：verification-before-completion

**逻辑链**：对抗一种心理倾向——改完代码后急着宣布"搞定了"。尤其是用 AI 编程时，AI 说"修复已完成"你就信了，但 AI 说的"已完成"可能只是"代码改完了"，不代表"验证过了"。

要求的验证证据：

- 测试全部通过（新旧都过）
- 手动验证确认
- 相关功能无回归

三条都满足才算完成。"新鲜"很关键——不是"昨天跑过"，是"刚刚验证过"。

## 相关页面

- [[superpowers-framework]] — Superpowers 框架综述
- [[superpowers-workflow-scenarios]] — 7 步工作流与场景裁剪
- [[agent-tdd-workflow]] — Agent TDD 开发流程
- [[superpowers-vs-openspec]] — Superpowers vs OpenSpec 哲学对比
