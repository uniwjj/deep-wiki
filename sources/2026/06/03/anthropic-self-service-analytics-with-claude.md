---
title: How Anthropic enables self-service data analytics with Claude
url: https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude
author: Chen Chang, Clement Peng, Justin Leder, Johanne Jiao, Josh Cherry (Anthropic Data Science & Data Engineering)
date: 2026-06-03
retrieved: 2026-06-08
type: article
ingested: 2026-06-08
wiki_pages: [agentic-analytics-anthropic.md]
---

# How Anthropic enables self-service data analytics with Claude

As many data science and data engineering teams can attest, enabling self-service business analytics has traditionally been a slog.

Making the data model more accessible to less technical coworkers via wide and denormalized tables often leads to overlapping views with inconsistent definitions as the business scales (and does little to bridge the gap for employees with little desire to learn SQL). Alternatively, creating more ringfenced environments for users often misses the long tail of business questions and leads to metric and dashboard bloat as teams silo their work.

The rise of LLMs provides an additional path for self-service analytics that avoids those challenges. However, pointing Claude at a warehouse and letting the agents execute can create a false sense of precision.

The initial elation of liberation from ad-hoc requests turns into dread with the realization that this setup separates stakeholders from the underlying infrastructure, documentation, and expertise that previously steered them toward carefully curated datasets. 

At Anthropic, 95% of business analytics queries are automated via Claude, with ~95% accuracy in aggregate. By giving this often rote, repetitive work to Claude, their data science team can focus on more strategic work like causal modeling, forecasting, and machine learning. 

After meeting with dozens of Anthropic's top Claude Code users and having seen myriad design patterns for analytics agents, they've cultivated some best practices for other data teams working with LLMs.

## Data is not software

LLMs' generative abilities are a double-edged sword: the mechanisms that enable creative solutions to complex problems can also hallucinate erroneous output. 

Coding is an open-ended solution space that rewards the models' creativity, while documentation and tests provide natural guardrails against hallucination. In contrast, for analytics use cases, there's often only a single correct answer using a single correct source in which there's no deterministic way of proving the correctness. 

For self-service agentic business analytics, the complexity mainly lies in the ambiguity of the data. The central problem comes down to the ability to map a user's question to specific and up-to-date entities in the data model and know the correct way of working with them.

### Three failure modes

1. **Concept <> entity ambiguity**: with hundreds of viable options in a data model (out of potentially millions of fields), the agent is unable to choose the correct fields that best answer a user's question. For example, in measuring the number of active users: what actions constitute being "active"? Do you include fraudulent users? What lookback window do you use?

2. **Data staleness**: data sources, business definitions, and schemas change constantly; assets and agent knowledge go stale and start returning subtly wrong answers.

3. **Retrieval failure**: the right information may actually be in the data model and properly annotated, but given the vastness of the search space, the agent simply doesn't find it.

## The agentic analytics stack

At Anthropic, the main way they minimize these three errors is via their agentic data stack. Each layer exists primarily to attack one or more of these problems: 

1. **Entity ambiguity**: data foundations and sources of truth shrink the space of plausible entities until there's a single governed answer. 
2. **Staleness**: maintenance and validation processes keep everything from rotting as the business changes.
3. **Retrieval failure**: skills make sure the agent reliably finds and correctly uses that answer. 

### Data foundations

The most important aspect of ensuring analytics agents are accurate is via strong data foundations, which include the data models, transforms, tests, and tables in a data warehouse, along with the metadata describing them. Standard data engineering and data quality practices such as dimensional modeling, shift-left testing, freshness and completeness checks on critical pipelines all still apply.

What changes is that the end user of the data model is no longer a data expert, but rather agents acting on behalf of users with varying degrees of data expertise. This shift presents a challenge in that the results can't require the user to validate the underlying correctness simply because the end user doesn't know.

Best practices:
- **Create canonical datasets**: By far the most common failure is that the agent can't map a concept to the single correct table, column, and metric definition. The fix is fewer, more heavily governed logical models: curate a small set of canonical, single source-of-truth datasets that are clearly owned, consumption-ready, and discoverable, then aggressively deprecate the near-duplicates.
- **Enforce your standards**: The foundations only hold if the canonical models and metric definitions are enforced by tooling (structural routing), by CI (changes that bypass them fail review), and by mandate (downstream teams build on the governed layer or explain why not).
- **Colocate artifacts**: Nearly all data code (modeling, semantic layer, reference docs, canonical dashboard definitions) lives in a single repo, with CI checks that protect cross-layer integrity.
- **Treat metadata as a first-class product**: Column and table descriptions, canonical metric definitions, grain documentation, valid value ranges, lineage, ownership, and model tiering must be maintained with the same rigor as the transformations themselves.

### Sources of truth

Sources of truth are the reference surfaces the agent consults to navigate the data warehouse. This layer reduces concept <> entity ambiguity.

Roughly in descending order of trust:
- **Semantic layer**: the compiled metric and dimension definitions. If a question maps cleanly to a defined metric, the agent calls a function and gets one number. Agents are structurally required (by skill instruction) to leverage the semantic layer first. One idea that didn't work: bootstrapping the semantic layer by having an LLM auto-generate metric definitions from raw tables and query logs. It produced plausible-looking definitions that encoded the very ambiguities they were trying to eliminate. Recommendation: generate the documentation with Claude, but have a human own the definition.
- **Lineage and the transformation graph**: when the semantic layer doesn't cover a question, lineage and table ranking let the agent reason about which upstream models feed a concept. This transforms "I don't know the metric" into "I know which governed model to aggregate from."
- **Query corpus**: historical SQL from dashboards, notebooks, and prior analyses. In practice, giving the agent direct raw retrieval access to thousands of prior queries moved accuracy by less than a point. What does work is distilling that corpus into structured per-domain reference docs and reusable analysis patterns described in skills.
- **Business context**: the layer most teams skip. An agent that doesn't understand your business will answer what the user asked, but not what they meant. Anthropic pipes in a company knowledge graph consisting of indexed docs, roadmaps, decision logs, and organizational structure.

### Skills

If the sources of truth are the agent's declarative knowledge (what a metric means), then a skill is its procedural knowledge: which sources to consult in what order, how to navigate ambiguous data, and what a finished analysis looks like.

In Claude Code, a skill is a folder of markdown the agent reads on demand. Without skills, Claude's ability to answer analytics questions accurately didn't exceed 21% on their evals. Adding skills gets these numbers consistently above 95% in aggregate and regularly around 99% in certain domains.

Best practices:
- **Create pairwise skills**: a knowledge skill acts as a thin top-level router that allows additional domain details to load on demand. An unbook skill encodes the process a senior analyst would follow: clarify the question, find sources, run the query, and loop the result through adversarial review sub-agents.
- **Create proper reference docs**: written for retrieval by an LLM. Reference docs describe tables (grain, scope, and exclusions), the mechanics of gotchas, and explicit routing triggers without prescriptive recipes that go stale.
- **Treat skill maintenance as a first class citizen**: Skill docs describe a data model that changes daily, so without active maintenance they're wrong within weeks. Their offline accuracy drifted from ~95% at launch to ~65% over a month before they treated this as an engineering problem.
- **Create a consistent experience across all surfaces**: the same skill must provide the same answer to questions in Slack, in the IDE, in a dashboard tool, and in standalone agent sessions.

### Reference doc skeleton

```markdown
# [Domain] Tables

## Quick Reference
### Business Context — [what this domain means in plain words]
### Entity Grain — [what one row represents]
### Standard Hygiene Filter — [the filter every query in this domain applies]

## Dimensions
- [How key dimensions are encoded, and how the same concept is named differently across tables]

## Key Tables
### [table_name]
- **Grain**: [...] · **Scope/exclusions**: [...]
- **Usage**: [when to use it, when NOT to, join keys, required filters]

## Gotchas
- [The wrong-answer modes a senior analyst would warn you about]

## Best Practices / Common Query Patterns
- [Default choices, standard cuts, worked patterns]

## Cross-References
- [Neighboring domain docs that own adjacent questions]
```

### Validation

#### Offline evaluations

Data teams often set up elaborate analytic environments without having any process to understand the accuracy of their analytics agents. Offline evals (simple question/answer pairs) address this gap.

Two kinds of offline evals:
- **Dashboard-based evals**: auto-generated by Claude (then human validated), covering the most common stakeholder questions.
- **Long tail evals**: feed Claude business context and have it generate plausible questions across the rest of the domain.

Best practices:
- **Anchor ground truth so it can't drift**: Pin every eval to a snapshot date, write it against a stable fact table, or have the grader judge the agent's query rather than its number.
- **Store results like telemetry**: Every run lands in a warehouse table with skill version, git SHA, model ID, per-assertion pass/fail, token count, and wall-clock.
- **Gate launches per domain**: A domain owner can't announce the agent to their stakeholders until the eval set clears some threshold (~90%).
- **Offline eval accuracy should be ~100%**: every correct answer should also be hitting the semantic layer.

#### Ablation techniques

Every structural decision is made by holding the offline eval set fixed, varying exactly one component, and comparing pass rates.

Key findings:
- Giving the agent direct grep access to entire dashboard/transformation SQL moved accuracy by less than a point. The bottleneck wasn't access to prior work, it was structure (mapping a question to the right entity).
- Stacking additional rounds of doc refinement past a certain point hit three consecutive net-negative iterations (docs were getting longer, not better).
- Swapping the adversarial reviewer to a cheaper model lost most of the accuracy wins for no real speedup.

#### Online validation

Steps taken:
- **Adversarial review**: employing a Claude skill to aggressively challenge all underlying assumptions increased accuracy by 6%, but at 32% more tokens and 72% higher latency.
- **Provenance footer**: every response carries which source tier it came from, how fresh the data is, and who owns the model.
- **Data quality checks**: basic checks to ensure the referenced field is up-to-date, complete, and has no anomalies.
- **Passive monitoring**: tracking share of queries that resolve through the semantic layer, and share of responses that use correction language.
- **Active correction harvesting**: a scheduled agent scans stakeholder channels for correction language, drafts a one-line fix to the relevant reference doc, and opens a PR tagged to the domain owner.

The failure mode none of this fully catches is the silent one: the answer is wrong but looks plausible and is used without objection.

## Getting started

Starting from zero, a handful of canonical datasets, a few dozen offline evals, and a thin knowledge skill will capture most of the upside.

Questions to align on:
- **How important is a correct answer today vs. in the future?** AI models progress quickly; building infrastructure for current model shortfalls may become moot.
- **How do you anticipate business complexity to change over time?** Some processes may be overkill for simple data models.
- **How technical is the audience?** Data scientists can recognize incorrect answers; less technical audiences need more guardrails.
- **How much are you willing to spend for improved accuracy?** Adversarial validation improves accuracy but at higher cost and latency.
- **What is your comfort around access controls and internal data privacy?** Agents are more performant with more context, but broad data access cuts against governance posture.

The greatest gains come from addressing each of the three failure modes: collapsing ambiguity into a single governed answer, making the answer easily discoverable, and flagging when either has gone stale.

## Appendix: Skill file skeleton

A template for the main warehouse skill, with internal specifics replaced by bracketed placeholders. Key sections:
- Priority paths for executing queries (managed connection → CLI fallback → auth request)
- Semantic layer as mandatory default path with required workflow (Load → Discover → Compile+Run → Fallback)
- MUST KNOW section: quick start workflow, business context, entity disambiguation, data integrity requirements
- HOW TO DO section: technical execution guide, analysis best practices, mandatory adversarial SQL review, provenance footer
- DATA REFERENCES section: knowledge base navigation per domain, troubleshooting guide, field naming gotchas
