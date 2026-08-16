# Clackworks — Vision

## The problem

Useful signals arrive constantly and from everywhere: mail, YouTube videos, half-formed ideas in
text files, TODOs scattered across repositories, issues, WhatsApp messages, bookmarks. Each one
implies work. Almost none of it gets done, because every item needs a human to notice it,
classify it, decide what it deserves, and then actually do the thing. We could let an agent do
that, but how do we wind up with deterministic results?

## The idea

Clackworks lets you build chain-reaction machines for work, similar to a Rube Goldberg machine.

Drop an item (mail, TODO, PR) in at the top, and a sequence of mechanisms fires: routing,
transformation, pipelines driven by agents, gated by humans or other agents.
This in turn may create other items, which may be the input for the next mechanism.

The pieces:

- **Items:** Can be email, videos, notes, TODOs, issues, messages, feeds, blog posts, PRs.
- **Producers** emit items. Abstraction around the item source (GMail producer, GitHub issue producer)
- **Routers** decide where an item goes and take it there, based on the item + rules. Abstraction around
  the transport (send by mail, send via WhatsApp, send to Slack)
- **Pipelines** do the work. A pipeline is either deterministic (fixed steps) or agent-driven.
  Pipelines work on items and optionally produce other items.
  Pipelines can act as producers.
- **Agents** run the agent-driven pipelines. Agents can run anywhere. They have a name, a personality,
  email address, maybe social accounts, and memory that persists across sessions.

- **Ingress** sits in front of a pipeline and filters, enriches, and can raise a gate before the
  item goes in.
- **Egress** is the pipelines output. An egress can act as a producer; routing what
  comes out of it means attaching a router to it.
- **Gates** can sit at any point and involve humans or agents.

- **Exhaust** catches everything unroutable — something arrived and the router cannot route it.
  You can decide to build a new pipeline for this, change routing and re-process it, handle it
  manually, discard it deliberately.

## What it looks like in use

**Mail triage.** Mail arrives from the mail producer. Invoices route to the tax advisor, who
files, extracts, and reconciles them. Recipes route to the nutrition coach, who evaluates them
against current goals and files the good ones. Surf-related mail routes to the surf coach.
Anything ambiguous lands in the exhaust with a proposal for where it might belong.

**Software development.** An issue or a repository TODO arrives from the repository producer,
routes to a development pipeline with a model and compute budget assigned, and produces a branch
and a PR. The PR is an item; a review pipeline consumes it; the merge is terminal. A human
gate sits before merge.

**Thinking out loud.** A stray thought in a text file, or a YouTube video worth chewing on,
arrives from the notes or video producer. A pipeline extracts the substance, checks it against
what has been written before, and produces a draft. Another pipeline turns accepted drafts into
blog posts on the website. Human gate before publishing.

## Constraints

Decided after looking at the [prior art](./docs/research/prior-art-product-overview.md).

- **Open source.** Build this in the open, share it and collaborate. Strength in numbers.
  Public scrutiny. If this ever makes money, it will be from people wielding it skillfully.
- **Self-hosted.** Everything needs to be able to run on hardware the user controls. This
  follows from what Clackworks handles — personal mail, messages, notes, agent memory.
- **Community first, Enterprise second.** Like gitlab does it. An enterprise product is a live
  possibility, so nothing may be adopted whose licence forbids that path.

## How Clackworks itself is licensed

Intent, not yet a licence decision. Open core, on the GitLab and Activepieces model:

- A **community edition that is fully featured for the use cases in this document**, free
  forever, self-hosted. Not a demo, not execution-capped, not workflow-capped. Someone running
  it for themselves should never hit a wall that only money removes.
- **Enterprise features as the paid edge**: Organisation-shaped concerns SSO/SAML, RBAC,
  multi-tenancy, audit logging, compliance may be features behind an enterprise license.

Every use case in the specification must be fully runnable on the free edition. If a
capability that a single user needs ends up behind the paid edge, the split is wrong.

## Positioning

Two comparisons do most of the explaining.

**Like Zapier or Make, but self-hosted and free forever.** The same idea — things arrive,
something fires, work happens — without the two things that make those products unusable here.
Nothing leaves the machine, and the free edition is the whole product rather than a sampler.

**Like OpenClaw, but with a more deterministic approach.** OpenClaw already supports
a lot of messaging channels, and runs several agents with separate personas and separate
memories. What it lacks, as far as we can tell today, is a good way to build deterministic pipelines.

## Why not use existing tools

Detail per product in [docs/research/prior-art-product-overview.md](./docs/research/prior-art-product-overview.md).

**Ruled out — no self-hosted variant.** Zapier, Make, Gumloop, IFTTT; the mail-triage products
Shortwave, Fyxer, Cora, Gmelius, SaneBox. Every one of them requires handing over
the mailbox, the messages, or both. That is the whole corpus this system exists to reason over,
and there is no configuration of a hosted product that keeps it on the premises.

**Ruled out — no workable free tier.** An independent disqualifier, and some products fail both
tests. Zapier caps a workflow at one trigger and one action, Make at two active scenarios,
Gumloop has no free tier at all.

**Ruled out — awkward licence.** n8n is the best-fitting engine in the survey and its
Sustainable Use Licence permits internal use only. Inngest is SSPL until each release ages out.
Restate is BUSL.

**Not ruled out, and closer than expected — OpenClaw.** It has deterministic
multi-step pipelines with built-in approval checkpoints and resume tokens (Lobster), durable
multi-step orchestration that survives restarts (Task Flow), isolated agents with their own
workspace, persona files and memory vaults, and channel-account-to-agent bindings.
The gaps we see are: no content-based routing, no ingress/egress, no email as a routable channel,
no end-to-end lineage for an item.

**Design reference.** Ruled out as a component does not mean ignored as a design. Zapier and
Make know a lot about what a routing UI should feel like, and they support a ton of adapters.
This is the beacon this project aims for.
