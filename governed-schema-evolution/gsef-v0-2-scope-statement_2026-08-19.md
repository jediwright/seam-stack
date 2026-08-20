# GSEF v0.2 Scope Statement

⚑ STAMP: SINGLE-CONTEXT — NOT PANELED

**Artifact class:** GSEF scope statement, v0.1 → v0.2 revision (canonicalizes the
Q-A verdict, the T-2 temporal-crossing check definition, and the M5 admissibility
naming).
**Date:** 2026-08-19
**Session:** GSEF v0.2 Scope Statement Session (Session Harness v0.2, Mode 1,
CONTEXTUAL)
**Scope authority:** `gsef-qa-resolution_2026-08-18.md` §5.1, §5.2 (revision
target and open-item sources); SL-0124 (Q-A verdict); SL-0125 (Q-D evidence unit).
**Status:** PROPOSED-CANONICAL pending operator apply. All coinages ⚑ PROPOSED
pending lexicon queue.
**Supersedes:** the v0.1 scope text in `session-handoff-gsef-spec-v0-1_2026-08-07.md`
(queue-don't-reopen: the v0.1 document is not modified; this statement supersedes
its scope text by issuance).

---

## 1. Scope statement (normative)

**GSEF (Governed Schema Evolution Framework) is a cross-layer governance
framework** specifying schema-evolution governance requirements at each of the
four Seam Stack layers — **Substrate, Governance, Boundary, and Evidence** —
with layer-appropriate parameterization of a single mechanism set.

What GSEF specifies at each layer:

| Layer | GSEF specification (parameterized) |
|---|---|
| **Substrate** | Lineage persistence: append-only recording of every schema change and its translation path at introduction (M4); locatability of translation artifacts (T-2 fact F1). |
| **Governance** | Change authorization: every schema change classified by blast radius and change driver (M1), authorized under the evidence path for its class (M2); emergency change path (M6); policy commitment to maintain governance until declared horizons (M3/F6). |
| **Boundary** | Crossing-time declaration: bundles commit to a schema version at crossing (M3/F3, the schema version declaration block); validation against the declared version as a precondition of governed read (M5, governed-read admissibility). |
| **Evidence** | Horizon records and deferred-party verifiability: declared deprecation horizons bounding the readability commitment (M3/F5, F8); lineage-traceable translation paths recorded as of crossing date (F4); supersession-not-reinterpretation of recorded steps (M7). |

**No single layer's governance is sufficient.** The deferred-party readability
gap that GSEF addresses — a cryptographically verifiable bundle that is not
schema-readable by a future party — arises precisely because no one layer owns
the temporal-crossing check (§2); the check composes from facts each layer
records.

**Coverage claim (precise):** the current eight mechanisms (M1–M8, GSEF v0.1
§3.1) provide full coverage at the **Substrate and Governance** layers. At the
**Boundary and Evidence** layers, the mechanism set requires extension before
full four-layer coverage:

- **M6 (emergency change path) does not apply at Boundary or Evidence in
  current form.** No "emergency crossing" concept exists in the pattern
  architecture. The framework requires either a new mechanism for
  out-of-governance crossing events, or an explicit scope restriction of M6 to
  Substrate/Governance with this stated as a named limit. (GSEF-OI-1 — open;
  gates FRAMEWORK.md.)
- **M8 (pre-cleared channel) carries parameterization strain at Boundary and
  Evidence.** A "pre-cleared crossing type" (Boundary) and a "pre-cleared
  evidence template" (Evidence) need mechanism design before M8 is complete at
  those layers. (GSEF-OI-2 — open; gates FRAMEWORK.md.)

These are named limits of v0.2, not evidence against the framework
classification (SL-0124).

## 2. Temporal-crossing check — canonical definition (M3 mechanism annotation)

*Canonicalizes the T-2 horizon-bounded restatement (closes GSEF-OI-5). This
annotation is inherited by FRAMEWORK.md when drafted.*

**Temporal-crossing check.** For a bundle produced under declared schema
version V and crossed at time t, the temporal-crossing check holds iff the full
translation path from V to the schema version of **any future reader within V's
declared deprecation horizon** is:

- **(a) governed** — every step on the path was authorized under the evidence
  path for its blast-radius class (M1 + M2);
- **(b) horizoned** — the horizon in force for V, as recorded at crossing time,
  had not expired at t (M3);
- **(c) lineage-traceable** — the path composes from append-only lineage
  entries recorded at each step's introduction (M4 + M7), resolvable from the
  crossing record by content address.

**Quantifier scope.** "Any future reader" is horizon-bounded by construction:
it means any reader whose read falls within the declared deprecation horizon.
The check is a **composition of per-layer atomic facts** (F1–F8, Q-A T-2
decomposition), each recordable at act time — not an emergent property.

**Beyond the horizon.** Reads beyond the declared horizon are **ungoverned
reads** — a named limit of the architecture's readability commitment, not a
failure state of the check and not a prohibition. The architecture bounds what
it commits to; it does not claim readability it cannot govern.

**Worked example (practice evidence):** the Q-D acceptance run (SL-0125).
AF-1/AF-2/AF-3 of the schema version declaration block operationalize (a)–(c)
at the Boundary/Evidence layers. A deferred party verified governed-read status
from the crossing record plus two artifacts (`LINEAGE.md`, `HORIZONS.md`) with
no third information source; three deliberate breaks produced three distinct,
correctly-named failures (`undeclared-version` / `lineage-unresolvable` /
`horizon-expired`); a supplementary run on a v0.4-declared bundle demonstrated
translation-path **composition** (rename-map → identity-additive) — the "full
path" clause exercised in practice.

*Not defined here:* where the check runs (bundle-embedded vs.
artifact-referenced lens/lineage location) — Q-B, open, dedicated session.

## 3. Admissibility — two senses, named (M5 annotation)

*Names the M5 Evidence-layer kind-shift found by T-1 (closes GSEF-OI-3). Both
coinages ⚑ PROPOSED pending lexicon queue.*

- **Governed-read admissibility** *(operational sense)*: the read is permitted
  as governed. Established by AF-1 (version declared), AF-2 (lineage bound),
  AF-3 (horizon unexpired) from the crossing record and lineage/horizon
  artifacts alone. Fail-closed: any AF failure ⇒ the read is ungoverned, in a
  distinct named state.
- **Forum admissibility** *(Evidence-layer sense)*: the record qualifies as
  evidence in a legal, regulatory, or adjudicative forum — a different standard
  under different authorities (authentication, chain-of-custody, hearsay
  treatment), none of which AF-1..3 address.

**Normative scope language:** M5 couples schema validation to **governed-read
admissibility at all four layers**. **Forum admissibility is a named limit at
the Evidence layer:** GSEF's artifacts are designed to be *usable toward* forum
admissibility (append-only, content-addressed, tamper-evident, distinct failure
states), but the framework makes **no claim** that they satisfy any forum's
evidentiary standard. GSEF prose must not use "admissible" without one of the
two senses attached.

## 4. Layer-relative blast radius (Q-C disposition — RESOLVED)

The blast-radius taxonomy (M1) is applied **once per schema change event**,
producing a single class assignment (highest component class governs, with
component breakdown where mixed). Each layer's GSEF parameterization then names
the **layer-relative consequences** of that class: one taxonomy application,
four consequence sets. The taxonomy is not re-applied per layer.

*First worked instance (illustration, not validation):* `LINEAGE.md` L-4 (D),
L-5 (D), L-6 (B) — one class per event, component breakdowns recorded.
~ single-context; classifications sit in the Counter-Pass lane if the lineage
record enters publication track.

**Named reopening condition:** Q-C reopens if a future change event surfaces
where the class assignment itself — not merely its consequences — is genuinely
layer-relative (e.g., Class B at Substrate but Class D at Boundary because the
crossing-record shape breaks while the storage shape does not). No current
evidence suggests this; recorded as trigger, not live doubt.

## 5. Evidence base (first practice-extracted unit)

The framework's first real-world evidence unit is the **employment-seam
artifact set** (Q-D, SL-0125, G-B — practice-extracted, not manufactured):

| Artifact | Mechanisms exercised |
|---|---|
| `LINEAGE.md` | M4 (append-only lineage), M1 (blast-radius + driver per change), M3 (horizon pointers), M7 (composing, never-edited translation steps) |
| Schema version declaration block v0.1 | M3 (crossing-time declaration), M5 (governed-read admissibility coupling, fail-closed AF-1..3) |
| `HORIZONS.md` | M3 (two-state deprecation record; policy-confirmed-only semantics; past-horizon = ungoverned read) |

Evidence strength, stated at its actual level: acceptance MET in-session
(✓, outputs verbatim in the Q-D session record); mechanisms are **not
prototype-verified at any layer beyond this unit**; blast-radius class
assignments ~ single-context; the horizon *value* ? pending operator
ratification. The unit demonstrates M1/M3/M4/M5/M7 in composition at the
Boundary/Evidence surface of one vocabulary. It does not demonstrate M2, M6, or
M8, and it supports **no generality claim** beyond the employment-seam
vocabulary — the ceiling posture applies to this scope statement reflexively.

## 6. Out of v0.2 scope (named)

Q-B (lens/lineage location); Q-E (lens-governance vs. TCF 14-day protocol);
Q1–Q7 (gate FRAMEWORK.md); G-D collision check (**still gates all public
novelty claims** — this scope statement makes none); M6/M8 mechanism design
(gaps named in §1 only); the SPECULATIVE superset fields fenced in
declaration-block §6 (none implemented, none claimed); FRAMEWORK.md drafting
(Tier 4 — gated on Q1–Q7, G-D, and the G-B unit, which now exists).

---

*GSEF v0.2 Scope Statement — UX Minds, LLC · J. Wright · 2026-08-19.
SINGLE-CONTEXT — NOT PANELED. PROPOSED-CANONICAL pending operator apply.
Delivery-not-application enforced.*
