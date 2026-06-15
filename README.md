# Deep Wiki

AI 驱动的个人技术知识库 —— 基于 LLM Wiki 框架的结构化知识沉淀系统。

## 这是什么

Deep Wiki 是一个由 AI 代理维护的 Obsidian 兼容知识库（vault），聚焦**大数据开发治理平台**相关技术领域。它不只是静态笔记集合 —— AI 代理会持续从各种来源（文章、论文、对话）中提取知识，自动创建相互关联的 wiki 页面，并定期进行健康检查。

## 知识领域

| 领域 | 覆盖内容 |
|------|---------|
| 大数据技术 | Spark, Flink, Kafka, Hudi, Iceberg, 数据湖, 流批一体, 数据治理 |
| 分布式系统 | Raft/Paxos, CAP 理论, 分布式存储与计算 |
| AI/ML/LLM | 大模型, RAG, 向量数据库, Prompt Engineering |
| AI Agent 开发 | Agent 框架, 工具调用, MCP, 多 Agent 协作, Agentic Workflow |
| 全栈开发 | React/Vue, TypeScript, Next.js, Node.js, tRTC |
| 后端工程 | Go/Java, 数据库, API 设计, 系统设计 |
| 平台工程 | DevOps, 可观测性, K8s, CI/CD |
| 架构与设计 | 架构模式, DDD, 事件驱动, CQRS, 微服务 |
| 产品与技术管理 | 产品方法论, 技术战略, 工程管理 |

## 目录结构

```
deep-wiki/
├── wiki/                  # Wiki 页面（Obsidian 兼容）
│   ├── big-data/          # 大数据技术
│   ├── distributed/       # 分布式系统
│   ├── ai-ml/             # AI/ML/LLM
│   ├── ai-agent/          # AI Agent 开发
│   ├── fullstack/         # 全栈开发
│   ├── backend/           # 后端工程
│   ├── platform/          # 平台工程
│   ├── architecture/      # 架构与设计
│   └── product/           # 产品与技术管理
├── sources/               # 原始来源文档（只读，按日期归档）
├── wiki-log.md            # 操作日志（仅追加）
├── wiki-purpose.md        # 知识库定位与范围
├── wiki-schema.md         # 组织规则与标签体系
├── wiki-agent.md          # AI 代理行为规则
├── AGENTS.md              # Agent 配置入口
└── .llm-wiki/             # 配置与同步状态
```

## 使用方式

使用 **Claude Code** 作为 AI 代理来维护本知识库：

```bash
# 启动 Claude Code
claude
```

代理会自动观察输入信息，评估是否值得纳入 wiki，并执行摄取、查询、检查等操作。

### CLI 命令

| 命令 | 说明 |
|------|------|
| `llm-wiki search <query>` | BM25 关键词检索 |
| `llm-wiki graph` | 社区分析、枢纽页、孤立页 |
| `llm-wiki status` | 统计 + 健康度摘要 |
| `llm-wiki sync` | 更新搜索索引与 embedding |

### 核心操作

通过 AI 代理对话执行：

- **摄取资料** — 发送文章/论文/对话给代理，自动提取知识并构建关联页面
- **查询知识** — 向代理提问，基于 wiki 内容综合回答
- **健康检查** — 代理定期扫描断链、孤立页、过期内容并自动修复
- **深度调研** — 代理联网搜索外部来源并纳入 wiki

## 核心特性

- **AI 自动维护** — 代理持续从输入中提取知识，无需手动整理
- **双向链接** — 使用 `[[wikilinks]]` 建立跨领域知识关联
- **不可变来源** — 原始资料完整归档，每项主张可追溯
- **健康自愈** — 自动检测并修复断链、孤立页、过期内容
- **Obsidian 兼容** — 所有页面为标准 Markdown + frontmatter，可直接用 Obsidian 打开浏览

## 快速开始

```bash
# 克隆仓库
git clone <repo-url>
cd deep-wiki

# 用 Obsidian 打开以浏览知识库
# 或用 Claude Code 启动 AI 代理
claude
```

在 Claude Code 中，代理会自动读取 `AGENTS.md` 获取行为规则，并根据 `wiki-purpose.md` 和 `wiki-schema.md` 维护知识库。
