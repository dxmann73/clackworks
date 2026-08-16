# Gates and directives

Status: draft

## Purpose

At every point there is a directive on whether the run halts for a decision and who makes it.
This defines what kinds of involvement exist, where directives sit, and how a human is reached
without being buried.

A gate is the point where a run halts until someone decides. The decider is a human (a **human
gate**) or an agent applying judgement instead of a person (an **agent gate**). A gate can sit
anywhere: at an [ingress](./ingress.md), between steps, before an egress. This file is about
gates in general; humans are the harder case, so most of it is about them.

## In scope

- Modes of involvement and where they attach.
- Who decides at a gate, and what an agent gate may decide.
- How a human is notified and how they respond.
- What happens while waiting, and what happens if nobody answers.

## Modes of involvement

| Mode      | Meaning                                                                 |
| --------- | ----------------------------------------------------------------------- |
| Autonomous | Run proceeds, human sees it only in the trace or a digest               |
| Notify    | Run proceeds, human is told it happened                                 |
| Approve   | Run pauses, proceeds only on explicit approval                          |
| Veto      | Run proceeds after a delay unless the human stops it                    |
| Decide    | Run pauses and the human supplies the decision the system cannot make   |

## Requirements

- Every point that can carry a directive does: routing decisions, ingresses, pipeline steps,
  egresses, outward actions, exhaust decisions.
- A directive names whether a gate sits at that point and who decides at it. An agent gate is
  configured the same way a human gate is; only the decider differs.
- An agent gate is recorded like a human one — which agent decided, on what, with what reason.
  An agent deciding is not the same as no gate.
- Directives are configuration, changeable without touching pipeline logic.
- Directives can be conditional — approve above a value threshold, autonomous below it;
  approve for outward-facing actions, autonomous for internal ones.
- Outward actions (send, publish, post, commit, pay) default to requiring a human until
  explicitly promoted to autonomous. Trust is earned per action, not granted globally.
- A paused run must show a human everything needed to decide, in one place, without hunting.
- Notification must be reachable where the human actually is, and must not become noise —
  batching and digests are part of the design, not an afterthought.
- Timeouts are defined per gate. A run waiting forever is a failure mode; what happens on
  timeout (proceed, abandon, exhaust) is declared.
- Human decisions are recorded with the same lineage as everything else — who, when, what they
  saw, what they chose.
- An emergency stop exists: halt everything, or halt one persona or pipeline.

## Open questions

- Where does a human actually answer — mail reply, chat, a dedicated interface, all three?
- Can a directive escalate itself: autonomous normally, approve when the agent is unsure?
- Does approving something teach the system, and if so is that an explicit act or automatic?
- Who is "the human" if there is ever more than one?
- How do we avoid approval fatigue turning into rubber-stamping?
- Can a human intervene mid-run at a point that has no gate?
- Which decisions may an agent gate make at all, and can an agent gate escalate to a human one
  instead of deciding?

## Not in scope

- Notification channel implementation.
