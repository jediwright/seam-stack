# PC#8 — The Substrate-Crossing Seam

**Pattern Commons #8 · Active Build**
*Governed data crossing from local-first into AT Protocol (Bluesky)*

---

## The Problem in Plain Language

Local-first architecture gives you a strong guarantee: the data lives on your device, under your control, governed by rules you set. That guarantee holds as long as the data stays in a local-first system.

The moment you publish something — to Bluesky, to a shared platform, to anywhere outside your local substrate — the guarantee changes. Once a record enters the AT Protocol relay network and is distributed to subscribers, you cannot recall it. Deletion is a broadcast request, not an enforcement. Third-party subscribers may retain the record regardless.

Most systems handle this by ignoring it, or by vaguely gesturing at terms of service. The substrate-crossing seam is an attempt to be honest about it architecturally: to design the moment of crossing so that it is explicit, consented-to, and recorded — and so that the record truthfully declares what the architecture can and cannot guarantee after the crossing fires.

---

## What Makes This Problem Hard

When the employment seam (PC#7) fires, it crosses within a local-first ecosystem. The epistemic properties of the record stay consistent across the boundary.

The substrate-crossing seam crosses into a different kind of system entirely. Three things change at once:

**Revocation semantics change.** On the local-first side, revoking a capability closes a stream — that is an enforcement. On the AT Protocol side, deletion is propagated as a request. Mirrors are expected to comply, but there is no architectural guarantee they will. Post-crossing revocation is a social and platform mechanism, not a cryptographic one.

**Exposure bounds can no longer be asserted.** The existing crossing-record vocabulary included a value called `exposure-upper-bound` — it records the maximum authorized exposure for a record. After a substrate crossing into a globally indexed, relay-distributed system, there is no honest upper bound to assert. The record has entered a system where the exposure is, in principle, unlimited and unrecallable.

**The grant must carry the regime change.** The person authorizing the crossing needs to understand — at the moment of authorization, before the crossing fires — that they are moving from a governed local-first system into a different epistemic regime. That understanding must be part of the crossing record, not a click-through disclaimer somewhere else.

---

## What Was Built

PC#8 added three architectural extensions to the crossing-record base shape that did not exist in PC#7:

| Extension | What it does |
|---|---|
| `exposure-unbounded` boundType | Honest vocabulary for a crossing into a globally indexed system — declares that no exposure bound can be asserted |
| `crossingType` conditional grant group | Allows the crossing record to declare the type of substrate being crossed into, with regime-appropriate grant conditions |
| Two-record intent/completion pattern | Separates the crossing intent (recorded before the crossing fires) from the crossing completion (recorded after) — the "write before fire" constraint |

The "write before fire" constraint is the core architectural position: governance provenance is embedded in the crossing record itself, pre-crossing, with no finality arbiter required. This is not a common pattern in the visible landscape of AT Protocol tooling.

---

## The Build Arc

**Phase 0 — Baseline establishment.** The first phase ran against a live Bluesky PDS to establish what the infrastructure actually does versus what the documentation says it does. Several findings:

- The `wantedCollections` filter for `com.whtwnd.blog.entry` (WhiteWind blog entries) was non-functional — the firehose returned all record types regardless of filter. A working pattern was adopted instead of the documented one.
- `jetstream1.us-east` was pinned over `jetstream2` due to measurable delivery variance between the two endpoints.
- First KL-1 baseline latency numbers captured with high spread — the infrastructure behaves differently than a controlled local environment, and the spec needed to account for that honestly.
- A credential exposure incident occurred during Phase 0 and was fully remediated via history rewrite (`git filter-repo`). The incident is documented in the build record. Hiding it would have been worse.

**Phase 1 — Crossing-intent record.** Built the crossing-intent record (the first half of the two-record pattern), including the new vocabulary extensions. All acceptance criteria met.

**Phase 2 — Crossing-completion verification.** Built and verified the crossing-completion record. KL-1 (latency) and KL-2 (delivery confirmation) converted from known limits to closing evidence at joint gate. 27/27 tests passing at close.

---

## The Credential Exposure Incident

During Phase 0, credentials were inadvertently committed to the repository. This is documented here because it is an example of what the governance methodology is actually for.

The incident was caught, escalated through the session harness as a blocking finding (F-4), and remediated via `git filter-repo` history rewrite. The repository HEAD advanced to `24ae732` post-remediation. Three AT-URIs from the affected runs are held pending confirmation that closing-evidence artifacts are ledger-stamped before any cleanup.

A build process that has no mechanism for catching and surfacing incidents like this is a build process that buries them. The governance layer caught it. The documentation records it.

---

## What the Two-Substrate Evidence Base Established

After PC#7 (local-first, Automerge + Keyhive) and PC#8 (local-first → AT Protocol), the architecture has been demonstrated on two named substrates with different epistemic properties.

Three architectural extensions were required by the second substrate that did not exist in the first:
- `exposure-unbounded` boundType
- `crossingType` conditional grant group
- Two-record intent/completion pattern

Each of those extensions arose from substrate properties the employment-seam domain does not share. This is what a second substrate crossing is supposed to produce: not just confirmation, but evidence of what the architecture actually requires to generalize — and what remains design intent rather than demonstrated fact.

The scope claim advanced from "local-first specific on current evidence" to "demonstrated on two named substrates; additional substrates remain design intent."

---

## What It Did Not Demonstrate

- Post-crossing recall is not architecturally enforceable. The crossing record can declare that a deletion request was issued; it cannot guarantee compliance.
- The `exposure-unbounded` declaration is honest about what the architecture cannot claim. It is not a solution to the exposure problem; it is a named acknowledgment of it.
- Additional substrates (dialog-db, other federated systems) remain design intent. The NI-5 generality discipline holds: substrate-agnosticism is a design goal, not a current scope claim.

---

## Relevant Artifacts

- `jediwright/employment-seam` — sub-package containing PC#8 implementation (`local-first-series/pattern-commons/`)
- `pattern-commons-08-substrate-crossing-seam-v0-1-4.md` — canonical spec (v0.1.4, post Phase 2)
- `pc08-kl1-kl2-closing-evidence_2026-08-18.md` — Phase 2 closing evidence artifact
- Observation log (Phase 0 run record) — baseline findings including F-1 through F-4

---

*Part of the [Pattern Commons series](./index.md). See also [PC#7](./pc7-employment-seam.md) and [The Governance Methodology](./methodology.md).*
