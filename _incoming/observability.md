# Observability

Status: draft

## Purpose

An item must carry lineage end to end, and the health of the machine must be visible at a
glance. A chain reaction you cannot watch is a chain reaction you cannot debug.

## In scope

- Lineage: following a single item through every hop.
- System health and the signals that matter.
- Audit and reproducibility.

## Requirements

- Lineage per item, end to end: where it came from, why the router chose what it chose, what the
  ingress did, every step of every run, every gate decision and who made it, everything produced,
  and what chained onward.
- Lineage survives chaining. Following a published post back to its origin is one query, not
  detective work.
- Decisions are recorded with their reasons, including model-made decisions and their
  confidence.
- Health signals that matter: exhaust size and growth, failure rate per pipeline, items in
  flight, runs waiting on a human and for how long, cost per pipeline and persona.
- Anything waiting on a human is visible with its age. A gate nobody answered for a week is a
  problem the system should surface.
- Reproducibility: for a deterministic pipeline, a past run can be re-run and compared. For an
  agent-driven one, the inputs, model, and prompt state are recorded well enough to explain
  what happened even if it cannot be reproduced exactly.
- Traces are inspectable by the user without special tooling gymnastics.

## Open questions

- How much content goes into a trace versus a reference to it — traces of mail bodies and
  transcripts get large and sensitive fast.
- Retention: how long are traces kept, and does that differ per trust level or channel?
- Is there a live view, a periodic digest, or both?
- What is the alerting threshold, and does alerting itself go through a human gate directive?
- Does the system report on itself — a weekly summary of what it handled and what it dropped?
- Do agents get to read their own traces as a form of self-correction?

## Not in scope

- Tooling and storage choice.
