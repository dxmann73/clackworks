# Producers

Status: draft

## Purpose

Define what counts as a source of items, what a producer must provide, and how a new one is
attached without touching anything downstream.

## In scope

- The catalogue of producer kinds we want at the start.
- The minimum contract a producer satisfies.
- How producers are added, paused, and removed.

## Producer kinds (initial target set)

| Kind             | Examples                                    | Notes                             |
| ---------------- | ------------------------------------------- | --------------------------------- |
| Mail             | Personal and business inboxes               | Highest-volume source, mixed kinds |
| Messaging        | WhatsApp, Telegram, Signal                  | Personal, consent-sensitive        |
| Video / media    | YouTube subscriptions, watch-later, podcasts | Needs transcript extraction       |
| Notes and ideas  | Text files in a folder, voice memos          | Unstructured, no metadata         |
| Repositories     | TODO/FIXME comments in code                  | Needs scanning, dedup across runs |
| Issue trackers   | GitHub issues, PRs, review requests          | Already structured               |
| Feeds            | RSS, newsletters, bookmarks                  | High volume, low signal           |
| Calendar         | Events, invitations                          | Time-bound                        |
| Internal         | Artifacts from a pipeline                    | Closes the loop; see chaining     |

## Requirements

- A producer emits items; it does not classify, route, or interpret them.
- Every item carries provenance: which producer, which account, when, an origin reference that
  can be followed back to the source.
- Producers must be idempotent about re-emission — seeing the same source object twice must not
  create two items. What "the same" means is per producer and must be stated explicitly.
- Adding a producer requires no change to any pipeline.
- A producer can be paused without losing what accumulates while it is paused, or explicitly
  drop it — that choice is per producer, and stated.
- A producer that cannot reach its source fails loudly. No silent empty batches.

## Open questions

- Does a producer push into intake, does intake pull from it, or both — see
  [triggers-and-scheduling.md](./triggers-and-scheduling.md)?
- Backfill: when a producer is attached, does it see history, or only what arrives afterwards?
- How much of a large source item travels with it (full video transcript, full mail thread,
  entire file) versus a reference to be fetched later?
- Who owns credentials for a producer — the producer, the control plane, or a shared secret
  store?
- Do attachments and embedded media become separate items or stay part of the parent?

## Not in scope

- Routing and classification. Producers stay dumb.
- Per-source parsing detail. Belongs with each producer's own spec once we start building.
