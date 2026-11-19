---
title: Databricks Agent 代码执行环境（官方文档摘录）
url: https://docs.databricks.com/aws/en/notebooks/code-assistant
author: Databricks
date: 2026-10-30
retrieved: 2026-11-19
type: docs
ingested: 2026-11-19
wiki_pages: [databricks-genie]
---

# Genie Code 到底跑在哪：serverless compute 与容器边界

> 节选。用途：核实"Genie Code 定时任务 / Job 任务是否运行在容器里"。三份官方文档：
> ① notebooks/code-assistant，2026-10-30；② compute/serverless，2026-11-17；③ security/network/serverless-network-security，2026-10-29；④ compute/custom-containers（Databricks Container Services），2026-09-11。

## 一、代码默认在 serverless 上执行

> ## Run code in Genie Code on serverless compute
>
> You can run code directly from the Genie Code side panel. Genie Code uses serverless compute by default, which is the default compute on Databricks.
>
> If you're on a page that already has a compute resource selected (for example, notebooks or the SQL editor), Genie Code automatically uses that compute instead.
>
> If you don't have access to serverless compute, you must have a compute instance available to run code in the Genie Code panel.

## 二、serverless compute 的定义与隔离口径

> Serverless compute is a Databricks-managed service that allows users to quickly connect to on-demand computing resources for notebooks, workflows, and Lakeflow pipelines.
>
> When you choose to use serverless compute, you can run workloads without provisioning any compute resources in your cloud account. Instead, Databricks automatically allocates and manages the necessary compute resources.
>
> Serverless workloads are protected by multiple layers of security and are designed to be enterprise-ready.
>
> Serverless compute resources run in the serverless compute plane, which is managed by Databricks. Account admins can configure secure connectivity between the serverless compute plane and their resources.

> Serverless compute is a versionless product, which means that Databricks automatically upgrades the serverless compute runtime to support enhancements and upgrades to the platform.

**注意**：以上文档均未描述 serverless 的容器/镜像实现，也没有自定义镜像入口。公开口径只到 "Databricks-managed" 与 "multiple layers of security"。

## 三、真正可指定容器的地方：classic compute + Databricks Container Services

> Databricks Container Services lets you specify a Docker image when you create compute.
>
> The following steps take place when you launch a compute that uses Databricks Container Services:
> 1. VMs are acquired from the cloud provider.
> 2. The custom Docker image is downloaded from your repo.
> 3. Databricks creates a Docker container from the image.
> 4. Databricks Runtime code is copied into the Docker container.
> 5. The init scripts are executed.

即：容器只在 classic compute 且启用 Container Services 时出现，粒度是**节点**（一台 VM 一个容器），不是"每次 Agent 运行一个容器"；而且 Genie Code 的 Job 任务/定时任务配置里没有 compute 选项，用不到这条路径。
