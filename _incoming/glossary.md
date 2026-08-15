# Glossary

Status: draft

## Purpose

Fix the vocabulary. Every other spec uses these terms exactly. If a concept needs a new word,
it gets defined here first.

## Terms

**Item** — one thing moving through the system. A mail, a video, a note, a TODO, an issue, a
message, an artifact from an earlier pipeline. The unit that gets routed and worked on.

**Producer** — a source that emits items. External (mail, YouTube, WhatsApp, repositories,
issue trackers, files) or internal (a pipeline whose artifact re-enters the system).

**Intake** — the single front door. Every item enters here. Intake normalizes items into a
common shape, checks authenticity and policy, and hands them to routing.

**Routing** — deciding which pipeline an item goes to. Produces either a target pipeline or a
verdict of unroutable.

**Pipeline** — a named, freely definable unit of work that consumes items and produces
artifacts. Either **deterministic** (fixed steps, predictable output) or **agent-driven** (an
agent with a goal and judgement).

**Step** — one stage inside a pipeline.

**Agent** — the worker of an agent-driven pipeline. Has a persona, memory, tools, and an
assigned model.

**Persona** — an agent's identity: name, personality, voice, and its own channels (email
address, social accounts) through which it can be reached and can act.

**Memory** — what an agent retains across runs, distinct from the working context of a single
run.

**Artifact** — what a pipeline produces. Terminal (published, sent, merged) or re-entrant
(becomes an item via a producer).

**Chaining** — an artifact from one pipeline becoming the input item of another.

**Exhaust** — where unroutable items land. A queue with a decision attached, never a
wastebasket.

**Human gate** — a configured point where a human is consulted, notified, or given veto before
the run continues.

**Directive** — the configured policy at a point in the system, including whether a human is
involved and how.

**Run** — one execution of a pipeline over one item.

**Control plane** — where producers, routes, pipelines, personas, and directives are defined
and changed.

## Open questions

- Is an item immutable once it enters, with each step producing a new version, or does it carry
  mutable state?
- Do artifacts and items share one type, given that artifacts can become items?
- Does an item belong to exactly one run at a time, or can it fan out to several pipelines?
- Is a deterministic pipeline that calls a model once still "deterministic"?

## Not in scope

- Data formats, schemas, and identifiers. Vocabulary only.
