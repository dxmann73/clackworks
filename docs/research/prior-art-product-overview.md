# Prior art — product overview

Status: draft

Surveyed 2026-08-15. Prices are list prices, monthly, in USD, before tax, at the cheapest
billing option shown. Every one of these will be wrong within a quarter — re-check before any
decision rests on a number.

## Purpose

[prior-art.md](./prior-art.md) maps systems to Clackworks layers but deliberately excludes
vendor detail. This file is that detail: what each named product actually is, who it is for,
whether it can be self-hosted on terms that are actually workable, and what it costs.
Reference material for the [falsification test](./README.md) and the build-versus-buy
question, not a decision.

## Filters applied

Two constraints, both from [VISION.md](../../VISION.md#constraints), decide what belongs in this
file:

1. **Self-hostable, and workable when self-hosted.** Not "has a community edition" — the free
   self-hosted build has to be capable enough to run the real use case. Open source is a bonus,
   not the bar; source-available is acceptable if the restrictions do not bite. Restrictions
   that do bite are recorded per product.
2. **Evaluable without paying.** A free tier that caps workflows below what the use case needs
   cannot be used to evaluate anything. Zapier's two-step ceiling is the canonical example.
   Clackworks itself is community-first with an enterprise edition left possible, so a
   dependency whose licence forbids that path is disqualified too.

Hosted-only products are named only where they teach something — a UX worth copying, or a
pricing band worth knowing. They are never candidates. A hosted product with no self-hostable
counterpart is **not** thereby evidence of a gap: see
[prior-art.md](./prior-art.md#what-appears-unclaimed) for why absence has three different
causes and only one of them is interesting.

## How to read the columns

- **Self-host** — `yes`, `yes*` (possible, with restrictions worth reading), or `no`.
- **Licence** — `OSS` (OSI-approved), `source-available` (published, restricted), or
  `proprietary`.
- **Free ceiling** — what the free or self-hosted build gives you before money is required.

## Workflow automation — producers to pipelines

The five shortlisted. The self-host filter cuts three of them, and the licence constraint in
[VISION.md](../../VISION.md#why-not-use-existing-tools) cuts n8n as well — reference only, not a
candidate.

| Product          | What it is                                                                | Typical use case                               | Self-host | Licence                              | Paid from                                 |
| ---------------- | ------------------------------------------------------------------------- | ---------------------------------------------- | --------- | ------------------------------------ | ----------------------------------------- |
| **n8n**          | Node-graph workflow engine, 1000+ integrations, first-class AI/agent nodes | Glue between SaaS apps; self-hosted automation  | yes       | source-available (Sustainable Use)   | Cloud Starter $20; Pro $50; Business $800  |
| **Activepieces** | n8n-alike, MIT core, "pieces" as TypeScript modules                        | Same as n8n, cleaner licence                    | yes       | OSS (MIT) core + commercial EE folder | Plus $16 (annual); Team $166 (annual)     |
| **Zapier**       | The incumbent. Trigger → action chains, huge connector catalogue           | Non-technical business automation               | **no**    | proprietary                          | Professional $19.99; Team $69              |
| **Make**         | Visual scenario builder, more branching power than Zapier; credit-based    | Ops automation with visual data mapping         | **no**    | proprietary                          | Core $12; Pro $21; Teams $38               |
| **Gumloop**      | AI-native flow builder, agents as the primary node type                    | AI-heavy business workflows                     | **no**    | proprietary                          | Pro $37                                    |

### What the free build actually gives you

| Product          | Free ceiling                                                        | Does it bite?                                                                   |
| ---------------- | -------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| **n8n**          | Self-host: unlimited workflows, unlimited executions, queue mode      | No. Missing pieces are org-shaped: SSO, projects/RBAC, environments, external secrets, log streaming, multi-main, workflow sharing, **git version control** |
| **Activepieces** | Self-host: full MIT core, unlimited flows and tasks                   | No. EE folder (SSO, projects, RBAC, git sync, audit log, branding, embedding) needs a paid licence |
| **Zapier**       | 100 tasks/mo, **two-step Zaps only**                                  | **Yes, fatally.** One trigger plus one action. No multi-step, so no pipeline is expressible. Cannot evaluate anything real |
| **Make**         | 1,000 credits/mo, **2 active scenarios**, 15-min minimum interval     | **Yes.** Two live scenarios cannot host a routing layer plus pipelines. 15-min floor rules out reactive mail handling |
| **Gumloop**      | None — 14-day trial                                                   | **Yes.** No permanent free tier at all                                            |

**Read:** On the self-host and free-tier filters only **n8n** and **Activepieces** survive, and
both survive comfortably — the free self-hosted builds are the full product minus organisation
features one person does not need. n8n's queue mode being included is the notable one: real
scaling is not paywalled. The licence filter then removes n8n as well, leaving **Activepieces**
as the one candidate in this section.

Between the two: n8n has vastly more integrations and mindshare, Activepieces has the cleaner
licence. Since [VISION.md](../../VISION.md#constraints) keeps the enterprise path open, that licence
difference is not cosmetic — see [Licence risk](#licence-risk-if-this-becomes-a-product) below.

The loss from dropping Zapier, Make, and Gumloop is smaller than it looks: their value is
connector breadth, and all three would carry personal mail and messages through someone else's
infrastructure, which is a [security-and-trust.md](../../_incoming/security-and-trust.md)
problem before it is a feature comparison. Keep them as UX reference for what a routing UI
should feel like.

**Excluded earlier:** Windmill (developer internal-tooling shape), Node-RED and IFTTT (IoT and
smart-home shape), Huginn (pre-LLM, no AI-native path). Huginn remains the closest structural
ancestor of the Clackworks idea and is worth reading as a design reference.

## Execution, durability, human gates

The runtime underneath a workflow builder: durable state, retries, resume, and pausing a run
for a human. Hosted-only options removed; every row below runs on your own hardware.

| Product         | What it is                                                                   | Typical use case                               | Self-host | Licence                       | Free ceiling (self-hosted)              |
| --------------- | ------------------------------------------------------------------------------ | ---------------------------------------------- | --------- | ----------------------------- | ---------------------------------------- |
| **LangGraph**   | Graph-structured agent runtime; checkpointing, `interrupt()` for human gates    | Agent apps needing resumable state             | yes*      | OSS (MIT) library             | Full library. Standalone server runs without an enterprise key; the key gates self-hosted observability only. Verify before relying on it |
| **Temporal**    | Durable execution. Workflows survive crashes, restarts, and deploys             | Long-running, must-not-lose processes          | yes       | OSS (MIT)                     | Everything. Cloud is optional convenience |
| **Kestra**      | Declarative YAML orchestration with a first-class `Pause` task and `onResume` inputs | Scheduled and event-driven flows with approvals | yes       | OSS (Apache-2.0) core + EE    | Full core. EE adds SSO, RBAC, audit      |
| **Hatchet**     | Postgres-backed task orchestration aimed explicitly at background jobs and AI agents | Durable agent steps without Temporal's weight | yes       | OSS (MIT)                     | Everything                               |
| **Trigger.dev** | Durable background jobs for TypeScript, long-running by design                  | Async work in a JS/TS product                  | yes       | OSS (Apache-2.0)              | Everything; you own ops                  |
| **Inngest**     | Event-driven durable functions, steps as retry boundaries                       | Background jobs and queues without infra       | yes*      | source-available (SSPL, Apache-2.0 future licence) | Self-hosting supported since v1.0, but vendor support is not guaranteed |
| **Conductor**   | Netflix-origin event-driven workflow engine, now agent-oriented                 | Large-scale service orchestration              | yes       | OSS (Apache-2.0)              | Everything; Orkes is the paid cloud      |
| **DBOS**        | Durable workflows as a library on top of Postgres — no separate server          | Durability without running an orchestrator     | yes       | OSS (MIT)                     | Everything                               |
| **LangChain Agent Inbox** | Reference UI for reviewing and resolving LangGraph interrupts         | The approval queue for a LangGraph app         | yes       | OSS                           | Everything                               |

**Read:** For [gates-and-directives.md](../../_incoming/gates-and-directives.md) there are now two credible
self-hosted patterns rather than a hosted service. **LangGraph** `interrupt()` plus **Agent
Inbox** is the agent-native one. **Kestra**'s `Pause` task is the workflow-native one, and it
persists the paused state in the database so an execution survives a restart — which is the
actual hard requirement behind "a human gate can sit at any point".

For durability alone, **Hatchet** and **DBOS** are the right weight class for a personal
system; **Temporal** is correct and considerably more machine than one person needs.

**Removed:** HumanLayer (hosted-only, and pivoted away from approval-as-a-service anyway).
**Rejected on licence,** both under [VISION.md](../../VISION.md#why-not-use-existing-tools):
Restate — genuinely good durable execution, but BUSL-1.1 is not OSS and converts only on a
delay; and Inngest, SSPL until each release ages out. Listed above for reference, not as
candidates.

## Agent memory

| Product      | What it is                                                                  | Typical use case                          | Self-host | Licence           | Free ceiling (self-hosted) |
| ------------ | ----------------------------------------------------------------------------- | ----------------------------------------- | --------- | ----------------- | --------------------------- |
| **Letta**    | Ex-MemGPT. Agents as stateful services; memory blocks the agent edits itself   | Agents that must remember across months   | yes       | OSS (Apache-2.0)  | Everything                  |
| **Mem0**     | Memory layer as an API. Extract, store, retrieve facts about a user            | Bolting memory onto an existing agent     | yes       | OSS (Apache-2.0)  | Everything                  |
| **Graphiti** | Temporal knowledge graph for agents; facts carry validity windows              | Memory where "when was this true" matters | yes       | OSS (Apache-2.0)  | Everything                  |
| **Cognee**   | Memory platform building a graph plus vector store over ingested data          | Persistent memory across heterogeneous sources | yes   | OSS (Apache-2.0)  | Everything                  |

**Read:** Letta's model — the agent as a persistent addressable thing rather than a function
call — matches [memory.md](../../_incoming/memory.md) and
[agents.md](../../_incoming/agents.md) most closely, and it is Apache-2.0 all the way down.

**Replaced:** hosted Zep is gone from this list, and not only on the self-host filter — the
`getzep/zep` repository is now examples and integrations, not a deployable server. The engine
that was underneath it, **Graphiti**, is Apache-2.0, self-hostable, and more actively developed
than the product that was built on it. Use Graphiti; do not plan around Zep Community Edition.

## Personal, self-hosted assistants

| Product      | What it is                                                                        | Typical use case                          | Self-host | Licence          | Free ceiling            |
| ------------ | ---------------------------------------------------------------------------------- | ----------------------------------------- | --------- | ---------------- | ----------------------- |
| **OpenClaw** | Far more than an assistant — see below. Gateway across WhatsApp/Telegram/Signal/iMessage/Matrix/Slack/Teams/Discord/IRC, multi-agent, pipelines, durable flows | Personal assistant reachable from any chat app | yes | OSS (MIT)        | Everything              |
| **Khoj**     | Self-hostable "second brain": RAG over your docs, agents, scheduled automations      | Personal search and research assistant    | yes       | OSS (AGPL-3.0)   | Everything, self-hosted |
| **Leon**     | Open-source personal assistant, skill-based, largely pre-LLM architecture            | DIY voice/text assistant                  | yes       | OSS (MIT)        | Everything              |

**Read:** OpenClaw is the strongest candidate for the messaging half of
[producers.md](../../_incoming/producers.md) — that integration work is genuinely done,
maintained, and MIT. Its full-system-access model is a direct problem for
[security-and-trust.md](../../_incoming/security-and-trust.md) and must not be adopted
uncritically.

### OpenClaw in detail, checked 2026-08-15

Checked specifically because the working assumption — that it lacked pipelines, gates, and
multiple agents with separate memories — was wrong. What it actually has:

| Capability                | What exists                                                                                                                 | Clackworks area                                     |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------- |
| Deterministic pipelines   | **Lobster** — multi-step tool pipelines as a small constrained DSL, run as one tool call. Pipelines are data: loggable, diffable, replayable | [pipelines.md](../../_incoming/pipelines.md)                       |
| Human gates               | Lobster halts on side effects (send, post, delete) and returns a resume token; approve and resume without re-running earlier steps | [gates-and-directives.md](../../_incoming/gates-and-directives.md)       |
| Durable orchestration     | **Task Flow** — multi-step flows with status, JSON state, revision counter, `waiting`/`blocked` states; survives gateway restarts | [pipelines.md](../../_incoming/pipelines.md)                       |
| Many agents, own memory   | Isolated agents, each with workspace, persona files (`SOUL.md`, `IDENTITY.md`, `USER.md`), own SQLite session store, per-agent memory-wiki vaults | [agents.md](../../_incoming/agents.md), [memory.md](../../_incoming/memory.md)   |
| Persona inbound addressing | **Bindings** map a channel account — a Slack workspace, a WhatsApp number — to one agent                                       | [personas-and-identity.md](../../_incoming/personas-and-identity.md) |
| Command-level approval    | **Exec approvals** — host guardrail with `deny`/`allowlist`/`ask`/`auto`/`full`, per agent                                     | [security-and-trust.md](../../_incoming/security-and-trust.md)     |
| Scheduling                | Cron jobs and background tasks invoking agent sessions                                                                        | [triggers-and-scheduling.md](../../_incoming/triggers-and-scheduling.md) |

The docs' own worked example for Lobster is recurring email triage with an approval halt before
sending drafts. That is UC-1 minus the classification step.

Two more, added after checking the obvious workaround:

| Capability            | What exists                                                                                     | Clackworks area                    |
| --------------------- | --------------------------------------------------------------------------------------------------- | ----------------------------------- |
| Email                 | IMAP/SMTP tool, or Gmail over OAuth. Not a *binding* target, but fully reachable                     | [producers.md](../../_incoming/producers.md)      |
| Agent-to-agent handoff | `sessions_send` targeting another agent's session key (`agent:tax-advisor:main`), same gateway only  | [routing.md](../../_incoming/routing.md)          |

Together those close the last apparent gap: a router agent classifies and hands off, so
content-based routing into personas is assemblable today without writing a platform.

What it genuinely does **not** provide — properties rather than parts:

- **Routing as inspectable data.** A router agent's judgement is a prompt: not diffable, not
  versioned, not answerable offline. Binding precedence is address-only — peer, guild, team,
  account, channel.
- **One normalized item shape.** No common shape, provenance, or dedup across heterogeneous
  producers.
- **End-to-end item lineage.** Sessions and tasks are traceable; an item crossing pipelines and
  re-entering as a pipeline's output is not a concept, and no item id survives a `sessions_send`.
- **An exhaust as a loop.** An agent can write unroutable items somewhere; nothing clusters
  them, proposes a pipeline, or re-processes a backlog.

**Read:** every *mechanism* in the Clackworks vision exists here, self-hosted and MIT. What is
missing is a set of *guarantees*. Whether those are a system or a plugin is now the central
question — see the falsification test result in [README.md](./README.md).

## Mail triage

The commercial mail-triage products — SaneBox, Cora, Fyxer, Shortwave, Gmelius — are all
hosted-only and all require handing over a mailbox. Under the self-host constraint none of them
is a candidate. They remain useful as UX reference for UC-1, and their pricing band ($5–$120)
is the honest benchmark for what this capability is worth to people. Detail dropped from this
file; see git history if needed.

Self-hostable options:

| Product         | What it is                                                                | Self-host | Licence          | Free ceiling            | State                                    |
| --------------- | --------------------------------------------------------------------------- | --------- | ---------------- | ----------------------- | ---------------------------------------- |
| **Inbox Zero**  | AI email assistant: categorises, drafts, bulk-unsubscribes, rule automation  | yes       | OSS (AGPL-3.0)   | Everything, self-hosted | Active. Gmail and Outlook                |
| **Mail-0/Zero** | Open-source AI email client, privacy-first framing                           | yes       | OSS (MIT)        | Everything, self-hosted | **Stalling** — no commits since 2026-05  |
| **Sieve / Rspamd** | Server-side rule filtering. No AI, decades old, completely reliable       | yes       | OSS              | Everything              | The boring baseline worth measuring against |

**Read:** **Inbox Zero** is the only maintained self-hostable AI mail assistant found, and it is
AGPL — fine for personal use, a real constraint if Clackworks ever ships as a product with its
code inside. It is a *destination* product (it triages into its own UI), not a producer, so it
competes with UC-1 rather than composing with it.

Worth stating plainly: Sieve rules plus a classifier is a viable UC-1 v1 and needs no product
from this table at all.

## Security research

| Name      | What it is                                                                                              | Licence | Cost |
| --------- | -------------------------------------------------------------------------------------------------------- | ------- | ---- |
| **CaMeL** | DeepMind research pattern, not a product. A privileged LLM plans; a quarantined LLM handles untrusted data; a capability system enforces the split | Paper   | Free |

**Read:** Relevant to [security-and-trust.md](../../_incoming/security-and-trust.md). A mail
producer taking mail from strangers is exactly the threat model CaMeL addresses. Adopting the
pattern constrains pipeline design; that trade-off is an open question, not a decision.

## Open-source health

Self-hostable candidates only. Snapshot 2026-08-15.

| Project      | Stars | Licence                                    | Last push  | Open issues |
| ------------ | ----- | ------------------------------------------ | ---------- | ----------- |
| OpenClaw     | 386k  | MIT                                        | 2026-08-15 | 5,527       |
| n8n          | 201k  | Sustainable Use Licence (fair-code, mixed) | 2026-08-15 | 1,302       |
| Mem0         | 63k   | Apache-2.0                                 | 2026-08-15 | 667         |
| LangGraph    | 40k   | MIT                                        | 2026-08-14 | 695         |
| Khoj         | 37k   | AGPL-3.0                                   | 2026-08-02 | 132         |
| Conductor    | 32k   | Apache-2.0                                 | 2026-08-15 | 243         |
| Cognee       | 30k   | Apache-2.0                                 | 2026-08-15 | 370         |
| Graphiti     | 30k   | Apache-2.0                                 | 2026-08-15 | 483         |
| Kestra       | 28k   | Apache-2.0 (core)                          | 2026-08-15 | 545         |
| Letta        | 24k   | Apache-2.0                                 | 2026-08-14 | 43          |
| Activepieces | 24k   | MIT core + commercial EE                   | 2026-08-15 | 463         |
| Temporal     | 22k   | MIT                                        | 2026-08-15 | 895         |
| Leon         | 17k   | MIT                                        | 2026-08-13 | 109         |
| Trigger.dev  | 16k   | Apache-2.0                                 | 2026-08-15 | 421         |
| Inbox Zero   | 12k   | AGPL-3.0                                   | 2026-08-15 | 180         |
| Mail-0/Zero  | 11k   | MIT                                        | 2026-05-26 | 17          |
| Hatchet      | 8k    | MIT                                        | 2026-08-15 | 137         |
| Inngest      | 6k    | SSPL with Apache-2.0 future licence        | 2026-08-15 | 234         |
| DBOS         | 2k    | MIT                                        | 2026-08-14 | 6           |

Only **Mail-0/Zero** looks unhealthy — three months without a commit on a young project.

## Licence risk if this becomes a product

Self-hosted personal use makes every licence here workable. The enterprise edition
[VISION.md](../../VISION.md#how-clackworks-itself-is-licensed) leaves open does not, and the
difference is worth knowing before code depends on it.

| Licence                        | Products                                       | If Clackworks ships as a product                              |
| ------------------------------ | ---------------------------------------------- | -------------------------------------------------------------- |
| MIT / Apache-2.0               | Activepieces core, Temporal, LangGraph, Kestra, Hatchet, DBOS, Trigger.dev, Conductor, Letta, Mem0, Graphiti, Cognee, OpenClaw, Leon | Clean. No obligations beyond attribution                        |
| AGPL-3.0                       | Khoj, Inbox Zero                               | Network use triggers source disclosure. Fine internally, a decision if hosted for others |
| SSPL (Apache-2.0 future)       | Inngest                                        | Offering it as a service pulls in the whole service stack until each release ages out |
| Sustainable Use Licence        | n8n                                            | **Internal and non-commercial use only.** No offering n8n-derived functionality to third parties, no white-labelling |
| BUSL-1.1                       | Restate (rejected)                             | Restricted until conversion date                                |

**Read:** this is the one place the two constraints collide. n8n is the best workflow engine here
and its licence is the one that forbids the enterprise path. Activepieces is the same shape under
MIT. While [VISION.md](../../VISION.md#constraints) keeps that path open, the asymmetry should
decide, not the integration count.

## Vendor drift found while checking sources

All of these were folded into [prior-art.md](./prior-art.md) on 2026-08-15. Kept here as the
record of what changed and when, since the same claims will drift again.

- **Zep Community Edition no longer exists** as a deployable server; the repository is examples
  and integrations. **Graphiti** — the engine that was underneath it — is the substitute.
- **HumanLayer** has pivoted to an AI coding environment; the human-gate approval API is no
  longer the headline product. It is also hosted-only, so it fails the self-host filter twice.
- **Gumloop** no longer has a free tier — trial only.
- **Shortwave** dropped its free tier; entry is $30.

## Open questions

- Does the human gate come from the workflow engine (Kestra-style `Pause`) or the agent runtime
  (LangGraph `interrupt()`)? The two imply different shapes for
  [pipelines.md](../../_incoming/pipelines.md).
- Is Inbox Zero a component, a competitor, or a producer? It is a destination product today.
- What does the assembled self-hosted stack cost per month at personal volume, counting model
  tokens and the machine it runs on? Nobody has costed this, and self-hosting moves the cost
  rather than removing it.

## Not in scope

- Recommending one. This is a survey.
- Feature-by-feature comparison matrices. The falsification test in
  [README.md](./README.md) is the cheaper way to find what matters.
