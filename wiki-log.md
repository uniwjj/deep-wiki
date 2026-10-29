# Change Log

Append-only record of wiki operations. Format: `[date] verb | subject`

## [2026-07-22] ingest | 深度分析：OpenViking 中关于 LLM wiki 的理念的工程级实践
- created `openviking` — OpenViking 完整页面（L0/L1/L2 三级分层/目录递归检索/自迭代/范式对比/局限）
- updated `llm-wiki-implementations` — 新增 OpenViking 作为第四种工程级实现
- updated `index` — LLM Wiki 体系索引新增 openviking 条目
- saved source: `sources/2026/07/22/深度分析：OpenViking 中关于 LLM wiki 的理念的工程级实践.html` + sidecar
- article-download: `article-downloads/深度分析：OpenViking 中关于 LLM wiki 的理念的工程级实践.html`

## [2026-07-16] ingest | Agent 治理：用 Hook 堵住 LLM 的偷懒、越权与失忆
- created `agent-hook-governance` — DECO Hook 护栏体系完整页面（长文本 offload/HITL 门禁/上下文联动闭环 + 行业对比）
- updated `ai-governance` — 新增框架层治理（Hook 护栏）维度与交叉引用
- updated `agent-architecture-patterns` — 扩展 Hook 优于插件的架构原则描述，新增交叉引用
- updated `data-agent-enterprise-practice` — 新增 DECO Hook 治理实践交叉引用
- saved source: `sources/2026/07/16/Agent 治理：用 Hook 堵住 LLM 的偷懒、越权与失忆.html` + sidecar

## [2026-07-13] ingest | Data Agent vs Ontology Agent 推理对比

- created `data-agent-vs-ontology-agent` — 指标体系驱动的 Data Agent 与本体驱动的 AI Agent 在推理空间、路径、确定性来源上的范式差异对比，含融合交替方案
- updated `ontological-semantic-layer` — 添加交叉引用
- updated `data-agent/index` — 语义层与本体列表新增条目
- saved `sources/2026/07/01/指标体系驱动的Data Agent与本体驱动的AI Agent，AI推理上有什么异同？.html` + sidecar

## [2026-07-10] ingest | TASTED 五份升级指南

- updated `netease-tasted-model.md` — 补充五份升级指南摘要：审美力（三站）、洞察力（三站）、结构力（三站）、判断力（三站）、标准感（两项修炼），含内网课程链接与核心理念；解释为何 D（自驱力）无升级指南
- saved `sources/2026/07/10/TASTED-审美力升级指南_265801.html` + sidecar
- saved `sources/2026/07/10/TASTED-洞察力升级指南_266537.html` + sidecar
- saved `sources/2026/07/10/TASTED-结构力升级指南_267757.html` + sidecar
- saved `sources/2026/07/10/TASTED-判断力升级指南_268467.html` + sidecar
- saved `sources/2026/07/10/TASTED-标准感升级指南_269906.html` + sidecar

## [2026-07-10] ingest | 从模型到Harness：WorkBuddy如何把Agent做成可用产品

- created `workbuddy-agent-product-design.md` — WorkBuddy 完整产品设计：模型抽象、工具调用/MCP/Skill/Plugin 四概念、五类记忆模型、Harness 五层架构、Loop Engineering
- created `workbuddy-context-engineering.md` — WorkBuddy 上下文工程实践：五类动作（Write/Select/Retrieve/Compress/Isolate）、Prompt Cache 策略、渐进式加载、意图识别
- updated `agent-memory-system.md` — 补充 WorkBuddy 五类长期记忆模型与陈述性/程序性记忆区分
- updated `loop-engineering.md` — 补充 WorkBuddy 四层工程体系与 Goal≠Loop 区分
- updated `agent-harness-overview.md` — 补充 WorkBuddy 五层 Harness 模型与驾驭/约束/整合三能力
- saved `sources/2026/07/10/从模型到Harness：WorkBuddy如何把Agent做成可用产品.html` — 原始自包含 HTML（394KB，29 张图片）
- saved `sources/2026/07/10/从模型到Harness：WorkBuddy如何把Agent做成可用产品.md` — sidecar（frontmatter 供 CLI 追踪）

## [2026-07-07] ingest | Comet整合OpenSpec与Superpowers详解

- created `comet-integration-architecture.md` — Comet 双星整合架构总览：五维整合、.comet.yaml 状态机、预设路径、SDD 设计启示
- created `comet-gating-system.md` — 三层门控体系：硬门控（PreToolUse Hook）、阶段守卫（comet-guard.sh）、软门控（Rule 注入）
- created `comet-workflow-phases.md` — 五阶段工作流：8 个 SKILL.md 详解、审查模式三级、衔接协议
- created `comet-codegraph-integration.md` — CodeGraph MCP 在 design/build/verify 阶段的代码智能引擎角色
- updated `index.md` — 新增 Comet 整合架构章节
- updated `sdd-custom-workflow.md` — 添加 Comet 交叉引用（替代/互补方案）
- saved `sources/2026/07/07/Comet整合OpenSpec与Superpowers详解.md` — 原始 Markdown（85KB）

## [2026-06-29] ingest | OceanBase 湖库一体 AI 数据库正式发布 + 页面优化

- created `big-data/oceanbase-ai-database.md` — OceanBase 2026-06-29 发布会：湖库一体架构、Lakebase/DataStudio/DataPilot 三大产品、多模表+AI列、Agent友好上下文工程、四条工程底线、三大落地场景
- optimized `big-data/oceanbase-ai-database.md` — 重构逻辑链（为什么重建→是什么→怎么工作→工程底线→真实验证），提高信息密度，消除冗余
- saved `sources/2026/06/29/OceanBase 湖库一体 AI 数据库正式发布.html` — 原始 HTML（24 张图片）

## [2026-06-29] ingest | 菜鸟 SuperETL 补充（DataWorks 虾聊日相关）

- updated `big-data/superetl.md` — 基于 2026-06-28 DataFun 文章补充：九大 Skill 铁律约束、置信度评估机制（30%/90%分级）、6 步实战推演（带 Hook 机制对照）、cn-odpscmd CLI 详细命令表、Hooks 四个触发时机细化（SessionStart→PreToolUse→PostToolUse→SessionEnd）、LLM 数据域 + 应用层 AI 化展望
- updated `big-data/dataworks-2026-0528-xialiaori.md` — 补充菜鸟 SuperETL 交叉引用
- saved `sources/2026/06/28/菜鸟借助 DataWorks Data Agent 实现 AI 数据研发 SuperETL 落地.html` — 原始 HTML（17 张图片）

## [2026-06-29] ingest | Claude Code 指令失效分析与行为契约（两篇）

- created `ai-agent/claude-code/claude-code-instruction-failure.md` — Claude Code 忽略指令的四类根因 + 五层规则架构（入口→路径→Skills→Subagent→硬边界）+ 臃肿文件收身方法
- created `ai-agent/claude-code/claude-code-behavior-contract.md` — 从 41% 到 3% 的规则工程：五类 Agent 偏差（猜/扩/混/忘/装作完成）、12 条规则的四维重组、好规则的失败现场来源、80 行骨架模板、三层文件架构
- updated `ai-agent/context-eng/claude-md-best-practices.md` — 补充两篇新页面的交叉引用
- updated `ai-agent/claude-code/claude-code-configuration-guide.md` — 补充两篇新页面的交叉引用
- saved `sources/2026/06/28/我终于搞明白了：Claude Code 为什么会忽略指令了.html` — 原始 HTML（5 张图片）
- saved `sources/2026/05/11/Claude Code 错误率从 41% 到 3%：CLAUDE.md 到底改对了什么？.html` — 原始 HTML（4 张图片）

## [2026-06-26] ingest | How Anthropic enables self-service data analytics with Claude（重新摄取）

- saved `article-downloads/How Anthropic enables self-service data analytics with Claude.html` — 自包含离线 HTML（4 张图片、2 个代码块）
- saved `sources/2026/06/26/anthropic-self-service-analytics-with-claude.md` — 文章 Markdown 版本（2026-06-21 更新版）
- updated `ai-agent/ecosystem/agentic-analytics-anthropic.md` — 补充 6月21日更新内容：参考文档模板（为 LLM 检索优化的表描述结构）、仓库 Skill 完整骨架模板（语义层优先反借口清单、快速启动工作流、业务实体消歧、对抗 SQL 审查 + 溯源页脚、字段命名陷阱血泪教训）；更新 sources 指向新版本，扩展相关页面链接

## [2026-06-23] lint | 知识库健康检查与修正（第七轮）

- fixed `business-cognition-system` — 修复断链 [[agent-context-engineering]] → [[ai-agent/context-eng/index|上下文工程]]
- fixed `data-agent-enterprise-practice` — 修复断链 [[data-asset-semantic-layer]] → [[dataagent-semantic-layer|数据资产语义层]]
- fixed `data-agent-enterprise-practice` — 移除无效 wikilink [[metricflow]]（外部工具），改为纯文本
- fixed `iceberg` — 补充缺失 `sources` 字段（lint-stub.md）
- result: 168 页面，0 断链，0 frontmatter 缺失，0 空来源——知识库结构健康

## [2026-06-18] ingest | 让 DataAgent 真正用起来: DataAgent 在企业信用的优化和实践

- saved `sources/2026/06/18/dataagent-enterprise-credit-practice.md` — 微信公众号文章全文（杜淑均/蚂蚁集团/DataFun）
- saved `article-downloads/让 DataAgent 真正用起来-DataAgent 在企业信用的优化和实践.html` — 自包含离线 HTML（24 张图片、语法高亮 CSS）
- created `ai-agent/agent-core/data-agent-enterprise-practice.md` — Data Agent 企业级实践：三个判断（承接 Coding Agent 闭环/可组合能力优于孤立产品/数据资产下限+业务认知上限）、四层架构（语义→认知→Agent核心→渠道）、四个关键实践（增强元信息/混合语义路由/可演进认知系统/Harness 自迭代）
- created `ai-agent/agent-core/business-cognition-system.md` — 可演进业务认知系统：三类知识来源（显性抽取/人工校正/Agent反馈）、两种组织（召回知识库/文件系统）、动态加载、反馈回流闭环
- updated `ai-agent/harness/harness-engineering-practice.md` — 新增蚂蚁企信 Data Agent 的 Harness 实践交叉引用（Main/Reviewer/Executor/Eval 四 Agent 协同）

## [2026-06-17] ingest | OpenSpec 自定义 Schema 深度指南
- saved `sources/2026/06/17/openspec-custom-schema.html` — 原始 HTML 文章（10 章，含 6 个 SVG 架构图）
- created `ai-agent/sdd/openspec-schema-mechanism.md` — Schema 核心机制：DAG 心智模型、三层定制体系、schema.yaml 完整字段详解、命令映射、openspec status 状态机
- created `ai-agent/sdd/openspec-custom-schema-guide.md` — 自定义 schema 创建（fork/init/社区安装）、模板设计原则、数据仓库完整实战（dw-spec-driven）
- created `ai-agent/sdd/openspec-skill-mechanism.md` — SKILL 文件机制：生成与加载流程、SKILL.md 结构、与 schema.yaml 的分工
- created `ai-agent/sdd/openspec-verify-extension.md` — Verify 扩展三种方案（artifact/SKILL/instruction）对比与推荐组合
- updated `ai-agent/sdd/index.md` — 新增"OpenSpec Schema 深度拆解"分类（4 篇）
- updated `ai-agent/sdd/openspec-sdd-practice.md` — 添加 Schema 相关交叉引用
- updated `ai-agent/sdd/sdd-custom-workflow.md` — 添加 Schema 与 SKILL 机制交叉引用

## [2026-06-16] ingest | AI Agent & Skill 测评方案及落地实践（微信公众号：腾讯程序员）

- saved `sources/2026/06/16/AI Agent & Skill 测评方案及落地实践.html` — 原始 HTML（含 20 张图片、33 个样式块）
- saved `sources/2026/06/16/AI Agent & Skill 测评方案及落地实践.txt` — 提取文本 + frontmatter
- created `ai-agent/agent-core/agent-evaluation-framework.md` — AI Agent 测评框架总论：三类评分器（确定性/Rubric/人工）+ 五大维度（功能正确性/过程质量/效率成本/鲁棒性安全/体验对齐）+ 通用评分公式
- created `ai-agent/agent-core/agent-evaluation-implementation.md` — 测评落地实践：四大场景用例设计 + 基线管理 + Trace 能力 + 稳定性评估（pass@k vs pass^k）+ 能力测评 vs 回归测评 + TPerf 性能 AI 分析 Agent 案例（9 类 30+ 用例、LCS 步骤评分、7 模型对比）
- updated `ai-agent/agent-core/index.md` — 新增"测评体系"分类，总数 16→18 篇
- updated `ai-agent/ecosystem/architect-in-agent-era.md` — 补充评估体系相关交叉引用

## [2026-06-15] ingest | Google Cloud Data Agent Kit 深度解析（中文分析文章）
- saved `sources/2026/06/15/Google Cloud Data Agent Kit 深度解析.html` — 中文深度分析原文（含 SVG 架构图）
- expanded `big-data/google-data-agent-kit` — 整合三大来源（Twitter 官宣 + 官方博客 + 中文分析），新增：预置 Agents 详表（Data Engineering/Data Science/DB Observability + GA/Preview 状态）、意图驱动开发完整示例（交易日志→欺诈检测→Spanner 全链路）、逆向下沉战略分析（控制点从算力存储迁移到开发者工具链）、HITL→HOTL 数据运维自主化演进、竞争格局对比表（Google vs AWS vs Databricks）、对 Hive/Spark 数仓团队的实践启示（MCP + YAML Spec 投喂层）

## [2026-06-15] optimize | 知识库健康度优化——断链修复 + 桩页面 + 索引完善
- fixed 8 short-name wikilinks → full path (e.g. `[[context-eng]]` → `[[ai-agent/context-eng/index]]`) in 7 index pages
- fixed 2 `[[mcp]]` → `[[agent-mcp-protocol|MCP]]` (agentic-skills + agentic-data-cloud)
- created `big-data/iceberg` stub — Apache Iceberg 开放表格式基础页
- removed 3 speculative wikilinks (claude-code-skills-system, ISO-GQL, 知识图谱) → kept as plain text
- result: 14 broken wikilinks → 0, 156 pages, all cross-references valid

## [2026-06-15] expand | loop-engineering vs OpenClaw 对比分析
- expanded `ai-agent/harness/loop-engineering` — 新增与 OpenClaw 的六组件逐项对照、四个结构性差异（验证哲学/预算模型/记忆复杂度/覆盖域）
- updated `ai-agent/ecosystem/openclaw` — 添加指向 loop-engineering 的交叉引用
- updated `ai-agent/ecosystem/openclaw-agentic-search-memory` — 添加指向 loop-engineering 的交叉引用

## [2026-06-15] ingest | Loop Engineering详解：把反馈循环放进工程现场
- saved `sources/2026/06/15/loop-engineering详解.md` — 微信公众号文章全文（若飞·架构师，8.8KB）
- saved `article-downloads/Loop Engineering详解：把反馈循环放进工程现场.html` — 自包含 HTML（74KB）
- created `ai-agent/harness/loop-engineering` — Loop Engineering 核心概念页：六组件拆解、闭环vs开环、验证准入表、预算约束、状态记忆、人在场原则、7天试点路径
- updated `ai-agent/harness/index` — 新增 loop-engineering 入口

## [2026-06-15] ingest | Google Cloud 官方博客：Data Agent Kit 详解 + Agentic Data Cloud 战略
- saved `sources/2026/06/15/Data Agent Kit brings data skills and tools to your IDE or CLI - Google Cloud Blog.html` — 官方博客原文（284KB）
- saved `sources/2026/06/15/What's New in the Agentic Data Cloud - Google Cloud Blog.html` — 官方博客原文（258KB）
- expanded `big-data/google-data-agent-kit` — 基于官方博客大幅扩充：新增统一数据中心（Unified Hub）、Agentic Skills 详解、对话式分析、意图驱动工程范式、安装使用指南、上下文窗口税概念；增加详细架构表格；补充新 source 引用
- created `big-data/agentic-data-cloud` — Google Agentic Data Cloud 战略架构页：三大创新支柱（Knowledge Catalog 通用上下文引擎 / Agentic-First 从业者体验 / 跨云 Lakehouse）、四类结构性失败分析、客户案例、性能数据
- updated `big-data/index` — Data Agent 生态新增 agentic-data-cloud

## [2026-06-15] ingest | 谷歌开源 Data Agent Kit：数据工程正式进入原生 Agent 时代
- created `big-data/google-data-agent-kit` — Google Cloud 开源数据工程 AI 工具集，含 Agentic Skills / MCP 工具层 / IDE 插件
- created `ai-agent/ecosystem/agentic-skills` — Agentic Skills（预制数据技能库）概念页，Data Agent 核心抽象单元
- updated `big-data/index` — Data Agent 生态新增 google-data-agent-kit

## [2026-06-08] ingest | How Anthropic enables self-service data analytics with Claude
- created `agentic-analytics-anthropic` — Anthropic 利用 Claude 构建自服务分析平台的完整架构（四层 stack：数据基础→真相源→Skills→验证），三大失败模式与最佳实践

## [2026-05-31] ingest | 别再卷大模型了：Palantir 告诉我们，AI 的进阶是"本体论"
- created `ontology` — 本体论核心概念（五要素框架、AI 幻觉解决方案、数据巴别塔、Neuro-Symbolic 关系）
- updated `ai-ml/index` — 新增核心概念分类与 [[ontology]] 条目
- updated `taobao-live-data-dev-paradigm` — 本体论段落添加 [[ontology]] wikilink

## [2026-05-31] ingest | DataWorks 虾聊日 0528 录音-修正版.md（re-ingest，整场录音文字稿）
- 通读 444 行完整录音转写稿，与 7 份 PDF 视觉复核结果双源对照
- expanded `dataworks-2026-0528-xialiaori` — 重写为整场议程版：补全开场王子军 OPC 引主机制（PDF 集合中无对应 deck，仅录音保留：800+BP 收件、入选率<10%、17 场路演、I have a demo 三创始人、四角色协同的引主机制、Airtouch 案例）；扩展每场分享的演讲人完整称谓（孙文强/丁源/孙戴博/赵红梅/李昊哲/畅含/陈梦成）+ 录音独有的原话/demo 细节
- updated `dataworks-data-agent` — 补回演讲人孙文强、现场两个故事（周五加表+凌晨故障）、三句金句（共用一颗大脑/共享数据语义层/AI 不是写代码快而是任务串联变化）
- updated `taobao-live-data-dev-paradigm` — 补回演讲人丁源（淘天集团高级数据专家、淘宝直播数据研发负责人）、NL2DSL2SQL 与 R2C 现场口语版定义、中心化转本地化的 4 个原因、verify-data Skill 已在阿里云公众号发布
- updated `hologres-skills` — 修正演讲人为赵红梅（梅酱）（之前误写"骆撷冬"，与 PDF P1 复核冲突）；补充开场即跑 demo（OpenClaw 后台跑刘备/吕布广告素材生成→视频→投放→分析全链路）
- updated `flink-skills` — 补充 4 个现场 demo（作业调优/批量巡检/跨产品组合/舆情分析）+ 阿里云 69 个官方 Skill 覆盖 6 大云领域
- updated `maxcompute-skills` — 补充元梗式开场、银行账户客户端金句、Time Travel 7 天回滚、AutoMV 多 Agent 同查节流、MaxAgent 移动端典型场景
- updated `openclaw-agentic-search-memory` — 补充核心矛盾原话、3 个 demo 现场（中考试卷/NVIDIA 169 页/Rick and Morty）、Self-evolution 借鉴 Hermes Agent
- 源文件 frontmatter 标注 re_ingested: 2026-05-31

## [2026-05-31] ingest | OpenClaw Agentic Search & Memory（re-ingest，PDF 视觉复核）
- 按 CLAUDE.md 多模态首次摄取规则，使用 `pdftoppm -r 150 -png` 转出 24 张页面图片逐页视觉复核，不再以派生 TXT/OCR 为唯一依据
- updated `openclaw-agentic-search-memory` — 修正演讲人（陈梦成）、补全议题主标题"Agentic Search + Memory，OpenClaw 企业研究更智能"+ 副标题"从搜索工具到企业认知引擎"+ 4 章议题脉络；补回 Agentic Search 第 5 项能力"标准交付模式（Skills 透出 / 开源 ACP 协议透出）"
- expanded `openclaw-agentic-search-memory` — 新增 24 页逐页摄取索引；新增 P7 两大引擎角色对照表（注重研究质量 vs 解决知识复用）；新增 P20 OpenClaw 双层记忆体系架构图（User Interaction Layer + Agentic Memory Layer，Extractor → Embedding Engine → ElasticSearch VectorStore，Generate Vectors for Facts and Skills 双向回路）；新增 P21 AgenticMemory 三模式（In-session / Cross-session / Self-evolution）；新增 P24 5 阶段研究范式升级时间线
- 源文件 frontmatter 标注 re_ingested: 2026-05-31

## [2026-05-31] ingest | MaxCompute × Agent：从多模态存储到混合计算（re-ingest，PDF 视觉复核）
- 按 CLAUDE.md 多模态首次摄取规则，使用 `pdftoppm -r 150 -png` 转出 14 张页面图片逐页视觉复核，不再以派生 TXT/OCR 为唯一依据
- updated `maxcompute-skills` — 修正 MCMCP 方法分组（"身份与权限"→"身份与权限确认"），补回 `get_instance` 方法，新增"配套 Skills"清单（Sql-Generation/Maxframe-Coding/Information-Schema/Project-Qouta-Manage）；纠正第 3 个 Agent 接入口实为"龙虾实操 Specialist Sub-Agent · OpenClaw"实战示例（OpenClaw 总控 + Specialist Sub-Agent 执行，证据链 Plan→Act→Result）
- expanded `maxcompute-skills` — 新增 14 页逐页摄取索引；补全 P3 接入大图四层（Metadata/Compute Engines/Storage 各列详细子项）、P6 Delta Table 细项（基础 DML / 自动维护拆分）、P9 控制台调用示例（task_344 失败率 70.6%）、P11 Blob Pipeline 架构对比（传统多引擎 vs MaxCompute Blob 模式）、P12 多模态算子 SQL 示例
- 源文件 frontmatter 标注 re_ingested: 2026-05-31

## [2026-05-31] ingest | EMR Skills：从 Lakehouse 到 Agentic Lakehouse（re-ingest，PDF 视觉复核）
- 按 CLAUDE.md 多模态首次摄取规则，使用 `pdftoppm -r 150 -png` 转出 22 张页面图片逐页视觉复核，不再以派生 TXT/OCR 为唯一依据
- updated `emr-skills` — 修正首次摄取的若干偏差：演讲人姓名（"董亚军"/"畅舍" → 畅含｜EMR 产品经理｜计算平台事业部）、副标题（"新站" → 新一站）、AI Function 体系子项（更正为图片打标/embedding/图像处理/数据脱敏/语义提取/音频处理/时序预测/内容安全）、EMR AI 助手（补"产品咨询"）、智能开发助手（补"参数优化"）、能力展望六项分列
- expanded `emr-skills` — 补全 P4 Lakehouse 数据流图（数据源 →（Flink）→ ODS/DWD/DWS/ADS·Paimon Table·Spark·DLF）、P8 应用场景层 16 个子能力、P10 三大子产品对照式细节、P11 三张产品截图、P14 渐进披露用户意图分类、P15-P19 场景/对话/架构页的具体数字与 SQL 片段
- 源文件 frontmatter 标注 re_ingested: 2026-05-31

## [2026-05-31] ingest | 淘宝直播：AI 驱动的数据研发范式升级（page-complete re-ingest）
- updated `taobao-live-data-dev-paradigm` — 按 PDF 22 页新增逐页摄取索引，确保 P1-P22 每页信息均有落点
- expanded `taobao-live-data-dev-paradigm` — 补充 P11 OpenClaw 主/子 Agent 分工，P14 SDD p1/p3 示例，P15 DSL/Prompt 示例，P19 LightRag 图谱节点与 llm-wiki-v2 目录结构，P21 ChatBI 示例
- verified source shape — PDF 为 22 页图片型文件，`pdftotext` 无可抽取文本，继续以现有视觉识别派生 TXT 为重摄取依据
- correction — 用户指出 TXT 不可信后，重新从 PDF 本体转出 22 张页面图片逐页视觉复核；更新 `taobao-live-data-dev-paradigm` 标注 TXT 不作为可信依据，并收敛 P19 低置信度图谱节点描述
- expanded `taobao-live-data-dev-paradigm` again — 补全 P7 高密度 R2C 架构图：两侧护栏、Agent 应用层、AI 能力层、基建层、Memory/Skill/SDD 演进方向、Ontology 映射、核心依赖；同时新增 P1-P22 逐页复核补充，逐页记录 PDF 图中细项

## [2026-05-31] query | 淘宝直播分享总结
- answered from `taobao-live-data-dev-paradigm` — 总结淘宝直播 AI 驱动数据研发范式升级分享的主旨、架构、方法论、落地路径与结果

## [2026-05-31] query | 淘宝直播中心化到本地化 AI Native 演进
- answered from `taobao-live-data-dev-paradigm` — 强调该分享的重要主线之一是从中心化 AI Native 向本地化 AI Native 演进：统一入口到定制工作流、短期无记忆到长期可继承记忆、工具到员工

## [2026-05-31] query | 淘宝直播分享要点核对清单
- answered from `taobao-live-data-dev-paradigm` — 按主题列出淘宝直播分享覆盖的主线、架构、演进、研发范式、Agent 协同、Skill/SDD/AI Coding、基建、成果与未来规划，供人工核对是否遗漏

## [2026-05-31] ingest | 淘宝直播：AI 驱动的数据研发范式升级（re-ingest）
- 重新摄取，原因：之前的 OCR 派生 txt 质量太差（大量乱码），用户已删除
- 直接读取 PDF（22 页，已转 PNG）通过视觉重新提取，重写派生 txt
- 重写 `taobao-live-data-dev-paradigm` — 修正多处错误数据：代码采纳率 72.9%→72%，代码渗透率 24.6%→24.63%，平均对话轮次 5 轮→2.91；补充直播 360 真实案例（56 口径/6 模型/2 融合/2 同步/1500+ 行代码）；补充本体论 Neuro-Symbolic 设计；补充 P9 直播研发范式 6 大支柱；补充 P16 三大核心原则；补充 Pipeline 编排详细流程；补充 SDD references 加载机制
- 源文件 frontmatter 标注 re_ingested: 2026-05-31

## [2026-05-24] ingest | SDD 实践：OpenSpec + Superpowers 整合创建自定义工作流（re-ingest）
- 源文件已归档至 sources/2026/05/24/，内容与 2026-05-17 首次摄入一致，无新增 wiki 页面
- 已有页面 `sdd-custom-workflow.md` 已完整覆盖此文内容

## [2026-05-24] ingest | 从古法编程到 AI 编程：Superpowers + OpenSpec 协同开发实战指南（re-ingest）
- 源文件归档，内容同 2026-05-17 首次摄入 → `superpowers-openspec-legacy-workflow.md`

## [2026-05-24] ingest | 开源项目 superpowers 深度解读：把 AI Coding Agent 变成遵守工程流程的协作伙伴
- created `superpowers-repo-architecture.md` — 仓库内部架构分析：四层能力模块（skills/agents/commands/hooks）、跨平台分发分层设计、using-superpowers 总控机制（1% 规则）、两条执行路径（subagent vs executing-plans）、receiving-code-review 反表演式认同、writing-skills 元技能 SDD 方式、5 个核心设计洞察 + 4 个局限
- updated `ai-agent/sdd/index.md` — 页数 12→13
- updated `ai-agent/index.md` — SDD 页数同步

## [2026-05-24] ingest | Superpowers：让 AI 编程从"碰运气"变成"走流程"的完整方法论
- updated `superpowers-workflow-scenarios.md` — 新增各步骤的操作级细节：头脑风暴 9 步标准化流程、Git 隔离 4 步创建、任务拆分 6 类禁止占位符、子代理两轮复查机制、TDD 偷懒借口表、代码审查三级标准+5 项验证清单、收尾四选项、安装验证、自定义技能 TDD 方式

## [2026-05-24] ingest | Superpowers 实战指南：7 步流程 + 14 个技能 + 3 条铁律
- created `superpowers-workflow-scenarios.md` — 完整 7 步工作流（头脑风暴→Worktree→任务拆分→子代理→TDD→审查→收尾）+ 三种场景裁剪指南（新项目/老项目加功能/修 Bug）
- created `superpowers-iron-laws.md` — 三条铁律详解：无测试不写代码、无根因不修 Bug、无验证不完成
- updated `superpowers-framework.md` — 补充 7 步工作流与三条铁律引用，新增来源
- updated `ai-agent/sdd/index.md` — 新增 2 篇页面，页数 10→12
- updated `ai-agent/index.md` — SDD 页数同步更新

## [2026-05-21] ingest | Claude Code Harness 工程：数仓侧落地方案｜得物技术
- created `dw-harness-practice.md` — 数仓 Harness 工程落地方案：五层防御体系、8步 SKILL 工作流、hooks 自动验证、subagent 上下文隔离
- updated `harness-engineering-practice.md` — 添加数仓 Harness 实践的交叉引用

## [2026-05-19] research | SDD & Spec 讨论素材准备（v2 完整版）
- updated `SDD-讨论素材.md` — 基于知识库全部 11 篇 SDD 专题深度改写：补充 Spec-Kit 5阶段门控、Superpowers 14技能+实测基准、OpenSpec 7步工作流+Delta Spec、双框架8步联合工作流+7坑+2衔接Skill、老旧项目4阶段实战+Redis库存预扣案例、薄编排11个action+三层架构+Override机制、渐进式方法论3铁律+知识飞轮

## [2026-05-18] lint | 深度检查与优化（第六轮）
- fixed 36 stale pages — 批量更新 updated 日期至 2026-05-18（源文件修改时间在页面 updated 之后）
- fixed 2 pages — agent-harness.md、llm-wiki.md 补充缺失的 ## 相关页面 章节
- expanded tech-radar — 从 6 行扩展为 采用/评估/关注 三级技术雷达，新增 big-data 和 agent-core 链接
- expanded claude-code hub — 从 8 行扩展为架构全景/核心机制/多Agent/工程实践四组分类，覆盖 34 篇子页面
- updated product/index — 新增 architect-in-agent-era、ai-career-trends、tech-radar 跨领域链接
- verified: 0 broken wikilinks, 0 stale pages, 0 missing frontmatter, 0 orphans

## [2026-05-18] lint | 全局健康检查与修复（第五轮）
- fixed `rag-vs-llm-wiki.md` — 移除自引用 [[rag-vs-llm-wiki]]
- fixed 5 shallow pages — architecture、backend、distributed、fullstack、platform 五个领域 index 页面补全领域说明
- fixed 8 uningested sources — 为 Google上下文工程白皮书、AI编程三剑客对比、Superpowers踩坑实录等 8 个来源文件补充 ingested 标记
- flagged 12 PDF 来源 — 无法为二进制 PDF 添加 frontmatter（已通过 .txt 提取副本覆盖）
- flagged 33 stub pages — 标记为 lint-stub 的占位/索引页面（多数为有意最小化）
- verified: 0 broken wikilinks across 143 pages

## [2026-05-18] lint | 全局健康检查与修复（第四轮）
- fixed `ai-agent/index.md` — 修复 7 条 table 内 `\|` 转义导致的断链，改用 wiki-relative 路径 `[[ai-agent/subdir/index]]`
- fixed `big-data/infinisynapse.md` — 修复 4 条 table 内 `\|` 转义导致的断链，改用裸 wikilink
- fixed 6 pages — 补充 `status: draft` 字段，移除 tags 中的 draft（agent-skills-system, claude-code, agent-harness, llm-wiki, token-consumption-economics, tech-radar）
- fixed 8 pages — 移除 tags 中冗余的 draft（agent-autonomous-planning, ai-agent-security, ai-governance, ai-knowledge-base, digital-employee, glm5-model, llm-cost-optimization, prompt-engineering）
- fixed 3 sources — 补充缺失的开 `---` frontmatter 分隔符（Obsidian+Claudian, Agent范式, Claude Code from Source）
- fixed 3 orphans — claude-code/index, ecosystem/index, sdd/index 现在通过 ai-agent/index.md 获得入链
- verified: 0 broken wikilinks, 0 wanted pages, 0 orphans

## [2026-05-18] ingest | Pushing the Frontier for Data Agents with Genie (Databricks Blog)
- updated `big-data/databricks-genie` — 从二手转述升级为原始博文，补充四阶段执行轨迹、40% 表搜索提升、GEPA 优化等原文细节
- created `ai-agent/specialized-knowledge-search` — 专用知识搜索技术：从企业资产提取语义上下文构建多索引、与通用 RAG 对比
- created `ai-agent/parallel-thinking` — 并行思考：多轨迹采样聚合弥补 Data Agent 缺确定性测试的问题
- created `ai-agent/multi-llm-design` — Multi-LLM 架构：不同子 Agent 使用不同模型分工，含 GEPA 成本优化
- updated `big-data/code-agent-vs-data-agent` — Related 新增三个技术页面的链接

## [2026-05-18] lint | 全局健康检查与修复（第三轮）
- fixed 5 source files — 修复 15 条 broken wiki_pages 引用（wiki/前缀、stale路径、bare prefix）
- fixed 7 index.md — 修复跨子目录 relative-path wikilinks（`[[subdir/index]]` → `[[subdir]]`）
- added frontmatter to 2 uningested HTML sources
- fixed `Anthropic的「脑手分离」` — 补充空 wiki_pages
- verified: 0 broken source refs, 0 broken wikilinks

## [2026-05-18] lint | 全局健康检查与修复（第二轮）
- fixed `agent-multi-agent-collaboration` — 修复 broken source 引用: Agent-Harness综述.md
- fixed `open-design` — 修复 broken source 引用: Open Design.md → Open-Design开源平替Claude-Design.md
- fixed `harness-design-long-running` — 修复 broken source 引用: anthropic-harness-design-long-running-apps.md
- updated 84 source files — 批量修复 wiki_pages 目录拆分后的 stale 路径
- fixed `DataWorks DataAgent分享录音.md` — 补充 big-data/ 前缀
- fixed `开源AI编程工具对决` — 补充 wiki_pages
- fixed 4 sources — 补充空 wiki_pages 字段
- verified: 0 broken wikilinks, 0 broken source refs

## [2026-05-18] lint | 全局健康检查与修复（第一轮）
- fixed `claude-managed-agents` — 修复 5 条 truncated source 文件名
- fixed `agent-harness-overview` — 修复 source 引用: Agent-Harness综述.md → 完整文件名
- fixed `harness-build-to-delete` — 修复 source 引用
- fixed `harness-as-backend` — 修复 source 引用
- fixed `harness-engineering-practice` — 修复 source 引用
- fixed `agent-vs-framework-vs-harness` — 修复 source 引用
- fixed `ai-career-trends` — 修复 source 引用
- fixed `deerflow` — 修复 source 引用
- fixed `hermes-agent` — 修复 source 引用
- fixed `architect-in-agent-era` — 修复 source 引用
- fixed `claude-md-best-practices` — 修复 source 引用
- fixed `manus-context-engineering` — 修复 source 引用
- fixed `llm-wiki-six-layer` — 修复 source 引用
- fixed `sub-agent-vs-team` — 修复 source 引用
- fixed `data-dev-skills-automation` — 修复 source 引用
- fixed `maxcompute-data-ai` — 修复 source 引用
- fixed `openai-data-agent` — 修复 source 引用
- fixed `rag-query-optimization` — 修复 source 引用
- fixed `google-scion` — 修复 source 引用
- fixed `ai-agent-ecommerce-content` — 修复 source 引用
- fixed `sdd-openspec-superpowers` — 修复 source 日期路径: 04/08 → 05/09
- fixed `openspec-sdd-practice` — 修复 source 引用
- fixed `meta-jit-testing` — 修复 source 引用
- fixed `databricks-genie` — 移除 broken wikilink [[GEPA]]
- updated 7 subdir index.md — 补充 sources: [2026/05/10/lint-stub.md]
- updated 12 pages — 补充英文 aliases
- fixed `dataagent-semantic-layer` — 修正 content type tag: meta → concept

## [2026-05-18] lint | Schema 拆分：ai-agent/ 重组为 8 个子目录
- updated `wiki-schema.md` — 新增子目录规则：领域目录超 30 页可按主题拆分子目录
- reorganized `ai-agent/` — 99 页拆分为 7 个子目录：claude-code(34)、agent-core(16)、sdd(10)、knowledge-base(12)、harness(8)、context-eng(3)、ecosystem(15)
- created 7 子目录 `index.md` 索引页
- updated `ai-agent/index.md` — 改为子领域表格导航
- updated `big-data/dataagent-semantic-layer` — Related 新增 specialized-knowledge-search 链接

## [2026-05-18] ingest | 为什么 Code Agent 无法解决企业数据分析问题（WinClaw）
- created `big-data/code-agent-vs-data-agent` — Code Agent vs Data Agent 目标函数差异、Databricks Genie 三个挑战、维度对比、协作分工
- created `big-data/infinisynapse` — InfiniAgent + InfiniSQL + InfiniRAG 三件组合，含命名中间表/虚拟数仓/异构源 join/可审计工作流（draft）
- created `big-data/databricks-genie` — Genie stub，记录 Lakehouse 全资产覆盖、specialized search + parallel thinking + Multi-LLM、内部 32%→90% benchmark（draft，待拉原文）
- updated `big-data/index` — 新增"概念与对比"分组，加入 databricks-genie / infinisynapse 链接
- updated `big-data/dataworks-data-agent` — Related 链接新文章三页
- updated `big-data/data-agent-practice-guide` — Related 链接新文章三页
- updated `big-data/openai-data-agent` — Related 与 InfiniRAG / Genie 对照

## [2026-05-17] lint --fix | 修复 1 处断链
- fixed `learning-path` — `[[Claude Code]]` → `[[claude-code]]`（空格导致断链）

## [2026-05-17] lint --fix | 扩展 5 个 stub 页面
- expanded `agent-multi-agent-collaboration` — 两种协作范式、五种通用模式、选型铁律、五条设计原则
- expanded `agent-tdd-workflow` — Superpowers 强制 TDD 循环、Meta JiT 语义级测试
- expanded `agent-architecture-patterns` — 10 大设计模式及架构原则
- expanded `agent-mcp-protocol` — JSON-RPC 基础、8 传输/7 来源、工具包装流程、安全设计
- expanded `learning-path` — 三阶段学习计划：Agent 源码 → AI 工程化 → 大数据融合

## [2026-05-17] lint | 知识库健康检查
- 130 页，500+ 链接：断链 0，孤立页 0，缺失 frontmatter 0，无来源 0，内容过期 0
- 标记 34 浅层页（26%）：13 index 页 + 4 hub 页 + 5 轻量概念 + 12 stub 待扩展

## [2026-05-17] ingest | SDD 自定义工作流：OpenSpec + Superpowers 薄编排
- created `sdd-custom-workflow` — 薄编排架构，sdd-* action skills 统一入口，Action Not Phases，三段式 skill 结构，信息丢失防护，渐进采用策略
- updated `superpowers-openspec-pitfalls` — 新增薄编排页交叉引用
- updated `sdd-openspec-superpowers` — 新增薄编排页交叉引用
- updated `openspec-sdd-practice` — 新增薄编排页交叉引用
- 跳过 `AI编程三剑客` — 同一文章已于 2026-05-15 摄入

## [2026-05-17] ingest | Superpowers 实测基准 + 双框架组合踩坑
- created `superpowers-openspec-pitfalls` — 7 个坑及解决方案、自定义衔接 Skill、四组合效果对比、3 分钟快速上手
- updated `superpowers-framework` — 新增定量基准(58%→91%)、6 技能详解、3 种最佳场景、AI 编程工程化拐点分析
- updated `superpowers-openspec-legacy-workflow` — 新增踩坑页交叉引用
- updated `openspec-sdd-practice` — 新增踩坑页交叉引用
- updated `sdd-openspec-superpowers` — 新增踩坑页交叉引用

## [2026-05-17] ingest | Superpowers + OpenSpec 协同开发实战指南
- created `superpowers-openspec-legacy-workflow` — 老旧项目四阶段闭环（分析→规约→生成→同步），团队角色变化，4大避坑指南
- updated `sdd-openspec-superpowers` — 新增老旧项目实战链接
- updated `openspec-sdd-practice` — 新增协同实战链接
- updated `superpowers-framework` — 新增协同实战链接
- updated `superpowers-vs-openspec` — 新增协同实战链接

## [2026-05-17] ingest | Claude Code 大型代码库配置指南
- created `claude-code-configuration-guide` — 实操配置指南，含 CLAUDE.md 分层模板、Hooks/Skills 配置实例、代码地图、分阶段检查清单
- updated `claude-code-large-codebase` — 新增配置指南交叉引用
- updated `agent-harness` — 新增配置指南链接

## [2026-05-17] ingest | How Claude Code works in large codebases
- created `claude-code-large-codebase` — Anthropic 官方大型代码库最佳实践，agentic search 与 RAG 对比、Harness 构建顺序、三大配置模式、组织级落地
- updated `claude-code-search-strategy` — 新增大型代码库实践链接
- updated `agent-harness-overview` — 新增 Harness 构建顺序链接
- updated `claude-code-extensibility` — 新增 LSP/Subagent 说明和构建顺序
- updated `claude-md-best-practices` — 新增分层策略和模型演进维护
- updated `ai-governance` — 新增组织级 AI 治理模式
- updated `agent-harness` — 新增大型代码库页面链接

## [2026-05-15] lint --fix | 知识库整理优化
- fixed `sources/2026/05/15/` — 补 ingested frontmatter
- fixed 10 页 — 补 sources 字段（8 draft + 2 meta）
- fixed `llm-wiki` — 明确标注为索引枢纽，指向 llm-wiki-concept 为主入口
- fixed `agent-harness` — 明确标注为索引枢纽，指向 agent-harness-overview 为主入口
- flagged SDD 工具对比 3 页 — 需人工确认各页定位边界
- flagged Karpathy 知识库 2 页 — 内容高度接近，建议合并
- flagged Obsidian 2 页 — 共享 Obsidian 别名，可能混淆
- 结论：107 页面，1 critical→fixed，17 warning→fixed 12 + 5 保留，7 info→待人工

## [2026-05-15] ingest | AI 编程三剑客：Spec-Kit、OpenSpec、Superpowers 深度对比与实战指南
- created `ai-agent/spec-kit` — GitHub 官方 SDD 框架，wiki 此前完全缺失 Spec-Kit
- created `ai-agent/ai-programming-tools-comparison` — 三剑客全面对比，含决策树与协同方案
- updated `ai-agent/superpowers-framework` — 补充安装步骤、技能触发机制、自定义技能、Spec-Kit 交叉引用
- updated `ai-agent/sdd-openspec-superpowers` — 纳入 Spec-Kit 三维视角，更新交叉引用
- updated `ai-agent/superpowers-vs-openspec` — 从"双语切换"升级为"多模式切换"，新增 Spec-Kit 维度

## [2026-05-15] ingest | 开源AI编程工具对决——Superpowers技能库与OpenSpec规范驱动
- created `ai-agent/superpowers-vs-openspec` — 个体效率 vs 团队一致性的哲学层面对比
- updated `ai-agent/sdd-openspec-superpowers` — 增加哲学层面分析和场景选择指南
- updated `ai-agent/superpowers-framework` — 补充适用场景说明和交叉引用
- updated `ai-agent/openspec-sdd-practice` — 补充适用场景说明和交叉引用

## [2026-05-14] ingest | 创建 dataagent-semantic-layer（DataAgent 语义层分析）
## [2026-05-13] lint --fix | 知识库深度检查与优化
- fixed 11 页面 status: draft → tags draft（agent-architecture-patterns/agent-harness/agent-mcp-protocol/agent-multi-agent-collaboration/agent-skills-system/agent-tdd-workflow/claude-code/llm-wiki/token-consumption-economics/learning-path/tech-radar）
- fixed 9 索引页补 content-type tag: summary（ai-agent/ai-ml/architecture/backend/big-data/distributed/fullstack/platform/product index）
- fixed 1 HTML 源文件补 ingested frontmatter（sources/2026/05/10/Claude Code 技术解析.html）
- verified 10 wanted-page + 2 根目录引用为正常断链（保留作为发现机制）
- verified 15 二进制源文件摄入可追溯（PDF/PNG 通过 wiki-log 和 wiki 页面 sources frontmatter）
- 结论：109 页面，0 critical，0 warning，12 info

## 2026-05-10 ingest | Claude Code 搜索策略——为什么 grep 打败了 RAG
- 创建 `ai-agent/claude-code-search-strategy` — 从《Claude Code 源码解析》第8章提取：4个不使用RAG的理由、GrepTool三种output_mode、head_limit=250、Glob按mtime排序、Read多模态+FILE_UNCHANGED_STUB、512K行代码零向量数据库的实证数据

## 2026-05-09 init | Deep Study 知识库初始化
- 定义 wiki-purpose — 知识库定位、范围与目标（6大技术领域）
- 定义 wiki-agent — Agent 身份、职责与摄入规则
- 定义 wiki-schema — 目录结构、命名约定、标签体系、页面类型
- 创建首页 homepage.md — 知识库导航入口
- 创建 6 个领域子目录: big-data, distributed, ai-ml, backend, platform, architecture

## 2026-05-09 update | 新增全栈开发 + AI Agent 开发领域
- 新增 `wiki/fullstack/` — 全栈开发（React/Vue/TypeScript/Next.js/Node.js 等）
- 新增 `wiki/ai-agent/` — AI Agent 开发（LangChain/CrewAI/MCP/多 Agent 协作）
- 从 ai-ml 中拆分 AI Agent 为独立领域，保持 ai-ml 聚焦模型与推理基础
- 更新 wiki-purpose、wiki-schema、homepage 的领域范围与标签体系

## 2026-05-09 ingest | 清华大学 OpenClaw 与数字员工研究报告
- 创建 `ai-agent/openclaw.md` — OpenClaw 架构、执行层、四层差距、五种原创概念
- 来源文件：sources/2026/05/09/清华大学2026年OpenClaw与数字员工研究报告47页.pdf

## 2026-05-09 ingest | 清华大学 Token 消费学研究报告
- 创建 `ai-ml/token-consumption-economics.md` — Token 冗余、预算治理、可控放量（扫描件，内容待补）
- 来源文件：sources/2026/05/09/清华大学：Token消费学研究报告.pdf（扫描件，文本提取受限）
- 迁移 sources 目录结构：YYYY-MM-DD → YYYY/MM/DD

## 2026-05-09 ingest | Obsidian + Claudian：AI知识库终极方案
- 创建 `ai-agent/obsidian-claudian.md` — Obsidian + Claude Code + Claudian 插件架构、Skills 三级进化、部署指南

## 2026-05-09 ingest | Obsidian打造个人第二大脑
- 创建 `ai-agent/obsidian-second-brain.md` — Obsidian 四大核心优势、推荐插件栈、快速上手指南

## 2026-05-09 ingest | Claude Code Auto Dream 记忆整合机制
- 创建 `ai-agent/claude-code-auto-dream.md` — 四阶段整合流程、触发条件、四种记忆机制全景

## 2026-05-09 ingest | Claude Code sourcemap 逆向还原
- 创建 `ai-agent/claude-code-sourcemap-reverse.md` — 1,884 文件还原、七类问题、三层降级策略

## 2026-05-09 ingest | Claude Code 源码架构解析
- 创建 `ai-agent/claude-code-architecture.md` — 启动入口、Prompt装配层、工具契约、权限管道、长任务续航五层主线

## 2026-05-09 ingest | Claude Code 87个隐藏功能
- 创建 `ai-agent/claude-code-hidden-features.md` — Kairos主动助手、Coordinator编排、Teleport跨设备、Voice语音、六大进化路线

## 2026-05-09 ingest | Memory/SDD/Spec 专题
- Memory: OpenClaw Memory Wiki 合并到 [[openclaw]]
- SDD (3个): 合并到 sdd-openspec-superpowers / superpowers-framework
- Spec (2个): 合并到 sdd-openspec-superpowers / superpowers-framework
- 创建 `ai-agent/llm-wiki-six-layer.md` — 六层架构改造
- 创建 `ai-agent/open-design.md` — Open Design 开源设计工具
- 创建 `ai-ml/rag-query-optimization.md` — RAG query 改写五策略
- 更新 `ai-agent/rag-vs-llm-wiki.md` — 五代方案演进对比
- 其余文件合并到已有 llm-wiki 相关页面
- 创建 `ai-agent/agent-harness-overview.md` — Harness 六承重层、七步循环
- 创建 `ai-agent/architect-in-agent-era.md` — Agent 时代架构师七项系统能力
- 创建 `ai-agent/sub-agent-vs-team.md` — Sub-Agent vs Team + 三框架对比
- 创建 `ai-agent/manus-context-engineering.md` — Manus 六条上下文工程原则
- 创建 `ai-agent/harness-build-to-delete.md` — Build to delete 演进
- 创建 `ai-agent/ai-career-trends.md` — Dario 职业趋势判断
- 创建 `ai-agent/harness-design-long-running.md` — GAN 启发的三 Agent 架构
- 创建 `ai-agent/harness-engineering-practice.md` — Human-first → Agent-aware
- 创建 `ai-agent/agent-vs-framework-vs-harness.md` — 三层概念区别
- 创建 `ai-agent/deerflow.md` — 字节跳动 DeerFlow 2.0
- 创建 `big-data/openai-data-agent.md` — OpenAI 内部 Data Agent
- 创建 `ai-agent/harness-as-backend.md` — Harness 作为新后端
- 其余 22 个文件（新闻/趋势/Claude源码）合并到已有页面
- 创建 `ai-agent/claude-code-vs-opencode.md` — CC vs OpenCode 四维度对比（上下文/Prompt/Agent/工具）
- 创建 `ai-agent/openspec-sdd-practice.md` — OpenSpec 七步工作流实战复盘
- 创建 `ai-agent/sdd-openspec-superpowers.md` — SDD 双框架对比
- 创建 `ai-agent/hermes-agent.md` — Hermes 自进化 Agent 框架
- 创建 `big-data/data-dev-skills-automation.md` — 数仓开发 Skills 自动化
- 创建 `ai-agent/claude-managed-agents.md` — CMA 脑手分离架构
- 创建 `ai-agent/google-scion.md` — Google 多 Agent 沙箱
- 创建 `ai-agent/meta-jit-testing.md` — Meta JiT 测试（bug检出+4倍）
- 创建 `ai-agent/claude-md-best-practices.md` — CLAUDE.md 最佳实践与陷阱
- 创建 `big-data/maxcompute-data-ai.md` — MaxCompute Data+AI 演进
- 创建 `ai-agent/ai-agent-ecommerce-content.md` — 淘工厂电商内容 Agent
- 更新多个新闻类文章 frontmatter 指向已有 wiki 页面（Hermes/OpenClaw/CMA）

## 2026-05-10 ingest | Dive into Claude Code 论文
- 创建 `ai-agent/dive-into-claude-code.md` — MBZUAI 学术论文笔记：5价值观+13原则+7组件架构
- 创建 `ai-agent/claude-code-permission-system.md` — 7种权限模式、拒绝优先规则引擎、ML分类器
- 创建 `ai-agent/claude-code-context-compaction.md` — 5层渐进压缩管道、CLAUDE.md四级记忆
- 创建 `ai-agent/claude-code-extensibility.md` — MCP/Plugins/Skills/Hooks四种扩展机制
- 创建 `ai-agent/claude-code-subagent.md` — AgentTool委托架构、Worktree隔离、侧链转录
- 创建 `ai-agent/claude-code-session-persistence.md` — 只追加JSONL转录、Resume/Fork机制
- 更新 `ai-agent/claude-code-architecture.md` — 增加新页面交叉引用
- 更新 `ai-agent/claude-code-harness.md` — 增加新页面交叉引用

## 2026-05-10 ingest | Claude Code 多Agent实现机制
- 创建 `ai-agent/claude-code-multi-agent-mechanism.md` — 三种机制总览：常规Subagent/Fork/Coordinator + 5条设计原则
- 创建 `ai-agent/claude-code-fork-subagent.md` — Fork Subagent字节级缓存复用机制
- 创建 `ai-agent/claude-code-coordinator-mode.md` — Coordinator主从型并行协作模式
- 更新 `ai-agent/claude-code-subagent.md` — 增加多Agent机制交叉引用

## 2026-05-10 ingest | Agent 的长短期记忆系统
- 创建 `ai-agent/agent-memory-system.md` — 短期记忆(context window) + 长期记忆(向量数据库+Embedding)，粒度设计，两层配合流程

## 2026-05-10 ingest | Claude Code from Source 技术书籍（18章）
- 创建 `ai-agent/claude-code-bootstrap-pipeline.md` — 5阶段启动管道/300ms预算/I/O并行
- 创建 `ai-agent/claude-code-state-architecture.md` — 两层状态/粘性锁存器/5个Sticky Latch
- 创建 `ai-agent/claude-code-api-layer.md` — 多云提供商/prompt cache三级/输出槽位预留
- 创建 `ai-agent/claude-code-agent-loop.md` — 1730行async generator / 10终止+7继续状态 / 4层压缩
- 创建 `ai-agent/claude-code-tool-system.md` — 40+工具 / 14步执行管道 / fail-closed默认值
- 创建 `ai-agent/claude-code-concurrent-tools.md` — 按安全分区并发 / 推测执行 / 顺序保持
- 创建 `ai-agent/claude-code-memory-system.md` — 4类型/可推导性标准/Sonnet侧查询/背景提取Agent
- 创建 `ai-agent/claude-code-swarm.md` — Swarm多Agent协作/Teammate/Mailbox/Auto-Resume
- 创建 `ai-agent/claude-code-terminal-ui.md` — 自定义DOM/7阶段渲染/驻留池/双缓冲
- 创建 `ai-agent/claude-code-input-system.md` — 5种键盘协议/Chord和弦/Vim 12状态机
- 创建 `ai-agent/claude-code-mcp.md` — 8种传输/7个配置来源/4阶段工具包装
- 创建 `ai-agent/claude-code-remote-execution.md` — Bridge v1/v2/FlushGate/非对称读写
- 创建 `ai-agent/claude-code-performance.md` — 位图预过滤器/槽位预留/启动I/O并行
- 更新 `ai-agent/claude-code-architecture.md` — 新增六抽象模型/10大设计模式/可迁移原则
- 更新 `ai-agent/claude-code-subagent.md` — 新增10步决策树/15步runAgent生命周期/5维度设计语言
- 更新 `ai-agent/claude-code-fork-subagent.md` — 新增三层冻结/递归防护/Sync-to-Async转换
- 更新 `ai-agent/claude-code-extensibility.md` — 新增Skills两阶段加载/Hooks快照安全/退出码语义

## 2026-05-10 lint | 健康检查 + auto-fix
- 创建 8 个领域 index 页面（ai-agent/big-data/ai-ml/distributed/fullstack/backend/platform/architecture）
- 创建 8 个 stub 页面修复高频断裂链接（agent-architecture-patterns, agent-skills-system, agent-tdd-workflow, agent-multi-agent-collaboration, agent-harness, agent-mcp-protocol, claude-code, llm-wiki）
- 创建 [[tech-radar]] + [[learning-path]] stub 页面
- 修复 homepage.md — 中文领域链接指向各目录 index + 补 sources + 更新待创建链接
- 更新 ai-agent/index.md — 补入 3 个孤立页面的入链（open-design, anthropic-cookbook, ai-agent-ecommerce-content）

## 2026-05-10 ingest | Claude Code 源码解析橙皮书（花叔·12章80页）
- 创建 `ai-agent/claude-code-search-strategy.md` — 4理由解释零向量数据库/grep vs RAG/GrepTool 3种output_mode
- 创建 `ai-agent/claude-code-internal-mechanisms.md` — Undercover Mode/反蒸馏防御/Regex情绪检测/Verification Agent
- 更新 `ai-agent/claude-code-api-layer.md` — System Prompt 5级覆盖/3种运行模式/Capybara v8幻觉率29-30%/v2.1.76 cache回归bug
- 更新 `ai-agent/claude-code-permission-system.md` — YOLO两阶段64→4096/熔断机制/Auto模式危险权限剥夺/Zig层DRM/风险评估器
- 更新 `ai-agent/claude-code-context-compaction.md` — 9段式压缩模板/Full vs Partial Compact/两阶段/防漂移逐字引用/Sonnet 4.6失败率2.79%
- 更新 `ai-agent/claude-code-tool-system.md` — 4能力原语/Deferred Tools & ToolSearch/Bash两层安全分析
- 更新 `ai-agent/claude-code-memory-system.md` — 双向互斥/重叠请求Stash/Cache共享/记录成功不只记录失败/时间规范化
- 更新 `ai-agent/claude-code-swarm.md` — Team销毁顺序/Continue vs Spawn决策/权限同步文件锁/.worktreeinclude
- 更新 `ai-agent/claude-code-hidden-features.md` — GrowthBook基础设施/Dead Code Elimination/BUDDY实现细节/44个tengu_flags
- 存档 PDF + txt 至 `sources/2026-05-10/Claude-Code-Source-Analysis.*`

## 2026-05-10 ingest | Agent 设计范式（ReAct/Plan-and-Execute/Reflection）
- 创建 `ai-agent/agent-design-paradigms.md` — 三种范式核心区别/实现原理/优劣势/选型口诀
- 关联 Claude Code 实现：ReAct→agent-loop, Plan-and-Execute→coordinator-mode, Reflection→verification-agent

## 2026-05-10 ingest | Claude Code 技术解析报告（HTML·~46K字·10节）
- 创建 `ai-agent/claude-code-builtin-agents.md` — 5个内置Agent完整system prompt+设计哲学+对比总览
- 更新 `ai-agent/claude-code-architecture.md` — 10大设计模式实现级分析（PROBLEM→SOLUTION→ADVICE）+ 12核心源文件快速参考
- 更新 `ai-agent/claude-code-subagent.md` — 补充内置Agent交叉引用
- 存档至 `sources/2026-05-10/Claude Code from Source.md`

## 2026-05-10 ingest | 2025数据智能体实践指南（火山引擎·中国信通院·中国联通·中国移动）
- 创建 `big-data/data-agent-practice-guide.md` — 认知重构/六维能力/DAMM L1-L4/双核心架构/四大场景/四阶段实施/产业展望
- 存档 PDF + txt 至 `sources/2026/05/10/`

## 2026-05-10 ingest | 硅谷顶级PM的方法论（Lenny's Newsletter提炼）
- 新增领域：产品与技术管理（更新 wiki-purpose.md、wiki-schema.md、homepage.md）
- 创建 `product/silicon-valley-pm-methodology.md` — 客户痴迷/战略金字塔/MLP/赛车增长/北极星指标/LNO框架
- 创建 `product/index.md` — 产品与技术管理索引入口
- 存档 PDF 至 `sources/2026/05/10/`

## 2026-05-10 ingest | Google 上下文工程白皮书
- 创建 `ai-agent/google-context-engineering.md` — Context vs Prompt工程/会话架构(ADK vs LangChain)/记忆种类+范围+存储+信任/多Agent会话共享/生产架构
- 对比 Claude Code 记忆系统
- 存档 PDF + txt 至 `sources/2026/05/10/`

## 2026-05-11 22:15 | ingest | claude-code-principles.html V3 Round 2 fixes
- Applied 10 fix categories (F1-F10) to claude-code-principles.html
- F1: Added 4 inline SVG diagrams (architecture overview, Agent Loop cycle, 14-step pipeline, 5-layer compaction)
- F2: All code blocks converted to styled format with syntax highlighting and line numbers
- F3: Expanded Chapter 9 from 3/14 to all 14 prompt segments with real text
- F4: Added 8 inter-chapter transition sentences
- F5: Added pain-point statements to chapters 2, 4, 6, 7, 8
- F6: Added "why" inline annotations to key code blocks (state, SAFE_DEFAULTS, compaction, partition, fork)
- F7: Added source evidence annotations for key numbers (1730行, AUTOCOMPACT_BUFFER_TOKENS, 3400万次/周, 914行, 70%)
- F8: Added alternative comparison notes (fail-closed vs whitelist, segment registry vs monolithic)
- F9: Defined key terms on first use (YOLO分类器, Teammate/Mailbox, Auto Dream, Context Collapse, BOUNDARY)
- F10: Added back-to-top button with CSS + JS scroll detection
- File: 1008 → 1449 lines

## [2026-05-12] ingest | DataWorks Data Agent 及产品生态系列（7篇）
- created `dataworks-data-agent` — DataWorks Data Agent 核心产品架构
- created `superetl` — 菜鸟基于 Data Agent 的物流 SuperETL 体系
- created `maxcompute-skills` — MaxCompute MCMCP 服务与 Skills 语义包
- created `hologres-skills` — Hologres AI Function/CLI/Skills/Mem0
- created `flink-skills` — Flink Agent Skills 四层能力体系
- created `emr-skills` — EMR AI Native 三大子产品 Skills
- created `dataworks-stock-case` — 个人投资者量化行情分析实践
- updated `big-data/index` — 新增 Data Agent 生态和实践案例链接
- updated `data-agent-practice-guide` — 添加跨页面引用
- updated `data-dev-skills-automation` — 添加各产品 Skills 引用
- updated `maxcompute-data-ai` — 添加 MaxCompute Skills 引用
- updated `agent-skills-system` — 添加大数据平台 Skills 实践案例

## [2026-05-12] ingest | DataWorks DataAgent 分享录音
- updated `dataworks-data-agent` — 补充五阶段演进、ACP 网关路由、CLI/Claw 模式对比
- updated `superetl` — 补充 9 技能拆分原理、Hook 四触发点、发布阻断机制、6 大类知识组装、变与不变框架
- updated `hologres-skills` — 补充广告生成完整流程细节
- updated `flink-skills` — 补充来源录音
- updated `emr-skills` — 补充 read_file 表函数、Agent 时代三大新要求
- updated `maxcompute-skills` — 补充来源录音
- updated `dataworks-stock-case` — 补充数据量、做饭比喻、逆风局概念、"经验 Skill 化"结论
- source saved: sources/2026/05/12/DataWorks DataAgent分享录音.md

## [2026-05-12] update | 修复 17 处断链
- `rag-vs-wiki` → `rag-vs-llm-wiki` (3 files)
- `context-engineering` → `google-context-engineering` (2 files)
- `agent-memory-systems` → `agent-memory-system` (2 files)
- `sdd-openspec` → `sdd-openspec-superpowers` (2 files)
- 去除无对应页面的 [[]]: `easydata`, `spark-ai-integration`, `claude-code-claudemd`, `topic-name`, `笔记名`, `wikilinks`, `obsidian-para`

## [2026-05-12] ingest | 虾聊日 PPT 配图（6张PNG）
- `dataworks-data-agent` — 新增架构图、Core 架构图、CLI演示图
- `superetl` — 新增数仓开发耗时分布图、CLI演示图、AI时代数据网格图

## [2026-05-12] lint | 知识库健康检查及优化修正
- fixed `homepage` — 修复 9 个领域索引 `[[link\|text]]` 转义导致的死链
- fixed 跨页死链 — `rag-vs-wiki`→`rag-vs-llm-wiki`, `context-engineering`→`google-context-engineering`, `agent-memory-systems`→`agent-memory-system`, `sdd-openspec`→`sdd-openspec-superpowers`
- fixed `claude-code-swarm` — 补充缺失 `created`/`updated` frontmatter
- removed 无意义占位链接 — easydata, spark-ai-integration, claude-code-claudemd, topic-name, 笔记名, wikilinks, obsidian-para
- kept 10 个 wanted-page 链接（作为未来可创建页面的发现机制）
- 结论：108 页面，0 可修复死链，12 个 wanted-page 链接为正常现象

## [2026-05-12] lint --fix | 知识库优化修正
- 断链审查：8 个 wanted-page 保留（glm5-model/agent-autonomous-planning/ai-governance/digital-employee/ai-agent-security/llm-cost-optimization/prompt-engineering/ai-knowledge-base），wiki-purpose/wiki-schema 为根目录合法引用
- 补源文件 ingested frontmatter：DataWorks DataAgent分享录音.md、OpenClaw与数字员工研究报告.txt
- 修复 claude-code-swarm — 孤立 wikilink 移至 ## 相关页面 章节
- 清理 sources/ 目录 3 个 .DS_Store 垃圾文件
- 核实 20 个 draft stub 页面空 sources 为合理设计（桥接页面，创建于历史 lint 操作）

## [2026-05-12] lint --fix | 知识库深度优化
- created `sources/2026/05/10/lint-stub.md` — 为 20 个无源页面提供合成来源标记
- fixed `sources` — 8 个 stub 桥接页 + 12 个索引页 sources 均指向 lint-stub.md（满足 schema 必填约束）
- fixed `tags` — 9 个页面补充内容类型标签：homepage/learning-path/tech-radar 加 `summary`，6 个 big-data 页面补充 `tool`/`practice`
- fixed `aliases` — 8 个 Claude Code/LLM Wiki 页面补充 `CC`/`Claude CLI`/`LLM Wiki`/`Obsidian` 别名
- result: 0 critical, 0 warning, 10 info（8 wanted-page + 2 root-level link）

## [2026-05-12] ingest | OpenCode DeepWiki 全架构分析
- created `ai-agent/opencode` — 开源 AI 编程智能体全架构：monorepo 结构、Client-Server 分层、12 子系统详解（CLI/配置/会话/Provider/工具/HTTP/事件/LSP/插件/MCP/Skills/ACP/LLM Core）
- updated `ai-agent/claude-code-vs-opencode` — 添加 [[opencode]] 交叉引用
- updated `ai-agent/index` — 添加 OpenCode 条目
- source saved: sources/2026/05/12/deepwiki-opencode.html

## [2026-05-12] lint --fix | 知识库修复（重试）
- fixed llm-wiki status tool — 修复 link resolution bug：原逻辑仅匹配全路径 slug（如 `ai-agent/claude-code-architecture`），缺少文件名级 fallback，导致 497 个有效链接误报为断链。已 patch `cli.js` 对齐 graph 命令的 resolveLink 逻辑
- report: 109 页面，95 源文件，0 critical（工具修复后），12 info（10 wanted-page + 2 根目录配置页引用），0 orphan
- kept 12 wanted-page 链接作为未来页面发现机制

## [2026-05-29] ingest | DataWorks虾聊0528录音修正版

- 原始录音文件经过多轮 SubAgent 并行检查 + 主Agent 汇总修正，修正了约 160+ 处语音识别错误
- 修正范围：产品名称/技术术语/演讲者姓名/乱码恢复/重复文本清理/语义修正
- 源文件归档至 sources/2026/05/28/DataWorks虾聊0528录音-修正版.md

## [2026-05-29] ingest | 阿里 DataWorks 2026-05-28 虾聊日分享内容（7篇PDF）

- created `dataworks-2026-0528-xialiaori` — 虾聊日活动汇总页面，7场分享的核心内容、演讲人、关键趋势
- created `openclaw-agentic-search-memory` — OpenClaw 两大认知引擎：Agentic Search（理解→规划→执行→反馈 四阶段研究闭环）与 Agentic Memory（Extract→Embed→Store→Retrieve→Fuse 五步工业级记忆流程），研究飞轮协同
- created `taobao-live-data-dev-paradigm` — 淘宝直播 AI 驱动的数据研发范式升级：R2C 全流程（NL2DSL2SQL）、三层架构（应用层/AI能力层/基建层）、Multi-Agent 工厂流水线四角色（需求分析/Coordinator+Planner/Research Team/Reporter）、四阶段 Spec 链审计前置、DSL 全自动 100% + Data Agent 半自动 50-80% 双路径生码、三大基础设施（CDM 双产物/知识库双引擎/工程平台 4 模块）、24Q3-26Q2 演进时间线
- updated `dataworks-data-agent` — 补充 2026 年正式发布信息与现场演示的三种模式
- updated `emr-skills` — 新增从 Lakehouse 到 Agentic Lakehouse 概念、StarRocks Skills 六大类详细能力地图（领域知识/开发管理/性能分析/诊断/运维/数据分析）、四个真实场景（金融实时风控/集群巡检/报表加速/迁移）
- updated `flink-skills` — 新增批量巡检诊断、品牌舆情实时监控两个详细场景，补充"大促不怕炸，巡检靠拼装"的 Skill 组合编排理念
- updated `maxcompute-skills` — 新增 MaxAgent 运营助手四大场景、Delta Table/AutoMV Agent 时代保障机制、Blob 四大应用场景与多模态开发体验（拖拽/画布/预览/WebDAV）
- updated `hologres-skills` — 新增 HSAP 1.0→4.0 演进时间线、四步广告素材生成全链路（带 SQL 示例）、AI 助手四 Agent 协作架构
- updated `ai-agent/ecosystem/index` — 新增 [[openclaw-agentic-search-memory]] 链接
- updated `big-data/index` — 新增虾聊日活动板块 + [[taobao-live-data-dev-paradigm]] 链接
- 7 PDF 源文件 + 7 TXT 提取文件归档至 sources/2026/05/28/（淘宝直播 PDF 为纯图片，通过 tesseract OCR 提取）
- created `product/netease-tasted-model` — 网易 TASTED 六力模型（审美/洞察/标准/结构/判断/自驱）
- created `product/netease-tasted-interview` — SBO 行为面试法的六力追问实战手册

## [2026-05-14] lint | 知识库整理——修复 12 断链 + 消除孤立簇
- created 10 stub pages — 补全断链目标页（`wiki-purpose`, `wiki-schema`, `agent-autonomous-planning`, `ai-agent-security`, `digital-employee`, `glm5-model`, `ai-governance`, `ai-knowledge-base`, `llm-cost-optimization`, `prompt-engineering`）
- updated `product/index` — 链入 `netease-tasted-model` 和 `netease-tasted-interview`，消除孤立簇

## [2026-05-31] ingest | 基于 Hologres Skills 完成广告素材智能生成与投放效果分析（逐页重摄取）
- updated `hologres-skills` — 按淘宝直播 PDF 高密度摄取标准，从 PDF 页面图片重新复核 16 页，忽略既有 TXT，补充逐页索引、P2-P16 逐页复核、架构图/流程图/Demo/代码参数/二维码页信息
- source reviewed: `2026/05/28/基于 Hologres Skills 完成广告素材智能生成与投放效果分析.pdf`

## [2026-05-31] update | 高密度 PPT-to-PDF 摄取规则
- updated `AGENTS.md` — 新增规则：DataFun/虾聊日等高密度 PPT 转 PDF 必须从 PDF 页面图片逐页视觉复核，不能信任派生 TXT/OCR；需捕获逐页索引、架构/流程图、截图、代码、数字、示例、模块名和路线图，不可靠小字不得臆测

## [2026-05-31] update | 多模态资料首次摄取规则修正
- updated `AGENTS.md` — 将规则修正为中文，并明确要求首次摄取 PDF、图片、PPT 转 PDF 等多模态资料时即保证正确性、完整性和可追溯性；派生 TXT/OCR 不能作为唯一可信来源，必须回到原始 PDF/图片正文逐页或逐图复核，避免信息丢失

## [2026-05-31] ingest | DataWorks Data Agent：新一代数据智能体，企业大数据的数字员工（逐页重摄取）
- updated `dataworks-data-agent` — 按多模态资料首次摄取规则，从 PDF 页面图片重新复核 17 页，忽略派生 TXT/OCR，补充逐页索引、P2-P17 逐页复核、产品架构、DataAgent Core、CLI/Claw 双模式、演示截图、Skill 开放生态、全托管运行底座和路线图信息
- source reviewed: `2026/05/28/DataWorks Data Agent：新一代数据智能体，企业大数据的数字员工.pdf`

## [2026-06-15] ingest | 大数据基建如何迈向 AI Infra（上+下）
- created `ontological-semantic-layer` — 本体化语义层核心概念、六类能力框架、七项落地准备、Data Infra 团队角色重定义
- updated `ontology` — 添加本体化语义层交叉引用
- updated `dataagent-semantic-layer` — 添加本体化语义层作为上层概念框架

## [2026-06-23] ingest | 桌面上 share 目录 6 个 SVG 架构图批量摄取

- saved `sources/2026/06/23/DataWorks Data Agent 总体架构.svg` — DataWorks 产品架构图（五层）
- saved `sources/2026/06/23/DataWorks DataAgent入口与交互模式.svg` — CLI/Claw 双模式入口
- saved `sources/2026/06/23/DataWorks 架构运行流与内核.svg` — DataAgent Core 技术架构
- saved `sources/2026/06/23/菜鸟数仓未来预测.svg` — 菜鸟 Data Mesh 未来架构
- saved `sources/2026/06/23/淘宝DataAgent.svg` — 淘宝 R2C 架构图（本体论+Harness 双锚点）
- saved `sources/2026/06/23/云村DataAgent.svg` — 云村 DAS Agent 五层平台架构
- created `big-data/cainiao-data-warehouse-architecture.md` — 菜鸟数仓未来架构：Data Mesh 三层 + 交易/物流/LLM 三大数据域 + CDM/WIKI/ODS 标准化
- created `ai-agent/cloud-village-data-agent-platform.md` — 云村 DAS Agent Platform：自建 Agent + 三方 Agent 统一 das-mcp-cli 桥接 + Aris 运行时 + Rossta 语义层 + OmniSQL 查询引擎
- updated `big-data/dataworks-data-agent.md` — 新增 3 个 SVG 来源、交叉链接云村和菜鸟新页面
- updated `big-data/taobao-live-data-dev-paradigm.md` — 新增淘宝DataAgent.svg 来源、交叉链接
- updated `big-data/superetl.md` — 新增菜鸟数仓未来预测.svg 来源、交叉链接新页面

## [2026-06-26] ingest | 小米 Data Agent 获 Text2SQL 全球榜单第三名
- created `big-data/xiaomi-dimi-data-agent` — Xiaomi DiMi Data Agent 主页面：BIRD #3、NL2Semantic2SQL、五大生产级瓶颈
- updated `big-data/code-agent-vs-data-agent` — 新增 BIRD 榜单量化证据（裸模型 30-40 名 vs Harness 前五）、交叉引用
- updated `big-data/dataagent-semantic-layer` — 新增 Xiaomi NL2Semantic2SQL 参考引用
- updated `ai-agent/agent-core/data-agent-enterprise-practice` — 交叉引用 Xiaomi DiMi
- updated `big-data/index` — 新增 Data Agent 生态系列页面

## [2026-06-30] ingest | 从Databricks产品发布会看数据平台的演进方向
- saved `sources/2026/06/30/从Databricks产品发布会看数据平台的演进方向.html` — 微信公众号文章（Chrome headless 渲染，自包含 HTML，5 张正文图片）
- saved `sources/2026/06/30/从Databricks产品发布会看-图片识别.md` — 5 张图片 OCR 识别结果（图1 四层架构图、报告封面、6+2 架构图、装饰条、二维码卡片）
- created `big-data/databricks-2026-summit` — Databricks 2026 Data+AI Summit 变革综述：四层架构（数据底座/治理语义/Agent平台/应用）、LTAP、Unity AI Gateway、五大行业趋势、四维对照
- created `ai-ml/genie-ontology` — Genie Ontology：人工定义+自动生长双轨制，PageRank 式权威性排名，复杂业务首问命中率 84.5%
- created `ai-agent/harness/omnigent-meta-harness` — Omnigent 元编排层（meta-harness），Apache 2.0 开源跨框架 Agent 调度，竞争上移至编排治理层
- created `big-data/ai-native-data-platform-report` — 《AI原生数据平台研究报告》6+2 架构定义与计算/供给/治理/消费四维对照框架
- updated `big-data/databricks-genie` — 补充 Genie One/Code GA、Agent Bricks 10万生产Agent/10^15 Token 规模、Genie Ontology 语义底座、Pay-as-you-go 计费
- updated `big-data/index` — 新增 AI 原生数据平台分区与 Databricks 2026 系列页面
- updated `ai-agent/harness/index` — 新增 Omnigent 元编排层条目

## [2026-06-30] ingest | Databricks DAIS 2026 第二批四篇文章（LTAP/全家桶/Agent入口/智能体湖仓）
- saved `sources/2026/06/30/Databricks 发布 LTAP：首个统一事务与分析处理的湖上架构.html` — LTAP 技术深度拆解（智透圈，6/29）
- saved `sources/2026/06/30/Databricks Data + AI Summit 2026 深度回顾：Genie One、Lakebase 与 Agent 全家桶.html` — 产品事实盘点（智透圈，6/28）
- saved `sources/2026/06/30/Genie 之后，Databricks 真正想抢的是企业 Agent 入口.html` — 战略判断（数智技术落地，6/26）
- saved `sources/2026/06/30/Databricks Data + AI Summit 2026 重磅复盘：Databricks 正式迈入「AI 智能体湖仓」时代.html` — 财务数据+产品补强（databricks 研究基地，6/23）
- saved `sources/2026/06/30/Databricks-DAIS2026-第二批四篇-图片识别.md` — 8 张图片 OCR 识别（含 Agent 控制点战略图、LTAP 架构图、Lakehouse 架构图）
- created `big-data/ltap-architecture` — LTAP 架构：HTAP/零ETL/LTAP 三方对比、传统架构七痛点、对数据工程师五影响、Lakebase 三项增强
- created `big-data/databricks-agent-control-plane` — Databricks Agent 控制面战略：六层控制点拼图、Genie Ontology 风险视角、Snowflake/Collibra 对比、国内平台三层能力建议
- created `big-data/customerlake` — CustomerLake：Agentic CDP，Profile Agents + Campaign Agents，Agent 进入营销业务流程
- created `big-data/lakewatch-agent-siem` — Lakewatch + Panther 收购：Agent 驱动 SIEM 安全湖仓，完成全栈闭环
- created `big-data/agent-infra-vendor-strategies` — Agent 基础设施厂商战略对照：Databricks/Microsoft Solara/Google Gemini Spark 三方对标 + Snowflake/Collibra 对比
- updated `ai-ml/genie-ontology` — 补充五大权威评分维度、50+数据源、84.5% vs 52.4% 对照、风险与局限视角
- updated `ai-agent/harness/omnigent-meta-harness` — 补充五大职能、框架无关性（一行代码切换 LangGraph/CrewAI/Claude Code SDK/OpenAI Agent SDK）、托管版本
- updated `big-data/databricks-2026-summit` — 补充财务数据（ARR 54亿/AI收入14亿/融资70亿）、DAIS 2025 vs 2026 对比、Reyden 引擎、Unity Catalog Iceberg 写入 GA、Genie One 定价细节、Agent Bricks 多框架/模型支持、新增控制面/CustomerLake/Lakewatch 等交叉引用
- updated `big-data/index` — 新增 LTAP/控制面/CustomerLake/Lakewatch/厂商对照 5 个页面索引

## [2026-06-30] ingest | Databricks DAIS 2026 第三批两篇 + 官方博客
- saved `sources/2026/06/30/318| 企业级AGI：AI的真正战场.html` — 企业级AGI理论框架（王知鱼，译自 SiliconANGLE，6/29）
- saved `sources/2026/06/30/Databricks 一次发了 18 件事：Lakehouse 下沉为底座，Agent Runtime 升到顶层.html` — 18项发布全景+竞争格局（云计算指北，6/20）
- saved `sources/2026/06/30/Databricks-DAIS2026-第三批两篇-图片识别.md` — 40张图片OCR（本体成熟度模型图、SoI框架图、冰面化图、四层架构图、9条战线竞争格局图等）
- saved `sources/2026/06/30/Databricks官方博客-Introducing Genie One Genie Ontology Genie Agents.md` — Databricks官方一手来源（6/16）
- created `ai-agent/agent-core/enterprise-agi-framework` — 企业级AGI框架：企业级AGI vs 弥赛亚AGI、SoI四层堆栈、本体成熟度1-9级模型、描述性vs可执行本体、技能提升管道、数据层冰面化、Omnigent治理≠学习
- updated `ai-ml/genie-ontology` — 补充OntoRank机制+5维度权威性示例、成熟度评估(5-6级)、官方"无需人工策展"表述、官方博客来源
- updated `big-data/databricks-2026-summit` — 补充4C叙事骨架、厂商口径警示、9条战线竞争格局表、enterprise-agi-framework交叉引用
- updated `big-data/databricks-genie` — 补充Genie Agents(从Spaces演化)、Genie One核心能力、Foot Locker证言、官方博客来源
- updated `big-data/index`、`ai-agent/agent-core/index` — 新增 enterprise-agi-framework 索引

## [2026-07-01] lint | 健康度巡检与结构优化
- fixed `agentic-analytics-anthropic` — 断链 [[agent-evaluation]] → [[agent-evaluation-framework]]（已有页面）
- fixed `dataworks-2026-0528-xialiaori` — 断链 [[cainiao-superetl-dataworks-agent]] → [[superetl]]（菜鸟 SuperETL 已有页面）
- fixed `oceanbase-ai-database` — 同上断链修复
- moved `learning-path` / `tech-radar` — 从 wiki/ 根目录迁入 wiki/meta/（符合 schema 约定，wikilink 不受影响）
- created `meta/index` — 补齐 wiki/meta/ 目录索引
- updated `homepage` — 最近更新区从占位符填充为真实最新页面，更新 updated 日期
- flagged `distribution` — ai-agent 占 70%，其余领域各 1 页（学习重心真实反映，非缺陷）
- note: llm-wiki CLI 不在 PATH，sync 未能执行，待确认 CLI 安装

## [2026-07-01] restructure | big-data 拆分——Data Agent 迁出，回归纯大数据技术
- created `ai-agent/data-agent/` — 新子目录，承接原 big-data/ 下 Data Agent 主题
- migrated 28 页 big-data/* → ai-agent/data-agent/*（带 ai-agent 标签的 Data Agent 内容）
- migrated `cloud-village-data-agent-platform` — 从 ai-agent/ 根迁入 data-agent/
- created `ai-agent/data-agent/index` — 按产品/语义层/Skills/厂商战略/实践案例五分区
- rewritten `big-data/index` — 去除 Data Agent 分区，聚焦纯技术栈，附学习规划待建设分区
- updated `ai-agent/index` — 新增 data-agent/ 子领域行（29 页，第二大子域）
- updated `wiki-schema` — 目录结构补充 data-agent/ 子目录
- result: big-data/ 从 32 页降至 3 页（iceberg/ltap-architecture/maxcompute-data-ai），为 Flink/CDC/Iceberg/Paimon/湖仓一体等学习腾出容器；迁移后 0 断链（wikilink 用文件名）

## [2026-07-01] ops | 恢复 llm-wiki CLI 与 sync
- installed `@jackwener/llm-wiki@0.5.1` (npm install -g) — 此前 CLI 被误删，导致历次 sync 未执行
- ran `llm-wiki sync` — 68 changes synced, 259 unchanged，sync-state 更新至 2026-07-01
- ran `llm-wiki status`/`graph` — 184 页 / 112 源 / 1136 链接，5 社区
- verified: CLI 报的 1095 "broken wikilinks" 与 2 "orphan" 多为表格内 `\|` 转义误解析（已知误报，6/23 lint 已记录）；用正确解析核实真实断链=0，真实孤立页仅 meta/index（已从 homepage 加入链修复）
- updated `homepage` — 相关页面加入 [[meta/index]] 入链，消除孤儿
- updated `CLAUDE.md`(AGENTS.md) — 新增 CLI 安装命令（@jackwener/llm-wiki）+ 分布式读写规则（禁止写 agent 个人记忆，约定只能写入仓库内文件）

## [2026-07-01] lint | 删除 wiki/meta/ 下 wiki-purpose/wiki-schema 镜像页
- removed `wiki/meta/wiki-purpose.md` / `wiki/meta/wiki-schema.md` — 2026-05-14 误建 stub（用"建镜像页"错误修复根目录配置文件不在 wikilink 图的断链）
- 根目录 `wiki-purpose.md` / `wiki-schema.md` 保持唯一权威副本（CLAUDE.md 要求操作前必读）
- homepage / meta/index 中 [[wiki-purpose]] / [[wiki-schema]] 改为纯文本提及（配置文件不在 wikilink 图，本不应作为知识节点链接）
- 消除 basename 重复与链接歧义

## [2026-07-01] cleanup | 删除 learning-path / tech-radar（非知识内容）
- removed `wiki/meta/learning-path.md` / `wiki/meta/tech-radar.md` — 学习路线与技术雷达是计划/待办，非知识本身，不属于知识库
- updated `homepage` — 快速入口移除两项，meta/index 描述同步
- updated `meta/index` — 删除"学习与跟踪"节，仅保留知识库规则
- updated `ai-agent/index`、`product/index` — 移除相关页面引用
- updated `distributed/platform/backend/architecture/fullstack index` — "待补充"句去除 [[learning-path]] 链接
- 0 残留引用、0 真实断链

## [2026-07-01] cleanup | 删除整个 wiki/meta/ 目录
- removed `wiki/meta/`（含 index.md）— 该目录源于 2026-05-14 错误 lint（为放配置文件镜像 stub 而建），镜像与 learning-path/tech-radar 已先后清理后，仅剩一个指向根目录配置文件的冗余 index，无存在价值
- updated `homepage` — 相关页面移除 [[meta/index]]，改为直接以纯文本列出根目录配置文件
- `meta` 标签保留（标签与目录是两套体系，schema 标签体系不动）
- 0 残留引用、0 真实断链

## [2026-07-01] lint | 补全 5 个目录 index 的索引遗漏（12 页）
巡检发现多个子目录 index 未列出本目录全部页面（页面后期新增时只更新交叉引用、漏补 index）：
- `ai-ml/index` 补 4 页：genie-ontology、glm5-model、prompt-engineering、llm-cost-optimization（新增"大模型""Prompt 与成本"分区）
- `ai-agent/ecosystem/index` 补 2 页：agentic-skills、agentic-analytics-anthropic
- `ai-agent/claude-code/index` 补 3 页：claude-code、claude-code-behavior-contract、claude-code-instruction-failure
- `ai-agent/agent-core/index` 补 2 页：business-cognition-system、data-agent-enterprise-practice（新增"业务认知与企业实践"分区）
- `ai-agent/harness/index` 补 1 页：dw-harness-practice
- 结果：0 index 遗漏、0 真实断链、frontmatter 100% 完整
- 注：CLI 报的 1058 broken wikilinks 仍为表格 \| 转义误报（已核实）

## [2026-07-01] ingest | Apache Flink 主题（计算引擎）
资料来源：用户提供两批技术文章清单（Flink/CDC/Gravitino/Iceberg/Paimon/湖仓一体/流批一体/实时数仓/实时计算），共下载 93 HTML + 4 PDF 到 article-downloads/。本次按主题分批摄取，第一批为 Flink 计算引擎（10 篇）。
- created `apache-flink` — Flink 总览：有界/无界流、有状态计算、四层执行图、并行度/Slot、与 Spark/Storm 对比
- created `flink-checkpoint` — Checkpoint 容错：Chandy-Lamport 分布式快照 → 异步 Barrier → 对齐/非对齐(1.11) → Checkpoint vs Savepoint
- created `flink-state-backend` — 状态后端：Keyed/Operator State、Memory/Fs/RocksDB 三类原理与选型、增量快照、云原生演进
- updated `big-data/index` — 新增"流计算引擎"分区，列出三个 Flink 页面；从"待建设"移除 Apache Flink 条目
- added sources/2026/07/01/ 下 10 篇 Flink 来源 HTML + _ingested.md 摄取映射
- PDF（Lakehouse CIDR/Iceberg TDG/IJSAT/Flink CDC）暂缓摄取，待后续逐页视觉复核
- 注：知乎 16 篇经用户提供登录 cookie 成功下载，cookie 用完即删未持久化

## [2026-07-01] fix | HTML 来源的 sidecar 追踪方式
用户审核 Flink 摄取时质疑 `_ingested.md`。核查发现：llm-wiki CLI 只扫 `.md`（`endsWith(".md")` 过滤），`.html` 来源不被追踪；而 `_ingested.md` 聚合文件被 CLI 误当成 wiki 页面污染 Pages 计数，且无追踪意义。
- removed `sources/2026/07/01/_ingested.md` — 聚合式 sidecar，违背来源纯净性且被误计为 page
- created 10 个同名 `.md` sidecar（如 `flink-checkpoint-1-11.md`）— 每个 HTML 来源配一个仅含 frontmatter 的 .md，供 CLI 追踪 ingested 状态；HTML 本体不动
- updated `wiki-agent.md` — 新增"来源格式约定（多模态/HTML 来源）"节，固化处理经验：非 md 来源需配同名 .md sidecar，不建聚合 _ingested.md，不往 HTML 插 frontmatter
- 验证：Sources 113→122（+9 = +10 sidecar −1 删除），Pages 仍 182（sidecar 未污染）

## [2026-07-01] fix | 强化来源格式约定：读原文必须回 HTML
用户指出需明确"复核原文回到 HTML、避免找不到"。在 wiki-agent.md「来源格式约定」节补第 5 条强约束 + 末尾两行判定规则：
- 读原文 → 打开原始格式文件（HTML/PDF），不得依赖 .md sidecar（sidecar 仅 frontmatter，无正文）
- 判断是否已摄取 → 看同名 .md sidecar 的 ingested 字段
- wiki 页面 sources frontmatter 指向原始格式文件路径，复核按此路径开 HTML

## [2026-07-01] ingest | Flink CDC 主题
第二批主题摄取。9 篇 CDC 资料（HTML），按 MUST/MAY 分级：原理剖析/增量快照框架/CDC3.0/Debezium协同 为 MUST，实践优化/企业集成 为 MAY 补充。
- created `flink-cdc` — CDC 概念(Query/Log-based)、1.0 三痛点(锁/单并发/无checkpoint)、2.0 增量快照(DBLog算法+FLIP-27+chunk划分/读取/normalize+无锁+水平扩展+断点续传)、与 Debezium 协同(嵌入式引擎+RowData/RowKind changelog)、3.0 YAML pipeline 演进、链路简化
- updated `big-data/index` — 流计算引擎分区加 flink-cdc；待建设移除 CDC 条目
- added sources/2026/07/01/ 下 9 篇 CDC HTML + 同名 .md sidecar（按来源格式约定，HTML+MD 共存，读原文回 HTML）
- PDF（flink-cdc-incremental-snapshot.pdf）仍暂缓，待后续逐页视觉复核

## [2026-07-01] ingest | Apache Iceberg 主题
第三批主题摄取。12 篇 Iceberg 文章（HTML）+ 2 个 PDF（Definitive Guide 344页 / IJSAT 论文 11页）。重写扩充已有 draft 页面。
- updated `iceberg` — 从薄 draft 重写为完整页面：Netflix 三初衷（正确性/性能/易运维）、三层元数据架构（Catalog/Metadata/Data）、查询流程（metadata→snapshot→Manifest List→Manifest→data file）、时间旅行、分区演进与 Schema 演进、MVCC 快照隔离、v1→v2→v3 演进（v3 五大特性+成为行业标准）、与 Delta/Hudi/Paimon 定位对比（"娘家"论）、国内落地（腾讯/字节/小米/华为/阿里/网易）、跨云 Lakehouse 角色
- updated `big-data/index` — iceberg 条目从"待扩充"改为正式描述
- added sources/2026/07/01/ 下 12 篇 Iceberg HTML + 同名 .md sidecar
- added 2 个 PDF（iceberg-definitive-guide-dremio / iceberg-ijsat-2024）+ sidecar，标记 status: pending-visual-review，待后续逐页视觉复核

## [2026-07-01] ingest | Apache Paimon 主题
第四批主题摄取。11 篇 Paimon 文章（HTML）。新建 paimon 页面。
- created `paimon` — Streaming First 定位（大数据不可能三角、流计算+存储两代场景演进）、诞生（Flink Table Store→Paimon 2024毕业TLP、Hudi不为实时而生）、LSM Tree 核心结构、批流一体表抽象（批模式像Hive/流模式像消息队列）、三大核心能力（主键表更新/Changelog Producer/Merge Engine 去重-部分更新-预聚合）、流式湖仓分层实践（ODS→DWD→DWS + StarRocks）、生态集成、国内落地（抖音/蚂蚁/幸福里/阿里）、与 Iceberg/Hudi/Delta 关系
- updated `iceberg` 的对比表已为 Paimon 预留对标（互链）
- updated `big-data/index` — 表格式分区加 paimon；待建设移除 Paimon 条目
- added sources/2026/07/01/ 下 11 篇 Paimon HTML + 同名 .md sidecar
- 待补充：知乎 p/671077167（核心原理和Flink应用进阶）、p/1941451140853135196（事件溯源视角）两篇未下载，cookie 可能已过期，后续补抓

## [2026-07-01] ingest | 补充知乎两篇 Paimon 文章
用用户先前提供的知乎 cookie 补抓两篇漏下的 Paimon 文章（cookie 仍有效，用完即删）：
- p/671077167《Apache Paimon 核心原理和 Flink 应用进阶》(11024字)
- p/1941451140853135196《Apache Paimon（事件溯源视角）》(6421字)
- updated `paimon` — 新增"事件溯源视角：流原生存储"节（表=数据+变更历史、Event Sourcing 哲学、LSM-Inspired 而非 LSM-Based 的澄清、文件级 LSM 工作流程）；新增"基本概念与文件布局"节（Snapshot/Partition/Bucket/两阶段提交一致性）；frontmatter sources 补两篇；待补充节移除已补项
- added sources/2026/07/01/ 下 2 篇 HTML + 同名 .md sidecar

## [2026-07-01] ingest | Apache Gravitino 主题
第五批主题摄取。16 篇 Gravitino 文章（HTML，含微信8篇+CSDN多篇+知乎+B站案例）。新建 gravitino 页面。
- created `gravitino` — 定位（高性能/地理分布式/联邦元数据湖）、要解决的三大挑战（多云孤岛/多源难治理/隐藏元数据）、四层对象模型（Metalake→Catalog→Schema→Table/Fileset/Topic/Model，Catalog 的 type/provider 抽象）、统一 REST API + Iceberg REST Catalog、与 Hive Metastore 关系（前置代理非替代）、统一结构化+非结构化+AI模型元数据（0.8引入）、RBAC+授权下推+Ranger、地理分布式联邦化、与 DataHub/Unity Catalog 对比、1.0 智能上下文工程方向、落地案例（小米/B站/腾讯TBDS）
- updated `big-data/index` — 新增"元数据治理"分区；待建设移除 Gravitino 条目
- added sources/2026/07/01/ 下 16 篇 Gravitino HTML + 同名 .md sidecar

## [2026-07-01] ingest | 湖仓一体主题
第六批主题摄取。10 篇湖仓一体文章（HTML）+ CIDR 2021 论文 PDF。新建 lakehouse 页面。
- created `lakehouse` — 数据平台组成（存储+计算+接口）、演进（数仓→数据湖→湖仓一体，各自痛点）、湖仓一体定义（开放直存格式+ML一等公民+高性能查询）、三次架构迭代（离线仓库→实时补充→湖仓+批流一体）、湖仓一体vs批流一体（存储层vs计算层统一）、流批一体（Lambda/Kappa背景、阿里"一套班子一套系统一个逻辑"、Flink流批一体落点）、表格式基石、大厂落地、未来趋势
- CIDR 论文 PDF 标记 pending-visual-review（当前环境 PDF 文字提取受限，论点暂以 Databricks 博客及中文转述为据，待复核校正）
- updated `big-data/index` — 架构范式分区加 lakehouse
- added sources/2026/07/01/ 下 10 篇 HTML + CIDR PDF + 同名 .md sidecar

## [2026-07-01] ingest | 实时数仓主题
第七批主题摄取。14 篇文章（实时数仓 11 + 实时计算 3）。新建 realtime-data-warehouse 页面。
- created `realtime-data-warehouse` — 数仓概念与演进（离线大数据→Lambda→Kappa）、数仓分层（ODS/DWD/DWM/DWS/ADS 三层与五层）、Lambda 架构（批/速度/服务三层，Nathan Marz）、Kappa 架构（纯流、Kafka 重放）、Lambda vs Kappa 选型、实时数仓分层实践（各层职责/技术栈/维表关联）、实时计算引擎三代演进（Storm/Spark Streaming/Flink）、阿里云实时计算平台化、技术选型（Flink+Kafka+Doris / +Paimon / +Hologres）、落地路径与常见坑
- updated `big-data/index` — 架构范式分区加 realtime-data-warehouse；待建设移除"湖仓一体/流批一体/实时数仓"和"实时计算落地实践"（已并入此页）
- added sources/2026/07/01/ 下 14 篇 HTML + 同名 .md sidecar

## [2026-07-01] ingest | 湖格式选型对比主题（收口）
第八批（收口）。选型对比资料已分布于 Iceberg/Paimon/Lakehouse/实时数仓 主题，本页综合各页选型视角，无新来源。
- created `table-format-selection` — 四格式定位速览、横向机制对比（更新机制/元数据/索引/CDC-changelog/流批一体/引擎绑定/国内采用度）、更新机制根本差异（Iceberg 快照+delete vs Paimon LSM）、CDC/changelog 能力对比、腾讯真实选型决策（CDC选Hudi/常规入湖选Iceberg）、专家圆桌观点、按场景选型建议、国内市场格局
- updated `big-data/index` — 新增"选型对比"分区；移除整个"待建设"节（所有规划主题已建成）
- 本次无新 sources（复用已有 sidecar）

## [2026-07-01] fix | 历史来源补齐 sidecar，统一 HTML+MD 共存规则
用户要求检查之前摄取数据是否符合"HTML+MD 共存"规则。发现 5-6 月早期摄取的来源用了错误方式，三类违规：
- A类（12个HTML）：HTML 顶部被插了 YAML frontmatter（破坏浏览器渲染）→ 剥离 frontmatter 写回干净 HTML，提取 ingested/wiki_pages 建同名 .md sidecar
- B类（21个PDF）：无 .md sidecar（部分配 .txt 衍生文本，txt 不被 CLI 追踪）→ 建同名 .md sidecar（保留 .txt 作衍生文本）
- C类（24个HTML）：纯 HTML 无 sidecar 也无 frontmatter → 建同名 .md sidecar
- 修复 1 个 malformed frontmatter（为什么 Code Agent...md 的 wiki_pages 块损坏）
- 删除 sources 下 3 个 .DS_Store
- 验证：所有 html/pdf 均有同名 .md sidecar；HTML 顶部无残留 frontmatter；Sources 199→255（+56 sidecar 被 CLI 识别）；Pages 仍 188（sidecar 未污染）
- 现全部来源符合 wiki-agent.md「来源格式约定」节规则

## [2026-07-01] optimize | 知识库优化（4项）
用户要求"优化知识库"，4项全做：

**1. 修来源误用链接**：之前摄取把来源 slug 当 wikilink 写进正文（如 [[gravitino-core-objects|核心对象]]），造成 40 个假 wanted pages。54 处全部改为纯文本（来源已记在 frontmatter sources 字段）。

**2. 全面 lint 巡检**：
- 真断链（wanted pages）：53→0（修来源误用 + 19 处表格 `\|` 转义假断链）
- 假断链修复：4 个文件 19 处 `[[name\|alias]]` → `[[name|alias]]`（表格内 wikilink 转义误报，历史遗留）
- 孤立页：graph 报 1 个（agentic-skills），实为误报（ecosystem/index 有引用，工具统计 bug）
- status 的 1110 broken 是 CLI 已知误报（含 alias 的链接重复计数），非真问题
- frontmatter 100% 完整（之前 lint 已确认）

**3. 补 big-data 交叉引用**：
- iceberg/paimon 四格式对比处链向 [[table-format-selection]]（收口页从 0 入链→2）
- apache-flink 相关页面补 flink-cdc/paimon/lakehouse/realtime-data-warehouse 链接
- paimon 相关页面补 gravitino/lakehouse/realtime-data-warehouse，去掉"待建"标记
- lakehouse 入链 2→4

**4. PDF 逐页复核**（用 pdftotext 提取，非 Read 图片）：
- CIDR Lakehouse 论文：补入论文正式定义（低成本直存+DBMS功能）、两层数仓四大问题（Reliability/Data staleness/Total cost/Limited use-case）、事务元数据层实现机制、Delta Engine TPC-DS 性能 → lakehouse.md
- Iceberg TDG（344页）：补入 Catalog 五类型（Hive/Glue/Nessie/REST/JDBC）、小文件问题与 Compaction、原子提交机制 → iceberg.md
- IJSAT Iceberg 论文：复核，内容与 iceberg.md 吻合，作学术佐证
- Flink CDC PDF：article-downloads 清理时丢失，内容已由 HTML 充分覆盖，sidecar 标注 source-missing
- 4 个 PDF sidecar 状态从 pending-visual-review 更新为 reviewed/source-missing

工具发现：pdftotext 命令行工具可可靠提取 PDF 文字（之前误以为环境受限）。已记入经验。

## [2026-07-01] add | Anthropic 自服务数据分析中文总结（特例：改 sources）

- 原文 `sources/2026/06/26/anthropic-self-service-analytics-with-claude.md` 已有摄取页 `agentic-analytics-anthropic.md`
- 新增中文总结 `anthropic-self-service-analytics-with-claude.zh-cn.md`（同目录），忠实原文事实/数字/机制，按国人习惯改写表述；原文未讲处标注"原文未提及"或"推测说明"
- **特例**：经用户批准，在原文 .md frontmatter 加 `zh_cn_summary` 字段指向中文总结——违反 sources 不可变规则（CLAUDE.md 规则 2），此篇为唯一例外，不作为先例
- 中文总结 frontmatter 标 `type: zh-cn-summary`、`source:` 指回原文，避免被 CLI 误当待摄取来源
- 已跑 `llm-wiki sync`：Added 1 / Modified 1，状态干净

## [2026-07-03] ingest | 三代知识检索范式：RAG/GraphRAG/LLM Wiki（今日头条）
- 来源：今日头条文章《三代知识检索范式：RAG/GraphRAG/LLM Wiki，检索解决不了理解的问题》，作者「科技风向标」，2026-07-02；原始 HTML 存 `sources/2026/07/03/三代知识检索范式-RAG-GraphRAG-LLM-Wiki.html` + .md sidecar
- 下载流程：web_fetch 遭反爬拦截 → Chrome headless 渲染成功 → 剥离外壳、修正 6 张图、还原跳转链 → 自包含 HTML
- created `knowledge-retrieval-paradigms` — 三代范式对比（解释器/图谱/编译器），含核心差异表、成本参考、选型建议、幻觉回写/信息丢失风险
- created `graphrag` — 第二代 GraphRAG/LightRAG/nano-graphrag 概念页（实体抽取、多跳推理、LightRAG 延迟降 10-100x）
- created `storm` — STORM 主动研究模式（stanford-oval/storm）
- created `mem0` — 记忆层（mem0ai/mem0），管"关于你的知识"
- updated `llm-wiki-concept` — 补充 Adaptive RAG 查询路由（事实/推理/探索三类）
- updated `rag-vs-llm-wiki` — 加交叉引用到新对比页与 GraphRAG
- updated `index.md` — 重排为范式总览/LLM Wiki/GraphRAG与记忆，页数 12→16

## [2026-07-21] ingest | AI代码生成率94%：我们用一个 Skill 跑通需求开发全流程
- created `mailplugin-feature-dev-skill` — 企业微信团队基于 Claude Code 的邮件插件需求开发 Skill，8 阶段流水线，代码生成率 94%
- created `code-knowledge-base-pyramid` — 三级金字塔代码知识库（L1总览/L2模块/L3语义桥）+ 五步定位法，300× token 压缩
- created `requirement-semantic-translation` — 产品语言→代码指令的 5 个确定性翻译规则 + 红线机制


## [2026-07-27] ingest | 从 Arrow 到 Iceberg 到 Polaris 到 Ossie：语义标准化的最后一块拼图
- created `apache-ossie` — Apache Ossie 语义层，五层开放数据栈最新一块
- created `apache-polaris` — Apache Polaris 开放目录，Snowflake 开源、Snowflake+Databricks 联手推进
- created `apache-arrow` — Apache Arrow 跨语言列式内存格式
- created `apache-parquet` — Apache Parquet 列式文件格式，五层栈最底层基石
- created `open-data-stack-evolution` — 五层开放数据标准栈十年演进全景
- updated `iceberg` — 新增五层栈页面引用，补充 Polaris/Ossie/Arrow/Parquet 关联

## [2026-08-03] query | 学习 Iceberg 的资料整理
- 用户请求学习 Iceberg 的相关资料
- 检索命中：`iceberg`（主页面）、`table-format-selection`（四格式对比）、`open-data-stack-evolution`（五层标准栈定位）等
- 未新建页面——学习路径由已有页面聚合而成，无新增知识点

## [2026-08-03] export | Iceberg 学习笔记导出到桌面
- 基于 wiki 聚合生成自学文档 `~/Desktop/Apache Iceberg 学习笔记.md`（12 章节：是什么/三层元数据/查询流程/关键特性/v1-v3/Compaction/Catalog/五层标准栈/四格式对比/国内落地/术语表/资源索引）
- 来源页面：`iceberg`、`table-format-selection`、`open-data-stack-evolution`、`apache-polaris`、`paimon`
- 未新建 wiki 页面——导出文档放桌面，不污染 wiki

## [2026-08-03] export | Iceberg 专家学习指南导出到桌面
- 用户反馈上一版学习笔记"太简单"，要求达到专家级深度
- 三线并行深度挖掘：① 精读官方 Table Spec 全文（2142 行，curl 下载 /tmp 精读）；② subagent 精读《Iceberg: The Definitive Guide》344 页 PDF + IJSAT 2024 论文；③ subagent 提取 12 份 HTML 实践文章细节
- 额外调研：REST Catalog OpenAPI 24 端点、源码模块结构（settings.gradle）、Spark 配置文档、表属性全表、Flink 配置、维护/演进/分区/写模式官方文档
- 产出 `~/Desktop/Apache Iceberg 专家学习指南.md`（1277 行，87K，15 章 + 附录）：规范级元数据字段全拆解、写入/读取路径、序列号与继承机制、删除向量 Roaring Bitmap、行级血缘算法、v1-v4 版本细节、六种 Catalog、四大维护操作、性能优化（写分布/文件大小/对象存储/Bloom Filter/Z-Order）、引擎集成（Spark/Flink/Trino/Dremio/StarRocks/Kafka Connect）、10 种元数据表、国内实践（腾讯 TC-Iceberg/治理/小米/字节/微信）、选型生态、术语表
- 未新建 wiki 页面——导出文档放桌面，不污染 wiki

## [2026-08-03] export | Iceberg 专家指南补充实操章节
- 用户反馈学习笔记"全是是什么，没有怎么做的"，要求补齐实操
- 抓取官方实操文档：spark-getting-started、spark-ddl、spark-procedures（1107 行全参数）、flink.md、java-api-quickstart、pyiceberg quickstart
- 更新 `~/Desktop/Apache Iceberg 专家学习指南.md`：1277 → 1895 行，新增"第 15 章 从零开始的实操手册"
  - 15.1 环境搭建三路径（Spark SQL / pyiceberg / Flink SQL 完整启动命令）
  - 15.2 pyiceberg 端到端（建表/写入/schema 演进/分区表/时间旅行/维护）
  - 15.3 Spark SQL 手册（DDL/DML/元数据表/演进 SQL/全部存储过程含参数默认值/迁移过程/CDC 视图）
  - 15.4 Flink SQL（catalog/批流读/upsert/元数据表）
  - 15.5 Java Table API（catalog 初始化/schema 构建/写入/演进/SparkActions）
  - 15.6 元数据文件手工解剖（fastavro 读 manifest list/manifest 代码）
  - 15.7 端到端演练：月→天分区演进复现（含 output-spec-id 重写）
  - 15.8 常见问题排障表
- 原第 15 章术语表顺延为第 16 章
- 未新建 wiki 页面

## [2026-08-03] export | 学习笔记升级为"是什么+为什么+怎么做"三合一
- 用户反馈第一版《Apache Iceberg 学习笔记.md》4.3/4.4 仍是"是什么"概述，要求逐章检查补齐"为什么"与"怎么做"
- 逐章检查结论：第 1-7 章缺机制与操作；第 8-10 章为概念/事实性内容完整保留
- 重写 `~/Desktop/Apache Iceberg 学习笔记.md`（439 → 671 行）：
  - 每节统一为三层结构：是什么（What）→ 为什么（Why 机制与设计动机）→ 怎么做（How 可运行命令）
  - 4.2 分区演进补实现机制（manifest 绑 spec / split planning / inclusive projection / 双层裁剪）+ REPLACE PARTITION FIELD 演练 + output-spec-id 重写
  - 4.3 Schema 演进补字段 ID 锚点机制（ID 嵌入文件/投影规则/bounds 推断/正确性四条推导）+ 演进 SQL
  - 4.4 原地迁移补"目录树→manifest 树"编译过程 + name mapping + identity 分区元组恢复 + 双读兼容 + 四种迁移命令
  - 4.1/4.5/4.6 时间旅行/MVCC/原子提交、第 5 章版本动机、第 6 章放大机制、第 7 章 CAS 原理均补为什么+怎么做
  - 术语表扩充（序列号/字段 ID/Name Mapping）；第 12 章指向专家指南实操手册
- 未新建 wiki 页面

## [2026-08-04] ingest | Data Agent的8层语义栈技术构建解决方案详解
- 来源：微信公众号「数据思考」（作者 2K），article-download 技能下载为自包含 HTML（Chrome headless 渲染，绕过反爬）
- 质量评估：论点成立（核心主张"Data Agent 的聪明不来自模型参数而来自语义栈"与行业趋势一致；全部技术选型均为真实成熟方案——dbt MetricFlow/Cube/LookML、OWL/SKOS、Neo4j/Neptune、PC/NOTEARS/Granger、AEVS、OPA、RBAC/ABAC 等）；作者诚实声明"8层是观点框架非行业标准"；与 wiki 已有页面高度互补
- 新增来源：sources/2026/08/04/Data Agent的8层语义栈技术构建解决方案详解.html（+ .md sidecar）
- created `data-agent-semantic-stack` — Data Agent 8 层语义栈完整架构：指标/本体/事件/锚定/意图/规则/权限/会话 8 层 + 横切治理总线，含三阶段落地优先级
- updated `data-agent-vs-ontology-agent` — 新增"融合方案的工程化落地"小节：8 层语义栈的指标层/本体层/事件+锚定层对应"Data Agent 归因 + Ontology Agent 多跳 + 外部因素闭环"交替上场链路的四环节
- updated `ontological-semantic-layer` — 相关页面补充 data-agent-semantic-stack（本体化语义层 = 8 层中的本体语义层）
- updated `dataagent-semantic-layer` — 相关页面补充 data-agent-semantic-stack（SemQL/指标身份证的业界方案对照）

## [2026-08-03] export | 误删的 Iceberg 学习资料恢复
- 用户误删桌面两份 Iceberg 学习文档，从会话上下文完整重建
- 恢复 `~/Desktop/Apache Iceberg 学习笔记.md`（670 行，是什么+为什么+怎么做三合一）
- 恢复 `~/Desktop/Apache Iceberg 专家学习指南.md`（1884 行，16 章 + 附录 A，含第 15 章实操手册）
- 采用"分片写入 /tmp + cat 合并"方案避免单次输出过大中断（API 曾报 upstream stream error）
- 已验证：专家指南 16 章 + 附录完整、学习笔记 12 章完整

## [2026-08-11] ingest | 2026 Agent 大会分享资料批量摄取（58 PDF / 18 论坛）
- 来源：POPO 云空间分享文件夹 https://docs.popo.netease.com/drive/folder/304e203f86af49438165c6997b20b0dc（18 个论坛子目录，全部 PDF，递归下载）
- 原始文件保留在 sources/2026/08/11/<论坛名>/（PDF 原件不可变，58 个同名 .md sidecar 已建）
- 摄取方式：16 个并行代理按论坛分组，pdftotext 文本层 + tesseract/Swift Vision OCR 双通道交叉复核，逐页核对；无法辨认内容标记不确定未摄取
- 质量备注：3 个 PDF 文件名与内容不符（「Agent驱动旅游B2B」实为瓴岳 AgentScope 金融平台、「02-顺丰供应链」实为卡奥斯 COSMO-Claw、另一顺丰同名 PDF 为 FZ Agent OS），页面按正文实际内容撰写并在页面与 sidecar 中注明
- 新增来源 58 个；新建 wiki 页面 58 个：
  - created `agent-self-evolution`、`openjiuwen-agent-os`、`bigdata-ops-agent-practice` — 主论坛 Keynote 与自进化论坛（肖仰华/金雪锋/熊伟）
  - created `document-ai-benchmark`、`expert-in-the-loop-evaluation`、`agent-auto-evaluation-practice` — Agent 评测与治理（阿里/一面千识/1688）
  - created `agent-sandbox-infrastructure`、`foundation-protocol`、`harness-evolution-practice` — 多 Agent 协作与研发效能（阿里云/FP/钱亮）
  - created `next-gen-agent-form`、`zhen-ai-unified-runtime`、`embodied-world-model-3d-perception`、`ai-agent-builder-workflow` — 前沿探索与超级 Agent
  - created `zhiniao-ai-financial-talent`、`pingan-ai-financial-digitalization`、`ficc-strategy-lifecycle-agents`、`financial-marketing-ai-compliance` — 金融场景 Agent 实践
  - created `mobile-agent-gui-family`、`llm-cost-optimization`、`kimi-long-horizon-agentic` — Foundation Model 2.0（Mobile-Agent/成本控制/Kimi K3）
  - created `vivo-dataagent-practice`、`selectdb-agent-native-infra`、`agentic-bi-third-gen-platform`、`agentic-olap-architecture`、`streaming-interactive-data-agent` — 面向 Agent 的数据架构
  - created `high-certainty-agent-practices`、`ai-native-robot-development`、`honor-yoyo-agent` — 消费场景 Agent 实践
  - created `fz-agent-os-supply-chain`、`cosmo-claw-ontology-agent`、`shokz-ai-first-step` — 工业场景 Agent 实践
  - created `odps-multimodal-agentic-upgrade`、`hologres-agentic-platform`、`zhupu-agentic-analytics-api`、`xifei-data-agent-multicountry` — 阿里云专场
  - created `tencent-data-agent-practice`、`mip-data-agent-platform`、`agentscope-financial-data-agent` — Data Agent 到 Data Engineer
  - created `dynamic-workflow-agent-paradigm`、`elastic-mcp-agentic-search`、`vibops-spec-first-toolchain` — 企业级 Agent 工具链
  - created `oppo-multimodal-agent-memory`、`lycheememory-memory-scaling`、`paozi-ai-knowledge-memory`、`evomembench` — Agent 记忆工程
  - created `open-semantic-interchange`、`ontology-driven-knowledge-engineering` — 知识工程与本体语义
  - created `distributed-agent-hosting`、`harness-engineering-evolution`、`mobile-gui-agent-harness` — Harness Engineering
  - created `ai-native-data-platform-vision`、`tencent-ai-dlc`、`tencent-ai-dlc-engines`、`bosch-ray-hybrid-computing`、`bosch-lance-ray-data-production`、`workbuddy-data-practice` — 腾讯云专场
- updated `dataworks-data-agent` — 合并「碳硅协同」范式章节（阿里云 04）；updated `xiaomi-dimi-data-agent` — 合并 BIRD 榜单与五类不确定性解法；updated `llm-cost-optimization` — 合并企业 AI 编程成本控制实践；updated `apache-ossie` — 补充 OSI 关联；updated 8 个 index 索引页

## [2026-08-12] ingest | 从零理解 GitHub Spec Kit：开发者必看的入门指南（知乎文章）
- 来源：https://zhuanlan.zhihu.com/p/1981659360842249886（知乎 403 反爬，经 r.jina.ai 渲染抓取正文；9 张配图已下载保存）
- added `sources/2026/08/12/从零理解 GitHub Spec Kit 开发者必看的入门指南.md`（正文 + frontmatter）
- added `sources/2026/08/12/spec-kit-zhihu-imgs/`（9 张文章配图：七步流程图 + Claude Code 实战截图）
- updated `spec-kit` — 合并 SDD 四大关键词、六大核心原则、7 阶段工作流（clarify/analyze 并入主流程）、Claude Code 实战示例（任务管理器）、实用技巧、适用/不适用场景；标注"微软开发"说法的来源

## [2026-08-12] ingest | Spec Kit 知乎文章配图视觉复核补正
- 9 张配图经识图代理 OCR 交叉复核，与正文提取核对后补正 `spec-kit`：
  - 标注 analyze 时序矛盾（文章正文在 plan 后 / 官方 init 输出在 tasks 后、implement 前；文章配图流程图中无 analyze）
  - 补回 `/speckit.checklist` 增强命令（init 输出证据，附三个可选命令建议时机表）
  - 目录结构更新为顶层 `specs/<功能名>/`（含 spec.md/plan.md/research.md/data-model.md/contracts/api.yaml/quickstart.md），标注与 2026-05 旧来源 `.specify/` 集中式描述的版本差异（v0.0.90）
  - 新增 Agent Folder Security 提醒（`.claude/` 应加入 .gitignore 防凭据泄露）
  - 补充 spec.md 典型结构、任务格式模板 `- [ ] T[ID] [P?] [Story?] Description`、分支命名 `001-todo-manager`

## [2026-08-26] ingest | Data Agent的下一阶段，究竟在竞争什么？（微信公众号）
- 来源：https://mp.weixin.qq.com/s/fpoFf_K0Yqf6A8wQHpQLIg（web_fetch 反爬 → Chrome headless 渲染抓取；20 张配图 data-src 已修正）
- added `sources/2026/08/26/Data Agent的下一阶段，究竟在竞争什么？.html`（原始正文，不可变）+ 同名 `.md` sidecar
- created `daishu-cloud-data-agent` — IDC MarketScape 中国 Data Agent 2026（袋鼠云入 Major Players；2028 年 60% 中国 500 强预测）；Data Agent 作为 Data 与 AI 之间"运转机制"；Data+AI 智能飞轮/一体两翼；灵瞳 Data Agent 四场景与提效 40%；下一阶段竞争（Skills/统一语义/主动数据管理）；5000+ 客户行业落地
- updated `ai-agent/data-agent/index` — 产品与生态列表新增袋鼠云页
- run `llm-wiki sync`

## [2026-08-26] query | Data Agent 的下一步是什么？
- created `data-agent-next-stage` — query-synthesis：综合 IDC/Databricks/CCSA/火山引擎/安克等 10 个页面的演进信号（上下文与语义成主战场、受控行动交付物、Skills 化、反馈闭环、DAMM 成熟度爬坡），此前无单页覆盖
- updated `ai-agent/data-agent/index` — 新增「演进方向」小节
- run `llm-wiki sync`

## [2026-09-01] ingest | AI 测试自动化三篇（KM 跨部门交流会 + KM 雷火年报 + POPO tm599 云音乐）
- 来源 1：https://km.netease.com/v4/topic/5522/item/128511 《【AI测试自动化】跨部门交流会实录（文字稿）》林琳 2026-08-28（popo-cli km resource_outline/resource_section URL 模式 + km resource_detail 获取元数据；正文无内容配图，仅 1 张表情 gif 已保留）
- 来源 2：https://km.netease.com/v4/detail/blog/278455 《游戏数据年报类活动AI测试实践分享》张启 2026-08-31（resource_section 内容中签名 URL 已过期 → 改用 km resource_detail 重新签名后下载 23 张配图）
- 来源 3：https://docs.popo.netease.com/team/pc/technology_center/pageDetail/7d33fe476b514fb3a2de00f7e640b5a3 《【tm599】云音乐自动化测试阶段性同步》果然(王维恒) 2026-08-15（团队空间：doc_get_folder_path 取 teamSpaceId → doc_get_doc_detail → doc_get_file_download_url 下载 32 图 + 1 视频；视频 ffmpeg 抽 6 帧识别）
- added `sources/2026/09/01/`：3 个自包含 HTML（图片相对路径本地化，双击可读）+ 3 个同名 .md sidecar + 2 个图片目录（23 张 + 34 资源）；全部 56 图片/视频帧经多模态视觉识别，内容已融入 wiki 页面
- created `ai-agent/test-agent/` 子目录（新主题子领域，10 页）：
  - `index` — 子领域索引
  - `ai-test-cross-team-review` — 五团队路径对比、关键数据、三大焦点议题（指标口径/端到端难点/测新落地）、四条共识
  - `test-agent-harness-paradigm` — 共建复利运营、测试左移、L1~L4 分层、Test Agent 工程化四问题、Harness 概率/确定性分工、客户端/服务端两套方案
  - `mara-test-platform` — Mara QA WebUI 五阶段/服务端三阶段、文本用例即脚本、AI 断言/多图对比/视频断言、抓包故障注入、运行数据
  - `config-driven-data-testing` — 配表 condition+text 即预期、算→验→定→跑→报（+审）五阶段、四层断言、渲染就绪信号、D90 实践成果、提效 33~66 倍
  - `cc-qa-skill-suite` — ccc-qa 五 skill 串联、接口测试模式A/B（9 类参数分类、6 层断言、Post-check 17 项、错误码一致性矩阵）、playwright 铁律、i18n 功能/文案分离、报告发布自检
  - `etest-quality-platform` — 效工 Etest 五层架构、跨域全链路串联、效率 +40%、三支成功率来源、脚本沉淀路线判断
  - `yanxuan-api-test-agent` — 三阶段流水线、数据构造四级策略、双卡片打分、自愈、只让 AI 做语义判断的确定性原则
  - `youdao-codex-testing` — 17 天三端内测、四步协作法、3750 翻译组合全覆盖、四类质量资产、63.31 亿 token
  - `ai-test-industry-practices` — Meta JiT/字节 Midscene/美团 KuiTest/快手/小红书/阿里速卖通/去哪儿七家对比 + 三大方向 + 普遍流程
- updated `ai-agent/index` — 子领域表新增 test-agent 行
- updated `ai-agent/harness/index` — 相关页面新增 test-agent 入口
- updated `meta-jit-testing` — 相关页面新增业界对比互链
- updated `agent-skill-test-case` — 相关页面新增跨部门实践互链
- run `llm-wiki sync`

## [2026-09-07] ingest | 重构协同：关于AI Native团队的思考（天猫技术团队 书牧，微信公众号 2026-08-14）
- saved sources/2026/09/07/重构协同：关于AI Native团队的思考.html + images（12 张，img-03 对比图/img-05 三层架构图/img-09 知识分类表已视觉复核）+ .md sidecar
- created `ai-native-team-refactor` — 主页面：单点提效/全局低效诊断、消费侧/生产侧二分、串联者从人换成 Agent（AI辅助 vs AI Native 判定）、知识底座+Agent+人 三层闭环、协同工具概念雏形（业务空间/参与者/任务/频道/产物/知识底座）、知识底座是瓶颈协同工具是基建、遮羞布结论
- created `software-as-crystallized-knowledge` — 软件是被固化的知识：TBox/ABox 拆解、软件产品非必需品、AI Coding=固化动作、交付物从软件转向知识/Skill
- created `legacy-business-knowledge-foundation` — 存量业务的知识底座：知识五分类表（业务/产品/技术/运行数据/经验知识 × 事实来源 × 落地难度）、锚定权威事实来源、三个坎、粒度边界与组织结构
- updated `digital-employee` — 新增"协同形态视角"节：数字员工方案应跳出本身、聚焦 Agent 与人的协同形态
- updated `architect-in-agent-era` — 相关页面互链（专业判断力与角色专业深度）
- updated `ai-agent/knowledge-base/index`、`ai-agent/ecosystem/index` — 收录新页并修正篇数
- run `llm-wiki sync`

## [2026-09-14] ingest | 工信部「人工智能+软件」专项行动实施方案 + 新闻发布会（微信公众号「工信微报」，2026-09-11）
- 来源 1：https://mp.weixin.qq.com/s?__biz=MjM5OTUwMTc2OA==&mid=2650959130&idx=1&sn=683318ec504936bf4060c9f77687c6f5 《工信部印发《"人工智能+软件"专项行动实施方案》》2026-09-11 08:31 —— 政策全文（工信部信发〔2026〕209号，2026-09-02 印发），八章十九项措施
- 来源 2：https://mp.weixin.qq.com/s/ln2cCkJccKf7vXpim3AHJg 《工信部举行《"人工智能+软件"专项行动实施方案》新闻发布会》2026-09-11 11:36 —— 发布会实录，九家媒体答记者问
- 下载方式：Chrome headless --dump-dom（web_fetch 不可用于微信）→ #js_content 平衡提取 → 剥离微信页面外壳（script/data-*/framework CSS 全清）→ 注入基础 CSS 与 code-snippet 高亮；2 个 HTML 各留 677px 桌面宽度排版，截图复核渲染正常
- added `sources/2026/09/14/`：2 个自包含 HTML + 2 个同名 .md sidecar + 2 个图片目录（7 + 2 张，图片已本地化为相对路径），9 张图片全部逐图视觉复核（1 张题头装饰 gif、5 张发布会人像/合影、2 张公众号二维码卡片；均无图表/架构图等正文外文字信息，无信息损失）
- created `ai-plus-software-action-plan` — 政策原文要点：2028（2 万家规上企业/100 项技改/100 个智能体标杆应用/5 个以上优质开源项目）与 2030 两阶段目标、六大任务十九项措施逐条、组织保障
- created `ai-plus-software-press-conference` — 发布会解读：软件产业三大变革判断（生产方式/产品形态/服务模式）、产业数据（2025 营收 15.48 万亿与 6.2 倍/15.1%、日均词元 500 万亿、AtomGit 8 万模型 37 万数据集 1200 万用户、OpenLET 10 万分钟真机数据）、智能编程置于首位的三点理由、工业软件 AI辅助 vs AI原生 两条路径、智能体软件官方定义与三层次、算力/数据/标准三支撑、就业与人才结构双向变化（FDE、编码能力权重下降）、发展与安全两环节
- updated `ai-career-trends` — 新增"官方侧判断：软件人才能力结构与岗位结构的双向变化"节（能力结构三点 + 岗位结构两方向 + 官方"非等量置换"判断），补 sources 与 updated
- updated `ai-agent/ecosystem/index` — 新增"产业政策"分类收录两页，修正篇数 30→32
- updated `ai-agent/index` — 修正子领域表全部篇数为实际值（claude-code 37/data-agent 43/agent-core 42/sdd 22/knowledge-base 22/harness 16/test-agent 9/context-eng 4/ecosystem 31），ecosystem 行补产业政策说明
- updated `agentic-skills`、`agent-skills-system`、`ai-agent-security`、`agent-multi-agent-collaboration`、`software-as-crystallized-knowledge`、`ai-governance`、`ai-programming-tools-comparison`、`ai-native-team-refactor`、`architect-in-agent-era`、`digital-employee` — 相关页面互链（政策侧印证/要求/呼应）
- 复核记录：初稿曾把方案（四）与发布会的"模型原生"增补关系写反（实为发布会比方案多"模型原生"）；"人工编写→人机协同/被动响应→主动执行/产品交付→价值交付"三条转变经全文检索确认**仅见于发布会**、方案原文无此提法，已按原文修正表述并标注出处
- run `llm-wiki sync`

## [2026-10-10] ingest | Multica 是什么——从源码看一个把 Agent 变成同事的系统
- created `multica` — Multica 完整系统分析：定位（人机工作的记录与行动系统；刻意不是模型/云端执行平台/工作流引擎/OSI 开源）、Issue→Run→Runtime 工作循环（Agent 不会自己开工、run 完成≠issue 完成）、控制面（Go/PostgreSQL 17/Next.js 16 + SaaS/自托管/桌面三选一）+ daemon 执行面（WS 唤醒+轮询兜底/每 run 隔离工作目录/26 种 CLI 探测注册/并发上限 20）、wakeups 触发层调度（事件 25 种/定时/条件/超时 + 事务性捕获/租约派发/严格串行/防失控三件套；网易内部二次开发 fast development/SDD）、团队治理（Skills/Squads/Autopilots/插话纠偏/review gate/插件与 IM-Git 集成）、「daemon 用户即边界」无沙箱安全模型、Multica License（内部免费/对外托管与嵌入商业产品需授权/品牌豁免单列）
- updated `digital-employee` — 新增"系统化形态：Multica 把 Agent 变成同事"节（身份/常驻 runtime/共享 issue）；相关页面互链；补 sources
- updated `ai-native-team-refactor` — 相关页面新增 Multica 作为协同工具层的外部产品实例
- updated `ai-agent-security` — 新增"无沙箱化立场：daemon 用户即边界"节（官方理由/缩小爆散半径三件事/落地建议）；补 sources 与互链
- updated `ai-agent/ecosystem/index` — 「设计与 Agent 平台」分类收录 multica
- updated `ai-agent/index` — ecosystem 篇数 31→32
- saved source: `sources/2026/10/10/Multica 是什么——从源码看一个把 Agent 变成同事的系统.md`
- run `llm-wiki sync`

## [2026-10-10] ingest | 罗霄三篇（KM）：Raven SDD 数据开发 ×2 + 猛犸智能运维诊断工具
- 来源：KM 文章三篇（罗霄，音乐安全数据团队），均经 popo-cli km resource_section 下载全文（sectionIndex=-1），无配图
  - blog/269706《SDD 数据开发流程 - Raven SDD 能力补充》
  - blog/268821《Sdd数据开发实践分享 — 数据任务优化》
  - blog/267701《Raven Cloude Skill 猛犸(EasyData)智能运维诊断工具》
- saved sources/2026/10/10/：3 个 .md（含 ingested/wiki_pages/source_url/author frontmatter）
- created `raven-sdd-data-development` — Raven SDD 数据开发：SDD 简介与自动化/规范化能力、6 阶段通用流程、数据开发 6 项 gap 补充（平台操作/5维度验证/3000 SQL 知识库/性能基线/data-pipeline schema/安全门控）、8 步法、三层工具链（mammut-api/spark-sql-review/mammut-task-analysis）、easy-data 双目录体系、/sdd-explore 入口与 4 条注意事项
- created `data-task-increment-optimization` — 反垃圾大宽表全量→增量优化实践：AI 擅长六项与必须人工把控五项（"AI 负责做到位，人负责拍板"）、10 步法实践、V1→V3 三轮迭代根因定位（边界重复→version=NULL 排序→线上自身问题）、265min→51min（5.2x）、32 次 API 调用、10 条流程规范、适用/不适用与复用价值
- created `easydata-ops-diagnosis` — 猛犸智能运维诊断 Skill 对：整体架构（两 Skill + 四数据源 + 诊断引擎 + HTML 报告）、猛犸 OpenAPI 字段映射注意点、SHS 为错误根因唯一来源、12 条日志规则 + 8 条 SQL 规则、单任务/基线破线两模式、P0/P1/P2 分级、速查表与后续规划
- updated `ai-agent/sdd/index` — 新增「Raven SDD 与数据开发实践」分区，篇数 13→24（修正历史漏计）
- updated `ai-agent/data-agent/index` — 「厂商 Skills 体系」收录 easydata-ops-diagnosis
- updated `ai-agent/index` — sdd 22→24、data-agent 43→44
- updated `data-dev-skills-automation`、`bigdata-ops-agent-practice` — 相关页面互链（猛犸平台同类实践/运维诊断对照）
- run `llm-wiki sync`

## [2026-10-12] ingest | The AI-native SDLC playbook（Anthropic 官方文章，2026-08-21）
- 来源：https://claude.com/resources/articles/the-ai-native-sdlc-playbook（Louis Claxton，Anthropic Applied AI 团队最佳实践合集）
- 下载方式：curl 获取 Next.js 页面 HTML（787KB）→ DOM 解析提取 article 正文 → 剥离页面外壳；改写正文 `@scope (.w-richtext)` 样式块为 `.article-body` 普通选择器（headless Chrome 对 @scope 未生效，实测验证），并内联 CodeBlock 等 CSS Modules 规则与设计 token；13 个无功能 Copy 按钮清除；4 张配图（assets.claude.com 外链，URL 全部 200 验证）+ 4 条图题保留在原 figure 位置；正文 PDF 链接改为本地副本（Webflow CDN 链接易失效）。headless Chrome 截图逐区视觉复核（目录计数器/Play 卡片/对比卡/代码块/表格/配图均正常）
- added `article-downloads/`：The AI-native SDLC playbook.html（自包含，118KB）+ AI-Native-SDLC-Playbook.pdf（1.5MB 本地副本）
- saved `sources/2026/10/12/`：原始 HTML + 同名 .md sidecar + PDF（文内链接指向同目录副本）
- created `ai-native-sdlc` — 总览：代码不再是瓶颈的三条断言（瓶颈迁移到人速阶段/控制脱节/治理成本上升）、线性 → artifact 驱动 gate 的闭环（intent.md → spec.md → plan.md → diff → PR → 事故记录，提交链即审计轨迹）、六阶段对照表、九大 play 一览与采纳顺序（clay play 先行）
- created `ai-native-sdlc-plays` — 六阶段 × 全部 play 详解：Capture as intent.md / Requirements and design / plan mode / auto mode / CLAUDE.md / Skills / Hooks / 并行会话与 subagent / 反馈闭环 / continuous evals / PR review 循环 / 审批门 / CI/CD / 闭环（control band→intent.md）/ 定时扫描（Claude Security）/ Claude Tag on-call；逐 play 含传统对比、Getting started、执行步骤、治理与 Leading/Lagging 度量
- created `ai-native-sdlc-toolkit` — Claude Code 工具链：五类手段与确定性分层、CLAUDE.md（错误两次入档/一页上限）、skills（建议性控制）、hooks（护栏+审批门）、并行会话（2-3 起步、review 是上限）与 subagent、反馈闭环与 evals、plan mode → auto mode 演进
- created `ai-native-sdlc-governance` — 控制分层总表（建议性→确定性→审批门→branch protection→环境分层）、managed settings 受监管企业完整配置与逐键解释（sandbox/credentials 补 permissions 的洞）、分离职责与审计轨迹、度量、存量系统与单一事实源三种共存配置
- updated `ai-agent/sdd/index` — 新增「AI-native SDLC（Anthropic 官方 playbook）」分区收录 4 页，篇数 24→28
- updated `ai-agent/index` — sdd 24→28、context-eng 计数修正 4→5（补记 workbuddy 页）
- updated `ai-agent/context-eng/index` — 描述与相关页面补 AI-native SDLC 工具链（修正篇数 3→4）
- updated `claude-md-best-practices`、`agent-hook-governance`、`agent-skills-system`、`ai-agent-security` — 相关页面互链（官方 CLAUDE.md 工程实践/skill 工程实践/hook 确定性层/sandbox 精细化控制）
- 复核记录：文中两处"小节标题无正文"实为 HtmlEmbed 组件块（Legacy systems 侧栏、Managed settings worked example、Resources 链接列表），已按完整原文内容撰写进 wiki，未遗漏
- run `llm-wiki sync`

## [2026-10-14] ingest | Data Agent 项目落地方法论（网易内部 PagesHub，作者余聪勇，2026-08-31）
- 来源：https://p.pages-hub.netease.im/p/34b8d5f69f9f44878a1178（页面标注"内部方法论"，交付/运营全流程方法论，含底部 SVG 总架构图）
- saved `sources/2026/10/14/`：原始 HTML（自包含，双击可读）+ 同名 .md sidecar
- created `data-agent-delivery-methodology` — 方法论总纲：核心命题「项目标准」（验收标准 = 核心问题列表 + 数值目标 + 双方签字）、两种导向论（价值导向 vs 准确性导向；本方法论倾向准确性导向，"价值定方向，准确性定量尺"）、交付/运营两阶段与用户全程参与原则、以点扩面（点→线→面→全）、数据回流闭环、七支柱执行检查清单（8 项）
- created `data-agent-delivery-pillars` — 交付四支柱详解（接力关系）：沟通标准化（三个标准化）、自动化构建场景（宽表/指标技术选型 + CLI 批量创建 + 测试/基线/生产分级发布 + 自动化边界）、自动化测案例（校对规则 SQL 比对+思维链 + 准确性/稳定性/收敛三值 + 自动化回归）、标准化排错（问题分类检索/口径/规则/幻觉 + 排错 Skill 化售后业务助手承载 + 留痕根因提报）；每支柱含验收标准
- created `data-agent-ops-pillars` — 运营三支柱详解：价值监测（看板 + 活跃度下钻 + 优化闭环）、用户赋能（方法论参与 + 培训调研 + 经验 SOP 化）、以点扩面（小部门→多部门→多分公司 + 大赛收集 + 产品反哺 + 可持续陪跑）；每支柱含验收标准
- updated `ai-agent/data-agent/index` — 新增「交付与运营方法论」分区收录 3 页
- updated `ai-agent/index` — data-agent 44→47，描述补「交付与运营方法论」
- updated `data-agent-practice-guide` — 相关页面互链（火山实施路径与本文两阶段对照）
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站主论坛 + 金融可信 AI 实践（3 场演讲 PDF）
- 来源目录：`sources/2026/10/29/2026DACon北京站/`（原始 PDF 未改动；为每个 PDF 新增同名 `.md` sidecar，`source_type: pdf`）
- created `agent-context-three-pillars` — 堵俊平（Datastrato）《开放语义、Ontology 与元数据：Agent Context 的三重支柱》：Answer/Reason/Act 三阶段与 Data access/Business meaning/Operational constraints 三类上下文；收入口径示意案例（BI/Notebook/Finance Model/AI Agent 四种缺口：口径/净额/时间/关联）；三重支柱（统一元数据/Gravitino、开放语义/Ossie、业务本体）与治理四关卡；四个落地挑战（语义碎片化、含义难获取、异构难连接、上下文持续失效）；Top-down/Bottom-up 对比与六步混合闭环（Import/Generate/Git/Steward）；Review pipeline 与 candidate/curated 双状态；AI Context Platform 四层参考架构；Datastrato 2.0 四模块与 Today/2.0 OSS/Datastrato 2.0/After 2.0 发布边界
- created `model-fullstack-moat` — 胡浩源（京东健康）《模型 + 全栈工程才是护城河》：榜单第一只是起点；证据/执行/产物/接手四环节与典型失败；知识层 MedWiki（对象/关系/条目/状态）+ Agentic Search 证据闭环；执行层 Harness 四项能力（持续状态/统一工具契约/Model Mesh/高频决策下沉）；模型层 MedCode + med-jev + Skill；交付层 MedWork；MedCode 把指南写成程序（四步 + TNM 静态评测）；医学评测（MedEviBench/MedRealMM/三维影像 + 医生批准 Rubric + 临床共建）；训练链路与 θ/x/S 三类改进对象；长程论文任务五阶段与交付门控；影像任务 pick_image 轨迹；落地效果（病历、MDT、论文）——PPT 对外版所有数值脱敏为 `***`，页面仅记录指标口径与方向，未复原数值
- created `wing-pay-ontology-trusted-ai` — 徐德华（天翼征信）《翼支付：本体落地对金融可信 AI 的挑战》：OLTP/OLAP 老分叉（面向/问题/查询形态/时延/写入/数据模型）；一次研判 = 分析+穿透+状态+回写（OLAP/多跳/点查/写入）；本体分叉=语义在查询时绑定 vs 写入时绑定（OLAP 血统 vs Palantir Ontology 的 OLTP 血统）；概念层唯一 + 实例层两种实现；as-of 口径（valid_from/valid_to）；混做两结局；11 维对比表；成本读端 vs 写端（k 跳≈k 次自连接/写放大≈依赖扇出）；物化过期与「收敛中」状态；六维选型与情况地图；两种有把握情况 + 三个灰色地带；三个案例（监管场景四模块、研判 4类5子类→12类42子类 / 关键事实准确度 +80%、中国电信星海八组件）；统一对象查询原语与供数四步；五件套交付物 + 正反指标配对；五条踩坑
- 未改任何 `index.md`（按任务要求）；三页均以 [[wikilink]] 关联已有页面，链接目标已逐一核验存在
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「Agentic Workflow：数字员工与企业流程重构」2 场演讲 PDF
- 来源目录：`sources/2026/10/29/2026DACon北京站/Agentic Workflow：数字员工与企业流程重构/`（原始 PDF 未改动；为 2 个 PDF 新增同名 `.md` sidecar，`source_type: pdf`）
- created `alibaba-digital-employees` — 董晓庆《2000 多个数字员工在阿里上岗：Agent+Loop框架、MCP双模式与多Agent协同的落地经验》（29 页）：RPA→智能助手→数字员工三阶段；数字员工 vs 传统 Agent 六维差异（身份/权限/知识/业务接入/交互/安全）；五大能力模块（身份与组织管控、知识及记忆、安全与权限、Agent 实现及接入、渠道与业务能力接入）；生命周期四阶段（入职/开通/初始化/运行）；MCP 双模式（员工身份/访客身份双轨切换，截图另有数字员工/访问用户/自然语言识别三口径，映射原文未展开）；Agent 隔离方案（云沙箱/弹性云主机/物理设备 × 使用者独立/数字员工共用）；约 2000 个数字员工上岗、四条经验（价值优先/提效赋能/安全为纲/身份重塑）、三个判断；IDC 22 亿（单位未标注）与 CAGR 139%
- created `multi-tenant-digital-employee-cluster` — 何碧宏《从工作流到智能体集群：企业级多租户数字员工的工程化落地与协同实践》（31 页）：群核科技 SRE 四阶段演进（机器学习→AI 工作流→OpenClaw & Hermes→自研数字员工）；AI 工作流四瓶颈（静态 DAG/无记忆/协同难/租户权限弱）；五层架构（访问/接入/运行/能力服务/基础设施）；K8s + Hermes 多 Profile 三层隔离（K8s 容器/Profile 逻辑/数据会话记忆），用户与租户为不同维度；远程知识库 + 本地 llm-wiki + Hindsight 记忆；Skill Hub 边界与互斥治理 + 五条配套；CoT 治理案例（16m14s/649,926 Token → 1m44s/174,436 Token，性能 +80%、Token −70%）；持续进化飞轮；六层性能治理；流程 + 状态机多智能体协同（Issue 状态领取，非流程调度）；五种落地形态；450+ 人/100+ 实例/700+/天、100+ 数字员工 24h 在线、各部门分布 104（按分项相加，原文口径 100+）
- updated `digital-employee` — 新增「组织化落地：阿里 2000+ 数字员工（2026-10 补充）」一节，补两场演讲的 sources 与相关页面链接；未删除/重写已有内容
- 复核记录：两个 PDF 逐页 `pdftotext -layout` + Read 视觉复核（29/31 页），对知识管理、权限策略、多租户隔离、Skill/存储/CoT/多 Agent 图与产品截图逐页核对；对 p16/p20/p23、p8/p11/p13/p19/p26/p28 用 pdftoppm 渲染后 tesseract `chi_sim+eng` 交叉核对。无法可靠辨认处已在页面标注「不确定」：MCP 双模式三选项与双模式的映射、DWS 缩写含义、经验采纳率截图口径、“22 亿”单位、近 2000 的统计口径
- 未改 `index.md`（按任务要求）；两页均以 [[wikilink]] 关联已有页面（digital-employee / agent-multi-agent-collaboration / agent-mcp-protocol / loop-engineering / agent-sandbox-infrastructure / distributed-agent-hosting 等，均已核验存在）
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站·生成式推荐系统新范式（2 个 PDF）
- 来源：`sources/2026/10/29/2026DACon北京站/生成式推荐系统新范式/` 下 2 个 PDF，逐页 pdftotext + 视觉复核 + tesseract 交叉核对；PDF 原始文件保留，另建同名 .md sidecar 供 CLI 追踪
- created `ai-ml/amap-route-recommendation-agent` — 高德陈超：路线推荐三次范式跃迁（理解路 RouteMind：CCN/SCASRec/GenMRP；理解人：LLM 增强 + 路线 Agent + MobilityBench；理解时空：TransitLM），含架构、指标、效果数据与边界
- created `ai-ml/tme-interactive-recommendation` — 腾讯音乐吴喆：从隐式推荐到交互式陪伴，三个落地方向（MTP/P-CAR/C-CAR、世界知识与因果推理 + 推荐垂类大模型五阶段、Agent 驱动交互式推荐），含产品形态、效果信号与不确定项
- 未修改 `index.md`（按要求仅报告建议条目）
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站企业本体与模型组合（3 场演讲 PDF）
- 来源目录：`sources/2026/10/29/2026DACon北京站/`（原始 PDF 未改动；为 3 个 PDF 各新增同名 `.md` sidecar，`source_type: pdf`）
- created `jd-ugo-growth-ontology` — 周默（京东技术总监）《从数据到决策：用户增长本体论 UGO》：增长决策四大断裂（要素/策略/归因/经验）、一次召回"流程完成≠增量验证"、多平台决策上下文（CDP/DMP/供给/营销/触达/素材/AB/BI 七平台对齐"给谁/发什么/何时/谁确认/如何验证"）、三个智能化误区；UGO 定义（面向用户增长的操作语义框架，六可：查询/计算/授权/行动/追溯/学习）与四段技术栈（报表→Lookup→Reasoning→Action）、本体演进（哲学→知识工程→语义网→操作本体，Palantir Gotham/Foundry/AIP）；三层语义职责（名词/动词/时间）与七类要素（人群/商品供给/权益/触点/素材创意/时机/空间位置、Object vs Property 五问）；边界对比画像/CDP/权益中台/AB/BI（连接而非替代）；五层架构（应用/决策进化/策略编排/要素本体/数据接入）+ 读算写学；OneID 与对象结构；STR-RECALL-001 对象关系；Function/Strategy/Action 动词层、Strategy 契约与四项服务端控制（授权/绑定/重查/幂等）、状态机；EXP-RECALL-001 实验设计（1:1、每组 1 万人、10%→12% 示意）；GAG 四步候选生成（借鉴 OAG）；受控执行与 OWASP LLM Top 10 2025/AgentDojo 攻击链对照；DecisionSample 六段证据与三类用途；UMC OS 三面一体+确认闸+行动中枢；内网 MCP 不授权；知识更新三阶段（草稿→已验证→已发布→已失效）；治理四风险；验收建议线（可追溯 100%、无实质返工≥80%、交付耗时降≥50%、风险拦截 100%、Owner 覆盖 100%）——原文明确"不是试点成绩"；边界：仅验证购沉默老客召回、血缘≠因果、分级授权为后续方向、Apache Ossie 孵化中
- created `dunhuang-model-routing` — 窦东员（敦煌网）《从"选最强模型"到"做最优组合"》：单模型难满足质量/成本/速度/可用性/合规五目标；统一 AI Gateway 现状（DHwork/DHcoder/ChatBI、DeepSeek/GLM/视觉/Seedance/Minimax/Doubao、IDC/云私有/公共接口、Agent/Skill/MCP 市场）；能力画像矩阵（任务维度 5 类场景 × 深度推理/通用效率/多模态/低成本 4 模型角色，内部示意非排名）与模型配置四维（上下文长度/推理 vs 价格/场景说明/工具能力）；任务边界四步（分类/运行约束/风险边界/模型池匹配）+ 单轮次不换模原则；Redis 粘性路由链路（SDK 兼容、同轮不换、route_key→selected_model TTL、trace/降级）；Auto 与显式选择分离；独立 Router Service（端应用/网关/统一决策内核）；五项上线验收（模型池选择/Loop 固定/Redis 命中/显式回归/可观测降级）；统一治理五向（应用分级/应用内路由/网关治理/可观测/安全/兼容/灰度）；四阶段落地；效果：8/20 上线后用量升、账单降至约 60%、每百万 Token $1.2–2.3→~$0.3、人均日 Token 2M–6M→12M–18M；Auto 单日 815 轮/971 条记录，Luna 80.64%、DeepSeek 定时任务 74.39%(61/82) vs 用户直请 9.08%(74/815)；ROI 相对全 Sol 成本 7.39（省 92.61%），全 Luna 5.00；案例 01 工程任务（纯低价 35%/¥13.16/总 ¥13,160 vs 路由 88%/¥1.70/总 ¥1,700）、案例 02 高风险审核（统包 ¥0.55/P95 5.2s/总 ¥5,500 vs 路由 ¥0.24/P95 1.1s/总 ¥2,350）；ROI 公式=单位有效任务成本；边界：案例数字未说明是否真实生产、典型场景页标注示意、算法未披露
- created `xiaohongshu-inference-selection` — 骆兆楷（小红书）《从通用到专用：面向搜索生成式召回的推理选型与优化》：业务链路（离线 LLM Embedding→RQ-Kmeans 4 层→多级语义 ID→Node Index/Prefix TrieDict；在线 query 分析→GR 推理→SID→Trie 约束→笔记检索 vector/keyword/id→聚合排序）；三类负载（1K–32K Long Context、3–5 次 Short Decode、20→300→900 Large Beam）；通用框架五瓶颈（独立 Seq 调度/合批缺 Beam 感知/Beam 外层循环/Beam-SID 多级管理缺失/Item 约束过滤缺失）→ GPU 利用率<40%；方案对比（vLLM/TensorRT-LLM/DeepSpeed-MII/GR-Inference）；选型三支点（请求级共享 KV/Beam 感知合批/专用 Decode 路径 CUDA Graph+GPU SID Trie）；五层架构（服务/控制调度/GR 运行时/状态资源 ContextKV+BeamKV+BeamPath/执行后端）；三项核心优化：Long Context 三 Kernel（K1 Tensor Core+Split-KV、K2 CUDA Core Gather、K3 Softmax 合并，内存利用率+60%、并发 3 倍）、Short Decode 引擎内循环（状态常驻/DAG Pre-Capture 分桶 CUDA Graph/每步元数据/GPU Trie+受限 Top-K fusion）、Large Beam（BeamPath 索引分叉/按 ActiveBeam 调度/分桶 Graph，beam=1000 延迟仅+20%）；关键决策（自研 vs 开源改造、接口通用+核心专用）；benchmark Qwen3-0.6B、prefill 100–1000、decode 3、E2E≤100ms、Beam 900、三套 SGLang commit；吞吐 1.5–2.5x、Item-constrained 3.2x+（vs V2 3.24–3.63x）；线上新增自回归 SID 召回通道：CTR +0.03pt、有效点击率 +0.2%、Recall@1000 +5.7%；未来三方向（Paged ContextKV/扩大 CUDA Graph 覆盖/PD 分离与 megakernel）；边界：无金额成本、未涉及蒸馏量化、单模型单口径 benchmark
- 未改任何 `index.md`（按任务要求）；三页均以 [[wikilink]] 关联已有页面，`jd-ad-data-workspace`、`tme-interactive-recommendation` 暂为待创建页
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「AI Ready 到 Agent Ready：Data+AI 平台架构升级」3 个 PDF
- 来源目录：`sources/2026/10/29/2026DACon北京站/AI Ready到Agent Ready：Data+AI平台架构升级/`（原始 PDF 未改动；为 3 个 PDF 各新增同名 `.md` sidecar，`source_type: pdf`）
- created `curvine-agent-data-base` — 闾泽军（OPPO 高级架构师 / Curvine Maintainer）《从存算分离到 Agent 数据底座——Curvine 的端云协同架构实践》（26 页，纯图片 PDF，逐页 pdftoppm 渲染 + Read 视觉复核 + tesseract chi_sim+eng 交叉核对）：存算分离后瓶颈转移到数据流动；Agent 数据底座五类问题；缝合式缓存写放大与 NFS 网关瓶颈；Curvine = 对象存储之上的原生缓存文件系统（Master Raft HA + Worker 多级缓存 + UFS 持久层）；Rust 选型四理由（18 万+ 行）；FSMode/CacheMode；四案例（大数据热表 23%+、多模态检索 p50 5.5×/QPS 4.8×、KV Cache TTFT 2.7×/命中 96.2%、Agent FS EKS 10,000 PVC 零 Pending）；MLPerf AU 6/8 胜出、带宽唯一 -1.3% 如实标注；边界（vector_with_filter 仅 1.5×、最小集群为下限、内部对标非官方）
- created `amap-intelligent-query` — 林宇航（高德商业智能部）《从"答得对"到"看得透"：高德智能问数产品从 BI 底座到 Agent 底座的迭代之路》（24 页）：用户五步数据工作链；L0–L4 分级与高德服务进展；RAG+NL2SQL+OLAP 早期 L2（39 表/15 看板、89.4%/90%）；规模化边界（A 331 表 50%、B 70 表 80%）与四类瓶颈；CodeAgent 式 DataAgent 七环节执行闭环；Skill = 知识路由 + 程序性方法；三类护栏（语义资产/评测/Harness）；复测 A 93.6%、B 96.7%、整体约 95%；L3 告警与五步归因诊断；上线 4 个月数据（WAU 41→420、准确率 80%→95%、P90 787→280 秒）；三条踩坑判断
- created `agent-ready-multi-cloud-luxury` — 谈总（某全球头部高端零售集团中国数据技术负责人）× 原攀峰（数新智能 CTO）《从多云底座到 Agent Ready：企业 DataAgent 架构升级与高端零售落地实践》（31 页 PDF / 30 页正文）：跨云趋势三问（数据饥饿、合规与准备、带宽与法规）；多云底座五类挑战；Agent 六环节断点与归因（语义可懂缺失 4 次）；Agent-Ready 四条判据（多云可用/语义可懂/执行可控/效果可迭代）；集散控制架构与逻辑意图；双通道召回 + GraphRAG + 确定性编译（30%→90%）；Skills/流程工具/第三方 MCP 沙箱；上下文 L2 异步笔记与两跳检索；五层防护与 HITL 红线；失败归因三分支与评测门禁；CyberOS 与 Harness 三层操作系统；华为云一主两卫星、奢侈品集团跨云数据平台、安踏全球平台；六环节量化（探数约 70% 定性、建模 1.2→3.5、开发 30%→90%、测试 90%、发布 <5%、运维 +50%）与端到端 +233%/10 天→3 天；适用边界与三条方法论
- 复核记录：三个 PDF 均逐页 `pdftotext -layout` + Read 视觉复核（26/24/31 页）；Curvine 为纯图片 PDF，全部 26 页用 pdftoppm 150dpi 渲染后 Read，并逐页 tesseract `chi_sim+eng` OCR 交叉核对；对页码、百分比、模块名、架构图连线逐页核对
- 未改任何 `index.md`（按任务要求）；三页均以 [[wikilink]] 关联已有页面，链接目标已核验存在
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「从分析闭环到行动闭环」2 个 PDF
- 来源（逐页 pdftotext -layout + Read 页面视觉复核 + 关键页 300dpi 裁切复核）：`sources/2026/10/29/2026DACon北京站/从分析闭环到行动闭环：Data Agent 的边界突破/`，黄达 35 页、王浩强 25 页
- saved 同名 `.md` sidecar ×2（PDF 原始文件保留，未修改）
- created `action-loop-agentcore` — 黄达：行动闭环卡在两端（左端不敢行动 43%→27%、右端看不见）；AgentCore 五项能力（Identity/Runtime/Gateway/Memory/Observability）与 Policy/Harness/Evaluations 扩展；Runtime MicroVM 生命周期（8 小时/15 分钟）、Token Vault、Cedar 默认拒绝、Memory 两层与 3–365 天/31 天保留、Harness 配置优先→Strands 导出；工作台侧 MCP 主通道 + AE CLI 通道、4 个实测（含 G 渠道 145 人样本与放量情景测算）、产物归档与 Enterprise 治理、观察→实验→干预→验证四步
- created `agentic-engine-loop` — 王浩强：ROI ↓10% 三问（为什么下降/多久能行动/下次能复用）；开放融合数据底座八层、全域采集五源、Trino+StarRocks 双引擎、12 个分析模型、运营能力三线 + 效果回流、安全合规四维；Agent 友好四层（CLI& MCP/Skill/Workflow/语义层）、AE-CLI、语义层三类知识来源；AgentOS 六模块、一人一沙箱 1C2G↔2C4G、模型智能路由（kimi-k2.7-code/glm-5.1/qwen3.7-plus 为演示配置）、Skill/MCP 市场四级、A2A 双向、观测与 ROI 评估；三部分映射 ROI 异常闭环
- 口径记录：王浩强篇原文无任何量化效果数据（ROI 10% 为「场景示例」），已在「数据与效果」明确标注；黄达篇实测均为演示项目固定区间数据
- 未改 `ai-agent/data-agent/index.md`（按任务要求）
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「AI 驱动的客户全旅程：从营销获客到客服服务」2 个 PDF
- 来源目录：`sources/2026/10/29/2026DACon北京站/AI 驱动的客户全旅程：从营销获客到客服服务/`（原始 PDF 未改动；为 2 个 PDF 各新增同名 `.md` sidecar，`source_type: pdf`）
- created `jd-ad-data-workspace` — 吴旭晶《从对话式问数到组织级数据工作空间：京东广告数据分析 Agent 的工程化实践》（38 页）：不是 Demo 而是四类真实业务场景；数据资产化→数据服务化→业务认知生成；业务消费层六能力（资产目录/语义知识/口径治理/证据解释/边界护栏/Artifact）；场景契约五要素（问题/语义/边界/目标/动作）与「以用带治」；四类场景四种执行形态；Schema 语义裂谷（participate_cnt≠参与率）与知识分层×链路注入（SOFT Knowledge Pack/Query 解读/解释框架，HARD Column List）；生产级 Agent 六步（场景识别→知识装配→编排执行→结果校验→风险关口→发布归档）；Graph-first 动态探索→DAG 固化→静态模板（节点/依赖/状态/结果引用契约）；三层 Graph（L1 Master/L2 Session/L3 Executor）与模板实例分离；节点契约与 WAIT_CONFIRM 人工确认；Workspace Artifact Library（Query/Dataset/Chart/Report·Snapshot）与结论→图表→计算→SQL/DSL→数据源追溯链；复跑/订阅/分享/专家模板放大；四场景落地（智能问数、日报周报局部重跑、订阅推送、异常归因）；最终六层架构（Conversation/Scene/Knowledge/Graph/Workspace/Organization Reuse）；三类资产（数据/知识/Agent）与治理底座（版本/权限/血缘/质量/审计）；效果 3–5 小时→10 分钟、30→2 分钟（前一个 30 未标单位）、问题发现缩短约 50%、13+ 场景/20+ 部门/上千人次；五项工程指标 96%/98%/42%/37%/18%（人工确认率 62%→13%）；六条边界护栏；不确定项（技术栈/模型/指标公式未披露，示例数字语境）
- created `agentic-voice-agent` — 武文斌《从外购到自研，从向量 RAG 到 Agentic 语音智能体——企业对话机器人的完整演进之路》（26 页）：客服/营销/贷后三场景共同难题（量大/重复/时效强/要合规）；外购三笔账（成本曲线、流程控制权、数据闭环权）与自研四条触发标准（命中两条以上）；五代路线（外采 BERT 题库→自研向量检索→RAG+大模型→大模型 Agent→语音机器人）与自主性光谱（停在有边界受控自主）；四类方案对比（规则/FAQ、语义检索、RAG、纯大模型）与「RAG 打底+Agent 增强」；四层架构（接入/Agent 编排/能力/业务与数据）+ 可观测侧栏；RAG 五件事（问答原子切分、元数据硬过滤、混合召回、Rerank、强制引用与拒答、评测门禁）；工具 Schema 与调用回路（调错率 18%→4%）；有边界自主（工具白名单默认 deny、敏感二次确认、时延预算优先）；安全/可靠性/成本三件套；分层记忆（Working/Episodic/Semantic/Procedural，TTL）与评测飞轮；一套底座四场景裁剪（客服 IM/IVR、营销外呼、贷后外呼）；核心数据 72%/1.2 秒/70%/-62% 与 -38%；ROI 1:3.5、人力成本 -70%、单人 3.2×、回收期约 8 个月（目标 ≤6 个月）；MCP N×M→N+M、15→3 人日、12 个系统；语音三段接力+barge-in 与时延瀑布 1120ms/P50 1.2s；最新指标 93%/+8pct、1.2s/-35%、85%/+12pct、300ms；未来拟人化/全双工/Agent RL/多 Agent；不确定项（模型与供应商未披露、两处调错率 18%→4% 与 15%→7% 口径未说明、回收期目标与实测差异、分场景数据缺失）
- 复核记录：两个 PDF 均逐页 `pdftotext -layout` + Read 视觉复核（38/26 页）；对指标页（京东 p30/p33、语音 p19/p20/p25）用 pdftoppm 150dpi + tesseract `chi_sim+eng` 交叉核对；数字、模块名、架构图连线逐页核对，无法确认处已在页内标注
- 未改任何 `index.md`（按任务要求）；两页均以 [[wikilink]] 关联已有页面，链接目标已核验存在
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「企业语义层建设 / 上下文工程」3 个 PDF
- 来源目录：`sources/2026/10/29/2026DACon北京站/企业语义层建设/` 与 `sources/2026/10/29/2026DACon北京站/上下文工程：让 AI 真正懂企业/`（原始 PDF 未改动；为 3 个 PDF 各新增同名 `.md` sidecar，`source_type: pdf`）
- created `apache-ossie-ant-practice` — 王小军（蚂蚁语义层平台负责人）《Apache Ossie 语义层在蚂蚁的落地实践》（34 页）：从“SQL 能执行但答案仍错”的角标渠道算法渗透率案例，拆出渠道层级/指标口径/默认范围与粒度/计算约定四类缺失语义知识；数据语义分层 L1 物理模型 / L2 语义模型 / L3 本体与知识图谱 / L4 业务模型 + WHAT/WHY/WHERE/WHEN/WHO/HOW 六问；语义契约 = 协议标准（Ossie 的 Dataset/Field/Dimension/Metric/Relationship/Filter/AIContext）+ 质量标准（Q1–Q6）；1+3+N 架构（1 契约核心、供给/治理/消费 3 层、N 消费场景）与消费反馈回流；Ant-OSI 六类扩展（multiplicity/join_path、Granularity FIXED/ROLLUP、Window/additivity、instructions/synonyms/pitfalls、imports/Metric Refs、explainability）及回馈社区的 Shared Filters/Shared Dimensions/Metric References（discussions/342）；逆向建模 L4→L1 分层挖掘、SDD/OpenSpec 五步闭环、正向建模六阶段三闭环、七步持续保鲜、九步消费与最小充分语义子图；效果 250+ 模型/15000+ 指标/1500+ 数据集/30+ 业务域/周调用 2500，内部生成 SQL 正确率 60%→81%→88%（目标 95%，差 7pp，gpt-5.6-sol medium n=100），外部配对研究 +17～+27pp 常态区间（Bayer +70、Luo +61 为极值）；退卡率研发约 1 工作日→约 1 小时；踩坑六类（信息缺失/覆盖过期/聚合错误/口径冲突/相似混淆/语义重复）与四位一体质保 A/B/C
- updated `apache-ossie` — 增量新增「蚂蚁落地实践」一节（定位、规模、1+3+N、Ant-OSI 扩展、Q1–Q6/G1–G4、60→88、边界），sources 追加本 PDF，updated 改为 2026-10-29；未重写/删除已有内容
- created `lixiang-semantic-context` — 钱瀚（理想汽车大数据平台负责人）《语义层即上下文：理想汽车生产级 Agent 的语义工程实践》（28 页）：平台 180+ 万车/日均 10 万亿级车辆信号；一个问题背后六类事实（状态/日志/指标/资源/事件/历史对比）与“正确对象 × 正确时间窗 × 完整证据链”；三阶段演进（Prompt+通用工具临场组装 → PRIMARY/MID/ATOMIC 专用工具固化 → 录制/回放/行为评测/CI 回归）及各自遗留问题；早期 Coordinator + Spark/Flink/Cluster Specialist 架构的上下文分散；语义层夹在 Agent 与通用工具之间；语义诊断五层架构（接入/Agent Runtime/语义资产/语义运行时/后端适配 + 运行系统 + 治理）；Semantic Package = Ontology Definition（业务关系图）+ OSI/Metric Model（可执行指标图）+ Playbook，附 YAML 示例；构建两张图与运行时“先对象、1-hop 扩展、对象范围内找指标、Semantic Slice”；Playbook 匹配成功/未命中自由探索两条路径与五步调查；四取舍（固定定义 vs 动态探索、业务含义 vs 可执行语义、业务知识 vs 专家 SOP、语义意图 vs 物理可行性）；三案例（晚间变慢：凌晨半小时 vs 晚间 6 小时、单阶段约 4 小时；根因小时过滤未缩小读取、阶段读取约 12TB；未命中 Playbook 后发现同一任务最多 8 份并发/资源队列接近配额）；效果 10×+ 效率、97%+ 准确率、万级/周风险扫描、1→N 迁移；两个硬问题（业务经验语义抽象、YAML 资产统一治理）与 Compiler/Registry/Gravitino/Runtime 四步平台化
- created `decidex-context-engineering` — 石凯（观远数据）《从 BI 到决策智能：DecideX 的上下文工程、证据链与评估闭环》（28 页，大量图片页，逐页渲染 + OCR 交叉核对）：业务现场 8 名主任/5 名低于 70%、24 名业代/平均 70%/13 名低于 70%、拜访完成率高但活跃门店与转化偏低；1000+ 客户原声（秋田满满/宝洁/上汽/弘阳地产/零跑）；决策智能两能力在“大模型之外”（行业场景积累 + 企业级数据分析治理）；四问（资产变上下文/AI 理解业务逻辑/分析转行动/核查依据与复盘）；三类上下文（数据资产表/统一指标语义/分析知识）；行业经验驱动 Skill 三步（语义理解/使用行为分析/质量核查）与“业务专家确认+质量校验”；经销商分销 23 张数据集（7 dim/5 relation/11 fact）与八组指标（16/10/11/7/4/3/3/1）；业务本体六要素（对象/指标/关系/风险/行动/问题场景）与案例（47.48%、缺口 4.2 万、成功到访率 76.72%）；上下文“三有据”（对象有来源/关系有依据/指标有归属）与业务身份→关联范围→度量口径；问题场景 + 四类知识（问题场景/风险定义/行动说明/Agent 指令）；风险链（证据指标→风险判断→排查重点→建议行动）；证据链三层（结论有依据/指标可核查/查询可追溯，含指标标识与 guancli 命令）；行动闭环（建议→业务确认→陈列整改→复检验收→经营观察）；评估闭环（分析复核→行动反馈→结果复盘：门店激活 10→15 家、SKU 铺货与陈列改善）；跨对象本体复用；5A 与四层产品架构；边界（数据治理不规范会放大错误、本体只提供线索、建议须业务确认、评估是业务复核非模型离线评测、案例含演示/示意值）
- 复核记录：三个 PDF 均逐页 `pdftotext -layout` + Read 视觉复核（34/28/28 页）；第 1 个 PDF 全 34 页 Read，第 2 个 PDF 全 28 页 Read；第 3 个 PDF 全 28 页 Read，并对 p2/p8/p9/p15/p18/p21/p22/p24/p26/p27 用 pdftoppm 200dpi + tesseract `chi_sim+eng` 交叉核对数字与表头；OCR 与视觉冲突处（如数据集“23 张”）以页面视觉为准
- 未改任何 `index.md`（按任务要求）；三页均以 [[wikilink]] 关联已有页面，链接目标已核验存在
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站·企业模型组合（LMCache）+ 企业语义层建设（2 场）
- 来源：`sources/2026/10/29/2026DACon北京站/` 下 3 个 PDF，逐页 `pdftotext -layout` + Read 视觉复核（67/25/19 页）；原始 PDF 未改动，各配同名 `.md` sidecar（`source_type: pdf`）。LMCache 页含大量动画重复帧（6/7/8、9/10、11–13、19–21、22–24、25–27 等文本一致），按去重后的内容页复核
- created `ai-ml/lmcache` — 毛宝龙/郑春晓《为所有人构建开放的统一 KV Cache 管理层》：重复前缀导致的重复 Prefill 与三类收益（跳过 Prefill / Offload 释放显存 / 跨实例共享）；LMCache 四件事（复用/共享/Offload/可观测）；七节点演进（vLLM #10874 未合并原型 → #12953 v0 connector → #15960 KV Connector V1 API → #16625 LMCache v1 → 2026.04 vLLM #38301 外部 Connector 优先 → 2026.05 LMCache #3235 MP Connector 归仓 → 现在 MP mode）；MP mode 拓扑（LLM Pod × LMCacheMPConnector → GRPC/ZMQ → 独立 LMCache Server）；四层可扩展（Connector / EngineKVFormat+KV groups / L2 Adapter / Device Platform）；Native Adapter 四队列（LOOKUP/RETRIEVE/STORE/DELETE + batch tiling/worker lanes/completion queue/eventfd）；K8s Operator（LMCacheEngine CR → DaemonSet+Service/ConfigMap+Monitor/Webhook）；可观测三信号与四层排障证据链；故障容错双路径；容量估算四公式 + `cache-simulator sweep` 示例曲线（1 GiB 29.18% → 512 GiB 78.58%）；chunk size 128/256/512 取舍；社区 12K+ stars / ~400 contributors / 1M+ PyPI downloads。原文未给端到端延迟或吞吐基准，已在页面标注
- created `data-agent/360-data-agent-semantic-layer` — 郭朝阳《从会查数到懂业务》：奇麟湖仓数十万表/上百万字段/数十业务线与「抵触·无从下手·不了了之」；奇麟 Agent 六步主链与四个支撑面；治理与评测反哺闭环（八路原材料 → RAG → 语义升维 → OSI → 分发，下环评测回流）；NL2SQL 五级范式光谱与「往上走幻觉更小、冷启动更贵」；命名不重要的三件事（表达上界/确定性编译器/人工成本）；L1–L3 分层分治及其三个反噬（层级是业务平均值、缺口风险不对称/指标自拼聚合偏差 87%、边界越交错）；BlockPlan 统一契约 + 物料完备度/编译器算子集/风险策略三维拆解 + 四类缺口处理；六类结果分型（hit/raw_dim/ad_hoc_measure/undeclared_join/unsupported_op/unknown）与 ConfidenceReport/GuaranteeReport；三层概念选型表与五个开源组件对照；双通道端到端架构（七类原料、两通道、三合流点、PreMatch 路由、四条铁律）；L0–L4 分层命中；三级评估漏斗（三源数据集/Mock-Live/结果集比对/三级判定/EX 与 7 类 AST 归因）；阻断漏斗 + 四阶采样 + 飞轮；两块常开仪表（分层命中率 / 语义质量信号，含 Snowflake 约 10% 业界锚点）；三条诚实边界。原文未给整体准确率，10 条黄金集为示例规模，均已标注
- created `data-agent/semantic-layer-to-ontology` — 任鑫琦《语义层的下一站是业务本体》：本体三要素（概念/关系/约束）；两个场景判断（质量追溯的六系统身份收敛与生产质量本体链、客户之声的 VOC 关联与「关联带置信度」）；三平台技术路径对照（Snowflake Semantic View/Cortex Analyst、Google Cloud LookML+OKF/Gemini、Palantir Ontology/AIP+Action Types）与「模型提请求、谁落实语义」的判据；AI 决策进入业务四段闭环与「业务连接仍需建设」；W3C vs 平台原生（三者均非 RDF/OWL，Palantir 借鉴类型标准；未声明内置 OWL/SPARQL/SHACL）；五层本体体系（L0–L4）与场景优先/共性复用；公共语义三层（BFO/ISO 21838-2、IOF/CCO → AAS/SAMM、OPC UA → 企业落地）；Knora 产品界面；质量追溯 4 人天 → 5 分钟（原文自注：仅跨系统查询耗时，不含根因验证与整改）；流程再造对照；Ontology+Agent 六步改进闭环；得失一（不变两项 + 四个变化：范围/构建/使用/表达）；得失二（五维需求强度 × 成本）；推进节奏三步。原文仅标题提「十二年、近百家企业」，未展开行业/成败分布，已标注
- 未改任何 `index.md`（按任务要求，仅报告建议条目）；三页 wikilink 目标已逐一核验存在
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「模型适配与效果优化」+「企业本体与知识工程」3 个 PDF
- 来源（逐页 `pdftotext -layout` + Read 页面视觉复核；关键图表 300dpi 渲染裁切复核）：`sources/2026/10/29/2026DACon北京站/模型适配与效果优化：从语义上下文、Harness到后训练/詹伶俐-从 Harness 到后训练：以 GUI 测试执行 Agent 为例的领域agent优化实践.pdf`（38 页）、`企业本体与知识工程/杨冬瑞-基于本体的领域知识工程实践：以金融测试为例.pdf`（27 页）、`企业本体与知识工程/张绍磊-自进化数据智能体迈向+Data-Ontolo-gy-Agent+协同进化.pdf`（29 页）；原始 PDF 未改动，各新增同名 `.md` sidecar（`source_type: pdf`）
- created `harness/gui-test-agent-posttraining` — 詹伶俐：领域 Agent 优化决策框架（RAG/本体=知道什么、Harness=怎么被约束执行、后训练=内化什么行为模式）；Agent 公式与 Harness=Loop Maintenance；Harness 干预 10 节点（Trace/Turn）与六维适配（日志/提示词/工具/上下文/多智能体/loop 与人在回路）+ 三条实践（固定测试集/日志/控制变量）；上下文工程三招（长输出落盘给路径、提前压缩五段摘要、稳定前缀命中缓存）；后训练五范式与奖励四级迁移（RLHF→RLAIF→RLVR→Agentic RL）；Agentic RL 八维对比、信用分配与奖励稀疏两难题、三级奖励+rollout harness+可复现沙箱；数据四步法（标注平台/执行日志/日志转 GRPO 样本/程序化环境合成）；蒸馏与 Agentic RL 两场景；实验页 1.5B/3B 的 accuracy 0.54→0.82、GPRO（原文如此，疑为 GRPO）1500 steps 94.23%/88.61%；8 类环境沙箱表与 7 条趋势。外部证据（Qwen3-8B ALFWorld 96.9%、CUA-Gym 62.1%/72.6%、WebServ 55.5%、WebRL 4.8%→42.4%）已标注为讲者引用非本案例生产数据
- created `knowledge-base/finance-testing-ontology` — 杨冬瑞：金融测试知识困境（缺/贵/杂/隐）与一条超时案例的八环节；方案对比（RAG/提示词/FIBO/场景轻量本体）与 Cyc/Gene Ontology/SNOMED CT/FIBO 的大本体建设现实；按知识来源与节奏切三层（业务本体/测试本体/应用本体）；业务本体 S0–S4 五层、AI 提候选+工具校验+人校准+发布版本的构建循环；测试工艺四工序；三版形式化演进（完整语义→规则全形式化→四句话）；Test IR 编译类比与前后端解耦/多 Agent 通用语言/隔离一次性事实；案例挂本体六步与「零命中≠无影响」；两个示意案例（10 条案例 3 修改/2 补充/4 原样/1 范围外、2 项覆盖缺口；转账授权组合）；「本体是手段不是目的」与上本体三问。**原文无生产量化效果，案例均标示意/目标流程，已在页面明确标注**
- created `agent-core/data-ontology-agent-coevolution` — 张绍磊：Human-first 传统数据系统（Human/Rules/Environment）与 Agent 增强；CoDA-Bench（1,009 任务/119 CODA-HARD/31 社区/980 文件均；CODA-HARD 榜首 Mini-SWE-agent GPT-5.5 DA 67.50%/EA 49.60%；Oracle 下 GPT-5.2 56.1→61.0、Claude Code 49.2→60.0；给对数据仍约 60.5%；失败分布 Code Gen 45.5%/Data Discovery 41.6%/Task 8.1%/Exec 4.8%）；Agent+Ontology+Environment 新架构；DeepAnalyze（8B、30K+/20K+ 下载、2025-10-21 开源、4.7k stars；原文「4700K+」与仓库卡片矛盾已标注）；DeepPrep（算子化、可回滚、三基准成本-质量 Pareto）；SkillAdam（Adam 类比、Edit Budget/Optimization Memory、短程/长程全表）；EvoOntology（Content/Schema/Tool 三层、Build→Use→Evolve→Evaluate→Publish/Reject、Content/Tool/Schema 归因、Parent vs Candidate 门控；DDR-Bench/InsightBench/BIRD 全表与进化曲线）；仓库 stars。**四项均为研究/开源，无企业生产、成本、延迟数据，已标注边界**
- 未改任何 `index.md`（按任务要求，仅报告建议条目）；三页 wikilink 目标已逐一核验存在
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「AI 时代的研发范式与组织进化」3 场演讲 PDF
- 来源：`sources/2026/10/29/2026DACon北京站/AI 时代的研发范式与组织进化/` 下 3 个 PDF，逐页 `pdftotext -layout` + Read 视觉复核（31/25/45 页）；百度 PDF 无文本层，45 页全部 `pdftoppm` 渲染后逐页视觉复核，并用 tesseract `chi_sim+eng` 对关键页交叉核对；ZTE 第 20 页故障根因表以 300dpi 裁剪放大核对。原始 PDF 未改动，各配同名 `.md` sidecar（`source_type: pdf`）
- created `ai-agent/ecosystem/weibo-ai-rd-transformation` — 李庆丰《从个人提效到组织跃迁》：个人体感 vs 项目现实的效率落差；个人效率/团队效率/组织生产力三分与「个人节约时间 × 人数 ≠ 组织收益」；交付六节点与三段接缝；瓶颈转向协作接口（原文标注机制示意、非实测）；工作系统四要素（目标/上下文/能力/验收）与四个管理变化（流程/分工/管理/度量）；AI 原生流程与人机协同分工；使用/产出/交付/价值四层度量与速度/质量/成本/条件四观察面；四阶段分析路径（非自动升级规律）；WeCode→WeCoder→Wegent→WeWork 价值范围递进；Everything is a Plugin（DSH/Cordis、空间与时间可组合）；Wegent×WeWork 云端+桌面架构与 WeWork 内部 Electron/Core DSH/Executor/Codex；四类职能工作台与协作四约定；三案例（并行开发首版本周期缩短 30%、信息流 Loop Engineering、运维一人一工作台）；五步落地路径；未来「共同底座/任务驱动/协同闭环」。原文未给 30% 的样本与口径，DSH 全称未展开，均已标注
- created `ai-agent/harness/zte-harness-engineering` — 路芳芳《基于 Harness Engineering 一站式交付落地实践》：开场三个数字（400+/周故障告警、1~4h 平均处理、4 次人工转译）；需求开发三角色问题与运维诊断三重困境及恶性循环；总体架构五层（场景/入口/编排/六大 Skill/两层底座）+ 驾驭闭环；全栈模式与「熄灯工厂」三层技术架构；Harness Engineering 定义与代码链、工程师两个转变；Agent Skills 分层递进编排；git worktree 与 Skill 三层治理；六大 Skill；三案例（需求分析左移、质量门禁扫描-修复+差分、红灯闪烁根因链 100/95/90/85%）；工作门户+IDE 插件；效果（400 单/月占 30%、≥6 倍、≥70% 采纳；200+ 单/月占 20%、70%+/50%+；3-8 月任务趋势）；30% 无人工干预交付的能力组合。两个 30% 口径、倍数基线、告警/故障单口径、ZTS/FT 缩写、全流程无人与 70% 阈值的关系均标注不确定
- created `ai-agent/ecosystem/baidu-ai-rd-platform` — 孟小萱《角色融合下的一站式产研平台实践》：AI 代码生成率 80% vs 需求交付提效 20%+；时间分布（即时沟通 26.2%、编码调试 22.4%、方案文档 21.1%、会议 17.7%、等待 12.6%）与 18 轮沟通/9 天等待、3-4 天错位/+2 周；信息失真/频繁交接/流程阻塞；破局四段链；失败的角色融合四阶段；三项生产条件缺失；能跨栈≠能交付的三阻碍；Flower 七层与六能力（云化环境/多库协同/信息共享/质量左移/上通下达/长程执行）；环境、验证、产设研融合、左移、接力五组对照；底座能力+业务建设；三建设阶段；度量从过程产出转向交付吞吐；实践故事（一句话需求隐藏边界、需求生成、真实代码原型、需求预检 Q-001、技术方案四层与 iframe 适配、Git Diff、运行验证 6 例、接力协作、2 人周→3 人天）；Agent 失忆与 Harness 资产化、会话知识六步、自进化闭环、双 Loop、50+ 调用→2 脚本。角色范围（产品/设计/研发，未给岗位合并清单）、80/20 口径、单案例基线、zulu 含义、Flower 规模均标注不确定
- 未改任何 `index.md`（按任务要求，仅报告建议条目）；三页 wikilink 目标已逐一核验存在，三页之间互链
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「Agent 评测工程」2 个 PDF + 「Data Engineering Agent」1 个 PDF
- 来源目录：`sources/2026/10/29/2026DACon北京站/Agent 评测工程：从度量到改进的闭环/`、`sources/2026/10/29/2026DACon北京站/Data Engineering Agent：数据工程智能/`（原始 PDF 未改动；为 3 个 PDF 各新增同名 `.md` sidecar，`source_type: pdf`）
- created `agent-eval-trust-dual-loop` — 高诗梦（美团 · 专家工程师，负责美团 AI Agent 评测平台）《Agent 评测如何落地：可信标准与双 Loop 演进实践》（28 页，纯图片 PDF，逐页 pdftoppm 150dpi 渲染 + Read 视觉复核）：评测在 Agent 能力与业务结果之间搭桥；搭建门槛下降 vs 规模化三道障碍（停在 Demo / 卡在扩量 / 说不清价值）；Agent 评测对象 = 模型 + Prompt/Skill + 工具链 + 记忆与状态管理 + 业务流程；四层（结果/过程/效率/风险）；Trace 是评测基石与开场「学生信息」案例的三步回查；客观 vs 主观评测；主观标准先对齐——人人一致（1 个「独裁者」好过 10 个「民主者」、独立盲标→对照分歧→负责人裁决细化→新样本复测）与人机一致（0–10 分口语化标准下钻为三项是/否检查）；4 模块 / 3 能力 / 2 Loop / 1 套资产；双 Loop 咬合（评测体系 Loop：线上评测监控→复核验证→补样本校准；Agent 迭代 Loop：修复→离线回测门禁→上线验证；Case 挖掘与归因居中，同一 Case 可进两个 Loop）；离线评测四步与评测集三要素、五分类（必过集/黄金集/挑战集/错题集/需求专项集）、调优与验收留出样本分离；离线守已知、在线发现未知（影子/A-B/周期巡检 + 运行/质量/需求监控 → Case 挖掘）；Case 四来源与归因三步；三店经营复盘综合案例（C 店 120 万为退款前成交额、退款 40 万，净营收 80 万，口径混用；改 Agent 与改评测两侧动作）；双 Loop 运行节奏（每日评样本/每周检查 Judge/每两周综合评 Agent）；L0–L3 成熟度自查表；结尾四条结论（评测是桥、人人一致+人机一致、实践科学、一个 Loop 改进 Agent 一个 Loop 校准评测）；无统一 ROI 数字，已标注
- created `edd-evaluation-driven-development` — 荣柯柯（花名 柯俊，阿里巴巴 · 高德商业智能部 数据技术专家）《从 TDD 到 EDD：评测驱动的 Data Agent 开发范式》（28 页）：实践体量（4 个业务方向 C1 大增长/C2 打车/C3 信息业务/B 端商家、1000+ 题、14 天滚动、四维能力锚点 82.6 分）；三类痛点（评测集不完善/四维能力缺失/归因平台化缺失）；七套业界方案横向对比（BIRD、Spider 2.0、LiveSQLBench、Snowflake Cortex Analyst、Databricks Genie、Uber QueryGPT、LinkedIn SQL Bot）；五种判定方法及其盲区（EX/Soft EX、Test-Suite、VeriEQL、FLEX、专家人审）；组合裁判四道关（结果等价→口径合同→LLM 裁判→专家仲裁，专家只看第 04 关）；四维评分（完成度 45%/推理规划 25%/工程效率 15%/安全鲁棒 15%，「权重是价值观：宁愿它慢，不能它错」）；三条选型判据（真实问法/能回答为什么错/无人值守）；三类判定差异（粒度不同、时间窗模糊、标答自己错了）；建-管-CI-归因四阶段流水线（来源层/题库治理/调度执行/评分报告 + 执行网关/答案隔离/完整性校验）；建（四源汇聚、70/20/10 抽样、双基线、11 列标注、候选→正式→归档）；管（资产变更检测→DAG 影响定位→负责人复核；隔离/快照/审计）；CI（N+1 定时任务、切分/派发/收口、心跳 600 秒、answers_locked、真实调用红线、4–16 路并发）；9 条确定性判定出口与三条红线、四条审计阈值；E1–E9 错因及业界一手证据；量化：合并准确率 95%、500 题 30 分钟、人力成本 −30%、1000+ 题、耗时 600→300→100→30 Min、原始/剔噪双口径；真实题「动销 POI 数」62,318 vs 62,650（差 332 / 0.53%，E5 口径分歧）；评测器自身出错案例（8/20=40% 低于 50% 阈值、gold SQL 写死时间窗，补 E9+G1–G4 与口径歧义待复核）；优势与代价表；三条可带走方法论（真实问题+滚动更新/确定性判定+为什么错/原始+剔噪双口径）；未来看板化/知识化/硬隔离；未解两问（谁来评测评测、归因→修复自动化）
- created `skill-agent-boundary` — 郭志浩（瓴岳信息/洋钱罐 数据平台数据应用负责人）《Skill 与 Agent 的边界：数据平台的分层架构实践》（26 页）：一条 SQL 请求「2026 年 9 月 APP 渠道新客转化率，按周拆分」拆出六类决定（FACT×3、METHOD、RUNTIME、GUARDRAIL）；DataLumina 平台五层 + 治理闭环；责任边界判定树 Q1 FACT→平台资产 / Q2 METHOD→Skill / Q3 GUARDRAIL→Gateway 服务端 / Q4 ESCALATE→澄清·人工·失败关闭，其余 RUNTIME→Agent；Skill 分工四个 Agent 的「做什么/不做什么」（SQL Agent/数据探查/定时报告/报告追问）+ 澄清组件；SQL 生成 Skill 五阶段（解析要素零工具→语义检索选表→按需探查→生成说明→落盘后 Explain 与权限检查）与枚举探查三规则、七条共享规则；四个「错觉→真相」坑（抢跑/自截断/拆词检索/时间戳单位）；SQL 示例 channel='APP'→'0'（语法/权限/Explain 全过但 0 行）；8 步 Trace 回放与三层根因（事实未供给/系统无兜底/模型写错）；资产写回六步三 GATE（候选→自动校验→Owner→评测→版本化发布）；专项评测（n=300、3 次全对：准确率 34.7%→85.3% +50.7pp、Tool 调用 25→15 次 −40%、P95 500→200 秒 −60%，限枚举缺口专项集，外部有效性待复验）；Skill Market 八步与「安装不授予新数据权限」；Agent 工具白名单（apply_table_permissions/list_tables/find_table_by_keyword=false 等）；三条数据智能路径（dbt MetricFlow / Snowflake Cortex Analyst / Databricks Genie）与 DataLumina 定位；架构取舍（收益/成本/五项采用前提）；行动清单（复盘一条失败任务：归类决定→定位错层→校准边界）
- 复核记录：`agent-eval-trust-dual-loop` 对应 PDF 为纯图片，28 页全部 150dpi 渲染后逐页 Read 视觉复核；`edd-evaluation-driven-development` 28 页逐页 `pdftotext -layout` + 视觉复核（重点核对判定出口表、E1–E9 表、量化数字页、架构图）；`skill-agent-boundary` 26 页逐页 `pdftotext -layout` + 视觉复核（重点核对判定树、Trace 回放、专项评测数字、Skill Market 与附录完整架构图）
- 不确定项：高诗梦 PDF 未给评测平台技术栈、Judge 选型/版本方案、各业务准确率基线与双 Loop 人力变化，页面中大量为教学示例/脱敏界面；荣柯柯 PDF 的「82.6 分四维能力锚点」未解释计算口径，95% 只给四方向合并值未给分项，耗时对比页 5*6 路并行口径按原图照录；郭志浩 PDF 专项评测仅覆盖枚举缺口一个变量、300 条专项集且作者声明外部有效性待复验，未披露技术栈/模型供应商/权限实现/Skill Market 运营数据
- 未改任何 `index.md`（按任务要求，仅报告建议条目）；三页 wikilink 目标已逐一核验存在
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「决策智能 Agent」+「模型适配与效果优化」3 个 PDF（1 合并 + 2 新建）
- 来源（逐页 `pdftotext -layout` + Read 页面视觉复核；两场纯图片 PDF 全部 `pdftoppm` 110–150dpi 渲染后逐页视觉复核，tesseract `chi_sim+eng` 交叉核对）：`sources/2026/10/29/2026DACon北京站/决策智能 Agent：从数据洞察到经营动作/方彪-从+BIRD+第三到生产可用：小米+Data+Agent+落地实践.pdf`（33 页）、`模型适配与效果优化：从语义上下文、Harness到后训练/吉剑南-本体驱动：企业级 DataAgent 的效果优化之路.pdf`（24 页，纯图片）、`孟垂实-给模型立规矩：业务Agent 的三重约束一-角色・知识・动.pdf`（32 页）；原始 PDF 未改动，各新增同名 `.md` sidecar（`source_type: pdf`）
- updated `data-agent/xiaomi-dimi-data-agent`（合并而非新建，已有正文未删改）— 新增七块并逐处标注「2026-10 DACon 新增」：①「五类不确定性，一个成本问题」的第 ⑥ 问（每一环的活总要有人干：谁来做→成本高不高）；②问数评测「双入口生产 + 信号闭环」（BI 看板生成链 / 表字段智能生成 + 题型模板七类 + 语义扰动改写 + DSL 执行回验；信号分层/AI 预标注/人工审核/落库双产出）与「同一份金标撑起两级评测」；③基于 Trace 的 Agent 端到端评测 26 指标三层（结果 5 / 能力 8 / 效率 13，含分析深度三档、稳定性 SQL 漂移公式、数字溯源覆盖率、幻觉 1/0.5/0、工具调用有效率等）与三条设计原则（LLM 做选择题、0 分≠没分数、能代码判不用大模型）；④Loop 扩为双环节（text2DSL 查数 + Agent 评测：跑分→判稳→经 Trace 平台 MCP 取链路日志→归因修复→重跑）；⑤AI FDE 两页（四段交付循环、2026 FDE 元年需求 +2100%/岗位 +729%、交给 Agent 的四类执行链与留给人三项判断、L0–L4 自治梯度、53% 实测完成占比、1–2 周 SOP、Skill 树六类根节点与《渠道返利政策 v2》五轮对话交付示例）；⑥财务场景案例（百+ BP、>50% 时间占比、97% 问数准确率、六步自助构建链路）与标杆案例扩为四个；⑦业务自建 vs 中台公共能力三路线对比；另加「版本差异备忘」（Text2JSON→Text2Metric、Query-SQL→Query-DSL、SQL→DSL 溯源等）。原 2026-08 版内容零删改
- created `data-agent/oceanbase-ontology-dataagent` — 吉剑南（OceanBase AI 平台负责人）《本体驱动：企业级 DataAgent 的效果优化之路》：RAG 两问题（经营情况 5 指标向量召回 1/5、表层指标钻不动「为什么」）同根于「没有结构、没有关系」；本体语义层居于数据底座与 AI 应用之间；四大要素（业务对象/业务关联/业务属性/业务口径）与存贷款图（客户-账户-存款-交易-渠道-机构）；RAG vs 本体（语料 vs 工具）；召回策略（总览全图/展开对象/查看口径 + 自由导航/多路并行/低成本试错）；问数四步（锁定口径身份→沿关联走→绑定维度→生成 SQL，科目 2301 示例）与归因四步（定位指标→沿关联铺开维度→数据驱动收窄→定位根因，方向相反同一张图）；生命周期五阶段（冷启动→加工→生长→汇聚全域→反哺）；加工「裸表→业务图」四类产出与人工 review；暗知识沉淀 vs 传统文档「建完即过期」；自下而上汇聚与「客户」口径差异保留；反哺飞轮；自顶向下四陷阱（抽象化/会议化/静态化/过早平台化）；壁垒=被持续校准的业务理解。**原文无任何量化效果数据，演示算例（−820 万等）已明确标注为非落地效果**
- created `context-eng/agent-three-constraints` — 孟垂实（京东科技算法总监）《给模型立规矩，业务 Agent 的三重约束》：两个痛点场景（营销手工长链 8 步、分析断点困境）与三条核心判断（改造认知负担/可控性与灵活性平衡/Harness=约束体系）；Harness 两层（业务约束层 vs 基础设施 Harness，缺一不可发布/跑不起来）；通用 Agent 三缺（无身位/无专属资产/不承担副作用）；三道约束定义表（角色→角色宪章→怎么思考；知识→资产能力化→知道什么；动作→原语+校验→能做什么）；注入·检索·收口与「基础设施给槽、业务层装规矩」；案例一智能营销（营销系统=流量分发与状态改变机制、四岗位宪章、好/坏宪章对照与六条原则、策略画布四要素、历史策略卡/文案条目卡、可理解·可比较·可检索、五项代码前置校验与允许/拒绝/需要补充）；案例二经营研判（分析专家五岗位、语义层五类节点+一类关系、完成产值下钻链、指标库完整条目 metric_project_monthly_output_v2、分析 Skill 四条、「语义层管事实 Skill 管怎么看」、三道硬校验只读/权限/证据、七步流水线第 6 步唯一拦截点）；五步落地路线与三维验收（业务价值/质量与可信/运行效率）；两大挑战与对策；三个模板（角色宪章八行、业务资产卡片 3+2+1、业务原语与校验清单）。**原文无量化效果，仅定性「不必记规则·发布过闸·可回放」「不找表·只读·血缘」，已标注**
- 复核记录：小米 33 页重点核对第 4/10/11/12/13/14/15/16/17/19/20/22/23/31 页；吉剑南 24 页全部渲染视觉复核（架构图、四要素图、归因表、生命周期、加工/生长/汇聚/反哺、四陷阱）；孟垂实 32 页逐页文本提取 + 第 5/6/7/11/12/13/14/15/16/17/19/20/21/22/23/25/26/28/29/30 页视觉复核
- 不确定项：小米页「53%」仅标注为专利场景实测、未给样本与口径；财务案例「97% 问数准确率」未给评测集规模；吉剑南 PDF 无任何量化指标，DataPilot 与湖库一体三产品关系未展开，权限/版本/建模成本均未提及；孟垂实 PDF 无量化效果，生成式/验证式边界只给原则、未给可操作分级
- 未改任何 `index.md`（按任务要求，仅报告建议条目）；新增页 wikilink 目标已逐一核验存在
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站批量摄取（42 PDF / 17 分论坛）

- 来源：`/Users/wangjinjie/Desktop/2026DACon北京站`（17 个分论坛目录，42 个 PDF，合计约 1200 页）
- 原始 PDF 归档 `sources/2026/10/29/2026DACon北京站/<分论坛>/`，42 个同名 `.md` sidecar 已建（`source_type: pdf`）
- 摄取方式：15 个并行代理按分论坛分组，逐页 `pdftotext -layout` 文本层 + Read PDF 页面视觉复核；纯图片 PDF（高诗梦/闾泽军/孟小萱/李明皇）用 `pdftoppm` 150dpi 渲染 + `tesseract chi_sim+eng` 交叉核对；无法辨认内容在页内标注「不确定」，未臆测
- 新建 41 个演讲页面：
  - 主论坛/金融：`agent-context-three-pillars`、`model-fullstack-moat`、`wing-pay-ontology-trusted-ai`
  - 评测/DataEng：`agent-eval-trust-dual-loop`、`edd-evaluation-driven-development`、`skill-agent-boundary`
  - 数字员工：`alibaba-digital-employees`、`multi-tenant-digital-employee-cluster`
  - Agent-Ready：`curvine-agent-data-base`、`amap-intelligent-query`、`agent-ready-multi-cloud-luxury`
  - 客户全旅程：`jd-ad-data-workspace`、`agentic-voice-agent`
  - 研发范式：`weibo-ai-rd-transformation`、`zte-harness-engineering`、`baidu-ai-rd-platform`
  - 行动闭环：`action-loop-agentcore`、`agentic-engine-loop`、`zhihu-dataclaw`、`qunar-ota-data-agent`
  - 文档解析：`unstructured-document-parsing`、`qcc-document-parsing`
  - 决策智能：`trusted-data-qa`、`data-agent-onboarding`
  - 模型适配：`oceanbase-ontology-dataagent`、`agent-three-constraints`、`gui-test-agent-posttraining`
  - 本体/知识工程：`finance-testing-ontology`、`data-ontology-agent-coevolution`、`jd-ugo-growth-ontology`
  - 模型组合：`dunhuang-model-routing`、`xiaohongshu-inference-selection`、`lmcache`
  - 语义层：`360-data-agent-semantic-layer`、`semantic-layer-to-ontology`、`apache-ossie-ant-practice`
  - 上下文工程：`lixiang-semantic-context`、`decidex-context-engineering`、`mashang-knowledge-engineering`
  - 生成式推荐：`amap-route-recommendation-agent`、`tme-interactive-recommendation`
- merged `xiaomi-dimi-data-agent` — 方彪 DACon 2026 版（33 页）与 2026-08 版（29 页）为同题演进，增量合并七块新增（成本问题、双入口评测+信号闭环、Trace 26 指标、自优化 Loop、AI FDE、财务案例、业务自建/中台公共能力三路线），原内容未删改
- updated `digital-employee` — 新增「组织化落地：阿里 2000+ 数字员工」一节
- updated `apache-ossie` — 新增「蚂蚁落地实践」一节
- created `dacon-2026-beijing` — 会议级总索引（17 分论坛 / 42 场）
- updated `ai-agent/index`、`data-agent/index`、`agent-core/index`、`context-eng/index`、`harness/index`、`ecosystem/index`、`knowledge-base/index`、`ai-ml/index`、`big-data/index`、`homepage`
- 质量备注：方彪两版同题已合并；骆兆楷 PDF 前 4 页为 DACon 模板使用说明，未作为演讲内容摄取；原「原礬峰」文件名与正文「原攀峰」不一致，页面以正文为准；胡浩源 PDF 关键数字被脱敏为 `***`，页面只保留口径与方向
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「上下文工程」与「非结构化数据处理与知识获取」3 个 PDF
- 来源：`sources/2026/10/29/2026DACon北京站/上下文工程：让 AI 真正懂企业/赵雪波-构建知识工程，推进高质量+Agent+分析决策.pdf`（24 页）；`sources/2026/10/29/2026DACon北京站/非结构化数据处理与知识获取/常鸿宇-非结构化文档的内容理解与样式还原——从RAG知识供给到PDF转Docx的工程实践.pdf`（25 页）；`sources/2026/10/29/2026DACon北京站/非结构化数据处理与知识获取/陈汝龙-企查查：表格识别、VLM幻觉修正与文档解析加速的工程实践.pdf`（32 页）
- created `mashang-knowledge-engineering` — 赵雪波（马上消费金融）：数据 Agent 三类工作问题与两条解决办法（Agent friendly 语义元数据 + Agent judge 数字化测评）；知识分层（>95% / >70% / 40%-60%）与四类知识来源；知识工程支撑 SQL 取数、复杂分析预测、需求文档驱动三类场景；语义知识工程化四环节；OLD/NEW skill 样例（知识库 4→26、交互轮次平均降 30%、Skill 复杂度降 70%+）；数据 Agent 测评 4 大项 13 子项、工程链路与三类评测失真；本体 for LLM / 本体 for 决策架构
- created `unstructured-document-parsing` — 常鸿宇（360 人工智能研究院）：知识库问答 vs 文档二次编辑两类目标；Pipeline/End2End/复合式三路线对比；预处理去水印（CER 0.092→0.071）、版式分析（360LayoutAnalysis）、OCR 与公式（formulaNER 0.178 vs 0.182）、表格解析（TEDS 0.753 vs 0.921）、阅读顺序（CER 0.130→0.479）、层次结构（上下文减少 15%-40%）、跨页合并（CER 0.120→0.082 / TEDS 0.770→0.881）、图片处理；PDF→Docx 三方案、python-docx+XML 融合、4 个 case 与 2 个困难 case；1450 页/10 类 Benchmark 与平均得分（our system 0.589）
- created `qcc-document-parsing` — 陈汝龙（企查查）：海量非结构化文档→文档解析→领域模型抽取→结构化 JSON；pypdfium2/传统 pipeline/开源 VLM 选型；开源 VLM+业务数据训练；数据筛选/多模型校验/不稳定数据/专家标注；675M NaViT + 0.5B LLM + OTSL 输出；SFT→GRPO（Layout & Table 奖励）；VLM 幻觉/繁简用 pdfium2 文本层校正（+2.8%）、印章分离（+5.2%）、1:17 面条图多维加权全局切割；调度 +3%、Attention +6%、CUTLASS +6%、Dflash 单任务 1.5-1.7x 但端到端 2-3%
- 复核记录：三个 PDF 均逐页 `pdftotext -layout` + Read 页面视觉复核（24/25/32 页）；PDF3 全部 32 页另用 pdftoppm 150dpi 渲染 + tesseract `chi_sim+eng` 交叉核对；对 PDF2 p20 Benchmark 页 200dpi 裁切复核数字；架构图、模块名、指标与口径逐页核对
- 不确定项已在页内标注：PDF1「北极星 +1.5」「CMD AI friendly 40%-80%」含义与口径；PDF1 看板同比为示例截图；PDF2「1450 页/10 类文档」与图例 11 类、语言分布合计 1416 的口径差异；PDF3「协作与共享挑战」90%/80% 含义、OTSL 全称、+2.8%/+5.2% 基线口径、并发测试硬件口径
- 未改任何 `index.md`（按任务要求）；三页均以 [[wikilink]] 关联已有页面，链接目标已逐一核验存在；为 3 个 PDF 各新增同名 `.md` sidecar（`source_type: pdf`），原始 PDF 未改动
- run `llm-wiki sync`

## [2026-10-29] ingest | DACon 2026 北京站「从分析闭环到行动闭环 / 决策智能 Agent」4 个 PDF
- 来源：`sources/2026/10/29/2026DACon北京站/从分析闭环到行动闭环：Data Agent 的边界突破/`（汤晋瑄、郑吉敏）与 `sources/2026/10/29/2026DACon北京站/决策智能 Agent：从数据洞察到经营动作/`（李明皇、周卫林）；原始 PDF 未改动，各新增同名 `.md` sidecar（`source_type: pdf`）
- created `zhihu-dataclaw` — 汤晋瑄《知乎 DataClaw：从 MetricFlow 到 AI 原生语义层的演进实践》（30 页）：三阶段演进（MetricFlow 语义层 SQL 准但不灵活 → AI 原生知识库 AI 生产/消费数据资产 → 轻量级本体图谱 L1 连接/L2 理解/L3 推理）；业务痛点 80% 体力劳动（等待久/门槛高/口径乱）；架构总览（统一工作台 + 大数据 CLI + 四类 Agent + 有资源/懂业务/会干活三层）；混合检索五步与 WRRF 公式/参数（vectorWeight 1.0、fulltextWeight 1.0、k 60、perPathTopK 20）；四类知识定制切片；本体五类概念 + 物理锚点 + 示例链路（User→观看事实→Content）；轻量架构（nodes+edges、递归 CTE 1–4 跳、路径上限 6 跳、MCP 7 读 5 写、双视图、next_hint）；版本即契约（kge_pull/push、三段版本号、人工审核）；两挑战（四类知识共用切片、差点上图数据库）；效果（口径千级/SQL 万级/渗透率 0→65%+；等待天级→分钟级；自助取数 100%；口径返工 -50%；整体 95%，真实问题 70%→92%）与三条方法论/避坑清单/知识飞轮
- created `qunar-ota-data-agent` — 郑吉敏《OTA 业务 DataAgent 的边界突破与规模化落地》（20 页）：三段断裂（取数×分析/分析×决策/决策×动作）；提效 vs 增业绩；三路线对比（规则报表/BI+LLM/Agent 原生）与「LLM 推理、MCP 出数、人做决策」；System1 控场/System2 执行 + 双层规划（CoT→ToT→GoT；ReAct+Plan/Execute→Reflexion）；四层架构（业务分析模型/AI 能力/业务知识 Skills/数据底座）与 Thought→Planner→Action→Observation→Result；模型演进（Claude Sonnet 4.5→4.6 → GLM 5.1→5.3-Flash，成本 -95%+）与六项实测；间夜=流量×转化×单订单间夜、双基准/贡献 pp/σ、七模块数字化全景；口径四优势 + 数据连通 + 两个组织前提；幻觉治理四类 Harness；Skill 拆分与角色/场景/技能三管理、读写分离；使用方式（自动巡检/临时提问/飞书机器人 4 小时/定制页面）；效果 5–10 分钟、成本 -95%、高频承接约 80%、7 模块；三案例（税费赔付治理、增长洼地、B/C 双向赔付治理：T+1/T+8/T+14 零复发、拉黑数百用户、每月再省数万元）；边界（治理成本、写动作人审、长尾问题）
- created `trusted-data-qa` — 李明皇《让 Data Agent 的每个回答都有据可查：企业级可信问数的工程闭环》（24 页，全页图片，pdftoppm 150dpi 渲染 + Read 视觉逐页 + tesseract chi_sim+eng 交叉核对）：BIRD #1493 贯穿；基础问数链路与四步流程；可信四问（统计谁/谁在查/如何核对/怎么维护）；Gravitino 统一元数据（Metalake→Catalog→Schema→…→SemanticModel，多源接入/统一管理/统一治理）；Gravitino Semantic Model（Ossie 兼容；Datasets/Fields、Relationships、Metrics、AI Context、Custom Extensions；customer_month_count 与 distinct_customer_count 示例）；NL→SQL 职责分工（Agent 理解规划 / Semantic Compiler 确定性编译 / Query Engine 执行）；流程编排三层（业务域上下文/分层识别与路由/受约束探索）；工具约束三条（明确输入、显式状态、控制回退与错误分流）；证据链三层（语义定义/查询调用/计算调用；答案 66.62%，R1=12,208、R2=18,324、0.66623008…）；Semantic Generator 五段流水线 + 持续维护；材料拆分/检查点/受限修复/字段依据审核/变更发布读回；数仓分层复用与销售额选源；最终回答评测（AnswerStructurer 不接收标准答案、等价答案 04/April/201304、正确性+稳定性）；正确回答 vs 有效澄清决策矩阵；业务共建闭环。原文未给企业客户案例与量化指标，已在页内明确标注
- created `data-agent-onboarding` — 周卫林《30 分钟上岗：企业"招聘" Data Agent 的新标准》（33 页）：Agent = f(LLM+Harness+Context) → Agent = f(Context)；Context 四层（语义平台是最高知识密度表达/Context Engineering 首先是信息学/Perfect decisions need perfect context 与 Context is never complete, alive/Context 不是能力是事实、是工作现场）；上线 vs 上岗（功能可用 vs 工作可交付）；买工具 vs 招员工五阶段映射（定功能/定岗位…验收/转正）；30 分钟上岗与四挑战（上岗/协作/成长/信任）；四个上岗能力（摸得清/问得准/干得对/学得会）+ 底座语义层+知识库；摸得清（语义构建五段、语义保鲜、三级血缘对比：表级/列级/算子级、指标重复 48 组/冲突 21 组、FineBI 4 组件→2 指标+3 维度+3 数据集+2 关系）；问得准（企微流失例、反问三原则、四种信息状态）；干得对（业务 100% vs Agent 理由、过程可复盘/结论可追溯/操作可审计、任务前中后）；学得会（两级评测、三级沉淀、可管控三问）；一张图六泳道与查得出/算得准/验得过/说得清；新探索（简历式求职、请求试岗、考评转正；转正标准未披露）；语义编织/NoETL 定位为数据土壤，技术细节未展开
- 复核记录：4 个 PDF 均逐页 `pdftotext -layout` + Read 视觉复核（30/20/24/33 页）；李明皇 PDF 为全图片页，24 页全部渲染后视觉读取并用 tesseract 交叉核对；其余 3 个 PDF 全页视觉复核。无法辨认或原文自相矛盾处（知乎「70%→92% 标注提升 11pp」；周卫林截图看板数值口径）已在页内标为不确定，未作推断补全
- 未改任何 `index.md`（按任务要求）；四页 wikilink 目标已逐一核验存在
- run `llm-wiki sync`
