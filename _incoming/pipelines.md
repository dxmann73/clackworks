# Pipelines

Status: draft

## Purpose

Define the unit of work: what a pipeline is, what it guarantees, and how one is created without
touching routing internals or producers.

## In scope

- Deterministic and agent-driven pipelines, and what separates them.
- Composition, steps, inputs, outputs.
- Failure, partial completion, and resumption.

## Requirements

- A pipeline consumes items and produces items. Both ends are explicit.
- Two kinds, and the kind is declared: **deterministic** (fixed steps, same input gives same
  output, no judgement) and **agent-driven** (an agent pursues a goal with tools and
  judgement).
- Pipelines are freely creatable by the user, and creating one requires no change to routing
  internals or producers — only registering it as a destination the router can name, and an
  [ingress](./ingress.md) in front of it.
- A pipeline declares what it accepts. An item it cannot handle is refused loudly, not
  half-processed.
- Every pipeline can have a model and compute resources assigned — see
  [models-and-resources.md](./models-and-resources.md).
- Human gates can be placed at any step — see [human-in-the-loop.md](./human-in-the-loop.md).
- Failure is explicit and observable. A failed run says what failed, at which step, with what
  input, and where the item went afterwards.
- Fail fast inside a pipeline too: a step that gets a value it does not expect stops the run
  rather than substituting a default.
- Pipelines are composable — an output of one is an input of the next — without either
  pipeline knowing about the other.

## Open questions

- Are steps a first-class concept, or is a pipeline opaque with only its edges specified?
- Can a pipeline be partly deterministic and partly agent-driven, or does mixing mean two
  chained pipelines?
- Retry and resume: does a failed run restart from the beginning, or from the failed step?
- Are runs isolated from each other, or can two runs of the same pipeline share state?
- Concurrency: how many runs of one pipeline may be in flight, and does that matter for agents
  with shared memory?
- Versioning: when a pipeline changes, what happens to in-flight runs and to reproducibility of
  past runs?
- Timeouts: what is a long-running pipeline allowed to do, and what kills it?

## Not in scope

- Pipeline definition format and execution engine. Architecture, deferred.
- Specific pipelines. Sketches live in [use-cases.md](./use-cases.md).
