# Use cases

Status: draft

## Purpose

Concrete end-to-end walkthroughs. These are the tests the specification has to pass: if a
design cannot express these, it is wrong.

Second test, from [VISION.md](../VISION.md#how-clackworks-itself-is-licensed): every use case
here must be fully runnable on the community edition. If one of them needs a capability that
lands behind the enterprise edge, the split is wrong, not the use case.

## In scope

- Narrative walkthroughs from producer to terminal item.
- The awkward cases, not only the clean ones.

## UC-1: Mail triage into personas

The mail producer emits an item carrying its provenance, sender, and addressee. The router
decides on that alone: mail addressed to a persona's own address goes straight to that persona;
everything else is classified.

- **Invoice** to the tax advisor. Extracts amounts, dates, vendor; files it; flags anything
  that needs a decision. Terminal item: filed record. Human gate: notify only, approve for
  anything payable.
- **Recipe** to the nutrition coach. Evaluated against current goals, filed if worth keeping,
  discarded with a reason if not.
- **Surf-related** to the surf coach. Conditions, trips, gear.
- **Ambiguous** to the exhaust, with the classifier's best guess attached as a proposal.

Each of those pipelines has an ingress in front of it, which is where an unusual mail is stopped:
a trusted sender who has never sent an office document before gets a human gate before the
pipeline runs.

Awkward parts: a mail that is two things at once and fans out to both; a thread where the
classification changes halfway; a sender who is a stranger asking for something
plausible-sounding.

## UC-2: Software development from a TODO

A repository scan produces a TODO item. Routing sends it to the development pipeline, which has
a capable model and a real sandbox with compute assigned. The developer persona works the task
and produces a branch and a PR — a re-entrant item. A review pipeline consumes the PR and
produces review comments. A human gate sits before merge. Merge is terminal.

Awkward parts: the TODO is stale and the code changed; the task is too big and needs splitting
into new items; the run exceeds its compute budget mid-way.

## UC-3: Thought to blog post

A stray thought in a text file, or a YouTube video worth chewing on, arrives from the notes or
video producer. A pipeline
extracts the substance and checks it against what has already been written. The editor persona
produces a draft — a re-entrant item. A second pipeline turns approved drafts into posts on
the website. Human gate before publishing, always.

Awkward parts: the thought duplicates something written a year ago; the draft is rejected and
needs to go back for revision rather than forward; the video transcript is an hour long.

## UC-4: Working the exhaust

The only use case neither falsification arm expresses — see
[research/README.md](../docs/research/README.md#what-arm-2-shows). Both give somewhere to put
unroutable items; neither clusters them, proposes a pipeline, or re-processes a backlog.

Fourteen unroutable items accumulate. The system clusters them: they are all newsletters from
the same category. The user decides — build a pipeline, or discard the lot deliberately. If a
pipeline is built, the fourteen items are re-processed through routing.

Awkward parts: re-processing stale items that were time-sensitive; a cluster that is actually
two different things.

## UC-5: Someone mails the surf coach directly

Mail arrives addressed to the surf coach's own address. Routing shortcuts on the address. The
persona replies as itself, in its own voice, from its own address. Directive decides whether
the reply goes out automatically or waits for approval.

Awkward parts: the mail is not about surfing; the sender does not know they are mailing an
agent; the reply commits to something.

## Open questions

- Which use case is built first, and what is the minimum machine that supports it end to end?
  UC-1 is expressible on both surveyed arms already, so building it first proves nothing; UC-4
  is the one nothing expresses.
- Are there use cases here that any sane design should refuse, rather than support?

## Not in scope

- Pipeline definitions. These are narratives, not specs.
