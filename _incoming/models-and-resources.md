# Models and resources

Status: draft

## Purpose

Pipelines and agents get models and computing resources assigned. This defines what can be
assigned, at what granularity, and what bounds it.

## In scope

- Model assignment and override.
- Compute resources, budgets, and quotas.
- Behaviour at the limit.

## Requirements

- A model can be assigned per pipeline, and per agent. Precedence between the two is explicit.
- Assignment is configuration, not code. Changing the model of a pipeline does not require
  changing the pipeline.
- Compute resources are assignable: a heavy development pipeline may need a sandbox with real
  CPU, disk, and network; a mail classifier does not.
- Budgets exist and are enforced — per run, per pipeline, per persona, per period. Something
  must stop an agent from spending unboundedly.
- Hitting a limit is a visible event with a defined outcome, not a silent truncation or a
  degraded answer that looks normal.
- Cost and resource use per run are recorded and attributable to a pipeline and persona.
- Model choice may be conditional (cheap model for triage, expensive for the actual work), and
  that condition is declared, not hidden in a prompt.

## Open questions

- Fallback policy when a model is unavailable or over budget: fail the run, queue it, downgrade
  to a cheaper model, or ask a human?
- Are there local models in the picture, and does that change the resource model?
- Who may change a model assignment — user only, or can an agent propose or self-tune?
- Does a run get one model for its whole life, or can it switch mid-run?
- Sandboxing: what isolation does a code-executing pipeline need, and is that a resource
  attribute or a security attribute?
- How are provider credentials and rate limits shared across many concurrent runs?

## Not in scope

- Specific providers, model IDs, and prices. Those change; the mechanism should not.
