# Open questions

Status: draft

## Purpose

Cross-cutting unknowns that no single area file owns, plus decisions deliberately deferred.
Area-specific questions stay in their own files.

## Scope and ambition

- Personal system for one person, or something others could run? The answer changes almost
  every other decision.
- What is the smallest version that is genuinely useful, and which use case does it serve?
- Is this replacing existing tooling, or sitting alongside it?

## Conceptual

- Is "item" one type throughout, given that artifacts become items?
- Is routing a component or is it the system, with pipelines as plugins?
- Do personas own pipelines, or do pipelines borrow personas?
- How much should the system be able to change itself — new pipelines proposed by agents,
  routing rules learned from corrections — and how does that stay safe and visible?

## Human factors

- What does the daily loop actually feel like? If it costs more attention than it saves, it
  fails regardless of how good the machinery is.
- How is approval fatigue avoided?
- What does the system do when the user ignores it for two weeks?

## Legal and ethical

- Agent disclosure to outside correspondents — per channel, per persona, or always?
- Third-party personal data flowing through mail and messaging: retention, minimization, and
  whether GDPR obligations apply.
- Liability for autonomous outward actions (a reply that commits to something, a payment).

## Deliberately deferred

- Stack, language, framework.
- Storage, queueing, execution engine.
- Hosting and deployment.
- Data schemas and identifiers.
- Model providers and specific model IDs.

These stay deferred until the area specs above are settled enough that the choice is obvious
rather than arbitrary.
