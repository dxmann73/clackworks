# UC-5: Project request to the career advisor

Status: draft

## Walkthrough

An agency mails a project request. Routing sends it to the career advisor persona, which holds the
current stance on what work is wanted. The pipeline scores the request against that stance: hard
criteria first (freelance, rate floor, availability window), then fit as judgement. The persona
replies as itself, from its own address: interested with the current CV resolved at send time, wrong
kind of work, not available. Human gate before any reply that
commits.

## Awkward parts

- The same project arrives from three agencies and the replies must not contradict each other.
- The request states no rate, so the pipeline must ask rather than reject.
- Current stance on projects and what is acceptable needs to live somewhere - with the agent?
