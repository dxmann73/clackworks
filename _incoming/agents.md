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

- An agent runs an agent-driven pipeline. It has a persona, a goal, an item, tools, memory, and
  an assigned model.
- An agent is not bound to one machine. It runs wherever it is hosted, and nothing upstream of
  it depends on where that is.
- An agent's capabilities are declared and bounded. The tax advisor can file and read invoices;
  it cannot push to a repository. Capability is per agent, not global.
- Agents can produce items and can hand items back in for routing. They cannot invoke arbitrary
  pipelines directly — chaining goes through the normal path so it stays observable.
- An agent must be able to say "I cannot handle this" and hand the item back with a reason —
  to routing, or to the exhaust when routing has nowhere left to send it — instead of producing
  something plausible and wrong.
- An agent must be able to escalate to a human mid-run, not only at predefined gates.
- Every agent action that touches the outside world (send mail, post, commit, pay, publish) is
  recorded, and each such capability carries its own directive about human involvement.
- Agents can be counterparts to each other: one agent's output can be another's input, and
  agents can address each other through their channels.
- An agent's run reports what it did, what it decided, and what it deliberately did not do.

## Why the agents have identities

An agent with a name, an inbox, a personality, and a memory is addressable. You can mail the
tax advisor directly. The nutrition coach can reply to you as itself. The surf coach remembers
what it told you in March. This is not decoration — identity is what makes an agent a stable
counterpart instead of a stateless function call, and it is what lets agents be producers and
consumers for each other.

## Open questions

- Do agents run one item at a time, or can one agent have several concurrent runs? Memory makes
  this non-trivial.
- Can an agent decide at a gate on another agent's run, and does an agent gate need a persona of
  its own — see [gates-and-directives.md](./gates-and-directives.md).
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
