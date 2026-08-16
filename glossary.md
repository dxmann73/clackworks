# Glossary

## Purpose

Fix the vocabulary. Every other spec uses these terms exactly. If a concept needs a new word,
it gets defined here first.

## Terms

### Flow of work

The chain is **producer → router → pipeline**, with an **ingress** in front of the pipeline and
an **egress** behind it. A pipeline can itself act as a producer, and its egress is the part that
does so; if what leaves it should be routed onward, a router is attached to that egress. An item
the router cannot route goes to the **exhaust**.

**Item** — one thing moving through the system. A mail, a video, a note, a TODO, an issue, a
message, the output of an earlier pipeline. The unit that gets routed and worked on.

**Produced item** — an item a pipeline made. Not a separate type: it is an item, and anything
true of items is true of it. The phrase is a statement about where the item came from, nothing
more.

**Producer** — a source that emits items. External (mail, YouTube, WhatsApp, repositories,
issue trackers, files) or internal (a pipeline feeding its produced items back in, through its
egress). A producer emits and
attaches provenance; it does not filter, judge, or route.

**Router** — decides where an item goes and takes it there. It decides on what the item itself
carries — provenance, sender, addressee, content, whatever the item brought with it — and needs
nothing else to make the call. The router is also the technology-open layer: how something
reaches its destination is deliberately not fixed to one transport. An item it cannot route is
**unroutable** and goes to the exhaust.

**Pipeline** — a named, freely definable unit of work that consumes items and produces
items. Either **deterministic** (fixed steps, predictable output) or **agent-driven** (an
agent with a goal and judgement). A pipeline works on the item and optionally produces new
items.

**Step** — one stage inside a pipeline.

**Ingress** — sits in front of a pipeline. Filters and enriches the item, and can raise a gate
before the item enters. It is the place for judgement about an item in context: a mail from
a trusted sender carrying an office document, when that sender never sends documents, is flagged
for a human at the ingress rather than dealt with inside the pipeline. Not a front door and not
a normalizer — arrival and provenance belong to the producer.

There is no single front door: arrival is a producer, admission is an ingress, and the work is
the pipeline behind them.

**Egress** — attached to a pipeline's output. Where items leave the pipeline and are delivered
onward, to a terminal destination or back into the system. The egress is the part of a pipeline
that makes the pipeline a producer: it emits the produced items and attaches their provenance.
Routing what comes out of it means attaching a router to it.

**Exhaust** — where unroutable items land: something arrived and the router cannot route it, so
no pipeline gets it. A queue with a decision attached: build a new pipeline, change routing and
re-process, handle it manually, discard deliberately.

**Re-processing** — running items already sitting in the exhaust through routing again after
routing or pipelines changed.

**Lineage** — the end-to-end trace of an item: where it came from, every hop, every decision,
every human touch, what it became.

### Agents

**Agent** — the worker of an agent-driven pipeline. Has a persona, memory, tools, and an
assigned model. Not bound to one machine — an agent may run wherever it is hosted.

**Persona** — an agent's identity: name, personality, voice, and its own email address and
social accounts, through which it can be reached and can act. Mail addressed to a persona
carries that fact in the item, so the router can send it to that persona on the strength of the
address alone.

**Memory** — what an agent retains across sessions, distinct from the working context of a
single run.

**Session** — a bounded stretch of agent interaction that carries its own working context.
Memory outlives it.

**Run** — one execution of a pipeline over one item.

### Control

**Gate** — a configured point where the run halts until someone or something decides. The decider
is a human or an agent. A gate can sit anywhere: at an ingress, between steps, before an egress.

**Human gate** — a gate whose decider is a human: consulted, notified, or given veto before the
run continues.

**Agent gate** — a gate whose decider is an agent applying judgement instead of a person.

**Directive** — the configured policy at a point in the system, including whether a gate sits
there and who decides at it.

**Control plane** — where producers, routing, pipelines, personas, ingress, egress, and
directives are defined and changed.

### Editions

**Community edition** — the free, self-hosted edition. Fully featured for every use case in the
specification. Not execution-capped, not workflow-capped.

**Enterprise features** — organisation-shaped concerns (SSO/SAML, RBAC, multi-tenancy, audit
logging, compliance) that may sit behind a paid licence. Nothing a single user needs belongs
here.
