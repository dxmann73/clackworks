# UC-2: Software development from a TODO

Status: draft

## Walkthrough

A repository scan produces a TODO item. Routing sends it to the development pipeline, which has a
capable model and a real sandbox with compute assigned. The developer persona works the task and
produces a branch and a PR: a re-entrant item. A review pipeline consumes the PR and produces review
comments. A human gate sits before merge. Merge is terminal.

## Awkward parts

- The TODO is stale and the code changed.
- The task is too big and needs splitting into new items.
- The run exceeds its compute budget mid-way.
