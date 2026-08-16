# Personas and identity

Status: draft

## Purpose

Agents are addressable counterparts, not anonymous function calls. This defines what makes up a
persona and what it means for an agent to have its own channels in the world.

## In scope

- What a persona consists of.
- Owned channels: email address, social accounts, messaging handles.
- Disclosure, boundaries, and what a persona may say or do as itself.

## Requirements

- A persona has: a name, a personality and voice, a domain of competence, and its own channels.
- Channels are per persona: the tax advisor has its own mail address; the surf coach has its
  own. Mail to a persona's address routes to that persona's pipelines.
- A persona's address works in both directions. Inbound, mail to the tax advisor's address
  arrives through the mail producer like any other mail, and the address travels with the item,
  which is all the router needs to send it to that persona. Outbound, the persona replies from
  the same address through an egress.
- Routing to a persona needs no classifier. Typically the item lands in a general-purpose
  pipeline where the agent looks at it and decides what to do — see [routing.md](./routing.md).
- A persona can act outward through its channels — reply to mail, post, message — subject to
  directives about human approval.
- Personality shapes voice and judgement style, not competence boundaries. Boundaries are
  capabilities, defined in [agents.md](./agents.md).
- Personas are stable over time. Renaming or re-scoping a persona is a deliberate act with
  consequences for its memory and its channels.
- A persona must be identifiable as an agent to outside recipients where that matters — this is
  a policy decision per channel, and it must be an explicit one, not an oversight.
- Personas can address each other, and messages between personas are as traceable as anything
  else.

## Initial persona sketches

| Persona         | Domain                                 | Channels                    |
| --------------- | -------------------------------------- | --------------------------- |
| Tax advisor     | Invoices, receipts, filings, deadlines | Mail                        |
| Nutrition coach | Recipes, meals, goals                  | Mail, messaging             |
| Surf coach      | Conditions, trips, gear, sessions      | Mail, messaging             |
| Developer       | Issues, TODOs, branches, PRs           | Issue tracker, repositories |
| Editor          | Thoughts to drafts to published posts  | Website, mail               |

## Open questions

- Do outside recipients get told they are talking to an agent? Per channel, per persona, or
  globally? Legal and ethical implications differ per jurisdiction and channel.
- One persona to one pipeline, or one persona across many pipelines?
- Can two personas share a channel, or is that always one-to-one?
- What happens to a persona's memory and history if the persona is retired?
- Does the user have a persona in the system, and are they routable like the others?
- Do personas have visible profiles — is there a place where you can see who exists and what
  they do?
- How are persona credentials and accounts provisioned and kept out of the repo?

## Not in scope

- Concrete account setup and provider choice.
- Prompt content for personalities. That is implementation.
