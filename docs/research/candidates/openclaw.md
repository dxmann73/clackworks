# OpenClaw

Status: draft. Checked 2026-08-15, from vendor docs and repository metadata — not from a running
instance.

Far more than an assistant — a gateway across WhatsApp/Telegram/Signal/iMessage/Matrix/Slack/
Teams/Discord/IRC, multi-agent, pipelines, durable flows.

|           |                                                                                                      |
| --------- | ---------------------------------------------------------------------------------------------------- |
| Licence   | MIT                                                                                                  |
| Self-host | yes                                                                                                  |
| Paid from | —, everything                                                                                        |
| Health    | ★386k, pushed 2026-08-15, 5,527 open issues                                                          |
| Verdict   | **Passes the one-product test.** Lobster is the pipeline and the gate; the router-agent hop is fine. |

## What it gives you

| Capability                 | What exists                                                                                                                                              | Clackworks area                                                                      |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Deterministic pipelines    | **Lobster** — multi-step tool pipelines as a small constrained DSL, run as one tool call. Pipelines are data: loggable, diffable, replayable             | [pipelines.md](../../../_incoming/pipelines.md)                                      |
| Human gates                | Lobster halts on side effects (send, post, delete) and returns a resume token; approve and resume without re-running earlier steps                       | [gates-and-directives.md](../../../_incoming/gates-and-directives.md)                |
| Durable orchestration      | **Task Flow** — multi-step flows with status, JSON state, revision counter, `waiting`/`blocked` states; survives gateway restarts                        | [pipelines.md](../../../_incoming/pipelines.md)                                      |
| Many agents, own memory    | Isolated agents, each with workspace, persona files (`SOUL.md`, `IDENTITY.md`, `USER.md`), own SQLite session store, per-agent memory-wiki vaults        | [agents.md](../../../_incoming/agents.md), [memory.md](../../../_incoming/memory.md) |
| Persona inbound addressing | **Bindings** map a channel account — a Slack workspace, a WhatsApp number — to one agent. Address-based only; email is not a binding target              | [personas-and-identity.md](../../../_incoming/personas-and-identity.md)              |
| Email                      | IMAP/SMTP tool, or Gmail over OAuth with `watch` publishing to PubSub and pushed to a Gateway hook. Reachable as a tool, not as a binding                | [producers.md](../../../_incoming/producers.md)                                      |
| Agent-to-agent handoff     | `sessions_send` targeting another agent's session key (e.g.`agent:tax-advisor:main`), same gateway only. Off by default                                  | [routing.md](../../../_incoming/routing.md)                                          |
| Command-level approval     | **Exec approvals** — host guardrail with `deny`/`allowlist`/`ask`/`auto`/`full`, per agent                                                               | [security-and-trust.md](../../../_incoming/security-and-trust.md)                    |
| Untrusted input            | Documented reader shape: dedicated agent, `sandbox.mode: all` with `scope: session`, `workspaceAccess: none`, `tools.profile: minimal`, `deliver: false` | [security-and-trust.md](../../../_incoming/security-and-trust.md)                    |
| Scheduling                 | Cron jobs and background tasks invoking agent sessions                                                                                                   | [triggers-and-scheduling.md](../../../_incoming/triggers-and-scheduling.md)          |

### Lobster, in detail

Lobster is the pipeline mechanism that makes OpenClaw worth checking at all: a small constrained
DSL for multi-step tool pipelines, run as a single tool call rather than as free-form agent
reasoning. Because a Lobster pipeline is data, it is loggable, diffable and replayable, as
required by [pipelines.md](../../../_incoming/pipelines.md) asks for.

Lobster is also the gate mechanism: it halts on side effects (send, post, delete) and returns a
resume token, so approving and resuming does not re-run the steps that already ran. The docs' own
worked example for Lobster is recurring email triage with an approval halt before sending drafts —
[UC-1](../../../_incoming/use-cases.md#uc-1-mail-triage-into-personas) minus the classification
step. A router agent that classifies and calls `sessions_send` supplies that step, so
content-based routing into personas is assemblable today.

## For

- **Every mechanism in the vision exists here**
- Persona files (`SOUL.md`, `IDENTITY.md`, `USER.md`) are the cleanest reference for what
  [personas-and-identity.md](../../../_incoming/personas-and-identity.md) should look like
- Widest channel gateway of any candidate: WhatsApp, Telegram, Signal, iMessage, Matrix, Slack,
  Teams, Discord, IRC in one process.

## Against

- **Content-based routing:** The router is an agent making a judgement and calling
  `sessions_send`; the route itself is neither data nor a record. Is a bit off compared against
  [routing.md](../../../_incoming/routing.md)'s bar.
- **No email binding.** Email is reachable as a tool, not as a first-class producer address —
  weaker than Windmill's per-flow inbound SMTP addresses.
- `sessions_send` **is same-gateway only**
- **Full-system-access default** is a direct problem for
  [security-and-trust.md](../../../_incoming/security-and-trust.md) and must not be adopted
  uncritically.

## Mapping to use cases

