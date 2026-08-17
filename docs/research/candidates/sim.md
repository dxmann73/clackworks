# Sim

Status: draft. Checked 2026-08-16, from vendor docs and repository metadata — not from a running
instance.

Block-graph workflow builder, agent-native. One trigger per workflow, Human in the Loop block
with resume forms, knowledge base and tables built in.

|           |                                                                                          |
| --------- | ---------------------------------------------------------------------------------------- |
| Licence   | Apache-2.0, whole repo                                                                   |
| Self-host | yes — `docker compose` with Postgres and pgvector                                        |
| Paid from | Cloud plans; enterprise tier is org-shaped features only                                 |
| Health    | ★29.4k, pushed daily                                                                     |
| Verdict   | **Passes the one-product test. Second** ([why](../prior-art.md#the-all-in-one-product-verdict)) |

The enterprise tier is access-control groups, SSO, audit logging and workspace forking -
organisation shape only, which is exactly where
[VISION.md](../../../VISION.md#how-clackworks-itself-is-licensed) says the paid edge belongs.

## What it gives you

**Shape.** A workflow has exactly one trigger and a graph of blocks. Native triggers: Start
(manual, API, chat), Schedule, Webhook, RSS, Table row-change. Integration triggers cover Gmail,
Outlook, **IMAP against any provider** with filters on sender, subject and folder plus attachment
extraction, GitHub, WhatsApp, Teams and a long tail of others. Blocks split three ways: work
blocks (Agent, Function, API), flow blocks (Condition, Router, Loop, Parallel), and run-shaping
blocks (Response, Guardrails, Wait, Human in the Loop). A **Workflow block** calls another
workflow's latest deployed version and waits for it, which is how pipelines chain.

**Gates are a first-class block.** Human in the Loop pauses the run with no timeout until someone
answers through the approval portal, the API, or a webhook. It carries display data assembled from
earlier block outputs, a notification channel (Slack, Gmail, Teams, webhook) that carries the
approval URL, and a **Resume Form** for structured input on the way back in. All of it is in the
Apache-2.0 build — no enterprise gate, which is where it beats [Windmill](./windmill.md).

**Memory and retrieval.** Knowledge Base and Tables are native, so duplicate-detection against
past writing needs no external pgvector.

## For

- **Apache-2.0 across the whole repository.** Cleanest licence of the finalists, and the
  enterprise edge sits exactly where the vision puts it.
- Gates in the free build include a resume form and an approval portal — the thing that is
enterprise-only on Windmill.
- Native knowledge base and tables cover persona memory and duplicate detection without a second
system.
- IMAP against any provider, with sender/subject/folder filters and attachment extraction.

## Against

- **One trigger per workflow**, so a multi-producer intake would need one workflow per producer
- **Triggers fire the active deployment.** An edited workflow does nothing until redeployed. A Gotcha.
- **No per-persona inbound addresses.** IMAP means one mailbox per account, so persona addressing
  has to be a `To:` filter over a catch-all.
- **The Router block is model-driven**, so routing is a prompt rather than data unless Condition
  blocks are used instead.
- **Code runs in** `isolated-vm`, maybe problematic for a coding agent that needs a real filesystem
- **Crash recovery mid-run is unverified.** Comparison pages claim durable pause and resume for
  the Human in the Loop block; whether a worker crash mid-Agent-block resumes or restarts is not
  clear from the docs.

## Mapping to use cases

