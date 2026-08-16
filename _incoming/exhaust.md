# Exhaust

Status: draft

## Purpose

Unroutable items land here: something arrived and the router cannot route it, so no pipeline
gets it. The exhaust is a decision queue, not a wastebasket. Its size is a health metric.

The shape of the case: a mail arrives from some entity, the router cannot route it, no pipeline
knows what to do with it, and it fits nowhere. It still has to go somewhere.

## In scope

- What ends up in the exhaust and why.
- The decision loop that empties it.
- Keeping it from becoming a graveyard.

## Requirements

- Nothing is silently dropped. An unroutable item lands here with a reason attached. Whether
  items refused at an ingress, failed runs, and low-confidence classifications share this pile
  or get their own is open below.
- Each entry carries: the item, its provenance, why it ended up here, and what was tried.
- Entries are grouped or clustered where possible, so that twenty similar unroutable items
  present as one pattern and one decision, not twenty chores.
- Every entry has a set of available decisions: build a pipeline for this, route it to an
  existing pipeline after all, handle it manually, or discard deliberately.
- Discarding is an explicit act and is recorded. Deliberate discard is fine; silent decay is
  not.
- The system should propose: "these fourteen items look like the same kind of thing, a pipeline
  here would handle them".
- Re-processing must be possible: after routing or pipelines change, exhaust items are run
  through routing again.
- Exhaust size and growth rate are visible, because a growing exhaust means routing is losing.

## Open questions

- What counts as unroutable, concretely — the router found no destination, it found one and the
  destination refused, or a classifier came back below confidence?
- Are failures, ingress refusals, and unroutables in the same pile, or are those separate queues
  with different urgency? The glossary scopes the exhaust to unroutable items, and the other two
  currently have nowhere else to go.
- Retention: how long does an item sit in the exhaust before it is escalated, archived, or
  auto-discarded?
- Who does the clustering — a rule, a classifier, an agent with its own persona?
- Does re-processing an old item risk acting on something stale (an invoice long since paid),
  and how is that guarded?
- A produced item its egress router cannot place is unroutable and lands here. Does it need
  distinguishing from an unroutable arrival, or is one pile enough?

## Not in scope

- The user interface for working through the exhaust — see
  [control-plane.md](./control-plane.md).
