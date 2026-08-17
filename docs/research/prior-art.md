# Prior art

Status: draft

Surveyed 2026-08, against the constraints in [VISION.md](../../VISION.md#constraints). Product
detail comes from vendor docs and repository metadata, not from running instances. The landscape
moves fast; re-check before any build decision.

## Purpose

Every piece of Clackworks exists somewhere already today. The combination may not. This file
records what exists by layer, which products pass the product test in
[README.md](./README.md#the-bar), and the gap between what exists and what the vision claims.

Per-product reference — what each one is, self-host status, licence, cost, ceilings, project
health — lives in [prior-art-product-overview.md](./prior-art-product-overview.md).

## In scope

- Existing systems mapped to Clackworks layers.
- The shortlist under the one-product test, and how the use cases would be built on it.
- What is unclaimed, and whether being unclaimed means anything.
- The gap between what exists and what the vision claims.

## The landscape

| Clackworks layer         | Candidates (self-hostable)                                                                                                                                                                                           | Reference only                                                      |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| Producers → pipelines    | Windmill, Sim, Activepieces. Visual flows with AI steps as a first-class node type                                                                                                                                   | n8n and Dify (both ruled out on licence), Zapier, Make, Gumloop     |
| The chain-reaction shape | Huginn (2013, "agents that watch and act" — the whole idea without models)                                                                                                                                           | IFTTT                                                               |
| Pipeline execution       | Windmill and Sim flows; OpenClaw Lobster (pipelines-as-data DSL) and Task Flow (durable); LangGraph, Temporal, Kestra, Hatchet, Trigger.dev, Conductor, DBOS                                                         | Inngest (SSPL), Restate (BUSL) — both ruled out on licence          |
| Human gates              | Windmill suspend/approval steps; Sim Human in the Loop with a resume form; Activepieces Wait for Approval plus Todos inbox; OpenClaw Lobster resume tokens; LangGraph `interrupt()` plus Agent Inbox; Kestra `Pause` | HumanLayer — hosted, and pivoted away from approvals                |
| Persistent memory        | Letta (ex-MemGPT), Mem0, Graphiti, Cognee                                                                                                                                                                            | Zep — its community edition no longer exists as a deployable server |
| Agent identity and email | OpenClaw — isolated agents with own workspace, persona files, session store, memory vaults; Windmill — inbound SMTP with a per-flow address. Identity itself is text files in a repo; there is nothing here to buy   | —                                                                   |
| Personal, self-hosted    | OpenClaw — one local gateway across WhatsApp, Telegram, Signal, iMessage, Matrix, Slack                                                                                                                  | —                                                                   |
| Content-based routing    | Windmill `branch one` predicates, Sim Condition and Router blocks, Activepieces Router step; in OpenClaw a router agent plus `sessions_send` assembles it                                                            | Shortwave AI filters — route to labels, not to workers              |
| Mail triage and routing  | Inbox Zero (AGPL; a destination product, not a producer). Sieve/Rspamd as the non-AI baseline                                                                                                                        | Shortwave, Fyxer, Cora, Gmelius Meli, Sanebox                       |
| Data-versus-instruction  | CaMeL (DeepMind), dual-LLM pattern. Directly relevant to [security-and-trust.md](../../_incoming/security-and-trust.md)                                                                                              | —                                                                   |
| Exhaust                  | A dead-letter queue is a component of any automation platform, not a product. It belongs inside whatever runs the pipelines                                                                                          | —                                                                   |

Every layer has at least one credible self-hostable candidate, and the best single approximation
of the whole idea is one workflow product rather than an assembly.

## The all-in-one-product verdict

The test is in [README.md](./README.md#the-bar): validate one system, not an assembly, providing
pipelines, routing between pipelines, gates, filters and cycles, self-hostable, with a licence and
a free edition that fit [VISION.md](../../VISION.md#constraints).

| Product          | Licence                              | Pipelines | Routing between pipelines | Gates                    | Filters | Cycles                   | One-product verdict            |
| ---------------- | ------------------------------------ | --------- | ------------------------- | ------------------------ | ------- | ------------------------ | ------------------------------ |
| **Windmill**     | AGPLv3 + proprietary enterprise bits | yes       | yes (inner flows)         | yes (suspend/approval)   | yes     | loops + re-entry         | **Passes. Strongest fit**      |
| **Sim**          | Apache-2.0, whole repo               | yes       | yes (Workflow block)      | yes (Human in the Loop)  | yes     | loops + re-entry         | **Passes. Second**             |
| **Activepieces** | MIT core + `packages/ee`             | yes       | yes (sub-flows)           | yes (Wait for Approval)  | yes     | loops + re-entry         | Passes. Third                  |
| **OpenClaw**     | MIT                                  | yes       | agent-to-agent handoff*   | yes (resume tokens)      | yes     | yes                      | **Passes.** Routing hop is prompt-shaped, not data — the rest of the loop is |
| Flowise          | Apache-2.0 core + commercial dir     | partial   | weak                      | yes (HITL)               | weak    | yes                      | No. LLM-app builder shape      |
| Langflow         | MIT                                  | partial   | weak                      | weak                     | weak    | yes                      | No. LLM-app builder shape      |
| Node-RED         | Apache-2.0                           | yes       | yes (link nodes)          | no                       | yes     | yes                      | No. No gates, no durability    |
| Automatisch      | AGPL-3.0 + `.ee.` files              | thin      | no                        | no                       | thin    | no                       | No. Also ~6 months unpushed    |
| Dify             | **modified** Apache-2.0              | yes       | partial                   | partial                  | yes     | yes                      | Ruled out on licence           |
| n8n              | Sustainable Use Licence              | yes       | yes                       | yes                      | yes     | yes                      | Ruled out on licence           |
| Temporal         | MIT                                  | yes       | code, not config          | yes, if you build the UI | code    | yes                      | Ruled out — too heavy          |
| Hatchet          | MIT                                  | yes       | code, not config          | yes, if you build the UI | code    | DAG per run; child spawn | Ruled out — DAG-shaped         |

Licence exclusions are detailed in [prior-art-product-overview.md](./prior-art-product-overview.md#workflow-automation--producers-to-pipelines).

## Why Temporal and Hatchet are not contenders

Both keep getting mistaken for products of the same kind as Windmill and Sim. They are engines.

**Temporal** (MIT) is a durable execution engine. A workflow is ordinary code in Go, Java,
TypeScript, Python, .NET or PHP; Temporal records every step's result in an event history, and
after a crash it re-runs the code and replays recorded results instead of re-executing them, so
execution continues where it stopped. Side effects live in *activities* with retry policies and
timeouts. `Signals` deliver external input into a running workflow, `queries` read its state and
`updates` do both — that trio is how a human gate gets built. Timers survive restarts, so a
workflow can sleep for a month. Self-hosting means the server plus Postgres, MySQL or Cassandra,
optionally Elasticsearch for search. No connectors, no producers, no visual builder, no approval
UI, no notion of an LLM step.

**Hatchet** (MIT) is the same category, lighter and Postgres-only. A distributed task queue with
orchestration on top: DAG-shaped workflows, event triggers, cron, retries, timeouts, concurrency
keys with fairness, rate limits. Its **durable tasks** add durable sleep and durable events, so a
task can wait for an approval or a timeout without holding a worker slot and resume after a crash
without redoing completed work — the docs name human-in-the-loop and agent loops as the motivating
cases. SDKs are Python, TypeScript and Go. Same absences as Temporal.

Ruled out:

- **Temporal — too heavy.** Decided on operational weight, not capability. A server plus a
Postgres/MySQL/Cassandra store plus optional Elasticsearch, in front of a personal self-hosted
system, is more machinery than the rest of the stack put together. Steve Yegge hit the same wall
building Gastown and dropped it for the same reason.
- **Hatchet — DAG-shaped.** Clackworks is not acyclic: drafts go back for revision, PRs re-enter,
produced items route to the pipeline that produced them. Hatchet reaches cycles the long way
round, by having a durable task spawn children and loop in your own code, but then the cycle
lives in the code rather than in the machine, which is the thing being avoided.

Either is a plausible engine *under* a Clackworks that is written rather than configured.

## Not in scope

- Vendor evaluation, pricing, and project health. In
[prior-art-product-overview.md](./prior-art-product-overview.md).
- Per-candidate deep dives. In [candidates/](./candidates/).
