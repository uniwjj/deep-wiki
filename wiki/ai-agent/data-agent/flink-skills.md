---
title: Flink Skills
description: 阿里云实时计算 Flink 的 Agent Skills 体系——四层能力（Manage/Console Ops/Knowledge/Diagnose）、三层安全防护、多 Skill 协同编排，自然语言驱动从实例创建到实时数仓搭建
aliases: [Flink Skills, Flink Agent, 实时计算Skills]
tags: [big-data, ai-agent, platform, tool]
sources: [2026/05/12/05. Flink Skills—重塑实时计算开发与运维体验.pdf, 2026/05/12/DataWorks DataAgent分享录音.md, 2026/05/28/Flink Skills—重塑实时计算开发与运维体验.pdf]
created: 2026-05-12
updated: 2026-05-29
---

# Flink Skills

**Flink Skills——重塑实时计算开发与运维体验**。演讲人：李昊哲（阿里云实时计算 Flink 产品经理）。

录音补充 4 个现场 demo 场景（2026-05-28 杭州虾聊日）：

1. **作业调优 demo** — 原本需翻 3 个页面对比监控指标查不同文档；现在一句话搞定，Agent 自动调好整体作业的并发度和 CPU；再问"验证一下作业反压有无缓解"，Agent 实时反馈"反压状态已消除"
2. **批量巡检 demo** — 大促前对成百上千 Flink 实例全面巡检：一句话"帮我巡检一下所有的 Flink 实例"，Flink Instance Manage Skill 自动发现全部地域所有实例（杭州、上海等），用户无需指定区域或实例列表，自动遍历所有可用区，输出详细产出报告（运行状态 + 大促风险 + 建议动作）
3. **跨产品组合 demo（实时数仓搭建）** — 8 条对话搭通：Flink Instance Manage 创建实例 → MySQL Skill（RDS 数据库）→ Hologres Skill 建表 → DMS Skill 校验 schema → CMS 配置告警，全在 OpenClaw 终端完成，期间未登录任何控制台
4. **舆情分析 demo（市场部）** — 一句话"帮我部署一个实时舆情分析的 AI 作业"：DataAgent 模拟实时社交媒体评论 → AI Sentiment 函数实时情感分析 → 结果写入 Hologres → 搭建实时看板（正负面评分、点赞、评论实时滚动）

录音原话补充：

- 阿里云已开放 **69 个官方 Skill**，覆盖 6 大云领域；Flink 控制台原生内置 Skill 技能包，开箱即用、零配置启动
- **Create Skill 能力**：用户可基于现有原子能力像搭积木一样搭自己的定制 Skill（举例 Gartement 大促分析 Skill）

## 为什么需要 Skill

从“人操作控制台”到“Agent 操作云资源”的演进：

| 阶段 | 模式 | 特点 |
|------|------|------|
| 2018 控制台 | 点击式操作 | 逐页跳转、人肉校验 |
| 2020 SDK | 基础设施即代码 | 需专业知识 |
| 2023 Copilot | 对话式问答 | 能回答问题，不能执行操作 |
| 2024 Agent+API | 直接调用云接口 | 能执行但不安全，缺乏边界和护栏 |
| 2025-26 Agent+Skills | 安全可控的云操作 | 三层防护、多 Skill 协同 |

**Skill 做了三件事**：
1. **SOP 注入**：将人的经验、手册固化为 Agent 本能
2. **逻辑封装**：屏蔽工具使用的复杂度，实现意图 → 动作直达
3. **记忆是资产，工具是设备，Skill 是生产工艺**

## 核心能力

### Easy：对话即运维，门槛归零

- 自然语言驱动，一句话完成实例创建、作业部署、告警配置
- 多个控制台操作收敛到一个对话窗口，效率提升 **10 倍**
- Flink 控制台内置智能助手，零安装、零学习成本

### Skill Collections：四层能力

| 层级 | 能力 | 说明 |
|------|------|------|
| **Manage** | 实例和命名空间生命周期管理 | 跨地域批量巡检 |
| **Console Ops** | SQL 开发、作业提交、部署上线 | 覆盖日常开发运维全场景 |
| **Knowledge** | 文档与最佳实践沉淀 | 操作前自动校验参数合理性 |
| **Diagnose** | 作业健康诊断 + 性能分析 + 根因定位 | 秒级输出报告 |

### Safety：生产级安全，三层防护

| 防护层 | 机制 | 说明 |
|--------|------|------|
| `--confirm` 门控 | 代码级拦截 | 所有写操作必须显式确认，不是弹窗 |
| 目标锁定 | 操作锚定 | 防止 Agent 操作漂移，创建后只操作同一目标 |
| Read-back 验证 | 结果核实 | 不信 API 返回码，轮询确认实例真正 RUNNING 才成功 |

### Reliable：多 Skill 协同，端到端闭环

- Skill 自由编排，按需组合，不绑定不耦合
- 一次对话跨产品联动
- 渐进式扩展能力边界，不需要一步到位
- 支持自定义 Skill，团队可按场景定制

## 六大典型场景

### 1. 一键 Flink CDC 数据同步
大模型友好的 YAML 声明式管理，全模态数据一键入湖入流。

### 2. 诊断修复
Agent 调用诊断 Skill 分析全生命周期事件，输出风险等级 + 根因 + 修复建议。

### 3. 运维巡检
成百上千 Flink 作业实例批量巡检，30 秒输出结构化矩阵，异常项高亮 + 处置建议。

### 4. 一键搭建高可用实时数仓
一键拉起 Flink + Hologres/EMR StarRocks，快速搭建实时数仓。

### 5. AI 实时舆情分析
针对运营、市场、财务、HR 等非技术人员，调用 Skill 进行实时分析与推理。

### 6. Agent For Agent
使用 Agent 对 Agent 全链路风险感知与实时告警，防止链式攻击注入。

## 场景详解：作业诊断与修复

**痛点**：Failover 只有一行报错、Checkpoint 超时/背压/数据倾斜排查套路不同、半夜告警翻日志。

**方案**：Console Ops Skill + Flink 智能助手 — 自动解析定位，全生命周期事件分析，不用在页面间跳转、不用肉眼比对 Metrics/日志/事件，分析修复一步到位。

## 场景详解：作业诊断与修复

**痛点**：Failover 只有一行报错、Checkpoint 超时/背压/数据倾斜排查套路各不同、半夜告警翻日志。

**方案**：Console Ops Skill + Flink 智能助手一键诊断，自动解析定位，调用诊断 Skill 分析全生命周期事件，输出风险等级 + 根因 + 修复建议，不用在控制台页面间跳转、不用肉眼比对。

### 场景详解：批量巡检诊断

**痛点**：成百上千 Flink 作业实例大促前需批量巡检，AI 写脚本对接 OpenAPI 复杂麻烦，担心遗漏巡检风险项。

**方案**：Flink Instance Manage Skill + 程序化批量调用：
- "帮我巡检所有 Flink 实例"——Skill 自动发现全部地域、逐一检查
- 不再需要记住每个地域有哪些实例，Skill 替你维护全局台账
- 30 秒输出"实例 × 地域"结构化矩阵，异常项高亮 + 处置建议

### 场景详解：品牌舆情实时监控

**痛点**：搭建舆情系统要分别操作多个产品控制台，市场部需提工单给大数据团队排期，等报表出来热点早凉了。

**方案**：Skill 组合编排，从建表到看板一条龙：
- 自然语言描述需求，Agent 自动编排全链路：Datagen 模拟评论 → AI_SENTIMENT 情感分析 → Hologres 写入 → BI 看板 + 钉钉告警
- 市场部同事只需看 BI 看板 + 收钉钉告警，不需要写代码操作控制台
- 所有配置在一次对话中完成

### 场景详解：实时数仓搭建

**痛点**：多个控制台反复横跳（Flink 实例 → RDS → Hologres Sink → 云监控告警），手动填表耗时费力。

**方案**：Skill 联动组合 —

| Skill | 作用 |
|-------|------|
| `flink-instance-manage` | 一句话创建 Flink 实例，自动选规格和可用区 |
| `flink-console-ops` | 自然语言生成 CDC 作业，MySQL → Hologres 全量增量实时同步 |
| `dms-skill` | 目标库自动建表、Schema 一致性校验 |
| 全程不登录控制台 | 从零到实时链路跑通 < 10 条对话 |

大促不怕炸，巡检靠拼装——通过 Create-skill，可以像搭积木一样灵活编排技能组合，打造个性化数据流水线。

## 使用入口

- **阿里云 Skill 门户**：https://skills.aliyun.com/ — 首批 69 个官方 Skills，覆盖六大云领域
- **Flink 控制台内置**：原生内置 Skill 能力包，开箱即用，零配置启动

## 相关页面

- [[dataworks-data-agent]] — DataWorks Data Agent 整体产品
- [[dataworks-2026-0528-xialiaori]] — 2026-05-28 虾聊日全部分享内容汇总
- [[maxcompute-skills]] — MaxCompute 同类 Skills 体系
- [[hologres-skills]] — Hologres Skills（Flink + Hologres 联动）
- [[emr-skills]] — EMR Skills（Flink + EMR StarRocks 联动）
- [[superetl]] — 类似的多 Skill 编排实践经验
- [[agent-mcp-protocol]] — MCP 协议原理
