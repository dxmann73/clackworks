# Memory

Status: draft

## Purpose

What agents retain across sessions, who can see it, and how it stays accurate rather than
accumulating into noise.

## In scope

- Memory scopes: per persona, per pipeline, shared, per correspondent.
- Writing, reading, forgetting, correcting.
- Privacy boundaries between personas.

## Requirements

- Memory is distinct from the working context of a session. Context dies with the session;
  memory outlives it.
- Memory is scoped, and the scope is explicit. The surf coach remembering a conversation is not
  the same as the tax advisor being able to read it.
- Memory must be inspectable by the user — you can see what a persona believes it knows.
- Memory must be correctable and deletable by the user. A wrong memory is worse than none.
- Provenance on memory: an entry records where it came from — which run, which item, which
  human statement.
- Fail fast: a memory entry that cannot be trusted or attributed is not written.
- Memory must not silently grow unbounded. There is a policy for what is kept, summarized, or
  dropped, and it is visible.

## Open questions

- What kinds of memory do we actually need — facts about the user, history of interactions with
  a correspondent, decisions and their rationale, learned preferences, or all of these as
  distinct stores?
- Is there shared memory across personas, and what governs access to it?
- Do memories expire, and does a stale memory decay or just get flagged?
- How is a contradiction handled — the user said X in March and not-X now?
- Can an agent write memory about the user without the user seeing it happen?
- Does the routing classifier have memory, or is it stateless?
- What is the retention policy for content from personal channels (WhatsApp, mail from third
  parties who never consented)?

## Not in scope

- Storage technology, embeddings, retrieval mechanics. Architecture, deferred.
