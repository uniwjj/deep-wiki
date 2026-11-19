---
title: Databricks Genie Code 调度能力（官方文档摘录）
url: https://docs.databricks.com/aws/en/genie-code/scheduled-tasks
author: Databricks
date: 2026-10-30
retrieved: 2026-11-19
type: docs
ingested: 2026-11-19
wiki_pages: [databricks-genie]
---

# Databricks 调度 Agent：两份官方文档摘录

> 本文件是官方文档的**节选**（保留原文英文关键段落，未逐字全录），用于支撑 [[databricks-genie]] 的「调度与自动化」一节。
> 文档一：Genie Code scheduled tasks，https://docs.databricks.com/aws/en/genie-code/scheduled-tasks（Last updated 2026-10-30）
> 文档二：Genie Code task for jobs，https://docs.databricks.com/aws/en/jobs/tasks/genie-code（Last updated 2026-11-05）

## 一、Genie Code scheduled tasks（个人级定时任务）

> Create scheduled tasks to run Genie Code prompts on a schedule, so you can turn recurring analysis into a background task. For example, you can have Genie Code summarize your overnight job results every morning, review your assigned Jira tickets and start working on them, or re-run a weekly investigation without prompting it each time.
>
> Each task executes your prompt as a full Genie Code session and produces a new chat. The chat appears in your chat list, and you can open it to review the results or continue the conversation, like any other Genie Code chat.

创建方式三种：

- **Describe it in chat** — 直接用自然语言要求（例："Every morning at 6 AM, summarize the status of my overnight jobs."），Genie Code 可能追问时间等细节后创建。
- **Start from a suggested task** — 在 full page Genie Code 的 `Schedules` 面板选建议任务。
- **Create it manually** — `Scheduled tasks` 面板 → Create → Create manually，填 Title / Prompt / Schedule / Timezone 后创建。

运行行为：

> **Auto-approve is always on for scheduled runs.** Because a scheduled run happens without you present to approve individual tool actions, Genie Code approves tool actions automatically during the run. An AI classifier reviews each action against your prompt and blocks risky ones. You cannot turn this off for a scheduled run.
>
> **Each run creates a new chat.** Every run produces its own Genie Code chat rather than adding to a single ongoing conversation.
>
> **Runs use your permissions.** A scheduled run accesses data and performs operations using the permissions of the user who created the task, governed by your Unity Catalog permissions.

管理：Run now（手动触发）、暂停（turn it off）、Edit（标题/prompt/日程/时区）、Delete、Run history（每次运行打开为一个 chat）。

限制：

> - Scheduled tasks cannot be shared with other users. However, you can share individual chats from scheduled runs like any other Genie Code chat.
> - Scheduled tasks count towards your Genie usage. If you reach your Genie Code usage limit, Genie Code still attempts the scheduled run, and it fails the same way an interactive session would.

## 二、Genie Code task for jobs（工作流级任务）

> Use the Genie Code task to run an autonomous agent as part of a job. Genie Code works from a natural-language prompt and runs with auto-approve.
>
> The Genie Code task type launches a new Genie Code chat with your prompt and produces a response. Genie Code reads data, calls tools, and acts on your prompt without requiring additional input or approval. After the run completes, you can open the chat and continue to interact with Genie Code.

适用场景（原文示例）：

- Summarize overnight job results and email a report.
- Analyze incoming data and flag anomalies.
- Investigate a Jira ticket and propose a fix.
- Generate a weekly compliance audit.

配置：Jobs UI → Tasks → Add task → Type 选 `Genie Code` → 填 Prompt。Prompt 支持 `{{ }}` 插值：

> - **Job parameters**: `{{name}}`, where `name` is the job parameter's key.
> - **Dynamic value references**: for example, `{{job.id}}`, `{{job.start_time.iso_date}}`, a value set by an upstream task with `{{tasks.<task_name>.values.<key>}}`, or the current input of a For each task with `{{input}}`.

> If a dynamic value can't be resolved when the job runs, for example a task value that the upstream task didn't set, the task fails before it starts.

运行机制：

> **Autonomous execution with auto-approval** — When a Genie Code task runs, the agent executes your prompt autonomously. Auto-approval is always on for job tasks, so you do not confirm tool calls during the run. The agent uses an AI classifier to check each action against your prompt and blocks risky operations that fall outside the intended scope.
>
> **Auto-approve is a productivity feature, not a security boundary, and you cannot disable it for a job task.** Because the task runs unattended, be deliberate about what production data, sensitive workspaces, or shared resources the prompt can reach.
>
> **Reading upstream task outputs** — The agent can read the outputs of upstream tasks in the same job as part of its work.
>
> **Genie Code settings and MCP connections** — The task inherits your Genie Code settings and any custom Model Context Protocol (MCP) connections you have configured.

输出与下游引用：

> Each task run produces a continuable Genie Code conversation thread. … A downstream task can use the final response of a successful run with the dynamic value reference `{{tasks.<task_name>.output.response}}`, for example in a task parameter, an If/else condition, or another Genie Code task's prompt.

限制：

> - **Auto-approval cannot be disabled**: Job tasks always run with auto-approval on. You cannot turn it off or review actions before they execute.
> - **Jobs UI only**: To add a Genie Code task to a job, use the Jobs UI. Defining a Genie Code task in Declarative Automation Bundles, the Jobs API, or the Databricks SDKs isn't supported.

## 三、背景（新闻稿）

Databricks 新闻稿（2026-11-11，https://www.databricks.com/company/newsroom/press-releases/databricks-launches-new-always-genie-autonomous-ai-complex ）称第二代 Genie 为 "always-on, autonomous agent for enterprise work"：员工给出目标，Genie 持续推动工作。定时任务与 Job 任务是这一方向上的落地形态。
