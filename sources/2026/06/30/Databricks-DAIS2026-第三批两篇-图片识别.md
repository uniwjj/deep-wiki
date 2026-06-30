# 图片内容识别结果（第三批两篇文章）

识别方式：Tesseract OCR (chi_sim+eng) + 视觉复核

## 文章一：318| 企业级AGI（王知鱼，译自 SiliconANGLE Breaking Analysis）

### agi_1.png (1080×720) — 威利狼隐喻开篇图
Sam Altman 冲过企业级机会冲下超级智能悬崖，Ali Ghodsi 和 Satya Nadella 在边缘冷眼旁观。

### agi_2.jpg — 方法1：数据共产主义（Data Communism）
最聪明的人把推理轨迹贡献给前沿模型，智能被打包进模型分发给每个人。每个人得到相同的内置智能。马克思主义隐喻"各尽所能，按需分配"。

### agi_3.jpg — 方法2：数据资本主义（Data Capitalism）
智能作为每家企业独有的资产。堆栈：System of Record（数据平台，发生了什么）→ System of Intelligence SoI（为什么/可能/接下来）→ System of Agency（Agent 通过 SoI 操作化决策）→ System of Engagement（参与系统）。Jamie Dimon 作为 Owner/Capitalist 隐喻。

### agi_4.jpg — Databricks 企业智能堆栈映射
将 SoI 框架映射到 Databricks：System of Engagement（Genie One/Agents/App Builder/CustomerLake）→ System of Agency（Agent Bricks/Omnigent/Genie Code）→ System of Intelligence（Genie Ontology + Unity Catalog）→ Data Platform（Lakeflow/Lakehouse//RT/Lakebase）。

### agi_5.jpg — Genie One：Agentic Client
"data-smart AI coworker"，数据感知型 AI 同事。

### agi_6.jpg — Many Genies：Everyone. Everywhere. Everything.
Genie 扩展到多角色（ONE/CODE 等），连接一切（Lakehouse/Slack/Teams/Mobile/工具），到处访问。

### agi_7.jpg — Genie Ontology：智能后端内核
当前层级：Semantically harmonized entities and metrics（语义协调的实体和指标）。数字 450,185 / 2,305 / 63 / 733（知识片段统计）。

### agi_8.jpg — Genie Ontology 如何学习：动态上下文图谱
输入支流（人类信号）+ 实时交互循环 → 动态上下文图谱（按来源/权威性/频率/新鲜度加权）。

### agi_9.jpg — 澄清循环：客户端如何教导本体
行为排气 + 终端用户澄清。Sales Opportunities 示例。

### agi_10.jpg — 本体成熟度模型 1-9级（核心图）⭐
当前 Databricks 处于 5-6级。三级维度：Data model scope and fidelity / Analytic sophistication / Actionability。
- 1级 Stored Reporting（存储报告）："这个季度多少交易"——单一系统
- 2级 Enterprise Data Warehouse（企业数仓）：Acme=3条未链接记录（CRM/credit/KYC）
- 3级 Real-Time Event Hub（实时事件中心）：每个事件保留上下文可重放
- 4级 Behavioral Analytics（行为分析）：相关性
- 5级 Predictive Analytics Hub（预测分析中心）：Acme 流失23%+$125M预期——银行家决定
- **6级 Enterprise Knowledge Graph（企业知识图谱）**：Acme-BlueStar 类型化锚点边——Agent在狭窄受治理通道按意图行动
- **7级 Semantic Action Layer（语义行动层）**："提交信用备忘录"=数据：前提条件/效果/护栏——Agent在护栏内选择并运行行动
- 8级 Real-Time Digital Twin（实时数字孪生）：Acme实时状态——分析=运营
- 9级 Autonomous Operations Platform（自主运营平台）：工作流本身成为可编辑数据——Agent规划优化，人类设定目标

### agi_11.jpg — 构建受治理可执行本体：自下而上+自上而下混合（核心图）⭐
三层：Structure（结构，自下而上最强）/ Operating logic（运营逻辑，难）/ Authority & cross-cutting norms（权威与跨领域规范，最难）。
技能提升管道（promotion pipeline）：01 Author locally（本地编写）→ 02 Abstract（抽象）→ 03 Route by risk（按风险路由）→ 04 Govern（治理）→ 05 Promote（提升），且愿意 walk back（回退）/ re-fragment（重新碎片化）。
"the waterline is the seam"——冰面线是接缝。

### agi_12.jpg — Omnigent 连接第三方 SoE Agent 到 Unity AI Gateway
治理异构 Agent 资产。Unity AI Gateway 作为"for your entire AI estate"。

### agi_13.jpg — 通往企业级AGI的竞赛
各厂商从不同位置进入：前沿实验室（参与系统）/ 数据平台（Databricks/Snowflake，受治理数据）/ SaaS和流程供应商（Salesforce/SAP/Palantir/Celonis/Blue Yonder/RelationalAI，记录系统）。所有道路通向 SoI（智能系统=数字孪生或企业本体）。

### agi_14.jpg — 企业软件演化为两大类：冰面之上/冰面之下（核心图）⭐
詹姆斯·邦德隐喻。
- **Above the Ice（冰面之上）**：从模型化数据学习业务（SoI/SoA/SoE），走向 outcome pricing（成果定价）。Applications / Platform (business process model)
- **Below the Ice（冰面之下）**：构建基础设施（数据平台/程序化应用及以下），utility pricing（公用事业定价）。Infrastructure

## 文章二：Databricks 18件事（云计算指北）

### eighteen_1.png (1080×720) — 峰会主视觉/封面图

### eighteen_2.png — 四层架构图（核心图）⭐
"底座→治理→Agent平台→应用，Agent Runtime 升到最上层；治理被显式塞在 Agent 与数据之间，成为必经关卡。"
- 应用层：Genie One / CustomerLake / Lakewatch
- Agent 平台：Agent Bricks / Omnigent / Genie Code
- 治理/语义：Unity Catalog / Unity AI Gateway（Agent 调用数据必须经过这道治理关卡）
- 数据底座：LTAP / Lakebase / Lakehouse//RT

### eighteen_10.png — Genie Ontology 上下文产品图（核心图）⭐
三段工作机制：自动抽取（表/查询/Dashboard/Pipeline/Wiki/Jira/Slack/SharePoint/Drive）→ 类PageRank权威加权（5维度）→ 强制继承权限。
5维度：1.来源系统权威性 2.作者组织权威 3.使用频率 4.是否关联已认证资产 5.新鲜度。

### eighteen_11.png — LTAP 三份副本→一份数据（核心图）⭐
CDC时代：OLTP(Aurora/Postgres) + OLAP(Snowflake/BigQuery) + CDC管线 = 一份数据三份副本，延迟分钟到小时。
LTAP时代：One Copy，开放格式 Delta/Iceberg，不再CDC不再复制 = Lakebase + Delta UniForm + Lakehouse//RT。权限治理监控只做一遍。

### eighteen_21.png — Apps on Marketplace
合作伙伴 App 跑在客户 Workspace，IP 不出域、数据不出域。

### eighteen_22.png — OpenSharing Genie Agents
合作伙伴 OpenShare 聚焦的 Genie Agent，客户用自然语言提问，数据/上下文/访问受控。

### eighteen_25.png — 9条战线竞争格局图（核心图）⭐
强挤压/正面对撞/被拉成生态伙伴三档。
- 强挤压：Snowflake（LTAP/Lakehouse//RT/CustomerLake三线）/ Pinecone等向量库（Lakebase Search蚕食）/ Splunk等SIEM（Lakewatch+Panther）
- 正面对撞/蚕食：Salesforce Data Cloud / LangChain等框架（Omnigent降维）/ AWS+GCP
- 被拉成生态伙伴：Adobe（RT-CDP被拉成CustomerLake启动伙伴）

### eighteen_26.png — CIO 5条务实建议（核心图）⭐
1.内部高价值流程试点Genie One/Agents 2.评估Tableau/Power BI迁移 3.治理先行 4.重新建模Token与Tool Call成本 5.盯紧Lakebase与LTAP。
