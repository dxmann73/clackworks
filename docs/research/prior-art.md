# Prior art

Status: draft

Surveyed 2026-08, revised 2026-08-15 against the constraints in
[VISION.md](../../VISION.md#constraints). Landscape moves fast; re-check before any build decision.

## Purpose

Every piece of Clackworks exists somewhere already. The combination does not. This file records
what exists, what is genuinely unclaimed, and what to steal rather than reinvent.

Per-product detail — what each one is, self-host status, licence, cost, free-tier ceiling —
lives in [prior-art-product-overview.md](./prior-art-product-overview.md). This file stays at
the level of layers and gaps.

## In scope

- Existing systems mapped to Clackworks layers.
- The gaps that justify building at all.
- The gap between what exists and what the vision claims.

## The landscape

| Clackworks layer          | Candidates (self-hostable)                                                                | Reference only                                                            |
| ------------------------- | ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| Producers → pipelines     | Activepieces. Visual flows with AI nodes added on                                            | n8n (ruled out on licence, see below), Zapier, Make, Gumloop                  |
| The chain-reaction shape  | Huginn (2013, "agents that watch and act" — the whole idea without models)                   | IFTTT                                                                        |
| Pipeline execution        | OpenClaw Lobster (pipelines-as-data DSL) and Task Flow (durable, restart-surviving); LangGraph, Temporal, Kestra, Hatchet, Trigger.dev, Conductor, DBOS | Inngest (SSPL), Restate (BUSL) — both ruled out on licence |
| Human gates               | OpenClaw Lobster — approval checkpoints with resume tokens; LangGraph `interrupt()` plus Agent Inbox; Kestra `Pause`, paused state persisted to database | HumanLayer — hosted, and pivoted away from approvals |
| Persistent memory         | Letta (ex-MemGPT), Mem0, Graphiti, Cognee                                                    | Zep — its community edition no longer exists as a deployable server          |
| Agent identity and email  | OpenClaw — isolated agents with own workspace, persona files, session store and memory vaults; bindings address chat channels, and an IMAP/SMTP tool covers mail. Identity is text files in a repo; there is nothing here to buy | — |
| Personal, self-hosted     | OpenClaw — one local gateway across WhatsApp, Telegram, Signal, iMessage, Matrix, Slack. Khoj, Leon | —                                                                      |
| Content-based routing     | Nothing native. OpenClaw bindings match on address only (peer, guild, team, account, channel) — but a router agent plus `sessions_send` assembles it | Shortwave AI filters — route to labels, not to workers |
| Mail triage and routing   | Inbox Zero (AGPL; a destination product, not a producer). Sieve/Rspamd as the non-AI baseline | Shortwave, Fyxer, Cora, Gmelius Meli, Sanebox                               |
| Data-versus-instruction   | CaMeL (DeepMind), dual-LLM pattern. Directly relevant to [security-and-trust.md](../../_incoming/security-and-trust.md) | —                                                               |
| Exhaust                   | A dead-letter queue is a component of any automation platform, not a product. Build it as part of whatever runs the pipelines | — |

Closest single approximation to the whole idea: **OpenClaw + a workflow engine + Letta**,
assembled by hand. Many people are already doing roughly that with n8n as the engine; all the
parts are self-hostable, so the shape of the assembly is not ruled out — only n8n itself is, on
licence.

One layer has an empty candidate column: **content-based routing** — and even that is
assemblable from parts OpenClaw already ships, as the falsification test in
[README.md](./README.md) shows. Earlier
revisions of this file read empty columns as evidence of opportunity. That inference was wrong
and is corrected below.

Note how much OpenClaw now occupies. It appears as a candidate in four rows. Anything Clackworks
claims as novel has to survive the question "does OpenClaw already do this", and as of
2026-08-15 the answer is yes more often than this document originally assumed.

## What appears unclaimed

Earlier revisions treated "no product does this" as evidence that something was unclaimed. That
does not follow. Absence from the market has three causes, and only one of them is interesting:

- **Too trivial to sell.** Nobody productises it because anyone who needs it builds it in an
  afternoon.
- **Not a product.** It is a component of something larger, so it ships inside other things and
  never appears as a line item.
- **Genuinely hard or genuinely unserved.** The interesting case.

Re-reading the survey against that test:

1. **Agent identity — trivial, struck.** No self-hostable identity product exists because
   identity is a handful of text files in a repository. OpenClaw demonstrates exactly this:
   `SOUL.md`, `IDENTITY.md`, `USER.md` in a per-agent workspace. Clackworks should copy that
   shape and claim nothing for it. The hosted "AI employee" vendors were never prior art for
   this; they are sales tools that happen to name their agents.
2. **Exhaust — not a product, struck as a differentiator.** A dead-letter queue is standard
   plumbing and belongs inside the automation platform, not beside it. What remains worth
   specifying is the *loop over* the queue — cluster the unroutable pile, propose "these
   fourteen items want a pipeline", help build it, re-process the backlog. That is a workflow and UI
   claim about a personal system, not a novel mechanism, and it should be argued on whether it
   is pleasant to use rather than on whether anyone else sells it.
3. **Inbound mail addressing per persona — unserved, and small.** OpenClaw binds chat accounts
   to agents; nothing binds mail addresses. With a provider catch-all plus a forwarding
   integration this is an afternoon's work too. Real, but not a reason to build a system.
4. **Content-based routing across all producers — also expressible, see the falsification
   test in [README.md](./README.md).** Every surveyed system *binds* on where an item arrived:
   which account, which channel, which webhook. But a router agent that classifies and then calls `sessions_send`
   reproduces the behaviour in OpenClaw today. What is not reproduced is routing as inspectable,
   testable data rather than a prompt — a difference in guarantees, not in capability.
5. **Re-entrancy with no back doors.** Chained items re-entering through routing
   and the next pipeline's ingress, so lineage and security policy apply uniformly. Frameworks
   let agents call agents directly and the trace degrades. Supporting detail for item 4 rather
   than a claim of its own.

Net after running the falsification test: **no mechanism in this vision is unavailable today.**
What is left is a set of guarantees — inspectable routing, item lineage, dedup, a worked
exhaust — and the open question of whether those are a system or a plugin.

## Honest assessment

- Overlap with existing tools is larger than the original ~70% estimate. Verifying OpenClaw
  moved pipelines, human gates, agent memory, and chat-channel persona binding from "to build"
  to "already exists, self-hosted, MIT". Striking agent identity and the exhaust as
  differentiators moved two more. What is left is content-based routing as inspectable data, and
  the loop over the unroutable pile as a matter of usability rather than novelty.
- That is a thinner claim than this document started with, and it is still enough. A
  self-hosted system for one person does not have to be novel to be worth having, and
  [VISION.md](../../VISION.md#constraints) commits to that shape first. The risk is not that the
  idea is small; it is pretending it is bigger and building accordingly.
- If the differentiator drifts off routing and the exhaust loop, this becomes a worse
  Activepieces or a worse OpenClaw.
- Scope is large enough to never ship. Eighteen spec areas and five use cases with no stack is
  a real risk, not a hypothetical one — and it is now clearly disproportionate to what is
  actually being added.
- The self-hosting constraint costs less than expected. It removes hosted convenience, not
  capability: every layer has a credible self-hostable candidate except content-based routing.

## The falsification test

Run on two arms — OpenClaw as one integrated system, and an Activepieces + Letta assembly —
and written up in [README.md](./README.md). Both arms express UC-1 in full, so neither produced
a reason to exist. What survived is a guarantee list, not a mechanism
list: [What survives arm 1](./README.md#what-survives-arm-1).

## Open questions

- Does Clackworks build on one of these (Activepieces as the execution layer, Letta for memory)
  or start clean? Building on top is the default assumption until falsified. n8n is not a
  candidate — [VISION.md](../../VISION.md#why-not-use-existing-tools) rules its Sustainable Use
  Licence out, so the engine question is Activepieces or nothing.
- Does the human gate come from the workflow engine (Kestra-style `Pause`) or from the agent
  runtime (LangGraph `interrupt()`)? The two imply different shapes for
  [pipelines.md](../../_incoming/pipelines.md).
- **Is Clackworks a layer on OpenClaw rather than a system beside it?** OpenClaw supplies
  channels, pipelines with gates, agents with memory, and durable orchestration. Content-based
  routing, the ingress, and the exhaust would be the addition. This is now the leading
  build-on-top candidate and it did not exist as an option when this file was written.
- Is OpenClaw's channel gateway reusable as the producer layer for messaging, given that its
  full-system-access model conflicts with [security-and-trust.md](../../_incoming/security-and-trust.md)?
- Is CaMeL's data/instruction separation adoptable directly, or does it constrain pipeline
  design too much?
- Content-based routing is the surviving claim. Is that a product, or a feature someone adds to
  n8n or OpenClaw in a month once it is specified properly?

## Not in scope

- Vendor evaluation and pricing. Moved to
  [prior-art-product-overview.md](./prior-art-product-overview.md).
- Build-versus-buy decision. Recorded as an open question, deliberately not answered yet.
- The falsification test itself, and what it decided. In [README.md](./README.md).
