# Research

Status: draft

Background that informs the specification without being part of it.

## What is here

| File                                                             | Covers                                                                                                                                                            |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [prior-art.md](./prior-art.md)                                   | The landscape by Clackworks layer, the candidate shortlist, what is genuinely unclaimed, what no candidate gives you, honest assessment                           |
| [prior-art-product-overview.md](./prior-art-product-overview.md) | Per-product reference: what each one is, self-host status, licence, cost, ceilings, project health                                                                |
| [candidates/](./candidates/)                                     | One file per candidate worth reading in full — Windmill, Sim, Activepieces, OpenClaw: what each gives you, pros, cons, use-case fit, what running it would decide |

## Method

Three rules:

- **Absence from the market is not evidence of opportunity.** "No product does this" has three
  causes and only one is interesting: too trivial to sell, not a product, or genuinely unserved.
  Every gap is tested against that before it counts. See
  [What appears unclaimed](./prior-art.md#what-appears-unclaimed).
- **Everything here goes stale.** Surveys are dated at the top of each file. Re-check before any
  decision rests on a number, a licence, or a feature row.
- **Test by sketching a use case, not by building a comparison matrix.** Take a use case from
  [the use-case specification](../specification/use-cases/README.md), sketch it on the candidate,
  see where it lands.

## The bar

The bar is the **product test. We are only looking at systems that do all of this (not an assembly
of best-of-breed products)**:

1. **Pipelines.** Named, multi-step units of work, deterministic or agent-driven.
2. **Routing between pipelines.** A pipeline's output can be routed into another pipeline.
3. **Gates.** A run halts until a human or an agent decides, and resumes afterwards.
4. **Filters.** Predicates that admit, drop, or divert an item before the work starts.
5. **Cycles.** Work comes back around — a rejected draft goes back for revision, a PR re-enters as
   an item, a produced item routes to the pipeline that produced it. A single run may be acyclic;
   the machine as a whole is not, and anything that can only express a DAG is not a fit.

Plus the standing constraints from [VISION.md](../../VISION.md#constraints): self-hostable, a
free edition that is not capped below the use cases, and a licence that does not forbid a later
enterprise edition.

## Where it stands

- **No mechanism in the vision is unavailable today.** Pipelines, durable resume, human gates,  
agents with their own memory and voice, inbound mail as a producer, content-based routing —  
all of it exists, self-hosted, under workable licences.
