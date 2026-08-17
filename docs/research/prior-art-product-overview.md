# Prior art — product overview

Status: draft

Surveyed 2026-08-15, extended 2026-08-16. Prices are list prices, monthly, in USD, before tax,
at the cheapest billing option shown. Licences are read from repository `LICENSE` files, not from
listicles.

## Purpose

[prior-art.md](./prior-art.md) maps systems to Clackworks layers and deliberately excludes vendor
detail. This file is that detail: what each named product actually is, who it is for, whether it
can be self-hosted on workable terms, what it costs, and how healthy the project is.

Deep dives on the four candidates worth reading in full are in [candidates/](./candidates/).

## Filters applied

Two constraints, both from [VISION.md](../../VISION.md#constraints):

1. **Self-hostable, and workable when self-hosted.** Not "has a community edition" — the free
   self-hosted build has to run the real use case. Open source is a bonus, not the bar;
   source-available is acceptable if the restrictions do not bite. Restrictions that do bite are
   recorded per product.
2. **Evaluable without paying, and licence-compatible with a later enterprise edition.** A free
   tier that caps workflows below what the use case needs cannot evaluate anything — Zapier's
   two-step ceiling is the canonical example. A licence that forbids the open-core path is
   disqualifying on its own.

Hosted-only products appear only where they teach something: a UX worth copying, or a pricing
band worth knowing.

Column conventions: **Self-host** is `yes`, `yes`* (possible, with restrictions worth reading),
or `no`. **Licence** is `OSS` (OSI-approved), `source-available` (published, restricted), or
`proprietary`. **Free ceiling** is what the free or self-hosted build gives you before money is
required.

## Workflow automation — producers to pipelines

| Product                                          | What it is                                                                                                                                                            | Self-host | Licence                                           | Paid from                                 |
| ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ------------------------------------------------- | ----------------------------------------- |
| **Zapier**                                       | The incumbent. Trigger → action chains, huge connector catalogue                                                                                                      | **no**    | proprietary                                       | Professional $19.99; Team $69             |
| **Make**                                         | Visual scenario builder, more branching power than Zapier; credit-based                                                                                               | **no**    | proprietary                                       | Core $12; Pro $21; Teams $38              |
| **Gumloop**                                      | AI-native flow builder, agents as the primary node type                                                                                                               | **no**    | proprietary                                       | Pro $37                                   |
| **Pipedream**                                    | Developer-first hosted automation; Workday agreed to acquire it, Nov 2025                                                                                             | **no**    | proprietary                                       | Cloud plans                               |
| **[Windmill](./candidates/windmill.md)**         | Rust engine, Postgres-only. Scripts and flows, branch steps, suspend/approval, AI agent steps, inbound SMTP triggers, worker groups                                   | yes       | OSS (AGPLv3) core + proprietary EE bits           | Team $10/user; Enterprise on request      |
| **[Sim](./candidates/sim.md)**                   | Block-graph workflow builder, agent-native, one trigger per workflow, Human in the Loop block with resume forms                                                       | yes       | OSS (Apache-2.0), whole repo                      | Cloud plans; EE is org-shaped features    |
| **[Activepieces](./candidates/activepieces.md)** | n8n-alike, "pieces" as TypeScript modules, Todos inbox as an approval surface                                                                                         | yes       | OSS (MIT) core + commercial `packages/ee`         | Plus $16 (annual); Team $166 (annual)     |
| **[OpenClaw](./candidates/openclaw.md)**         | Far more than a workflow tool. Gateway across WhatsApp/Telegram/Signal/iMessage/Matrix/Slack/Teams/Discord/IRC, multi-agent, Lobster pipelines-as-data, durable flows | yes       | OSS (MIT)                                         | —, everything                             |
| **n8n**                                          | Node-graph workflow engine, 1000+ integrations, first-class AI/agent nodes                                                                                            | yes       | source-available (Sustainable Use)                | Cloud Starter $20; Pro $50; Business $800 |
| **Dify**                                         | LLM-app and workflow platform with a visual builder                                                                                                                   | yes*      | **modified** Apache-2.0                           | Cloud plans                               |
| **Node-RED**                                     | Message-passing flow editor with an enormous node catalogue, IoT heritage                                                                                             | yes       | OSS (Apache-2.0)                                  | —                                         |
| **Flowise**                                      | LLM-application builder: chains, agents, RAG on a canvas, genuine HITL                                                                                                | yes       | OSS (Apache-2.0) core + commercial enterprise dir | Cloud plans                               |
| **Langflow**                                     | Same category as Flowise                                                                                                                                              | yes       | OSS (MIT)                                         | Cloud plans                               |
| **Automatisch**                                  | Thin open-source Zapier clone                                                                                                                                         | yes       | OSS (AGPL-3.0) + commercial `.ee.` files          | —                                         |

### What the free build actually gives you

| Product          | Free ceiling                                                                                                                                                                             | Does it bite?                                                                                                                                                                       |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Windmill**     | Full engine self-hosted; 3 workspaces, 50 users, 4 permission groups, 10 GiB object storage, **30-day job-run detail retention**, 100 emails/day on email triggers, git sync for 2 users | **Slightly.** Run retention and approval *forms* (enterprise) are the two that touch a single user; the rest is org-shaped                                                          |
| **Sim**          | Whole repo Apache-2.0, `docker compose` with Postgres and pgvector                                                                                                                       | No. EE is access-control groups, SSO, audit logging, workspace forking — org-shaped only                                                                                            |
| **Activepieces** | Full MIT core, unlimited flows and tasks                                                                                                                                                 | No. The EE folder (SSO, projects, RBAC, git sync, audit log, branding, embedding) is org-shaped                                                                                     |
| **OpenClaw**     | Whole repo, MIT                                                                                                                                                                          | No. Everything, self-hosted                                                                                                                                                         |
| **n8n**          | Self-host: unlimited workflows and executions, queue mode included                                                                                                                       | Sustainable Use Licence permits internal and non-commercial use only. No offering n8n-derived functionality to third parties, no white-labelling. Best-fitting engine in the survey |
| **Dify**         | Self-host: the platform                                                                                                                                                                  | Yes - modified Apache-2.0 that forbids operating a multi-tenant service without a commercial licence and forbids removing the console branding.                                     |
| **Zapier**       | 100 tasks/mo, **two-step Zaps only**                                                                                                                                                     | **Yes, fatally.** One trigger plus one action. No pipeline is expressible                                                                                                           |
| **Make**         | 1,000 credits/mo, **2 active scenarios**, 15-min minimum interval                                                                                                                        | **Yes.** Two live scenarios cannot host routing plus pipelines; the 15-min floor rules out reactive mail                                                                            |
| **Gumloop**      | None — 14-day trial                                                                                                                                                                      | **Yes.** No permanent free tier at all                                                                                                                                              |

**Ruled out on shape:** Node-RED (no gates, no durable resume; the licence was never the problem),
Flowise and Langflow (LLM-app builders — no producer catalogue, no inbound-mail path, no
pipeline-to-pipeline routing; good reference for agent-step UX), Automatisch (thin, and last
pushed 2026-02-11), IFTTT (smart-home shape). **Huginn** is pre-LLM with no AI-native path, and
remains the closest structural ancestor of the Clackworks idea — worth reading as design
reference.

The loss from dropping Zapier, Make, Gumloop and Pipedream is smaller than it looks: their value
is connector breadth, and all of them would carry personal mail and messages through someone
else's infrastructure. Keep them as UX reference for what a routing UI should feel like.

**OpenClaw** is far more than a workflow tool — a gateway across WhatsApp, Telegram, Signal,
iMessage, Matrix, Slack, Teams, Discord, IRC, multi-agent, with Lobster as its pipelines-as-data
and gate mechanism. Every *mechanism* in the vision exists here, self-hosted and MIT. What is
missing is the guarantee list in
[README.md](./README.md#what-appears-unclaimed). Full write-up, capability table, and
the Lobster detail: [candidates/openclaw.md](./candidates/openclaw.md).

## Execution, durability, human gates

The runtime underneath a workflow builder: durable state, retries, resume, and pausing a run for
a human. Every row runs on your own hardware.

| Product                   | What it is                                                                                                                    | Self-host | Licence                                            | Free ceiling (self-hosted)                                                                                       |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | --------- | -------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Temporal**              | Durable execution. Workflows are ordinary code; an event history replays on crash. Signals/queries/updates build a human gate | yes       | OSS (MIT)                                          | Everything. Cloud is optional convenience                                                                        |
| **Hatchet**               | Same category, lighter, Postgres-only. DAG workflows, durable sleep and durable events                                        | yes       | OSS (MIT)                                          | Everything                                                                                                       |
| **LangGraph**             | Graph-structured agent runtime; checkpointing, `interrupt()` for human gates                                                  | yes*      | OSS (MIT) library                                  | Full library. The standalone server runs without an enterprise key; the key gates self-hosted observability only |
| **Kestra**                | Declarative YAML orchestration with a first-class `Pause` task and `onResume` inputs                                          | yes       | OSS (Apache-2.0) core + EE                         | Full core. EE adds SSO, RBAC, audit                                                                              |
| **Trigger.dev**           | Durable background jobs for TypeScript, long-running by design                                                                | yes       | OSS (Apache-2.0)                                   | Everything; you own ops                                                                                          |
| **Conductor**             | Netflix-origin event-driven workflow engine, now agent-oriented                                                               | yes       | OSS (Apache-2.0)                                   | Everything; Orkes is the paid cloud                                                                              |
| **DBOS**                  | Durable workflows as a library on top of Postgres — no separate server                                                        | yes       | OSS (MIT)                                          | Everything                                                                                                       |
| **Inngest**               | Event-driven durable functions, steps as retry boundaries                                                                     | yes*      | source-available (SSPL, Apache-2.0 future licence) | Self-hosting supported since v1.0, vendor support not guaranteed                                                 |
| **LangChain Agent Inbox** | Reference UI for reviewing and resolving LangGraph interrupts                                                                 | yes       | OSS                                                | Everything                                                                                                       |

**Read:** these are engines, not products of the same kind as the section above — none has
connectors, a builder, or a gate UI. Under the one-product test they are all out; see
[prior-art.md](./prior-art.md#why-temporal-and-hatchet-are-not-contenders)
for why Temporal (too heavy) and Hatchet (DAG-shaped) were ruled out explicitly. For
[gates-and-directives.md](../../_incoming/gates-and-directives.md) the two credible self-hosted
patterns if Clackworks is *written* rather than configured are LangGraph `interrupt()` plus Agent
Inbox, and Kestra's database-persisted `Pause`.

**Rejected on licence,** both under [VISION.md](../../VISION.md#why-not-use-existing-tools):
Restate (BUSL-1.1, converts only on a delay) and Inngest (SSPL until each release ages out).
**Removed:** HumanLayer — hosted-only, and pivoted to an AI coding environment, so approvals are
no longer the product.

## Agent memory

| Product      | What it is                                                                   | Self-host | Licence          | Free ceiling |
| ------------ | ---------------------------------------------------------------------------- | --------- | ---------------- | ------------ |
| **Letta**    | Ex-MemGPT. Agents as stateful services; memory blocks the agent edits itself | yes       | OSS (Apache-2.0) | Everything   |
| **Mem0**     | Memory layer as an API. Extract, store, retrieve facts about a user          | yes       | OSS (Apache-2.0) | Everything   |
| **Graphiti** | Temporal knowledge graph for agents; facts carry validity windows            | yes       | OSS (Apache-2.0) | Everything   |
| **Cognee**   | Memory platform building a graph plus vector store over ingested data        | yes       | OSS (Apache-2.0) | Everything   |

**Read:** Letta's model — the agent as a persistent addressable thing rather than a function call
— matches [memory.md](../../_incoming/memory.md) and [agents.md](../../_incoming/agents.md) most
closely, and it is Apache-2.0 all the way down. Zep is gone: the `getzep/zep` repository is
examples and integrations, not a deployable server. **Graphiti**, the engine that was underneath
it, is the substitute; do not plan around Zep Community Edition.

## Security research

| Name      | What it is                                                                                                                                         | Licence | Cost |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ---- |
| **CaMeL** | DeepMind research pattern, not a product. A privileged LLM plans; a quarantined LLM handles untrusted data; a capability system enforces the split | Paper   | Free |

**Read:** relevant to [security-and-trust.md](../../_incoming/security-and-trust.md). A mail
producer taking mail from strangers is exactly the threat model CaMeL addresses. Adopting the
pattern constrains pipeline design; that trade-off is an open question, not a decision.

## Open-source health

Self-hostable candidates only. Snapshot 2026-08-15, extended 2026-08-16.

| Project      | Stars | Licence                                    | Last push  | Open issues |
| ------------ | ----- | ------------------------------------------ | ---------- | ----------- |
| OpenClaw     | 386k  | MIT                                        | 2026-08-15 | 5,527       |
| n8n          | 201k  | Sustainable Use Licence (fair-code, mixed) | 2026-08-15 | 1,302       |
| Mem0         | 63k   | Apache-2.0                                 | 2026-08-15 | 667         |
| LangGraph    | 40k   | MIT                                        | 2026-08-14 | 695         |
| Conductor    | 32k   | Apache-2.0                                 | 2026-08-15 | 243         |
| Cognee       | 30k   | Apache-2.0                                 | 2026-08-15 | 370         |
| Graphiti     | 30k   | Apache-2.0                                 | 2026-08-15 | 483         |
| Sim          | 29.4k | Apache-2.0                                 | daily      | —           |
| Kestra       | 28k   | Apache-2.0 (core)                          | 2026-08-15 | 545         |
| Letta        | 24k   | Apache-2.0                                 | 2026-08-14 | 43          |
| Activepieces | 24k   | MIT core + commercial EE                   | 2026-08-15 | 463         |
| Temporal     | 22k   | MIT                                        | 2026-08-15 | 895         |
| Windmill     | 17.5k | AGPLv3 + proprietary EE bits               | daily      | —           |
| Trigger.dev  | 16k   | Apache-2.0                                 | 2026-08-15 | 421         |
| Inbox Zero   | 12k   | AGPL-3.0                                   | 2026-08-15 | 180         |
| Mail-0/Zero  | 11k   | MIT                                        | 2026-05-26 | 17          |
| Hatchet      | 8k    | MIT                                        | 2026-08-15 | 137         |
| Inngest      | 6k    | SSPL with Apache-2.0 future licence        | 2026-08-15 | 234         |
| DBOS         | 2k    | MIT                                        | 2026-08-14 | 6           |

Only **Mail-0/Zero** looks unhealthy — three months without a commit on a young project.
**Automatisch** (not listed) was last pushed 2026-02-11 and is not carried.

## Licence risk if this becomes a product

See enterprise edition requirement in
[VISION.md](../../VISION.md#how-clackworks-itself-is-licensed).

| Licence                  | Products                                                                                                                                                              | If Clackworks ships as a product                                                                                                      |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| MIT / Apache-2.0         | Sim, Activepieces core, Temporal, LangGraph, Kestra, Hatchet, DBOS, Trigger.dev, Conductor, Letta, Mem0, Graphiti, Cognee, OpenClaw, Node-RED, Langflow, Flowise core | Clean. No obligations beyond attribution                                                                                              |
| AGPL-3.0                 | Windmill core, Inbox Zero, Automatisch                                                                                                                                | Network use triggers source disclosure. Fine internally; a decision if hosted for others, and a question if Clackworks links its code |
| Modified Apache-2.0      | Dify (rejected)                                                                                                                                                       | No multi-tenant service without a commercial licence; branding cannot be removed                                                      |
| SSPL (Apache-2.0 future) | Inngest (rejected)                                                                                                                                                    | Offering it as a service pulls in the whole service stack until each release ages out                                                 |
| Sustainable Use Licence  | n8n (rejected)                                                                                                                                                        | **Internal and non-commercial use only.** No offering n8n-derived functionality to third parties, no white-labelling                  |
| BUSL-1.1                 | Restate (rejected)                                                                                                                                                    | Restricted until the conversion date                                                                                                  |
