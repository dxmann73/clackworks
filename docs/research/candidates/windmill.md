# Windmill

Status: draft. Checked 2026-08-16, from vendor docs and repository metadata — not from a running
instance.

Rust engine with Postgres as the only hard dependency. Scripts and flows, branch steps,
suspend/approval gates, AI agent steps, inbound SMTP triggers, worker groups.

|           |                                                |
| --------- | ---------------------------------------------- |
| Licence   | AGPLv3 core + proprietary enterprise bits      |
| Self-host | yes                                            |
| Paid from | Team $10/user; Enterprise on request           |
| Health    | ★17.5k, pushed daily                           |
| Verdict   | **Passes the one-product test. Strongest fit** |

## What it gives you

**Triggers, on the community edition.** Webhooks, custom HTTP routes, cron schedules, WebSocket,
Postgres CDC, MQTT, native triggers for Nextcloud, Google Drive and Google Calendar, CLI, REST
API, MCP, and inbound email.

**Email triggers are a first-class producer.** Self-hosted Windmill runs its own SMTP server.
Every script and flow gets a default trigger address of the form
`<path>+<workspace+path+token base32>@<domain>`, and custom addresses shaped
`{workspace_id}-{local_part}@yourdomain.com` can be registered. Extra arguments ride along in the
address as `alerts+env=prod&debug=true@yourdomain.com`. The handler receives `raw_email`, a
`parsed_email` with headers, and `email_extra_args`. Custom triggers can be disabled and
re-enabled without being deleted. **Community edition caps this at 100 emails per day.** Closest
thing in the survey to a per-persona mail address.

**Routing is a branch step.** `Branch one` evaluates JavaScript predicates in order — e.g.
`results.classify.category === 'invoice'` — runs the first branch whose predicate is true, and
falls through to a default branch when none is. `Branch all` fans out to every branch, optionally
in parallel. Every branch is itself a flow. The predicates live in the flow definition, which is
versioned and diffable — the "routing as data" property
[routing.md](../../../_incoming/routing.md) asks for. The classification that *feeds* the
predicate is still a prompt: the decision is inspectable, the judgement behind it is not.

**Gates are suspend/approval steps.** A step suspends and emits resume and cancel URLs. The
community edition gives a configurable approval count, a timeout, "continue on disapproval or
timeout" so a following `branch one` can handle both outcomes separately, the approver list
exposed to the next step, flow-level pre-approval URLs, self-approving prompts, and a helper for
interactive Slack approvals. **Enterprise only:** attaching a schema *form* to the approval page,
and approver permissions (must be logged in, must be in a group, no self-approval).

**Agent-driven pipelines are a step type.** The AI agent step takes a provider and model
(Anthropic, OpenAI, Gemini, Mistral, Bedrock, OpenRouter, DeepSeek, custom endpoints), a
reasoning-effort setting, script tools drawn from inline code, workspace scripts or the Windmill
Hub, MCP servers as tools, a built-in websearch tool, and nested agents as tools to one level of
depth. `memory` is `auto` or `manual`; in auto mode it keeps the last N messages, keyed by a
`memory_id` when driven over a webhook — conversation-scoped, not persona-scoped.

**Composition and compute.** Flows call flows ("inner flows"), plus workflows-as-code for
script-shaped work. Worker groups with tags pin a heavy pipeline to its own workers, its own
memory limit and its own image, which is what
[UC-2](../../specification/use-cases/uc-2-software-development-from-a-todo.md) means by assigning
compute.

**Durability, precisely.** Jobs live in Postgres. A worker that stops pinging for
`ZOMBIE_JOB_TIMEOUT` (default 30s) has its job declared a zombie, and `RESTART_ZOMBIE_JOBS`
(default `true`) restarts it in place under the same UUID. That is at-least-once at step
granularity: the interrupted step re-runs from the beginning, so steps must be idempotent. Not
Temporal-style replay, and weaker than [Activepieces](./activepieces.md), whose documented
guarantee is that a completed step's output is checkpointed and never re-run.

## For

- Inbound SMTP with per-flow and custom addresses — no other candidate has persona addressing
  natively.
- Routing predicates are versioned, diffable flow data.
- Worker groups give per-pipeline isolation, image and memory budget — the only candidate that
can host a coding agent directly.
- Rich free-build gate surface: approval counts, timeouts, continue-on-disapproval.
- Postgres-only. Lightest operational footprint of the finalists.

## Against

- **AGPLv3 plus proprietary enterprise bits.** Live question against the open-core path in
  [VISION.md](../../../VISION.md#how-clackworks-itself-is-licensed) if Clackworks links Windmill
  code.
- **100 emails/day** on community email triggers.
- **Approval forms are enterprise.** A decision richer than approve/reject needs the
self-approving-prompt pattern or a separate small flow.
- At-least-once step restart, not checkpointing. Agent steps with side effects are not naturally
idempotent.
- 30-day job-run detail retention on the free build.

## Free-build ceilings

3 workspaces, 50 users, 4 permission groups, 10 GiB object storage, **30-day job-run detail
retention**, 100 emails/day on email triggers, git sync for 2 users.

## Mapping to use cases
