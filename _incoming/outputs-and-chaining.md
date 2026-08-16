# Outputs and chaining

Status: draft

## Purpose

Pipelines produce something. That something either ends the chain or starts the next one. This
defines what a produced item carries and how the loop back in works without becoming chaos.

What a pipeline produces is an item. There is no second type: it is the same kind of thing that
entered, and calling it "produced" only says where it came from.

## In scope

- What a produced item carries beyond what any item carries.
- Terminal versus re-entrant items.
- Loop prevention and lineage.

## Requirements

- Every run produces an item, even when that item is "nothing was produced, for this
  reason". A silent run is a bug.
- A produced item carries lineage: which item it came from, which pipeline and persona made it,
  which model, when, and which human touched it.
- A produced item is either terminal (published post, sent mail, merged PR, filed invoice) or
  re-entrant, and that is declared, not inferred.
- Re-entrancy happens through the egress acting as a producer, with a router attached to it. The
  item goes through routing and the next pipeline's ingress like anything else. No back doors
  between pipelines.
- Lineage survives chaining. From a published blog post you can trace back to the stray thought
  in a text file that started it.
- Cycles must be detectable and stoppable. A chain that feeds itself needs a limit and a
  visible reason when it hits it.
- A produced item that no pipeline consumes and that is not terminal is a defect, and it should
  surface — probably in the exhaust.

## Open questions

- Are produced items stored by the system, or only referenced where they live (in a repo, on the
  website, in a mailbox)?
- Versioning: a draft revised three times — one item with versions, or three items?
- Do produced items have a lifecycle (draft, approved, published, superseded), or is that
  pipeline-specific?
- What is the cycle-detection mechanism — hop budget, lineage inspection, declared chains?
- Can a produced item have several consumers, and what if they conflict?
- Are failed runs' partial outputs items, or discarded?
- Produced and arrived items are one type. Does a produced item then carry fields it never uses,
  or is "produced" purely a lineage property of an item with no extra fields at all?

## Not in scope

- Storage format and location. Architecture, deferred.
