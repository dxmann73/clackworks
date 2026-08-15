# Clackworks — Vision

## The problem

Useful signals arrive constantly and from everywhere: mail, YouTube videos, half-formed ideas in
text files, TODOs scattered across repositories, issues, WhatsApp messages, bookmarks. Each one
implies work. Almost none of it gets done, because every item needs a human to notice it,
classify it, decide what it deserves, and then actually do the thing.

The classification and the routing are the expensive part. The work itself is often mechanical,
or at least delegable.

## The idea

Clackworks is a chain-reaction machine for work. Something drops in at the top, and a sequence
of mechanisms fires: routed, transformed, handed to an agent, turned into an artifact, which
drops in at the top again as input for the next mechanism.

The pieces:

- **Producers** emit items — mail, videos, notes, TODOs, issues, messages, feeds, or another
  pipeline's output.
- **Intake** is the single funnel. Everything enters here. Intake normalizes, authenticates,
  applies security policy, and routes.
- **Pipelines** do the work. A pipeline is either deterministic (fixed steps, predictable
  output) or agent-driven (an agent with judgement, tools, and a goal). Pipelines are freely
  composable and created by the user.
- **Agents** run the agent-driven pipelines. An agent is not an anonymous model call — it has a
  name, a personality, its own email address and social accounts, and its own memory that
  persists across everything it handles.
- **Artifacts** are what pipelines produce. An artifact can be terminal (a published blog post,
  a sent mail, a merged PR) or it can become a producer, feeding the next pipeline.
- **Exhaust** catches everything unroutable. Nothing is silently dropped. The exhaust is a
  queue of decisions: build a new pipeline for this, handle it manually, or discard it
  deliberately.
- **Human gates** can sit at any point. Every step carries a directive about whether a human is
  consulted, notified, or bypassed entirely.

## Why the agents have identities

An agent with a name, an inbox, a personality, and a memory is addressable. You can mail the
tax advisor directly. The nutrition coach can reply to you as itself. The surf coach remembers
what it told you in March. This is not decoration — identity is what makes an agent a stable
counterpart instead of a stateless function call, and it is what lets agents be producers and
consumers for each other.

## What it looks like in use

**Mail triage.** Mail lands in intake. Invoices route to the tax advisor, who files, extracts,
and reconciles them. Recipes route to the nutrition coach, who evaluates them against current
goals and files the good ones. Surf-related mail routes to the surf coach. Anything ambiguous
lands in the exhaust with a proposal for where it might belong.

**Software development.** An issue or a repository TODO enters intake, routes to a development
pipeline with a model and compute budget assigned, and produces a branch and a PR. The PR is an
artifact; a review pipeline consumes it; the merge is terminal. A human gate sits before merge.

**Thinking out loud.** A stray thought in a text file, or a YouTube video worth chewing on,
enters intake. A pipeline extracts the substance, checks it against what has been written
before, and produces a draft. Another pipeline turns accepted drafts into blog posts on the
website. Human gate before publishing.

## Principles

- **One front door.** Everything enters through intake. Security and routing live there, not
  scattered across pipelines.
- **Nothing disappears.** Unroutable is a state with a queue attached, not a silent drop.
- **Fail fast.** A value that is not what the system expects stops the run. No silent defaults,
  no empty placeholders that surface as garbage three pipelines downstream.
- **Composable over clever.** Small pipelines that chain beat one large pipeline that branches.
- **Human by directive, not by accident.** Whether a human is involved is a configured
  decision at each point, visible and changeable.
- **Agents are named counterparts.** Identity, personality, and memory are first-class, not
  prompt decoration.

## What success looks like

- A new producer can be attached without touching any pipeline.
- A new pipeline can be created without touching intake.
- An item can be traced end-to-end: where it came from, every hop, every decision, every human
  touch, what it became.
- The exhaust stays small, and shrinking it is an obvious, low-effort activity.
- Work that used to need a human classifier happens without one, and the cases that genuinely
  need a human reach them with context attached.

## Explicit non-goals

- Not a general workflow engine competing with existing orchestrators. It is a personal system
  first.
- Not a chat interface. Conversation is one possible producer, not the centre.
- Not autonomous-by-default. Human gates are a feature, not a fallback.
- No architecture, stack, or vendor decisions until the specifications in `_incoming/` settle.
