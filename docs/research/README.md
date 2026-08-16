# Research

Status: draft

Background that informs the specification without being part of it. Nothing here is a
requirement. Nothing here is binding on `_incoming/` or `docs/specification/` except by being
true.

## What is here

| File                                                             | Covers                                                                   |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------- |
| [prior-art.md](./prior-art.md)                                   | The landscape by Clackworks layer, what is genuinely unclaimed, honest assessment |
| [prior-art-product-overview.md](./prior-art-product-overview.md) | Per-product detail: what each one is, self-host status, licence, cost      |
| This file                                                        | How the research is used, and the falsification test both files feed       |

## How this research is used

Two rules, both learned the hard way in earlier revisions of these files.

- **Absence from the market is not evidence of opportunity.** "No product does this" has three
  causes and only one is interesting: too trivial to sell, not a product, or genuinely unserved.
  Every gap gets tested against that before it counts. See
  [What appears unclaimed](./prior-art.md#what-appears-unclaimed).
- **Everything here goes stale.** Surveys are dated at the top of each file. Sketches are from
  vendor docs, not running instances. Re-check before any decision rests on a number, a licence,
  or a feature row.

The falsification test below is the mechanism that turns the survey into a decision. Its method
is to sketch a use case on existing tools and treat whatever they *cannot* express as the
specification. It was run on two opposite architectural shapes — one integrated system, one
best-of-breed assembly — called arm 1 and arm 2.

## Falsification test, arm 1 — OpenClaw, run 2026-08-15

The test was to sketch [UC-1](../../_incoming/use-cases.md) on existing tools and treat whatever
they cannot express as the specification. Sketched against OpenClaw alone. It expresses
effectively all of it.

| UC-1 step                              | OpenClaw mechanism                                                                     |
| -------------------------------------- | ---------------------------------------------------------------------------------------- |
| Mail arrives                           | Gmail API `watch` publishing to a GCP PubSub topic, pushed to a Gateway hook. `openclaw webhooks gmail setup` wires it; the Gateway auto-starts and renews the watch. IMAP/SMTP tool is the alternative |
| Mail reaches a router                  | Hook mapping matched on `path: gmail`, per-message session key `hook:gmail:{{messages[0].id}}`, routed to a dedicated reader agent. Forwarding to a bound chat channel also works |
| Classify: invoice, recipe, surf, other | A router agent turn, or a Lobster pipeline step                                            |
| Route to the right persona             | `sessions_send` targeting another agent's session key, e.g. `agent:tax-advisor:main`. Validated against local config; same gateway only |
| Persona has its own memory and voice   | Separate agent: own workspace, persona files, SQLite session store, memory-wiki vault      |
| Human gate before anything payable     | Lobster halts on side effects and returns a resume token                                   |
| Survives a restart mid-run             | Task Flow durable records, `waiting` and `blocked` states                                  |
| Mail from a stranger is untrusted      | Documented reader shape: dedicated agent, `sandbox.mode: all` with `scope: session` and `workspaceAccess: none`, `tools.profile: minimal`, an untrusted-data message template, `deliver: false` |
| Unroutable pile                        | Whatever the router agent writes it to — a file, a session, a task                         |

The last remaining claim in the survey — content-based routing — turns out to be a router agent
with a classification prompt plus one `sessions_send` call. The email gap recorded earlier was
also wrong in substance: bindings are address-based and email is not among them, but email is
reachable as a tool, so nothing is actually blocked.

**Conclusion: Clackworks as specified is closer to a configuration of OpenClaw than to a new
system.** Every mechanism the vision describes exists, self-hosted, MIT, today.

### What survives arm 1

Not mechanisms — guarantees. OpenClaw gives the parts; it does not give these properties:

- **Routing as inspectable data.** A router agent's judgement is a prompt. It cannot be
  diffed, versioned, replayed, or answered offline. [routing.md](../../_incoming/routing.md)
  requires the decision be recorded with its reason and be testable against a stored item
  *without running anything*. That is a difference in kind, and it is small in code.
- **Item identity and lineage.** OpenClaw traces sessions and tasks. Clackworks traces *items*
  across hops, personas, and outputs that re-enter. Nothing in OpenClaw carries an item id
  through a `sessions_send`.
- **Dedup and idempotent re-emission** across heterogeneous producers, defined per producer.
- **One normalized item shape** with provenance, so a new producer needs no pipeline change.
- **The exhaust as a worked surface** — clustering, proposing a pipeline, re-processing a
  backlog. A router agent can write unroutable items somewhere; nothing makes that a loop.

Whether these justify a separate system, or are a plugin and a set of conventions on top of
OpenClaw, is the decision this test exists to force. It is not answered here.

## Falsification test, arm 2 — Activepieces assembly, sketched 2026-08-15

Arm 1 tested one integrated system. Arm 2 tests the opposite shape: best-of-breed parts wired
together by hand. Different question. Arm 1 asked *does the mechanism exist*; arm 2 asks *what
does the assembly cost*.

**Method: paper sketch, deliberately not an implementation.** Two working UC-1 builds cost weeks
and — as arm 1 already showed — both would succeed, so a second build decides nothing. What is
wanted from arm 2 is the part count and the shape of the glue.

Stack under test, all self-hostable and all surviving the constraints in
[VISION.md](../../VISION.md#constraints):

- **Activepieces** (MIT core) as producer, pipeline, and routing layer.
- **Letta** (Apache-2.0) as persona memory, called over HTTP.
- Human gates from Activepieces itself. Kestra and LangGraph turned out not to be needed —
  see the gate row below — which removes a component the layer table assumed.

| UC-1 step                          | Activepieces assembly                                                                    | Against arm 1                            |
| ---------------------------------- | ------------------------------------------------------------------------------------------ | ----------------------------------------- |
| Mail arrives                       | Gmail piece, `New Email` trigger. Marked **scheduled**, i.e. polling                         | **Worse.** Arm 1 pushes via PubSub; a poll interval is a latency floor on every item |
| Mail reaches a router              | No hop. The trigger starts the flow, and the flow is the router                              | **Better.** Arm 1 needs a hook mapping or a channel-binding detour |
| Classify: invoice, recipe, surf    | An AI step against a platform-configured provider, then a Router step branching on its output | **Better on the property that matters.** Branch conditions are flow JSON under flow version history: diffable, versioned, reviewable. Arm 1's equivalent is a prompt |
| Route to the right persona         | One flow per persona, invoked as a sub-flow or by webhook. No agent registry exists           | **Worse.** Arm 1 has addressable agents; here persona addressing is a naming convention you maintain |
| Persona has its own memory and voice | Voice is prompt text in project variables. Memory is Letta, called per persona flow           | **Worse.** Second system, second datastore, second ops surface. Arm 1 ships workspaces, persona files, and a session store |
| Human gate before anything payable | Approval piece `Wait for Approval`, with the Todos inbox as the review surface                | **Parity on mechanism, better on surface.** A Todos inbox beats a resume token pasted into chat |
| Survives a restart mid-run         | Flows pause mid-execution and resume with execution context persisted across restarts and deploys | **Parity** |
| Mail from a stranger is untrusted  | Nothing equivalent. Steps run with whatever the connection grants; no per-step sandbox or tool clamp | **Much worse.** Arm 1's documented reader shape is the closest thing to CaMeL either arm offers |
| Unroutable pile                    | Router fallback branch writes to a table                                                      | **Parity, and both are a non-answer** — a place, not a loop |

### What arm 2 shows

- **UC-1 is expressible here too.** No mechanism is missing. This is the second independent
  confirmation of the arm 1 conclusion, from a stack with no agent runtime in it at all.
- **Routing-as-data is not a Clackworks invention.** Activepieces gets closer to it for free
  than OpenClaw does, because a branch condition is already flow JSON with version history.
  What it still lacks is the decision *record* — reason attached, replayable against a stored
  item without running the flow. Narrower claim than
  [routing.md](../../_incoming/routing.md) currently makes, and that file should be re-read
  against this.
- **Part count is the real trade.** Arm 2 is three systems plus glue you own forever. Arm 1 is
  one MIT system plus setup cost that is real but one-time: a GCP project, a PubSub topic, a
  public HTTPS endpoint for the push hook, and a sandbox image.
- **Neither arm expresses UC-4.** Both give somewhere to put unroutable items; neither clusters
  them, proposes a pipeline, or re-processes a backlog. [UC-4](../../_incoming/use-cases.md) is
  the only use case in this repo that no surveyed stack expresses.
- **Security is where the arms diverge most.** An untrusted mail body reaching a step with live
  connections is the arm 2 default. See
  [security-and-trust.md](../../_incoming/security-and-trust.md).

### Confidence and staleness

Sketched from vendor docs on 2026-08-15, not from a running instance. Verified: the Gmail piece
and its scheduled `New Email` trigger, the Approval piece and Todos inbox, persisted pause and
resume, platform-level AI provider configuration, MIT core with a commercial `packages/ee`
folder. **Not verified:** whether Activepieces' own agent feature changes the persona and memory
rows, and what the poll interval floor actually is on a self-hosted build. Both would have to be
checked before this sketch decides anything.

## What the test decided

**Conclusion: the assembly is priced, and it does not create a reason to exist either.** Neither
arm could have, because reason-to-exist was never about expressing UC-1 — both arms express it.
It rests on the guarantee list under [What survives arm 1](#what-survives-arm-1), plus UC-4,
which is now the only unexpressed thing found in two arms.

**The decision this forces:** the next build is not UC-1 a second time. It is the guarantee
layer plus UC-4 on top of one arm. Arm 1 is the cheaper host — fewer parts, sandboxing, agent
identity and memory included — even though arm 2 is closer on routing-as-data.

What is still open — build on top or beside, and which arm hosts it — is a Phase 0 question in
[_incoming/README.md](../../_incoming/README.md#phase-0--frame-the-thing), not settled here.

## Open questions

- Arm 2 leftovers: does the Activepieces agent feature change the persona and memory rows, and
  what is the actual poll-interval floor for the Gmail trigger on a self-hosted build?
- Neither arm was built. Does a paper sketch carry enough weight for the build-on-top decision,
  or does one arm have to be stood up first?

Landscape-level open questions stay in [prior-art.md](./prior-art.md#open-questions).
