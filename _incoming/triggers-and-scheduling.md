# Triggers and scheduling

Status: draft

## Purpose

Define what causes items to appear and what causes pipelines to run — the timing layer between
producers, intake, and pipelines.

## In scope

- Push versus pull for producers.
- Scheduled, event-driven, and manual runs.
- Retry, backoff, and what happens to items during downtime.

## Requirements

- Both push (webhook, callback, watch) and pull (poll, scan on a schedule) must be possible.
  Some sources support only one.
- A pipeline can be triggered by: an item arriving from routing, a schedule, another pipeline's
  artifact, or a human on demand.
- Manual trigger with a hand-supplied item must always be possible — for testing and for
  one-off work.
- Nothing is lost while the system is down. Items that arrive during downtime are processed
  afterwards, or the producer re-emits them.
- Retries are bounded and visible. An item that keeps failing ends up somewhere a human sees
  it, not in an infinite loop.
- Chained runs must not loop forever. A pipeline whose artifact eventually feeds itself needs a
  detectable stop condition.

## Open questions

- Real-time or batch? Is "mail arrives, is handled within seconds" a requirement, or is a
  scheduled sweep every few minutes enough for everything?
- Do items queue at intake, at routing, or per pipeline?
- Priority: does an urgent item overtake a backlog, and who decides urgency?
- What is the loop-prevention mechanism — hop count, provenance chain inspection, explicit
  cycle declaration?
- Are there quiet hours, where nothing that touches a human fires?

## Not in scope

- Concrete scheduler technology.
- Per-producer polling intervals; those settle with each producer.
