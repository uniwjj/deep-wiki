---
ingested: 2026-07-01
wiki_pages: [iceberg]
source_file: iceberg-definitive-guide-dremio.pdf
source_type: pdf
status: reviewed
---

# iceberg-definitive-guide-dremio

本文件为同名 PDF 来源的摄取元数据 sidecar。PDF 为 *Apache Iceberg: The Definitive Guide*（Tomer Shiran/Jason Hughes/Alex Merced，Dremio，344 页）。

已用 pdftotext 提取全文（708KB）并复核关键章节：Catalog 五种类型（Hive/Glue/Nessie/REST/JDBC）、小文件问题与 Compaction（Rewrite Datafiles/Manifests）、写入的原子提交机制已补入 [[iceberg]] 页面。其余章节（Spark/Dremio/Flink 集成实操、生产运维、迁移案例）偏 hands-on，未逐章搬入，需要时回 PDF 查阅。原文见同目录 `.pdf`。
