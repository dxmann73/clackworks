# Outputs and chaining

Status: draft

## Purpose

Pipelines produce something. That something either ends the chain or starts the next one. This
defines what a produced item carries and how the loop back in works without becoming chaos.

What a pipeline produces is an item. There is no second type and no extra fields: it is the same
kind of thing that entered, and "produced" is purely a statement about lineage. An item is
mutable and carries its history, so a step changing an item does not mint a new type of thing.

## In scope

- What "produced" means for an item's lineage and history.
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
- A produced item that is not terminal and that the router attached to its egress cannot place
  is unroutable like anything else, and lands in the exhaust rather than vanishing.

## Open questions

- Are produced items stored by the system, or only referenced where they live (in a repo, on the
  website, in a mailbox)?
- A draft revised three times: one item whose history carries the three revisions, or three
  items linked by lineage? Items are mutable with history, which permits either.
- Do produced items have a lifecycle (draft, approved, published, superseded), or is that
  pipeline-specific?
- What is the cycle-detection mechanism — hop budget, lineage inspection, declared chains?
- Can a produced item have several consumers, and what if they conflict?
- Are failed runs' partial outputs items, or discarded?

## Not in scope

- Storage format and location. Architecture, deferred.
