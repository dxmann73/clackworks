# Routing

Status: draft

## Purpose

Get an item to its destination, or declare it unroutable. The router sits between the producer
and the pipeline: it decides, and it takes the item there. This is the judgement layer and the
place where most of the system's value sits.

## In scope

- How a routing decision is made and expressed.
- Deterministic rules versus classifier-driven routing.
- What a destination may be, and the transports behind them.
- Ambiguity, multiple matches, no match.

## Requirements

- The router decides on what the item itself carries — provenance, sender, addressee, content —
  and needs no state beside the item to make the call. An item that arrives with too little to
  decide on is a producer problem, not a routing one.
- A routing decision names a destination, or the item is unroutable.
- A destination is not necessarily a pipeline. Writing a row to a database, sending a WhatsApp
  message, and sending a mail are all destinations. The router abstracts the transport; nothing
  upstream of it needs to know which one is in play.
- Mail addressed to a persona's own address routes to that persona on the strength of the
  address, without a classification step. The item typically lands in that persona's
  general-purpose pipeline and the agent decides what to do with it — see
  [personas-and-identity.md](./personas-and-identity.md).
- Routing what leaves a pipeline means attaching a router to its egress. It is the same router,
  configured against a different source, not a second kind of component.
- Both rule-based routing (sender matches, addressee matches, label matches, repository matches)
  and judgement-based routing (a classifier or agent decides "this is an invoice") must be
  supported.
- Routing configuration is inspectable, diffable, and changeable without touching pipelines or
  producers. See [control-plane.md](./control-plane.md).
- Every decision is recorded with its reason — which rule fired, or what the classifier
  concluded and how confidently.
- No match means unroutable means exhaust. It never means a default pipeline that silently
  swallows things.
- Low confidence is a first-class outcome, distinct from no match, and can be configured to
  trigger a human gate rather than a guess.
- Routing must be testable: given a stored item, show which pipeline it would go to now,
  without running anything.

## Prior-art evidence

Checked 2026-08-15 against OpenClaw and the Activepieces arm; see
[research/README.md](../docs/research/README.md) for the falsification tests these came out
of. Evidence, not
decisions — the stack is still undecided.

- **Only sessions are addressable, not pipelines.** OpenClaw's `sessions_send` selects a target
  by `sessionKey`, `label`, or `agentId`, and the docs are explicit that this "selects local
  model context, not an external destination". Lobster pipelines and Task Flows are invoked
  *inside* an agent turn; neither has an address a router can name. The requirement above —
  a decision names a target *pipeline* — has no native equivalent in either arm.
- **Thread-scoped session keys are not valid targets** (keys ending `:thread:<id>`), so a route
  into a live human conversation is rejected by construction.
- **Agent-to-agent routing is off by default.** `tools.agentToAgent.enabled` plus an explicit
  `allow` list. A router that hands off is a deliberate configuration, not a default capability.
- **The hook endpoint is the machine-shaped hop.** `POST /hooks/agent` accepts `message`,
  `agentId`, `sessionKey`, `idempotencyKey`, `sessionMode`, `deliver`, `model`, and
  `timeoutSeconds`. Compared with `sessions_send`, whose payload is prose, this carries a
  named session key and an idempotency key as typed fields — a decision expressed in machine
  fields rather than in a sentence.
- **Activepieces gets closer on inspectability, and still not all the way.** A branch condition
  is flow JSON under flow version history: diffable, versioned, reviewable. What is still
  missing there is the decision *record* — which rule fired, with what confidence — and the
  ability to answer it against a stored item without running the flow.

Net: the "recorded with its reason" and "testable without running anything" requirements above
survive both arms unmet. They are the routing-specific part of what
[research/README.md](../docs/research/README.md#what-survives-arm-1) calls guarantees rather than mechanisms.

## Open questions

- Is the routing target a pipeline, or the session that runs one? Only sessions are addressable
  in the leading candidate runtime — does Clackworks introduce a pipeline address, or accept
  the session as the address and lose that distinction?
- Does the decision record travel *with* the item as a field, or beside it as a log the item id
  joins against? The first survives a hop through a foreign runtime; the second does not.
- Can one item go to several destinations at once (invoice that is also a surf trip receipt)? If
  so, how do the resulting items relate, and does one failing destination fail the others?
- Precedence when several rules match: first match, most specific, explicit priority?
- Which transports does the router speak at the start, and what does adding one cost?
- Are non-pipeline destinations (a database row, a WhatsApp message) terminal by definition, or
  can they re-enter as items?
- Who or what does judgement-based routing — a dedicated classifier, a router agent with a
  persona, or the pipelines bidding for items?
- Is routing one hop or can a pipeline re-route an item it decides it should not handle?
- Does routing learn from human corrections in the exhaust, and if so, how is that not a silent
  behaviour change?

## Not in scope

- The pipelines themselves.
- Filtering, enrichment, and the gate decision in front of a pipeline — that is the ingress.
- Normalization and provenance — that is the producer.
