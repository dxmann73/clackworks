# Activepieces

Status: draft. Checked 2026-08-16, from vendor docs and repository metadata — not from a running
instance.

n8n-alike, with "pieces" as TypeScript modules and a Todos inbox as the approval surface.

|           |                                          |
| --------- | ---------------------------------------- |
| Licence   | MIT core + commercial `packages/ee`      |
| Self-host | yes                                      |
| Paid from | Plus $16 (annual); Team $166 (annual)    |
| Health    | ★24k, pushed 2026-08-15, 463 open issues |
| Verdict   | **Passes the one-product test. Third**   |

## What it gives you

Flows with sub-flows for pipeline-to-pipeline chaining, a Router step for content-based dispatch,
a **Wait for Approval** step plus a **Todos inbox** as the human-gate surface, and loops with
re-entry for cycles. Branch conditions are versioned flow JSON, so routing is data.

**Durability is its distinguishing property.** The documented guarantee is that a completed step's
output is checkpointed and never re-run — stronger than Windmill's at-least-once zombie-job
restart, and the reason it stays on the list at all.

Free build is the full MIT core with unlimited flows and tasks. The `ee` folder is SSO, projects,
RBAC, git sync, audit log, branding and embedding — org-shaped only.

## For

- **Step checkpointing.** The strongest durability claim of the four finalists.
- Todos inbox is a better standing gate surface than a bare resume token.
- Routing conditions are versioned flow JSON.
- MIT core, unlimited flows and tasks, org-shaped enterprise edge.

## Against

- **Producers are weaker.** The Gmail trigger polls; no inbound SMTP, no per-flow address.
- **Persona addressing is a naming convention**, nothing more.
- **No trust story.** Steps run with whatever the connection grants — no per-step sandbox, no
  tool clamp. Direct problem for [security-and-trust.md](../../../_incoming/security-and-trust.md).
- No agent runtime of its own, so memory needs a second system
  [Letta](../prior-art-product-overview.md#agent-memory) in the assembly sketch.
- Ranked third on the one-product test; carried as the fallback host, not the lead.

## Mapping to use cases

