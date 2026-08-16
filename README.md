# Clackworks

Workflow automation as a chain-reaction machine. Things come in, get routed, roll through
pipelines — deterministic or agent-driven — and produce something that can feed the next
pipeline. Whatever cannot be routed lands in the exhaust and gets a decision.

Named after the Rube Goldberg machine: many small mechanisms, chained into something
that does real work.

Full product vision in [VISION.md](./VISION.md).

## Layout

| Path                | Contents                                                          |
| ------------------- | ----------------------------------------------------------------- |
| `VISION.md`         | What Clackworks is, why it exists, what "done" looks like          |
| `glossary.md`       | The vocabulary every spec uses exactly                             |
| `open-questions.md` | Cross-cutting unknowns and decisions deliberately deferred         |
| `_incoming/`        | One markdown file per area that needs specifying, plus an index    |
| `AGENTS.md`         | Rules for agents working in this repo (`CLAUDE.md` symlinks to it) |

## Where to start

1. [VISION.md](./VISION.md) — the whole idea in one read.
2. [glossary.md](./glossary.md) — the vocabulary everything else assumes.
3. [_incoming/README.md](./_incoming/README.md) — index of areas, with reading order and status.

## How to work on this

Specs are drafts. Fill them in, argue with them, split them when they get too big. Add a new
file to `_incoming/` when an area shows up that no existing file owns, and register it in the
index. Move a file out of `_incoming/` once its area is genuinely settled.

Do not add implementation, dependencies, or architecture decisions to this repo yet.
