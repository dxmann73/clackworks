# Ingress

Status: draft

There is no single front door and no stage between producer and router. Arrival
and provenance belong to the [producer](./producers.md); getting an item to a destination belongs
to [routing.md](./routing.md); an ingress sits in front of a pipeline and decides, per pipeline,
whether and in what shape an item goes in.

## Purpose

Guard and prepare the entrance to one pipeline. Filter what should not go in, enrich what should,
and raise a human gate when something about the item looks off.

## In scope

- Filtering: what an ingress refuses on behalf of its pipeline.
- Enrichment: what it adds before the pipeline sees the item.
- Gates raised at the entrance rather than inside the pipeline.
- Trust and authenticity checks that belong to the entrance.

## Requirements

- An ingress belongs to a pipeline, not to a producer. Several pipelines fed by the same producer
  can guard their entrances differently, and one item may pass more than one ingress on its way
  through chained pipelines.
- An ingress may refuse an item. A refusal is recorded with its reason — never a silent drop.
  Whether the item goes back to routing or straight to the exhaust is open below.
- An ingress may raise a human gate, and this is its most characteristic job: judgement about the
  item *in context*. A mail from a trusted sender that carries an office document, when that
  sender has never sent documents before, is flagged for a human before the pipeline runs rather
  than after the pipeline has acted on it.
- Enrichment is additive and never destroys the original. Whatever the ingress adds — a fetched
  transcript, a reconstructed thread, an extracted attachment, a trust score — sits alongside the
  item as it arrived.
- Authenticity and trust checks happen here where they gate a specific pipeline: is this sender
  who they claim to be, is this source trusted enough for what this pipeline is allowed to do.
  See [security-and-trust.md](./security-and-trust.md).
- Fail fast: an item an ingress cannot evaluate is refused loudly with the reason. No partial
  items with empty placeholder fields going in.
- An ingress does no work on behalf of the pipeline. It admits, enriches, or stops. Taking the
  item apart is the pipeline's job.

## Prior-art evidence

Checked 2026-08-15 against OpenClaw and the Activepieces arm; see
[prior-art.md](../docs/research/prior-art.md). Evidence, not decisions.

- **A per-producer entrance exists in shape.** OpenClaw's Gmail path is push: Gmail API `watch`
  publishing to a GCP PubSub topic, delivered to a Gateway hook whose mapping matches on `path`.
  The mapping templates a per-message session key from the payload —
  `hook:gmail:{{messages[0].id}}` — which makes the provider's message id the de facto item
  identity. That is producer-shaped, not ingress-shaped: it is about arrival, not about guarding
  a pipeline entrance.
- **Dedup is expressible at the door.** `POST /hooks/agent` takes an `idempotencyKey` and an
  explicit `sessionKey`; the latter requires `hooks.allowRequestSessionKey: true` and a non-empty
  `hooks.allowedSessionKeyPrefixes` allowlist. Belongs with the producer question of who owns
  identity and dedup.
- **No common item shape across producers.** Each channel and each hook carries its own payload.
  Normalization is the addition, and it is the requirement no surveyed system meets. With no
  stage between producer and router, it has no owner — the question sits with
  [producers.md](./producers.md).
- **Admission control for untrusted senders has a documented precedent, and it is ingress-shaped.**
  OpenClaw's recommended Gmail reader runs a dedicated agent with `sandbox.mode: all`,
  `scope: session`, `workspaceAccess: none`, a minimal tool profile, an explicit untrusted-data
  message template, and `deliver: false`. That is exactly the "guard the entrance to this
  pipeline" pattern. Worth copying wholesale into
  [security-and-trust.md](./security-and-trust.md); the strongest such shape found in either arm.
- **Neither arm raises a gate at the entrance.** Lobster's approval checkpoints sit at steps
  inside a pipeline. Nothing surveyed holds an item *before* the run starts because something
  about it looks unusual in context.

## Open questions

- What does an ingress match on to decide "unusual" — a stored history per sender or channel, a
  classifier, an explicit rule set? The trusted-sender-with-an-attachment example needs a memory
  of what that sender normally does, and nothing owns that yet.
- Is the ingress a stage the router hands to, or the first declared part of the pipeline itself?
  The distinction matters for what a trace shows and for whether it can be reused across
  pipelines.
- Can an ingress be shared by several pipelines, or is it always one-to-one?
- Does refusal at an ingress mean unroutable (back to routing, try elsewhere) or terminal (into
  the exhaust)? Those are different failure modes.
- How much enrichment happens here — transcript fetching, thread reconstruction, attachment
  extraction — versus inside the pipeline?
- Rate limiting: what happens when a producer floods the pipelines behind it, and is that an
  ingress concern or a producer one?

## Not in scope

- Choosing a destination. That is [routing.md](./routing.md).
- Arrival, provenance, and idempotent emission. That is [producers.md](./producers.md).
- Doing work on the item. An ingress never transforms content beyond enrichment.
