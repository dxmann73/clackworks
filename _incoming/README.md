# _incoming — specification areas

One file per area that needs specifying. Everything here is a draft until marked otherwise.
No architecture, no stack, no implementation — descriptions of what must exist and what it must
do.

## Reading order

Read the [glossary](../glossary.md) first; every other file assumes its vocabulary. It and
[open-questions.md](../open-questions.md) live at the repo root — they are settled enough to
have left `_incoming/`.

| #  | Area                                                       | Status | Covers                                            |
| -- | ---------------------------------------------------------- | ------ | ------------------------------------------------- |
| 1  | [producers.md](./producers.md)                             | draft  | Sources that emit items                           |
| 2  | [triggers-and-scheduling.md](./triggers-and-scheduling.md) | draft  | How items get pulled or pushed, when              |
| 3  | [routing.md](./routing.md)                                 | draft  | Deciding where an item goes, and taking it there   |
| 4  | [ingress.md](./ingress.md)                                 | draft  | Guarding one pipeline's entrance: filter, enrich, gate |
| 5  | [pipelines.md](./pipelines.md)                             | draft  | Units of work, deterministic or agent-driven      |
| 6  | [agents.md](./agents.md)                                   | draft  | Agents as workers: goals, tools, behaviour        |
| 7  | [personas-and-identity.md](./personas-and-identity.md)     | draft  | Names, personalities, mail and social accounts    |
| 8  | [memory.md](./memory.md)                                   | draft  | What agents remember and for how long             |
| 9  | [models-and-resources.md](./models-and-resources.md)       | draft  | Model assignment, compute, budgets, limits        |
| 10 | [outputs-and-chaining.md](./outputs-and-chaining.md)       | draft  | Outputs, and outputs becoming inputs              |
| 11 | [exhaust.md](./exhaust.md)                                 | draft  | The unroutable pile and its decision loop         |
| 12 | [gates-and-directives.md](./gates-and-directives.md)       | draft  | Gates: who decides, notification, override        |
| 13 | [security-and-trust.md](./security-and-trust.md)           | draft  | Authenticity, secrets, blast radius, injection    |
| 14 | [observability.md](./observability.md)                     | draft  | Tracing an item end-to-end, health, audit         |
| 15 | [control-plane.md](./control-plane.md)                     | draft  | How the system is configured and changed          |
| 16 | [use-cases.md](./use-cases.md)                             | draft  | Concrete end-to-end walkthroughs                  |

Prior art is not a specification area and no longer lives here. The research — the landscape
survey, the per-product detail, and the falsification test run on both arms — is in
[docs/research/](../docs/research/README.md). It is an input to Phase 0 below, not a spec area.

## Working order

Reading order above is narrative — it follows an item through the machine. It is the wrong
order to *write* in, because it treats all sixteen areas as equally urgent. That is the
"never ships" risk named in [prior-art.md](../docs/research/prior-art.md#honest-assessment).

Working order is by what blocks what. Do not open a phase until the one above it is answered.

### Phase 0 — Frame the thing

Cheap to answer, and every later requirement changes depending on the answers. Nothing else
should be touched first.

Settled and no longer open here: scope and constraints are in
[VISION.md](../VISION.md#constraints); an arrived item and a produced item are one type, mutable
with history, per [glossary.md](../glossary.md); the falsification test has been run on both
arms and is written up in [docs/research/README.md](../docs/research/README.md).

| Area                                                             | What specifically                                                                                   |
| ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| [use-cases.md](./use-cases.md)                                   | **Pick one.** Declare which use case v1 serves and which four are out of scope until it works. Both falsification arms express UC-1; UC-4 is the only use case neither expresses |
| [docs/research/](../docs/research/README.md)                     | **Build on top, or beside?** Every mechanism exists already, so the decision is whether the surviving guarantees are a system or a layer on one arm. The falsification test and both arms are written up there; the survey and per-product detail sit beside it |

### Phase 1 — The core loop

The part that is actually Clackworks rather than a worse Activepieces or a worse OpenClaw.
Specify only as far as the chosen use case needs.

| Area                                                     | Why here                                                                              |
| --------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| [producers.md](./producers.md)                           | Only the kinds the chosen use case needs. Also inherits item shape, identity, and dedup, with no stage behind it to own them |
| [routing.md](./routing.md)                               | Where the value sits, per its own Purpose section. The router decides on what the item carries |
| [ingress.md](./ingress.md)                               | Filter, enrich, gate in front of one pipeline. Cheap now, and the gate belongs here rather than mid-run |
| [exhaust.md](./exhaust.md)                               | The pile is standard plumbing; the loop over it (UC-4) is what nothing else expresses. Also defines what routing's `unroutable` means |
| [security-and-trust.md](./security-and-trust.md)         | **Trust-boundary slice only.** Data-versus-instruction is cheap now and expensive to retrofit |

### Phase 2 — Doing the work

| Area                                                     | Why here                                                     |
| --------------------------------------------------------- | -------------------------------------------------------------- |
| [pipelines.md](./pipelines.md)                           | Needs a settled item shape and a routing contract to consume |
| [gates-and-directives.md](./gates-and-directives.md)     | Gates attach to pipeline steps, so steps come first          |
| [outputs-and-chaining.md](./outputs-and-chaining.md)     | Re-entrancy is meaningless until ingress and pipelines exist |

### Phase 3 — Agents

Deliberately late. An agent-driven pipeline is one kind of pipeline; the machine has to work
before its workers get personalities.

| Area                                                   | Why here                                                    |
| ------------------------------------------------------- | ------------------------------------------------------------- |
| [agents.md](./agents.md)                               | A pipeline implementation detail until pipelines are settled |
| [personas-and-identity.md](./personas-and-identity.md) | Persona-as-routing-target loops back into routing            |
| [memory.md](./memory.md)                               | Only meaningful once an agent has runs to remember across    |
| [models-and-resources.md](./models-and-resources.md)   | Budgets need something to budget                             |

### Phase 4 — Operating it

Real requirements, but none of them block the design of the loop.

| Area                                                       | Why here                                                    |
| ----------------------------------------------------------- | ------------------------------------------------------------- |
| [observability.md](./observability.md)                     | Traces the hops. Needs the hops to exist                     |
| [control-plane.md](./control-plane.md)                     | Configures the above. Needs to know what there is to configure |
| [triggers-and-scheduling.md](./triggers-and-scheduling.md) | Mostly a push-versus-pull mechanism choice; defer with the stack |

## File template

Every area file follows the same shape:

```markdown
# <Area>

Status: draft

## Purpose
## In scope
## Requirements
## Open questions
## Not in scope
```

## Adding an area

Create the file, follow the template, add a row to the table above. Split a file when it starts
covering two things that could be argued about independently.
