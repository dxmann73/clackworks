# Artifacts and chaining

Status: draft

## Purpose

Pipelines produce something. That something either ends the chain or starts the next one. This
defines what an artifact is and how the loop back into intake works without becoming chaos.

## In scope

- What an artifact is and what it carries.
- Terminal versus re-entrant artifacts.
- Loop prevention and lineage.

## Requirements

- Every run produces an artifact, even when the artifact is "nothing was produced, for this
  reason". A silent run is a bug.
- An artifact carries lineage: which item it came from, which pipeline and persona made it,
  which model, when, and which human touched it.
- An artifact is either terminal (published post, sent mail, merged PR, filed invoice) or
  re-entrant (goes back to intake as an item), and that is declared, not inferred.
- Re-entrant artifacts go through intake and routing like anything else. No back doors between
  pipelines.
- Lineage survives chaining. From a published blog post you can trace back to the stray thought
  in a text file that started it.
- Cycles must be detectable and stoppable. A chain that feeds itself needs a limit and a
  visible reason when it hits it.
- An artifact that no pipeline consumes and that is not terminal is a defect, and it should
  surface — probably in the exhaust.

## Open questions

- Are artifacts stored by the system, or only referenced where they live (in a repo, on the
  website, in a mailbox)?
- Versioning: a draft revised three times — one artifact with versions, or three artifacts?
- Do artifacts have a lifecycle (draft, approved, published, superseded), or is that
  pipeline-specific?
- What is the cycle-detection mechanism — hop budget, lineage inspection, declared chains?
- Can an artifact have several consumers, and what if they conflict?
- Are failed runs' partial outputs artifacts, or discarded?

## Not in scope

- Storage format and location. Architecture, deferred.
