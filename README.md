# Clackworks

Workflow automation as a chain-reaction machine. Things come in, get routed, roll through
pipelines — deterministic or agent-driven — and produce something that can feed the next
pipeline. Whatever cannot be routed lands in the exhaust and gets a decision.

Named after the Rube Goldberg machine: many small mechanisms, each dumb on its own, chained
into something that does real work.

## Status

**Specification only.** Nothing is implemented. No stack, no architecture, no framework has
been chosen — and choosing one is explicitly out of scope until the specs settle.

## Layout

| Path         | Contents                                                            |
| ------------ | ------------------------------------------------------------------- |
| `VISION.md`  | What Clackworks is, why it exists, what "done" looks like            |
| `_incoming/` | One markdown file per area that needs specifying, plus an index      |
| `AGENTS.md`  | Rules for agents working in this repo (`CLAUDE.md` symlinks to it)   |

## Where to start

1. [VISION.md](./VISION.md) — the whole idea in one read.
2. [_incoming/README.md](./_incoming/README.md) — index of areas, with reading order and status.
3. [_incoming/glossary.md](./_incoming/glossary.md) — the vocabulary everything else assumes.

## How to work on this

Specs are drafts. Fill them in, argue with them, split them when they get too big. Add a new
file to `_incoming/` when an area shows up that no existing file owns, and register it in the
index. Move a file out of `_incoming/` once its area is genuinely settled.

Do not add implementation, dependencies, or architecture decisions to this repo yet.
