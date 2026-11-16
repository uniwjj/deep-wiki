---
ingested: 2026-11-16
wiki_pages: [nvidia-auto-ontology]
source_file: index.html
source_type: article-html
title: NVIDIA Auto-Ontology 技术调研（2026-11-14）
author: 观策·行业与竞品情报（岱玹 / 蒋鸿翔）
published: 2026-11-14
retrieved: 2026-11-16
urls:
  - https://docs.popo.netease.com/lingxi/5d958d55fdab4ec1985ebe130d441ef3
  - file:///Users/wangjinjie/Desktop/NVIDIA%20Auto-Ontology%20技术调研（2026-11-14）.html
---

本文件是 sidecar，只承载摄取状态与来源定位；正文在同目录 `index.html`（桌面导出的完整版，自带样式，可双击阅读）。

两份链接是**同一份报告的两个载体**：POPO 云文档（2026-11-14 09:34 由「岱玹(蒋鸿翔)」创建，源文档）与桌面导出的自包含 HTML（排版版）。已逐段比对：POPO 正文所有段落均含于 `index.html`，无遗漏；`popo-doc.raw.html` 保存 POPO 接口返回的 `<doc>` 内容片段，仅作溯源（片段不含样式，不建议直接打开阅读）。

报告基于 NVIDIA/auto-ontology 公开 README、Tags、Commits 整理，**未实际部署运行**；官方未公布准确率。文中「【推断】」标记为报告作者的推断，wiki 页内保留该标记。

## 2026-11-16 补充（摄取后核实）

- `upstream/`：为核实「抽取边界」，抓取并归档上游仓库原始文档——`README.md`、`docs-architecture.md`、`connector-base.py`、`skill-management.md`（均取自 github.com/NVIDIA/auto-ontology 的 main 分支，2026-11-16 抓取）。
- 核实结论：抽取面只有数据库 connector（表/列/视图/主键/外键/查询历史 + 采样画像），**不含 ETL 任务代码与业务文档**；认证（certify）只是标记「已审核」，不改变 Agent 检索行为。详见 wiki 页 `nvidia-auto-ontology` 的「抽取边界」一节。
