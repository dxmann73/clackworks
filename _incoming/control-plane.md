# Control plane

Status: draft

## Purpose

Where producers, routes, pipelines, personas, directives, and budgets are defined and changed —
and how the person operating the machine actually works with it day to day.

## In scope

- What is configurable and where that configuration lives.
- Changing the system safely while it is running.
- The daily operating surface: exhaust, gates, traces.

## Requirements

- Everything that shapes behaviour is configuration, not buried logic: producers, routing
  rules, pipeline registration, persona definitions, directives, model assignments, budgets.
- Configuration is versioned and diffable. What changed, when, and why is answerable.
- Changes take effect predictably. What happens to in-flight runs when their pipeline or
  directive changes is defined, not incidental.
- Dry run: a routing change can be evaluated against stored items before it goes live.
- The daily operating surface covers three things — the exhaust queue, runs waiting on a human,
  and traces when something looks wrong. These are the screens that matter.
- Creating a pipeline should be cheap enough that the answer to an exhaust cluster is usually
  "build one".
- Fail fast on bad configuration: an invalid route, a pipeline with no model where one is
  required, a persona with no channel. Reject at change time, not at run time.

## Open questions

- Is configuration files-in-a-repo, a database with an interface, or both?
- Is there a UI, and if so how much of one? Terminal, web, mail-driven?
- Can the system be operated entirely from mail or chat, given that agents already have
  channels?
- Who may change configuration — the user only, or agents with approval?
- Environments: is there a place to try a pipeline before it touches real mail and real
  accounts?
- Bootstrapping: what is the smallest useful configuration that makes the machine run at all?

## Not in scope

- Deployment, hosting, and operations. Architecture, deferred.
