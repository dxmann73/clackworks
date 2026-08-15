# Prior art

Status: draft

Surveyed 2026-08. Landscape moves fast; re-check before any build decision.

## Purpose

Every piece of Clackworks exists somewhere already. The combination does not. This file records
what exists, what is genuinely unclaimed, and what to steal rather than reinvent.

## In scope

- Existing systems mapped to Clackworks layers.
- The gaps that justify building at all.
- A falsification test to run before choosing architecture.

## The landscape

| Clackworks layer          | Closest existing systems                                                                                                  |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Producers → pipelines     | n8n, Windmill, Activepieces, Zapier, Make, Gumloop. Visual flows with AI nodes added on                                     |
| The chain-reaction shape  | Huginn (2013, "agents that watch and act" — the whole idea without models), Node-RED, IFTTT                                 |
| Pipeline execution, gates | LangGraph (state checkpointing, `interrupt()`, human-in-the-loop first-class), Temporal, Inngest, Trigger.dev for durability |
| Agent identity and email  | Lindy, Relevance AI, Vellum (own email, own accounts, own presence), Carly (per-agent real email address), Artisan, 11x     |
| Persistent memory         | Letta (ex-MemGPT), Mem0, Zep                                                                                                |
| Human gates as a service  | HumanLayer, LangChain Agent Inbox                                                                                           |
| Personal, self-hosted     | OpenClaw — one local gateway across WhatsApp, Telegram, Signal, iMessage, Matrix, Slack. Khoj, Leon                          |
| Mail triage and routing   | Shortwave, Fyxer, Cora, Gmelius Meli, Sanebox                                                                               |
| Data-versus-instruction   | CaMeL (DeepMind), dual-LLM pattern. Directly relevant to [security-and-trust.md](./security-and-trust.md)                    |
| Exhaust                   | Dead-letter queues. Old infra concept, absent as a product surface in every agent platform surveyed                          |

Closest single approximation to the whole idea: **OpenClaw + n8n + Letta**, assembled by hand.
Many people are already doing roughly that.

## What appears unclaimed

1. **Exhaust as a working surface.** Dead-letter queues are plumbing everywhere. Nobody
   clusters the unroutable pile, proposes "these fourteen items want a pipeline", helps build
   it, and replays the backlog through it. This loop is the strongest candidate for what
   Clackworks actually is.
2. **Persona as a routing target.** Vendors give agents outbound identity — the agent sends as
   itself. Inbound addressing, where mailing the surf coach's address is itself the routing
   decision, is a different mechanism and is not standard.
3. **One front door for a whole life.** Commercial "AI employee" platforms are team- and
   sales-shaped. Personal, cross-domain, per-domain personas is not the market they pursue.
4. **Artifact re-entrancy with no back doors.** Chained artifacts re-entering through intake, so
   lineage and security policy apply uniformly. Frameworks let agents call agents directly and
   the trace degrades.

## Honest assessment

- Overlap with existing tools is large — a fair estimate is that ~70% of Clackworks is assembly
  of parts that already exist. The remaining share (routing plus exhaust loop, persona
  addressing) is the reason to build, and it has to stay the centre. If it drifts, this becomes
  a worse n8n.
- Scope is large enough to never ship. Eighteen spec areas and five use cases with no stack is
  a real risk, not a hypothetical one.

## Falsification test (do before choosing architecture)

Take [UC-1](./use-cases.md) — mail triage into personas — and sketch it on n8n plus LangGraph
plus Letta. Whatever those cannot express, or express only badly, is the actual specification
for Clackworks. Cheaper than deriving architecture from the spec files.

## Open questions

- Does Clackworks build on one of these (n8n or LangGraph as the execution layer, Letta for
  memory) or start clean? Building on top is the default assumption until falsified.
- Is OpenClaw's channel gateway reusable as the producer layer for messaging?
- Is CaMeL's data/instruction separation adoptable directly, or does it constrain pipeline
  design too much?
- Which of the four unclaimed items is actually the product, and which are supporting detail?

## Not in scope

- Vendor evaluation and pricing.
- Build-versus-buy decision. Recorded as an open question, deliberately not answered yet.
