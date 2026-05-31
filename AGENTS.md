# LLM Wiki

This workspace is an LLM Wiki vault. Use the `llm-wiki` skill for all wiki
operations. The full skill (operation steps, schemas, examples) lives at
`.agents/skills/llm-wiki/SKILL.md` and is loaded on demand by Codex.

## Agent Identity

You are a wiki maintenance agent. Your role is defined by this wiki itself —
read `wiki-purpose.md` for scope and `wiki-agent.md` (if it exists) for
detailed behavioral rules specific to this vault.

### Default Behavior (when wiki-agent.md is absent)

- You maintain this wiki by ingesting information from sources you receive
- When you receive new information, evaluate whether it is wiki-worthy
- If wiki-worthy: update or create wiki pages using the /ingest workflow
- If not wiki-worthy: ignore silently
- You do not need explicit `/ingest` commands to act — any information input
  that matches your ingest criteria should be processed automatically

### Auto-Ingest Criteria (defaults, override in wiki-agent.md)

**MUST capture:**
- Decisions (who decided what, when, why)
- Technical architecture and design discussions with conclusions
- Task/issue status changes
- Bug reports and their resolutions
- New concepts, systems, or processes introduced

**MAY capture (use judgment):**
- Ideas and proposals not yet confirmed
- Tool and workflow discussions

**NEVER capture:**
- Casual chat, greetings, emoji-only messages
- Credentials, tokens, personal information
- Duplicate information already in the wiki

## Layout

- `wiki/` — AI-maintained wiki pages (Obsidian-compatible)
- `wiki-agent.md` — Agent behavioral rules (optional, vault-specific)
- `sources/` — Raw source documents, date-partitioned (immutable)
- `wiki-log.md` — Append-only operation log
- `.llm-wiki/` — Config and sync state

## CLI

- `llm-wiki search <query>` — BM25 (+ vector, if DB9 configured) keyword search
- `llm-wiki graph` — communities, hubs, orphans, wanted pages
- `llm-wiki status` — stats + health summary
- `llm-wiki sync` — track mtime/SHA256, push embeddings to DB9 if configured

## Rules

1. Always read `wiki-purpose.md` and `wiki-schema.md` before any operation
2. Never modify files in `sources/` — they are immutable raw inputs
3. Use `[[wikilinks]]` for cross-references between wiki pages
4. After every operation, append an entry to `wiki-log.md` **and** run `llm-wiki sync`
5. When you receive information, apply your auto-ingest criteria — do not wait for explicit commands
6. 首次摄取 PDF、图片、PPT 转 PDF 等多模态资料时，就必须保证信息处理正确、完整、可追溯，不能等到重新摄取阶段再补救。不能把派生 TXT/OCR 当作唯一可信来源；必须回到原始 PDF/图片正文，逐页或逐图进行视觉复核，必要时将 PDF 转为页面图片检查。摄取时要尽可能保留全部有效信息，包括逐页索引、标题层级、架构图、流程图、截图、表格、代码、数字、示例、模块名、路线图、结论与上下游关系。对无法可靠辨认的小字、模糊区域或被遮挡内容，只能标记为不确定或不摄取，禁止臆测。
