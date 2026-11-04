---
title: EasyData Agent 网关与 1+4+N 技能体系
description: 数帆 EasyData 的 Agent 接入方式——1 个网关统一鉴权/限流/审计、2 项协议（OpenAPI 面向系统集成，MCP 面向 Agent）、3 种形态（CLI/Skill/OpenAPI，人机同源共用一套权限）；技能层按 1 个总控编排 + 4 个场景级技能 + N 个原子技能组织，读/写/删除分三类授权
aliases: [EasyData 网关, EasyData Agent 接入, 1+4+N 技能体系, easydata-recommend-task-offline, 数据平台 Agent 接口层]
tags: [ai-agent, big-data, platform, practice]
sources: ["2026/11/04/为 Agent 重建数据层：网易智企产品 AI Native 演进路径/index.html"]
created: 2026-11-04
updated: 2026-11-04
---

# EasyData Agent 网关与 1+4+N 技能体系

> 来源：DataFunSummit《为 Agent 重建数据层：网易智企产品 AI Native 演进路径》（蒋鸿翔，2026-11-03）第 3 节中的"接口层 / 技能层 / 语义层 / 入口层"部分。

## 为什么需要网关

原文问题陈述："治理和诊断能否交给 Agent 运行，还取决于底层接口有没有**统一的权限边界**。"数帆的做法是**一个网关接入 OpenAPI 和 MCP**，对外提供 CLI、Skill、OpenAPI 三种形态，**鉴权、限流和审计都在网关处理**。

## 1 个网关 · 2 项协议 · 3 种形态

| 维度 | 原文表述 | 配图补充 |
|---|---|---|
| **1 个网关** | 统一鉴权、统一限流、统一审计 | **命令级权限管控**：控制命令能否用（按用户/角色）；**命令类型分级**：读 / 写 / 删除分别放过、二次确认、直接拒绝；操作审计同步至安全中心，Agent 行为全程可追溯 |
| **2 项协议** | OpenAPI 与 MCP 两种对话方式 | OpenAPI：稳定契约，面向系统集成与既有工具链；MCP：Agent 原生工具协议，模型可直接发现与调用；控制台配合 CLI 完成 MCP Schema 改造；补数据 V2 采用三步式 OpenAPI 设计 |
| **3 种形态** | CLI / Skill / OpenAPI | CLI：人机同源，一条命令人与 Agent 都能跑；Skill：能力封装，可上架技能平台被复用；OpenAPI：供第三方系统深度集成；**三者共用同一套权限与审计，不产生旁路** |

原文给出的核心立场：

> **工具能被 Agent 发现，不等于 Agent 有权执行。**

这句话与"统一网关 + 命令级权限"的设计一一对应——MCP 让工具容易被发现和调用，权限边界则必须由平台在网关处兜住。

## 技能层：1 + 4 + N

原文："接口之上的 Skill 按 **1+4+N** 组织：一个总控编排、四类场景级技能、N 个原子 Skill。"

| 组成 | 职责（配图原文） | 原文补充 |
|---|---|---|
| **1 总控编排** | 负责意图理解与任务编排；决定调用哪些原子能力、按什么顺序；自身不硬编码业务逻辑，能力全部下沉到 Skill | 原文："场景负责组合调用，原子能力保留各自的版本与权限管理" |
| **4 场景级技能** | 面向完整业务场景的组合能力；一个技能解决一类问题、开箱即用；与 4 大场景一一对应，便于沉淀与复制 | 对应数据开发、任务运维诊断、数据治理、定开评估 |
| **N 原子能力技能** | 单一职责、命令级、可复现；可被不同 Agent、不同场景复用；可独立发布到技能平台，被全组织检索安装 | "任务下线推荐、值班诊断、数据源验证和知识清单校验可以独立发布，也能供不同 Agent 复用" |

配图列举的已沉淀真实技能（示例）：`easydata-recommend-task-offline`、`bigdata-duty-agent`、`easydata-datasource-validate`、`easydata-kb-checklist`、`bigdata-sdd-workflow`、`bigdata-knowledge-convert`。

原文对"场景 vs Skill"的界分：**"Skill 是原子操作，场景是 Task，由一系列的 Skill 组成，完成某项具体的工作。"**

## 网关在冷表治理场景中的具体形态

配图《最终效果：从 36,996 到 177 条精准动作》给出了一个完整的交付物示例——`easydata-recommend-task-offline` Skill：

| 维度 | 内容 |
|---|---|
| 权限边界 | 全程只读，只产出建议清单，不执行任何变更 |
| 依赖 | 极少：仅 Python 标准库，无第三方 pip 依赖，可离线部署（CLI 直连拉取，或本地离线分析） |
| 覆盖范围 | 全部 / 指定任务 / 清单文件 / 负责人；跨项目 / 集群（集群 A / 集群 B / 集群 C 一套流程） |
| 入口 | CLI（双数据源）· Skill（Skillhub）· Agent（Skill 被平台 Agent 复用） |
| 本机实测 | 600 行并发写入零丢失；4 进程 × 150 次提交同一库，WAL |

这与 [[easydata-cold-table-governance]]、[[bigdata-duty-agent]] 两个场景页直接对应。

## 设计取舍

- **能力放在平台侧维护**：入口变化时不必重新实现整套业务逻辑
- **不绑定任何 Agent 框架**：Agent 框架迭代极快，能力沉在接口层与技能层，"框架会换，能力资产不会"
- **人机同源**：同一条 CLI 命令人与 Agent 都能跑，避免为 Agent 另造一套通道而形成"旁路"

## 原文未说明

- 网关的具体实现（自研 / 开源网关）、限流策略与阈值
- "命令级权限"如何与 EasyData 既有的 RBAC / 数据权限体系映射
- Skill 上架审核流程、版本兼容策略

## 相关页面

- [[shufan-ai-native-data-platform]] — 总览：五层结构在整体改造中的位置
- [[easydata-cold-table-governance]] — 数据治理场景（`easydata-recommend-task-offline`）
- [[bigdata-duty-agent]] — 任务运维场景（值班 Agent）
- [[easydata-ops-diagnosis]] — 猛犸 OpenAPI + Claude Code 的任务诊断 Skill 对（本文网关所抽象的同类能力）
- [[agent-hook-governance]] — 另一条企业 Agent 治理路径（Hook 护栏）
- [[ai-governance]] — AI 治理：受控执行与可审计
- [[hologres-skills]] — 同为数据平台 Skills 体系化的厂商实践
- [[maxcompute-skills]] — 同为"平台能力 → MCP/Skills"的封装
- [[data-dev-skills-automation]] — 数开 Skills 自动化
- [[agent-mcp-protocol]] — MCP 协议本身
