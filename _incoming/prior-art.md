# Prior art

Status: draft

Surveyed 2026-08, revised 2026-08-15 against the constraints in
[VISION.md](../VISION.md#constraints). Landscape moves fast; re-check before any build decision.

## Purpose

Every piece of Clackworks exists somewhere already. The combination does not. This file records
what exists, what is genuinely unclaimed, and what to steal rather than reinvent.

Per-product detail — what each one is, self-host status, licence, cost, free-tier ceiling —
lives in [prior-art-product-overview.md](./prior-art-product-overview.md). This file stays at
the level of layers and gaps.

## In scope

- Existing systems mapped to Clackworks layers.
- The gaps that justify building at all.
- A falsification test to run before choosing architecture.

## The landscape

| Clackworks layer          | Candidates (self-hostable)                                                                | Reference only                                                            |
| ------------------------- | ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| Producers → pipelines     | n8n, Activepieces. Visual flows with AI nodes added on                                       | Zapier, Make, Gumloop                                                        |
| The chain-reaction shape  | Huginn (2013, "agents that watch and act" — the whole idea without models)                   | IFTTT                                                                        |
| Pipeline execution        | OpenClaw Lobster (pipelines-as-data DSL) and Task Flow (durable, restart-surviving); LangGraph, Temporal, Kestra, Hatchet, Trigger.dev, Conductor, DBOS, Inngest | —                       |
| Human gates               | OpenClaw Lobster — approval checkpoints with resume tokens; LangGraph `interrupt()` plus Agent Inbox; Kestra `Pause`, paused state persisted to database | HumanLayer — hosted, and pivoted away from approvals |
| Persistent memory         | Letta (ex-MemGPT), Mem0, Graphiti, Cognee                                                    | Zep — its community edition no longer exists as a deployable server          |
| Agent identity and email  | OpenClaw — isolated agents with own workspace, persona files, session store and memory vaults; bindings address chat channels, and an IMAP/SMTP tool covers mail. Identity is text files in a repo; there is nothing here to buy | — |
| Personal, self-hosted     | OpenClaw — one local gateway across WhatsApp, Telegram, Signal, iMessage, Matrix, Slack. Khoj, Leon | —                                                                      |
| Content-based routing     | Nothing native. OpenClaw bindings match on address only (peer, guild, team, account, channel) — but a router agent plus `sessions_send` assembles it | Shortwave AI filters — route to labels, not to workers |
| Mail triage and routing   | Inbox Zero (AGPL; a destination product, not a producer). Sieve/Rspamd as the non-AI baseline | Shortwave, Fyxer, Cora, Gmelius Meli, Sanebox                               |
| Data-versus-instruction   | CaMeL (DeepMind), dual-LLM pattern. Directly relevant to [security-and-trust.md](./security-and-trust.md) | —                                                               |
| Exhaust                   | A dead-letter queue is a component of any automation platform, not a product. Build it as part of whatever runs the pipelines | — |

Closest single approximation to the whole idea: **OpenClaw + n8n + Letta**, assembled by hand.
Many people are already doing roughly that, and all three are self-hostable, so the constraints
do not rule the assembly out.

One layer has an empty candidate column: **content-based routing** — and even that is
assemblable from parts OpenClaw already ships, as the falsification test below shows. Earlier
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
   fourteen items want a pipeline", help build it, replay the backlog. That is a workflow and UI
   claim about a personal system, not a novel mechanism, and it should be argued on whether it
   is pleasant to use rather than on whether anyone else sells it.
3. **Inbound mail addressing per persona — unserved, and small.** OpenClaw binds chat accounts
   to agents; nothing binds mail addresses. With a provider catch-all plus a forwarding
   integration this is an afternoon's work too. Real, but not a reason to build a system.
4. **Content-based routing into a single front door — also expressible, see the falsification
   test below.** Every surveyed system *binds* on where an item arrived: which account, which
   channel, which webhook. But a router agent that classifies and then calls `sessions_send`
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
  differentiators moved two more. What is left is the classifying front door, and the loop over
  the unroutable pile as a matter of usability rather than novelty.
- That is a thinner claim than this document started with, and it is still enough. A personal
  system does not have to be novel to be worth having — the vision already says personal first.
  The risk is not that the idea is small; it is pretending it is bigger and building
  accordingly.
- If the differentiator drifts off the front door, this becomes a worse n8n or a worse OpenClaw.
- Scope is large enough to never ship. Eighteen spec areas and five use cases with no stack is
  a real risk, not a hypothetical one — and it is now clearly disproportionate to what is
  actually being added.
- The self-hosting constraint costs less than expected. It removes hosted convenience, not
  capability: every layer has a credible self-hostable candidate except content-based routing.

## Falsification test, arm 1 — OpenClaw, run 2026-08-15

The test was to sketch [UC-1](./use-cases.md) on existing tools and treat whatever they cannot
express as the specification. Sketched against OpenClaw alone. It expresses effectively all of
it.

| UC-1 step                              | OpenClaw mechanism                                                                     |
| -------------------------------------- | ---------------------------------------------------------------------------------------- |
| Mail arrives                           | Gmail API `watch` publishing to a GCP PubSub topic, pushed to a Gateway hook. `openclaw webhooks gmail setup` wires it; the Gateway auto-starts and renews the watch. IMAP/SMTP tool is the alternative |
| Mail reaches a router                  | Hook mapping matched on `path: gmail`, per-message session key `hook:gmail:{{messages[0].id}}`, routed to a dedicated reader agent. Forwarding to a bound chat channel also works |
| Classify: invoice, recipe, surf, other | A router agent turn, or a Lobster pipeline step                                            |
| Route to the right persona             | `sessions_send` targeting another agent's session key, e.g. `agent:tax-advisor:main`. Validated against local config; same gateway only |
| Persona has its own memory and voice   | Separate agent: own workspace, persona files, SQLite session store, memory-wiki vault      |
| Human gate before anything payable     | Lobster halts on side effects and returns a resume token                                   |
| Survives a restart mid-run             | Task Flow durable records, `waiting` and `blocked` states                                  |
| Mail from a stranger is untrusted      | Documented reader shape: dedicated agent, `sandbox.mode: all` with `scope: session` and `workspaceAccess: none`, `tools.profile: minimal`, an untrusted-data message template, `deliver: false` |
| Unroutable pile                        | Whatever the router agent writes it to — a file, a session, a task                         |

The last remaining claim in this file — content-based routing — turns out to be a router agent
with a classification prompt plus one `sessions_send` call. The email gap recorded earlier was
also wrong in substance: bindings are address-based and email is not among them, but email is
reachable as a tool, so nothing is actually blocked.

**Conclusion: Clackworks as specified is closer to a configuration of OpenClaw than to a new
system.** Every mechanism the vision describes exists, self-hosted, MIT, today.

### What survives arm 1

Not mechanisms — guarantees. OpenClaw gives the parts; it does not give these properties:

- **Routing as inspectable data.** A router agent's judgement is a prompt. It cannot be
  diffed, versioned, replayed, or answered offline. [routing.md](./routing.md) requires the
  decision be recorded with its reason and be testable against a stored item *without running
  anything*. That is a difference in kind, and it is small in code.
- **Item identity and lineage.** OpenClaw traces sessions and tasks. Clackworks traces *items*
  across hops, personas, and outputs that re-enter. Nothing in OpenClaw carries an item id
  through a `sessions_send`.
- **Dedup and idempotent re-emission** across heterogeneous producers, defined per producer.
- **One normalized item shape** with provenance, so a new producer needs no pipeline change.
- **The exhaust as a worked surface** — clustering, proposing a pipeline, replaying a backlog.
  A router agent can write unroutable items somewhere; nothing makes that a loop.

Whether these justify a separate system, or are a plugin and a set of conventions on top of
OpenClaw, is the decision this test exists to force. It is not answered here.

## Falsification test, arm 2 — Activepieces assembly, sketched 2026-08-15

Arm 1 tested one integrated system. Arm 2 tests the opposite shape: best-of-breed parts wired
together by hand. Different question. Arm 1 asked *does the mechanism exist*; arm 2 asks *what
does the assembly cost*.

**Method: paper sketch, deliberately not an implementation.** Two working UC-1 builds cost weeks
and — as arm 1 already showed — both would succeed, so a second build decides nothing. What is
wanted from arm 2 is the part count and the shape of the glue.

Stack under test, all self-hostable and all surviving the constraints in
[VISION.md](../VISION.md#constraints):

- **Activepieces** (MIT core) as producer, pipeline, and routing layer.
- **Letta** (Apache-2.0) as persona memory, called over HTTP.
- Human gates from Activepieces itself. Kestra and LangGraph turned out not to be needed —
  see the gate row below — which removes a component the layer table assumed.

| UC-1 step                          | Activepieces assembly                                                                    | Against arm 1                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------- |
| Mail arrives                       | Gmail piece, `New Email` trigger. Marked **scheduled**, i.e. polling                         | **Worse.** Arm 1 pushes via PubSub; a poll interval is a latency floor on every item |
| Mail reaches a router              | No hop. The trigger starts the flow, and the flow is the router                              | **Better.** Arm 1 needs a hook mapping or a channel-binding detour |
| Classify: invoice, recipe, surf    | An AI step against a platform-configured provider, then a Router step branching on its output | **Better on the property that matters.** Branch conditions are flow JSON under flow version history: diffable, versioned, reviewable. Arm 1's equivalent is a prompt |
| Route to the right persona         | One flow per persona, invoked as a sub-flow or by webhook. No agent registry exists           | **Worse.** Arm 1 has addressable agents; here persona addressing is a naming convention you maintain |
| Persona has its own memory and voice | Voice is prompt text in project variables. Memory is Letta, called per persona flow           | **Worse.** Second system, second datastore, second ops surface. Arm 1 ships workspaces, persona files, and a session store |
| Human gate before anything payable | Approval piece `Wait for Approval`, with the Todos inbox as the review surface                | **Parity on mechanism, better on surface.** A Todos inbox beats a resume token pasted into chat |
| Survives a restart mid-run         | Flows pause mid-execution and resume with execution context persisted across restarts and deploys | **Parity** |
| Mail from a stranger is untrusted  | Nothing equivalent. Steps run with whatever the connection grants; no per-step sandbox or tool clamp | **Much worse.** Arm 1's documented reader shape is the closest thing to CaMeL either arm offers |
| Unroutable pile                    | Router fallback branch writes to a table                                                      | **Parity, and both are a non-answer** — a place, not a loop |

### What arm 2 shows

- **UC-1 is expressible here too.** No mechanism is missing. This is the second independent
  confirmation of the arm 1 conclusion, from a stack with no agent runtime in it at all.
- **Routing-as-data is not a Clackworks invention.** Activepieces gets closer to it for free
  than OpenClaw does, because a branch condition is already flow JSON with version history.
  What it still lacks is the decision *record* — reason attached, replayable against a stored
  item without running the flow. Narrower claim than [routing.md](./routing.md) currently makes,
  and that file should be re-read against this.
- **Part count is the real trade.** Arm 2 is three systems plus glue you own forever. Arm 1 is
  one MIT system plus setup cost that is real but one-time: a GCP project, a PubSub topic, a
  public HTTPS endpoint for the push hook, and a sandbox image.
- **Neither arm expresses UC-4.** Both give somewhere to put unroutable items; neither clusters
  them, proposes a pipeline, or replays a backlog. [UC-4](./use-cases.md) is the only use case
  in this repo that no surveyed stack expresses.
- **Security is where the arms diverge most.** An untrusted mail body reaching a step with live
  connections is the arm 2 default. See [security-and-trust.md](./security-and-trust.md).

**Conclusion: the assembly is priced, and it does not create a reason to exist either.** Neither
arm could have, because reason-to-exist was never about expressing UC-1 — both arms express it.
It rests on the guarantee list under [What survives arm 1](#what-survives-arm-1), plus UC-4,
which is now the only unexpressed thing found in two arms.

**The decision this forces:** the next build is not UC-1 a second time. It is the guarantee
layer plus UC-4 on top of one arm. Arm 1 is the cheaper host — fewer parts, sandboxing, agent
identity and memory included — even though arm 2 is closer on routing-as-data.

### Confidence and staleness

Sketched from vendor docs on 2026-08-15, not from a running instance. Verified: the Gmail piece
and its scheduled `New Email` trigger, the Approval piece and Todos inbox, persisted pause and
resume, platform-level AI provider configuration, MIT core with a commercial `packages/ee`
folder. **Not verified:** whether Activepieces' own agent feature changes the persona and memory
rows, and what the poll interval floor actually is on a self-hosted build. Both would have to be
checked before this sketch decides anything.

## Open questions

- Does Clackworks build on one of these (n8n or Activepieces as the execution layer, Letta for
  memory) or start clean? Building on top is the default assumption until falsified.
- Arm 2 leftovers: does the Activepieces agent feature change the persona and memory rows, and
  what is the actual poll-interval floor for the Gmail trigger on a self-hosted build?
- **n8n or Activepieces?** n8n has far more integrations; its Sustainable Use Licence permits
  internal use only, which conflicts with the product path left open in
  [VISION.md](../VISION.md#constraints). Activepieces is the same shape under MIT. This is a
  licence decision disguised as a feature decision.
- Does the human gate come from the workflow engine (Kestra-style `Pause`) or from the agent
  runtime (LangGraph `interrupt()`)? The two imply different shapes for
  [pipelines.md](./pipelines.md).
- **Is Clackworks a layer on OpenClaw rather than a system beside it?** OpenClaw supplies
  channels, pipelines with gates, agents with memory, and durable orchestration. Content-based
  routing, the ingress, and the exhaust would be the addition. This is now the leading
  build-on-top candidate and it did not exist as an option when this file was written.
- Is OpenClaw's channel gateway reusable as the producer layer for messaging, given that its
  full-system-access model conflicts with [security-and-trust.md](./security-and-trust.md)?
- Is CaMeL's data/instruction separation adoptable directly, or does it constrain pipeline
  design too much?
- Content-based routing is the surviving claim. Is that a product, or a feature someone adds to
  n8n or OpenClaw in a month once it is specified properly?

## Not in scope

- Vendor evaluation and pricing. Moved to
  [prior-art-product-overview.md](./prior-art-product-overview.md).
- Build-versus-buy decision. Recorded as an open question, deliberately not answered yet.
