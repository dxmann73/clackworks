# Agents

Status: draft

## Purpose

Define the agent as a worker: what it is given, what it may do, what it must report. Identity
and personality are covered separately in
[personas-and-identity.md](./personas-and-identity.md).

## In scope

- What an agent receives when it starts a run.
- Tools and capability boundaries.
- Autonomy limits, escalation, and reporting.

## Requirements

- An agent runs an agent-driven pipeline. It has a goal, an item, tools, memory, and an
  assigned model.
- An agent's capabilities are declared and bounded. The tax advisor can file and read invoices;
  it cannot push to a repository. Capability is per agent, not global.
- Agents can produce artifacts and can hand items back to intake. They cannot invoke arbitrary
  pipelines directly — chaining goes through the normal path so it stays observable.
- An agent must be able to say "I cannot handle this" and route the item to the exhaust with a
  reason, instead of producing something plausible and wrong.
- An agent must be able to escalate to a human mid-run, not only at predefined gates.
- Every agent action that touches the outside world (send mail, post, commit, pay, publish) is
  recorded, and each such capability carries its own directive about human involvement.
- Agents can be counterparts to each other: one agent's output can be another's input, and
  agents can address each other through their channels.
- An agent's run reports what it did, what it decided, and what it deliberately did not do.

## Open questions

- Do agents run one item at a time, or can one agent have several concurrent runs? Memory makes
  this non-trivial.
- Can an agent create or modify pipelines and routing rules, or is the control plane
  human-only?
- Do agents negotiate — can the nutrition coach ask the surf coach a question mid-run, and what
  does that do to the trace?
- Is there a supervisor or orchestrator agent, or is routing the only dispatcher?
- What happens when an agent's assigned model is unavailable — fail, downgrade, queue?
- How is agent output quality judged, and by whom?

## Not in scope

- Names, personalities, mail addresses, social accounts —
  [personas-and-identity.md](./personas-and-identity.md).
- What agents remember — [memory.md](./memory.md).
- Model choice and budgets — [models-and-resources.md](./models-and-resources.md).
