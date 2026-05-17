---
title: Superpowers + OpenSpec 老旧项目实战
description: 3人团队4阶段协同开发实战——用 OpenSpec 管需求层、Superpowers 管执行层，给旧商城加限时抢购的完整跑通流程
aliases: [协同开发实战, legacy project workflow, 老旧项目 AI 改造, Strangler Fig AI]
tags: [ai-agent, practice]
sources: [2026/05/17/从古法编程到 AI 编程：Superpowers + OpenSpec 协同开发实战指南.html]
created: 2026-05-17
updated: 2026-05-17
---

# Superpowers + OpenSpec 老旧项目实战

老旧项目接入 AI 编程的核心矛盾：**需求理解不准确，执行过程不可控**。OpenSpec 解决前者，Superpowers 解决后者。

## 双层分工架构

| | OpenSpec | Superpowers |
|---|---|---|
| **定位** | 规范驱动开发（SDD） | AI 编码代理工作方法论 |
| **管什么** | 需求层——需求→规范→验证 | 执行层——编码过程质量保证 |
| **核心机制** | 双文件夹模型 + Delta Spec | 7 步强制工作流 |
| **GitHub Stars** | 40.9k | 158k |

### OpenSpec 目录结构

```
openspec/
├── specs/        ← 当前系统规范，项目的事实来源
└── changes/      ← 每次变更的制品
    └── flash-sale/
        ├── proposal.md   ← 为什么要做
        ├── design.md     ← 技术方案
        └── tasks.md      ← 实施清单
```

Delta Spec 用 `ADDED/MODIFIED/REMOVED` 三种标记描述变更影响面。据 CSDN 对比测试，使用后 Token 消耗降低 30-50%，返工率下降 60% 以上。

### Superpowers 7 步工作流

```
brainstorming → git worktree 隔离 → 子代理执行 → TDD → 代码审查 → 分支收尾
```

每一步不可跳过，强制 TDD，双阶段代码审查（规范合规 + 代码质量）。

## 四阶段闭环工作流

### 阶段一：旧代码分析

1. `openspec init` 初始化
2. `/opsx:explore` 探索现有代码结构
3. AI 分析核心模块依赖关系和业务逻辑，结果写入 `openspec/specs/`
4. **架构师审查**：补充 AI 遗漏的隐含业务规则

### 阶段二：需求规约

1. `/opsx:propose` 生成变更提案（自动创建 proposal/design/tasks）
2. 审查 `design.md` 确保与旧系统兼容
3. 审查 `tasks.md` 粒度：每个任务 2-5 分钟可完成

### 阶段三：代码生成（Superpowers 主场）

1. 将 `tasks.md` 输入 Superpowers
2. Brainstorming 澄清边界条件
3. Git worktree 自动创建隔离分支
4. TDD 循环自动执行：RED → GREEN → REFACTOR
5. **人工重点把关**：旧系统兼容性（框架版本、命名规则、全局变量）

### 阶段四：协同同步

1. 接口变更时更新 OpenSpec Delta Spec
2. `/opsx:sync` 合并到主规范
3. `/opsx:archive` 归档完成的变更

## 实战案例：旧商城加限时抢购

**背景**：3 年老商城，Spring Boot + jQuery + MySQL，零测试，注释率 < 5%。

### 分析阶段的关键产出

架构师审查后补充的隐含规则：

- 库存扣减不是原子操作，高并发下可能超卖
- 商品价格在商品主表和订单快照表两处，需同步更新
- 用户 session 在 Redis，TTL 30 分钟

### 设计阶段的关键决策

| 决策 | 理由 |
|------|------|
| 抢购库存与商品主库存解耦 | 避免影响正常购买 |
| Redis 库存预扣 + MQ 异步落库 | 解决旧系统高并发超卖 |
| 抢购订单独立接口 | 不影响现有订单流程 |

### tasks.md 拆分示例

| 任务 | 预计 | 负责人 |
|------|------|--------|
| 创建 flash_sale 数据表 | 3 分钟 | 后端 |
| 实现 Redis 库存预扣接口 | 5 分钟 | 后端 |
| 抢购订单创建 API | 5 分钟 | 后端 |
| 抢购活动列表页面 | 4 分钟 | 前端 |
| 倒计时组件 | 3 分钟 | 前端 |
| 库存实时显示组件 | 3 分钟 | 前端 |

### TDD 执行细节

以"Redis 库存预扣"为例，Superpowers 自动拆解为：

1. 创建 RedisStockService 类
2. 实现 deductStock 方法（Lua 脚本保证原子性）
3. 编写 deductStock 测试 → 实现 → 重构
4. 实现 stockRevert 回滚方法
5. 编写回滚方法测试 → 实现 → 重构

审查中发现问题：AI 生成的代码用 `@Autowired` 注入，但旧项目用的是构造器注入——人工修正。

## 团队角色变化

| 角色 | 传统职责 | 新职责 |
|------|---------|--------|
| **架构师** | 画图、写方案、Code Review | 定义和消费规范：`/opsx:propose` → 审查 design.md → `/opsx:verify` |
| **后端** | 从零写业务骨架 | 消费 tasks.md + brainstorming + TDD，重点处理旧系统兼容性 |
| **前端** | 等接口好了再开发 | 消费 design.md 接口规范提前开发，子代理并行开发组件 |

## 避坑指南

| 坑 | 现象 | 解决 |
|----|------|------|
| **代码风格冲突** | AI 用 `let/const`，老项目用 `var`；AI 抛异常，老项目返回错误码 | 在 `openspec/specs/` 加代码风格规范，brainstorming 阶段读取对齐 |
| **老旧依赖不兼容** | Spring Boot 1.x vs 2.x API，jQuery 1.x vs 3.x | 规范中明确标注依赖版本号，brainstorming 阶段告知 AI |
| **过度设计 Spec** | 一个变更写几十页设计文档 | 每个变更只解决一个问题，tasks.md 超过 10 个任务就拆分 |
| **老项目零测试** | 没有测试框架，Superpowers TDD 无法启动 | 绞杀者模式：新代码用 TDD，只对被修改的老函数补测试 |

## 工具安装

```bash
# OpenSpec
npm install -g @fission-ai/openspec@latest
cd your-project && openspec init

# Superpowers（Claude Code）
/plugin install superpowers@claude-plugins-official
```

两者均为 MIT 开源，互不冲突，可同时使用。

## 相关页面

- [[superpowers-openspec-pitfalls]] — 双框架组合的 7 个坑及自定义衔接 Skill
- [[sdd-openspec-superpowers]] — SDD 双框架对比与搭配建议
- [[openspec-sdd-practice]] — OpenSpec 七步工作流实战
- [[superpowers-framework]] — Superpowers 框架详解
- [[superpowers-vs-openspec]] — 个体效率 vs 团队一致性的哲学深度对比
- [[ai-programming-tools-comparison]] — Spec-Kit、OpenSpec、Superpowers 三剑客全面对比
