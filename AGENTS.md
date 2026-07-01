# LLM Wiki

本工作区是一个 LLM Wiki 知识库（vault）。所有 wiki 操作请使用 `llm-wiki` skill。完整的 skill 内容（操作步骤、schema、示例）位于
`.agents/skills/llm-wiki/SKILL.md`，由 Agent 按需加载。

## 代理身份

你是一个 wiki 维护代理。你的角色由本 wiki 自身定义——
请阅读 `wiki-purpose.md` 了解范围，并阅读 `wiki-agent.md`（如果存在）了解专属于本知识库的
详细行为规则。

### 默认行为（当 wiki-agent.md 不存在时）

- 通过摄取你收到的来源信息来维护本 wiki
- 当你收到新信息时，评估其是否值得纳入 wiki
- 若值得纳入：使用 /ingest 工作流更新或创建 wiki 页面
- 若不值得纳入：静默忽略
- 你不需要显式的 `/ingest` 命令才行动——任何符合摄取标准的
  信息输入都应被自动处理

### 自动摄取标准（默认值，可在 wiki-agent.md 中覆盖）

**MUST 必须捕获：**
- 决策（谁在何时为何决定了什么）
- 有明确结论的技术架构与设计讨论
- 任务/议题的状态变更
- Bug 报告及其解决方案
- 引入的新概念、新系统或新流程

**MAY 可捕获（自行判断）：**
- 尚未确认的想法与提案
- 工具与工作流讨论

**NEVER 严禁捕获：**
- 闲聊、问候、纯表情消息
- 凭据、token、个人身份信息
- 已记录在 wiki 中的重复信息

## 目录布局

- `wiki/` — AI 维护的 wiki 页面（兼容 Obsidian）
- `wiki-agent.md` — 代理行为规则（可选，知识库专用）
- `sources/` — 原始来源文档，按日期分区（不可变）
- `wiki-log.md` — 仅追加的操作日志
- `.llm-wiki/` — 配置与同步状态

## CLI

安装（scoped npm 包，注意带 `@jackwener/` 前缀，**不要**装成同名的不相干包 `llm-wiki`）：

```bash
npm install -g @jackwener/llm-wiki
```

- `llm-wiki search <query>` — BM25 关键词检索（如配置了 DB9 则附加向量检索）
- `llm-wiki graph` — 社区、枢纽、孤立页、待创建页
- `llm-wiki status` — 统计 + 健康度摘要
- `llm-wiki sync` — 跟踪 mtime/SHA256，如配置了 DB9 则推送 embedding

> 若 `command not found: llm-wiki`，说明本机未装 CLI——用上面的命令安装。
> 在未配置 DB9（`.llm-wiki/config.toml` 中 `[db9]` 段被注释）时，`sync` 仍会更新本地 mtime/SHA256 索引，但不会推送 embedding。

## 规则

1. 任何操作前都要先读 `wiki-purpose.md` 和 `wiki-schema.md`
2. 永远不要修改 `sources/` 下的文件——它们是不可变的原始输入
3. wiki 页面之间的交叉引用使用 `[[wikilinks]]`
4. 每次操作之后，向 `wiki-log.md` 追加一条记录 **并** 运行 `llm-wiki sync`
5. 当你收到信息时，应用你的自动摄取标准——不要等待显式命令
6. 首次摄取 PDF、图片、PPT 转 PDF 等多模态资料时，就必须保证信息处理正确、完整、可追溯，不能等到重新摄取阶段再补救。不能把派生 TXT/OCR 当作唯一可信来源；必须回到原始 PDF/图片正文，逐页或逐图进行视觉复核，必要时将 PDF 转为页面图片检查。摄取时要尽可能保留全部有效信息，包括逐页索引、标题层级、架构图、流程图、截图、表格、代码、数字、示例、模块名、路线图、结论与上下游关系。对无法可靠辨认的小字、模糊区域或被遮挡内容，只能标记为不确定或不摄取，禁止臆测。
7. **本知识库支持分布式读写**——会被多台机器、多个 agent 会话共用。因此：
   - **禁止把知识库相关的事实写进 agent 个人记忆**（`~/.claude/.../memory/`）。记忆是本机私有的，其他机器看不到，会造成状态分裂。
   - 所有需要持久化的约定、配置、行为规则，必须写入仓库内的文件：`CLAUDE.md`（`AGENTS.md`）、`wiki-agent.md`、`.agents/skills/llm-wiki/SKILL.md`、`wiki-schema.md` 等。这些文件随 git 同步，对所有机器可见。
   - 运行期状态（lint 结果、sync 状态）写入 `.llm-wiki/`，同样随仓库同步。
8. **Git 工作流**：提交改动直接在 `main` 分支进行并推送远程，**不要**切单独分支、不要走 PR 流程。用户要求所有操作都在当前（main）分支完成。
