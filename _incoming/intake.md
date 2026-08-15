# Intake

Status: draft

## Purpose

The single front door. Everything from every producer enters here, gets normalized into a
common shape, gets admitted or rejected on policy, and gets handed to routing.

## In scope

- Normalization: turning wildly different sources into one item shape.
- Admission control: authenticity, trust, quotas, deduplication.
- The handoff to routing, and what happens when routing says unroutable.

## Requirements

- One front door. No producer bypasses intake, no pipeline is reachable without going through
  it.
- Normalization produces a common item shape carrying at minimum: provenance, arrival time,
  content or a reference to it, sender or author where one exists, and the raw original.
- The raw original is preserved. Normalization is additive, never lossy in a way that prevents
  going back.
- Fail fast: an item that cannot be normalized is rejected loudly and lands in the exhaust with
  the reason. No partial items with empty placeholder fields.
- Admission checks happen here, not in pipelines: is the sender who they claim to be, is this
  source trusted, is this within quota, have we seen this already.
- Deduplication is intake's job. The same mail arriving twice produces one item.
- Every item gets a stable identity at intake, which follows it through every hop and artifact.
- Rejection is never silent. A rejected item is recorded with the reason and is visible.

## Open questions

- Does intake do any classification, or is it purely normalize-and-admit with all judgement in
  routing?
- Is there one intake or several (per trust level, per volume class)? "Single front door" is a
  logical claim — does it need to be one physical path?
- What is the common item shape, concretely? Deferred until several producers are specified.
- How much enrichment happens here — transcript fetching, thread reconstruction, attachment
  extraction — versus in a pipeline?
- What is the dedup key per producer kind, and who defines it?
- Rate limiting: what happens when a producer floods intake?

## Not in scope

- Choosing pipelines. That is [routing.md](./routing.md).
- Doing work on the item. Intake never transforms content beyond normalization.
