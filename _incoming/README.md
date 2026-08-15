# _incoming — specification areas

One file per area that needs specifying. Everything here is a draft until marked otherwise.
No architecture, no stack, no implementation — descriptions of what must exist and what it must
do.

## Reading order

Read the glossary first; every other file assumes its vocabulary.

| #  | Area                                                       | Status | Covers                                            |
| -- | ---------------------------------------------------------- | ------ | ------------------------------------------------- |
| 0  | [glossary.md](./glossary.md)                               | draft  | Shared vocabulary                                 |
| 1  | [producers.md](./producers.md)                             | draft  | Sources that emit items                           |
| 2  | [triggers-and-scheduling.md](./triggers-and-scheduling.md) | draft  | How items get pulled or pushed, when              |
| 3  | [intake.md](./intake.md)                                   | draft  | The single front door: normalize, admit, classify |
| 4  | [routing.md](./routing.md)                                 | draft  | Deciding which pipeline gets an item              |
| 5  | [pipelines.md](./pipelines.md)                             | draft  | Units of work, deterministic or agent-driven      |
| 6  | [agents.md](./agents.md)                                   | draft  | Agents as workers: goals, tools, behaviour        |
| 7  | [personas-and-identity.md](./personas-and-identity.md)     | draft  | Names, personalities, mail and social accounts    |
| 8  | [memory.md](./memory.md)                                   | draft  | What agents remember and for how long             |
| 9  | [models-and-resources.md](./models-and-resources.md)       | draft  | Model assignment, compute, budgets, limits        |
| 10 | [artifacts-and-chaining.md](./artifacts-and-chaining.md)   | draft  | Outputs, and outputs becoming inputs              |
| 11 | [exhaust.md](./exhaust.md)                                 | draft  | The unroutable pile and its decision loop         |
| 12 | [human-in-the-loop.md](./human-in-the-loop.md)             | draft  | Gates, directives, notification, override         |
| 13 | [security-and-trust.md](./security-and-trust.md)           | draft  | Authenticity, secrets, blast radius, injection    |
| 14 | [observability.md](./observability.md)                     | draft  | Tracing an item end-to-end, health, audit         |
| 15 | [control-plane.md](./control-plane.md)                     | draft  | How the system is configured and changed          |
| 16 | [use-cases.md](./use-cases.md)                             | draft  | Concrete end-to-end walkthroughs                  |
| 17 | [prior-art.md](./prior-art.md)                             | draft  | What exists already, and what is unclaimed        |
| 18 | [open-questions.md](./open-questions.md)                   | draft  | Cross-cutting unknowns, decisions deferred        |

## File template

Every area file follows the same shape:

```markdown
# <Area>

Status: draft

## Purpose
## In scope
## Requirements
## Open questions
## Not in scope
```

## Adding an area

Create the file, follow the template, add a row to the table above. Split a file when it starts
covering two things that could be argued about independently.
