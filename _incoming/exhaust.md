# Exhaust

Status: draft

## Purpose

Everything that cannot be routed, cannot be handled, or failed lands here. The exhaust is a
decision queue, not a wastebasket. Its size is a health metric.

## In scope

- What ends up in the exhaust and why.
- The decision loop that empties it.
- Keeping it from becoming a graveyard.

## Requirements

- Nothing is silently dropped. Unroutable, refused, failed, and low-confidence items all land
  in the exhaust with a reason attached.
- Each entry carries: the item, its provenance, why it ended up here, and what was tried.
- Entries are grouped or clustered where possible, so that twenty similar unroutable items
  present as one pattern and one decision, not twenty chores.
- Every entry has a set of available decisions: build a pipeline for this, route it to an
  existing pipeline after all, handle it manually, or discard deliberately.
- Discarding is an explicit act and is recorded. Deliberate discard is fine; silent decay is
  not.
- The system should propose: "these fourteen items look like the same kind of thing, a pipeline
  here would handle them".
- Re-injection must be possible: after a new pipeline exists, exhaust items matching it can be
  replayed through routing.
- Exhaust size and growth rate are visible, because a growing exhaust means routing is losing.

## Open questions

- Are failures and unroutables in the same pile, or are those two different queues with
  different urgency?
- Retention: how long does an item sit in the exhaust before it is escalated, archived, or
  auto-discarded?
- Who does the clustering — a rule, a classifier, an agent with its own persona?
- Does replaying an old item risk acting on something stale (an invoice long since paid), and
  how is that guarded?
- Is there an exhaust for artifacts nobody consumed, separate from unroutable inputs?

## Not in scope

- The user interface for working through the exhaust — see
  [control-plane.md](./control-plane.md).
