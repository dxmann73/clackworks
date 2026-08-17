# UC-1: Mail triage into personas

Status: draft

## Walkthrough

The mail producer emits an item carrying its provenance, sender, and addressee. The router decides
on that alone: mail addressed to a persona's own address goes straight to that persona; everything
else is classified.

- **Invoice** to the tax advisor. Extracts amounts, dates, vendor; files it; flags anything that
  needs a decision. Terminal item: filed record. Human gate: notify only, approve for anything
  payable.
- **Recipe** to the nutrition coach. Evaluated against current goals, filed if worth keeping,
  discarded with a reason if not.
- **Surf-related** to the surf coach. Conditions, trips, gear.
- **Ambiguous** to the exhaust, with the classifier's best guess attached as a proposal.

Each of those pipelines has an ingress in front of it, which is where an unusual mail is stopped: a
trusted sender who has never sent an office document before gets a human gate before the pipeline
runs.

## Awkward parts

- A mail that is two things at once needs to be fanned out more than one pipeline.
- A sender asking for something plausible-sounding but dangerous.
