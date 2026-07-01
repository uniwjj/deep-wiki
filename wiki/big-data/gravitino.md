---
title: Apache Gravitino
description: 统一元数据湖——高性能/地理分布式/联邦化定位、Metalake→Catalog→Schema→Table 四层对象模型、统一 REST API、与 Hive Metastore 关系、RBAC 授权下推、AI 模型元数据、与 DataHub/Unity Catalog 对比
aliases: [Gravitino, Apache Gravitino, 元数据湖, Metadata Lake, 统一元数据管理]
tags: [big-data, tool, concept]
sources: [2026/07/01/gravitino-core-objects.html, 2026/07/01/gravitino-governance-paradigm.html, 2026/07/01/gravitino-enterprise-architecture.html, 2026/07/01/gravitino-federated-lake.html, 2026/07/01/gravitino-geo-distributed.html, 2026/07/01/gravitino-unified-bloodline.html, 2026/07/01/gravitino-ml-practice.html, 2026/07/01/gravitino-1-0-context.html, 2026/07/01/gravitino-xiaomi-upgrade.html, 2026/07/01/gravitino-ranger-auth.html, 2026/07/01/gravitino-tbds-upgrade.html, 2026/07/01/gravitino-hive-iceberg-kafka.html, 2026/07/01/gravitino-datahub-unity-comparison.html, 2026/07/01/gravitino-bilibili-practice.html, 2026/07/01/gravitino-multimodal-lakehouse.html, 2026/07/01/gravitino-ai-unified.html]
created: 2026-07-01
updated: 2026-07-01
---

# Apache Gravitino

Apache Gravitino 是一个**高性能、地理分布式、联邦化的统一元数据湖平台**（Metadata Lake）。它把数据湖、数据仓库、消息队列、文件系统、AI 模型等异构数据源的元数据统一到一套标准对象模型与 REST API 之下，实现统一发现、统一权限、统一血缘，解决多云多源导致的数据孤岛与元数据碎片化问题。

## 要解决的问题

来源 [[gravitino-governance-paradigm|数据湖治理新范式]] 指出新时代数据湖管理的三大挑战：

1. **多云导致数据孤岛** —— 公有云/私有云/数据中心分散部署，数据跨系统跨地域难流通；AI 发展使数据合规与地域保护要求达到前所未有的高度。
2. **数据源类型多，难统一发现治理** —— Spark/Trino/Flink 引擎与 Ray/PyTorch/TensorFlow AI 框架都要访问分散存储的数据，缺统一发现与访问机制。
3. **隐藏的元数据无人管** —— 数据属哪个业务、属主是谁、版权、生命周期、敏感分级等"描绘数据的数据"对治理至关重要，旧元数据系统满足不了新场景。

Gravitino 因此孵化，定位为**高性能、地理分布式、联邦化**的元数据湖——不迁移数据，统一访问和控制不同数据源的元数据。

## 四层对象模型

Gravitino 的核心是**严格四层树形结构**的对象模型（来源 [[gravitino-core-objects|核心对象]]、[[gravitino-enterprise-architecture|企业级架构]]）：

```
Metalake（元数据湖，顶层容器）
  └── Catalog（数据源，核心抽象）
        └── Schema（命名空间）
              ├── Table（关系表）
              ├── Fileset（文件集）
              ├── Topic（消息主题）
              └── Model（AI/ML 模型）
```

### Metalake：元数据湖（顶层容器）

Metalake 是最顶层容器，可理解为"元数据租户"——每个团队/业务线一个 Metalake，内部元数据互不可见。核心属性仅三个：`name`（全局唯一）、`comment`、`properties`。Metalake 间元数据完全隔离，RBAC 权限独立生效。小组织通常 1 个，大组织按业务线多个。

### Catalog：数据源（核心抽象）

Catalog 是 Gravitino 最重要的抽象——**每个 Catalog 代表一种数据源**。创建时指定两个关键字段：

- **type** —— 数据源大类：`relational`（关系型）、`lakehouse`（数据湖）、`messaging`（消息队列）、`file`（文件系统）、`model`（AI 模型）等。
- **provider** —— 具体实现：`mysql`、`hive`、`iceberg`、`paimon`、`kafka`、`hdfs` 等。

创建 Catalog 后，其下 Schema 和 Table 自动从底层数据源同步——Gravitino 的 Connector 直接操作底层系统获取元数据，无需手动逐个创建。

### Schema：命名空间

Schema 是 Catalog 下的逻辑分组，对应关系库的 database/namespace。连接 Catalog 后底层 database 自动以 Schema 形式出现。Schema 本身简单，价值在于作为叶节点对象容器。

### 叶节点对象

一个 Schema 下可**同级混搭**多种对象：Table（关系表）、Fileset（文件集，管理非结构化文件）、Topic（Kafka 等消息主题）、Model（AI/ML 模型）。无论底层是 MySQL 还是 Kafka，操作方式完全统一。

**层级固定不能跳级**，但同级类型可混搭，且统一 API。

## 统一 REST API 与 Iceberg REST Catalog

Gravitino 上层提供**统一 REST API**作为操作访问接口，所有对象（catalog/schema/table 等）的创建、修改、删除均通过标准 REST 完成。在此基础上可构建统一访问、统一血缘、统一权限等治理能力。

特别地，Gravitino 提供了 **Iceberg REST API 的实现**——对 Iceberg 生态原生支持。来源 [[gravitino-governance-paradigm|治理新范式]] 指出，在 Iceberg 场景下推荐直接用 Gravitino 的 REST Catalog，无需额外 Hive Metastore。

### 与 Hive Metastore 的关系

Gravitino **不是替代 Hive Metastore，而是前置代理/统一接口层**（来源 [[gravitino-governance-paradigm|治理新范式]]）。它通过 Connector 对接 HMS 等已有元数据源，在其上加统一模型、统一权限、统一血缘。已有 Hive/Iceberg 表无需迁移即可纳入 Gravitino 管理。

## 统一结构化 + 非结构化 + AI 模型元数据

Gravitino 的统一性体现在覆盖四类数据：

| 类型 | 对象 | provider 示例 |
|------|------|--------------|
| 关系型 | Table | mysql、postgresql |
| 数据湖 | Table | hive、iceberg、paimon、hudi |
| 消息队列 | Topic | kafka |
| 文件系统 | Fileset | hdfs、s3 |
| AI/ML 模型 | Model | （0.8+ 引入） |

**0.8 版本引入对 AI、机器学习模型元数据的管理**——这是 Gravitino 区别于传统数据目录的关键：它把 AI 模型也作为一等公民纳入统一元数据治理，让数据工程师与算法工程师用同一套元数据体系定位访问资产。深度集成 Trino/Spark/Flink 引擎与 Ray/PyTorch/TensorFlow AI 框架。

## 安全：RBAC 与授权下推

企业级安全是 Gravitino 的重要特性（来源 [[gravitino-enterprise-architecture|企业级架构]]、[[gravitino-ranger-auth|Ranger 授权]]）：

- **多协议认证** —— OAuth2、Kerberos、IAM 等。
- **RBAC 细粒度权限** —— 基于角色的访问控制，精确控制对元数据对象的操作权限。分层授权：管理员创建 Metalake 指定用户管理器 → 用户管理器创建角色分配权限 → 用户凭权限操作。
- **授权下推** —— 权限控制可下推到底层数据源，即使通过 Gravitino 访问也保持原安全策略，实现端到端安全。
- **Ranger 集成** —— 多集群 Ranger 配置，按 Catalog 维度加载授权插件。

配合操作日志全链路追踪，支撑安全管控与合规审计。

## 地理分布式与联邦化

Gravitino 的"地理分布式、联邦化"定位（来源 [[gravitino-geo-distributed|高性能地理分布式]]、[[gravitino-federated-lake|联邦元数据湖]]）：

- **联邦化** —— 不迁移数据，通过 Connector 统一访问不同数据源元数据。
- **地理分布式** —— 支持跨区域元数据同步，事件监听器 + 同步策略（最终一致性 vs 强一致性）+ 冲突解决，为全球数据治理提供基础。

## 与 DataHub / Unity Catalog 对比

来源 [[gravitino-datahub-unity-comparison|三者对比]] 的架构哲学差异：

| 平台 | 架构哲学 | 特点 |
|------|---------|------|
| **Gravitino** | 直接元数据管理 | 统一对象模型 + REST API，主动管理 catalog/schema/table 全流程操作 |
| **DataHub** | 事件驱动 | 从各系统抽取元数据构建知识图谱，偏元数据发现与血缘，不主动操作 |
| **Unity Catalog OSS** | 开放标准 | Databricks 开源，强调统一治理标准 |

Gravitino 的差异化在于**主动操作元数据**（不只读不写）+ **覆盖 AI 模型**+ **地理分布式联邦**。

## 1.0 与未来：从元数据管理到智能上下文工程

来源 [[gravitino-1-0-context|1.0.0]] 指出 Gravitino 1.0 的方向：从单纯的元数据管理演进到**智能上下文工程**——为 AI Agent 提供统一的上下文（数据资产 + 模型 + 权限 + 血缘），成为 AI 时代数据与 AI 资产的统一入口。这与多模态湖仓（Gravitino + Daft + Lance，来源 [[gravitino-multimodal-lakehouse|多模态湖仓]]）的方向一致。

## 落地案例

- **小米**：Gravitino 升级实践，把表格数据（Hive/Iceberg/MySQL/Oracle）与 AI 资产元数据统一管理（来源 [[gravitino-xiaomi-upgrade|小米升级]]、[[gravitino-ai-unified|小米 AI 框架]]）。
- **B 站**：生产环境统一元数据管理落地（来源 [[gravitino-bilibili-practice|B站实践]]）。
- **腾讯 TBDS**：Gravitino 助力架构升级，打造新一代数据湖仓产品（来源 [[gravitino-tbds-upgrade|TBDS 升级]]）。
- 与 Paimon 协同的元数据治理方案见 [[paimon-gravitino-collaboration|Gravitino 与 Paimon 协同]]。

## 相关页面

- [[iceberg]] — Iceberg 场景下推荐直接用 Gravitino REST Catalog
- [[paimon]] — Paimon 表格式（Gravitino 统一管理 Paimon 元数据）
- [[apache-flink]] — Flink 是 Gravitino 深度集成的引擎之一
- [[agentic-data-cloud]] — Gravitino 为 AI Agent 提供统一上下文
