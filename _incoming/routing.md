# Routing

Status: draft

## Purpose

Decide which pipeline an item goes to, or declare it unroutable. This is the judgement layer
and the place where most of the system's value sits.

## In scope

- How a routing decision is made and expressed.
- Deterministic rules versus classifier-driven routing.
- Ambiguity, multiple matches, no match.

## Requirements

- A routing decision names a target pipeline, or unroutable. Nothing else.
- Both rule-based routing (sender matches, label matches, repository matches) and
  judgement-based routing (a classifier or agent decides "this is an invoice") must be
  supported.
- Rules are inspectable and changeable without touching pipelines or producers.
- Every decision is recorded with its reason — which rule fired, or what the classifier
  concluded and how confidently.
- No match means unroutable means exhaust. It never means a default pipeline that silently
  swallows things.
- Low confidence is a first-class outcome, distinct from no match, and can be configured to
  trigger a human gate rather than a guess.
- Routing must be testable: given a stored item, show which pipeline it would go to now,
  without running anything.

## Open questions

- Can one item route to several pipelines at once (invoice that is also a surf trip receipt)?
  If so, how do the resulting artifacts relate?
- Precedence when several rules match: first match, most specific, explicit priority?
- Who or what does judgement-based routing — a dedicated classifier, a router agent with a
  persona, or the pipelines bidding for items?
- Is routing one hop or can a pipeline re-route an item it decides it should not handle?
- Does routing learn from human corrections in the exhaust, and if so, how is that not a silent
  behaviour change?
- Do personas route (mail addressed to the surf coach goes to the surf coach) as a shortcut
  around classification?

## Not in scope

- The pipelines themselves.
- Authenticity and trust checks — those happen at intake.
