# 图片内容识别结果

文章：从Databricks产品发布会看数据平台的演进方向
识别方式：Tesseract OCR (chi_sim+eng) + 视觉复核

---
ingested: 2026-06-30
wiki_pages: [databricks-2026-summit, genie-ontology, omnigent-meta-harness, ai-native-data-platform-report, databricks-genie]
---

## img1.jpg (1214×1114) — 图1 Databricks四层架构图（核心架构图）

标题：「从统一数据底座到AI Agent运行时，四层架构重构企业数据平台新范式」

四层架构（自上而下）：

1. **应用层** — AI驱动的业务应用与解决方案
   - Genie AI业务助手、行业与领域解决方案（金融/零售/制造/生命科学/AI）
   - 数据应用生态（合作伙伴+开发者生态）
   - 平台级能力贯穿四层：安全与合规（加密/隐私保护/SSO/IAM）

2. **Agent平台层** — 构建、部署与运营AI Agent
   - Agent Bricks（托管式Agent平台，全生命周期管理）
   - 工作流 / 多Agent协作
   - 工具与连接器：MCP / API / SDK，工具调用 / Prompt管理，数据库 / 第三方系统
   - 运行时与沙箱：会话管理 / 状态存储，沙箱 / 执行环境
   - 可观测性与评估：监控 / 指标 / 日志，评估 / ROII
   - Model Serving（模型服务）
   - 开放生态与框架兼容：LangChain / LlamaIndex / CrewAI 等
   - 系统管控：性能与健康度

3. **治理与语义层** — 统一数据与AI资产目录与运行时控制
   - Unity Catalog：统一数据与AI资产目录
     - 权限管理 / 分级分类、数据质量 / SLA、版本管理
   - Unity AI Gateway（策略执行 / 路由 / 安全防护 / 成本控制）
     - AI资产治理（运行时）：访问控制 / Prompt安全、PII脱敏 / 审计、Token预算 / 用量管控、成本分析 / 计费
   - 语义层：业务术语 / HR、知识抽取 / LLM

4. **数据底座层** — 统一、开放、高性能的数据基础设施
   - Lakehouse 统一存储与计算
     - 开放格式：Delta Lake / Iceberg（开放格式 / ACID事务）
     - Apache Spark（Serverless / Photon）
     - 流式处理 / Retina
     - HTAP / Postgres生态（向量 / 搜索 / AI增强）
     - Lakehouse Federation
   - 多云与开放基础设施：AWS / Azure / Google Cloud
   - 生态合作伙伴与集成：Microsoft / Snowflake / Fivetran / dbt Labs / Palantir
   - Finops（算力与加速）

## img2.png (471×663) — 《AI原生数据平台研究报告(2026年)》封面

- 报告名：AI原生数据平台研究报告（2026年）
- 发布方：CCSA TC601 大数据技术标准推进委员会
- 时间：2026年6月

## img3.png (1080×590) — AI原生数据平台 6+2层架构图（报告参考架构）

定义：AI原生数据平台是一个以"人+AI"为核心用户，通过自然语言交互理解用户意图，将数据资产封装为可调用的业务技能，并围绕具体任务自动编排执行路径，最终实现从数据到结果闭环的智能基础设施。

可见模块（自上而下/分层）：
- 应用层：智能BI/ChatBI、报表、Copilot类、行业垂直Agent（营销等）
- Agent编排与调度：Planner、模型推理网关、Agent、工具中心、人IDE
- 计算与数据加工：通用计算引擎、SQL、特征、模型（训练/推理/管理）、Prompt防护
- 数据集成：Kafka/Pulsar、Sqoop/DataX、Crawler、SFTP、API/MCP/Webhook
- 存储基础设施

## img4.png (462×40) — 装饰性分隔条

OCR误读为"人往基推荐"，实为文章排版装饰元素，无实质信息。

## img5.jpg (474×348) — 公众号二维码/关注卡片

含"专供干货"等推广文字与二维码，为公众号引流卡片，非文章技术内容。
