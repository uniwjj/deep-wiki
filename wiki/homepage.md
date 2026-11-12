---
title: Deep Study 知识库
description: 平台开发专家的技术学习与知识沉淀中心
aliases: [Deep Study, 知识库首页]
tags: [meta, summary]
sources: [2026/05/10/lint-stub.md]
created: 2026-05-09
updated: 2026-11-12
---

# Deep Study

平台开发专家的技术学习与知识沉淀中心，聚焦 **大数据开发治理平台** 相关技术领域。

## 知识领域

| 领域 | 目录 | 说明 |
|------|------|------|
| [[big-data/index|大数据技术]] | `big-data/` | Spark, Flink, Kafka, Hudi, Iceberg, 数据湖, 流批一体, 数据治理 |
| [[distributed/index|分布式系统]] | `distributed/` | 共识算法, CAP 理论, 分布式存储与计算 |
| [[ai-ml/index|AI/ML/LLM]] | `ai-ml/` | 大模型, RAG, 向量数据库, Prompt Engineering, 模型训练推理 |
| [[ai-agent/index|AI Agent 开发]] | `ai-agent/` | Agent 框架, 工具调用, MCP, 多 Agent 协作, Agentic Workflow |
| [[fullstack/index|全栈开发]] | `fullstack/` | React/Vue, TypeScript, Next.js, Node.js, tRPC, Tailwind CSS |
| [[backend/index|后端工程]] | `backend/` | Go/Java, 数据库, API 设计, 系统设计 |
| [[platform/index|平台工程]] | `platform/` | DevOps, 可观测性, Kubernetes, CI/CD |
| [[architecture/index|架构与设计]] | `architecture/` | 架构模式, DDD, 事件驱动, CQRS |
| [[product/index|产品与技术管理]] | `product/` | 产品方法论, 技术战略, 工程管理, 增长框架 |

## 快速入口

- `wiki-purpose.md` — 定位与范围（根目录配置文件）
- `wiki-schema.md` — 知识库组织规则（根目录配置文件）

## 最近更新

- [[flock-collaboration-space]] — 协同空间 Flock（阿里云 郑天，2026-11-11）：控制面/执行面分离、Channel 网与跨空间建任务、Sessionless 会话、Quest 责任状与「待验收」、任务链自然长出 DAG、互相唤醒的坑与记忆从推到拉；含与 DSH SuperAgent 的两处分歧
- [[superagent-prototype-internals]] — SuperAgent 原型（dsh-superagent-prototype，1.6 万行）源码级拆解：Ticket/Mandate/Policy、一条消息的五步链路、五条设计判断（模型提议程序裁决 / 先记账再动手 / 外部文本只是数据 / 主动性灰度 / 人的优先级最高）与原型边界校正
- [[agent-pattern-ladder]] — Agent 设计模式阶梯：RAG → 单 Agent → Multi-Agent RAG → Plan-and-Execute → ReAct 五种模式各自解决的问题、边界与代价、选型与组合
- [[data-to-knowledge-engineering]] — 从数据工程到知识工程（关涛，云器科技，DACon 2026）：知识是第三次数据革命，概念地图与五条公理、知识五种表达形态、本体五条路线（Palantir / Semantic View / Ossie / Genie / Agent Memory）、Trace 获取与四步加工、验证查询与文档蒸馏两个案例、四条架构共识与 Eco-loop
- [[databuddy-semql-metarag]] — 拆解腾讯 DataBuddy 的 Text-to-Semantic-to-SQL 与 MetaRAG：语义层四要素、SemQL 确定性编译、F-RAG/S-RAG 双路召回、覆盖度三级路由与指标 MCP
- [[shufan-ai-native-data-platform]] — 网易数帆 AI Native 数据平台（蒋鸿翔，2026-11-03）：为 Agent 重建数据层，统一存储计算 + EasyData 五层受控开放 + DataAgent 本体与 Harness
- [[easydata-cold-table-governance]] — 冷表治理：以"表"为锚反向定位"任务"，36,996 → 177 条精准动作
- [[bigdata-duty-agent]] — 大数据值班 Agent：规则库 + 大模型兜底 + 候选沉淀，多环境实测命中与归因
- [[dataagent-ontology-harness]] — DataAgent 业务本体与 Harness：准确率 <50% → 95%+ 与五道关
- [[table-knowledge-graph]] — 表知识图谱：物理/业务实体图谱 + LightRAG Local/Global 检索
- [[goalsong-ontology-thesis]] — 《GOALSONG 本体论纲》（高颂数科，2026-10，327 页）：面向智能体时代的企业本体方法论总纲——六篇十五章，从语义鸿沟到可信 AI 基础设施
- [[ontology-semantic-infrastructure]] — 第 3 章：本体作为智能体语义基础设施，破解身份/语义/行动三重瓶颈与能力边界
- [[minimal-world-model]] — 第 5 章：最小世界模型（SmallWorld-小世界）——先跑通一个闭环，再扩展世界
- [[ontology-six-step-method]] — 第 6 章：GB/T 48000.3—2026 落到工程的六步构建法与六条核心原则
- [[semi-auto-ontology-and-lightning-action]] — 第 7 章：半自动构建（机器提议→人工定夺）与八周闪电行动五步法
- [[small-world-network]] — 第 9 章：小世界网络——本体定义世界、合约定义规则、OBAC 定义边界，智能体自主结伴协同
- [[data-asset-operation-2]] — 第 10 章：数据资产运营 2.0——本体泛在连接、四条变现路径、四个运营机制
- [[effective-token-economy]] — 第 12 章：词元与有效词元——本体作为词元的四层语义锚点，有效词元=总词元×等级系数×使用系数
- [[ontology-governance]] — 第 14 章：本体治理——四层治理架构、RFC 五步变更、四维健康度评估、五类消费方生态
- [[trusted-ai-infrastructure]] — 第 15 章：可信 AI 基础设施四层架构——身份（AIC）/验证（TAV）/计量（词元运营中心）/连接（ioC），本体是"根"
- [[goalsong-methodology-system]] — 附录 B：高颂数科十套本体工程方法论、组合矩阵与选择决策树
- [[databuddy]] — 腾讯云 DataBuddy（2026-10-20）：Agent Native 大数据智能体工作台，四类 Agent + 统一语义层 + OneOps + Agent Runtime 三层安全
- [[databuddy-ecs-demo]] — DataBuddy 电商端到端演示：18 节点 Pipeline、字段改名故障自愈、指标语义模型、零售看板、协同过滤推荐上线
- [[super-agent]] — Super-agent 设计探索：DeepSeek Harness 团队提出的持续在线自主 Agent 架构（双层架构 / 事件驱动 / 全双工 / 存算分离 / Watchdog）
- [[super-agent-classic-cs-mapping]] — Super-agent 的经典计算机系统映射：Fast/Slow Path、Cache、投机执行、存算分离、Control Loop
- [[agent-event-driven-triggering]] — 事件驱动：Trigger 两级过滤与 Thread 订阅直通
- [[sora-system-of-record-for-agents]] — SORA：Ben Horowitz 万字长文《Databricks & the Future of the Firm》——智能体的记录系统（状态/本体/护栏/评估四问闭环）
- [[databricks-evolution]] — Databricks 四条技术 S 曲线：Spark → Lakehouse → Lakebase → Agents，1900 亿美元估值与 a16z 投资故事
- [[managerial-revolution-ai]] — 第二次经理人革命：AI 接管信息路由，"每个人都变成管理者"
- [[sovereign-firm]] — 主权企业：模型随时可换，记忆永不归零
- [[theory-of-the-firm]] — 企业的性质：科斯 + 西蒙，AI 改变的是企业形态而非大小
- [[agent-capability-moore-law]] — 智能体能力新摩尔定律：METR 任务时长约每 7 个月翻倍
- [[dacon-2026-beijing]] — DACon 2026 北京站：17 个分论坛、42 场演讲总索引（Data Agent / 评测 / 数字员工 / 语义层与本体 / 上下文工程 / 模型组合 / 生成式推荐）
- [[databricks-2026-summit]] — Databricks 2026 Data+AI Summit 变革综述：四层架构、LTAP、Genie Ontology
- [[enterprise-agi-framework]] — 企业级 AGI 框架：SoI 四层堆栈、本体成熟度模型
- [[oceanbase-ai-database]] — OceanBase 湖库一体 AI 数据库
- [[ltap-architecture]] — LTAP 架构：首个统一事务与分析处理的湖上架构

## 相关页面

- `wiki-purpose.md` — 定位与范围（根目录配置文件）
- `wiki-schema.md` — 知识库组织规则（根目录配置文件）
