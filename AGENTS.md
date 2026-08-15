# Clackworks — Agent Rules

Project-specific guidance. Global rules in `~/AGENTS.md` apply on top of this; only project
specifics live here.

## Project

- `clackworks` — chain-reaction workflow automation with agents. Concept in
  [VISION.md](./VISION.md).
- **Specification phase.** No implementation, no dependencies, no stack choice, no architecture
  decisions. If a task seems to need one, write the open question into the relevant
  `_incoming/` file instead.
- Stack: **undecided**. Fill in stack-relevant skill references once chosen.
- Repo private by default.

## Conventions

- Specs live in `_incoming/`, one file per area, registered in
  [_incoming/README.md](./_incoming/README.md).
- Every spec file carries a `Status:` line (`draft`, `in review`, `settled`).
- Vocabulary is defined once in [_incoming/glossary.md](./_incoming/glossary.md). Use those
  terms exactly; do not invent synonyms for Producer, Intake, Pipeline, Agent, Artifact,
  Exhaust, Human Gate.
- Open questions belong in the owning spec file under `Open questions`, and cross-cutting ones
  in [_incoming/open-questions.md](./_incoming/open-questions.md).
- Markdown: line length 100, config in `.markdownlint.json`. Use the `markdownlint` skill.
- Fail fast on bad input — no silent defaults. This is both a repo rule and a product
  principle.

## Stack-relevant skills (fill once decided)

- now: `markdownlint`, `brainstorming`, `grill-me`, `gg-commit-push`
- later, by stack: web/TS `tanstack-*`, `shadcn-ui`, `tailwind-design-system`, `no-use-effect`,
  `frontend-design`; JVM `quarkus`; any `verification-before-completion`
