# The Governance Methodology

**How AI-assisted builds stay defensible**

---

## The Core Problem This Solves

AI-assisted development is fast. It is also prone to a specific failure mode: the output looks right, passes surface-level review, and contains errors — in logic, in claims, in architecture — that only surface later when someone tries to build on it.

The governance methodology described here is a working solution to that failure mode, extracted from real build sessions over several months. It is not a theoretical framework. Every element of it was built to solve a specific problem that actually occurred.

---

## The Three-Layer Stack

The methodology operates at three levels:

**1. Session governance** — how individual working sessions are opened, run, and closed

**2. Claim governance** — how outputs are validated before they become canonical

**3. Project governance** — how the accumulated body of work stays internally consistent over time

Each layer has its own instruments. They compose.

---

## Session Governance — The Session Harness

Every substantive working session opens with a load attestation: which governing documents are in scope, what the ledger tail is (confirming continuity with prior sessions), what register the session is operating in, and what stamps apply to the output.

This is not ceremony. It solves a real problem: AI models have no memory between sessions. Without an explicit load-in at the start of each session, earlier decisions silently become unavailable — and the session produces output that contradicts prior work without knowing it. The load attestation is the mechanism that prevents that.

Sessions run in one of three modes:

| Mode | What it does |
|---|---|
| **Mode 1 — Design/Implementation** | Produces artifacts, code, or spec text |
| **Mode 2 — Counter-Pass** | Adversarial challenge of Mode 1 outputs |
| **Mode 3 — Critic** | Editorial and structural review |

Mode 2 runs in a fresh, isolated context — no project memory, no prior session artifacts. This is deliberate. A counter-pass that has access to the rationale behind a claim is not an adversarial check; it is a confirmation. The isolation is what makes the adversarial pressure real.

At close, every session produces: a compression (what happened, what changed, what is now canonical), a handoff document (what the next session needs to load), and ledger entries for any significant decisions.

---

## Claim Governance — The Survival Ledger and Confidence Tags

Every significant claim in the work carries an explicit confidence level:

| Tag | Meaning |
|---|---|
| `✓` | Counter-Pass confirmed, cross-context validated |
| `~` | Single-context, not yet adversarially tested |
| `PROVISIONAL` | May not enter publication-track copy |

The Survival Ledger is an append-only log of every significant decision — what was decided, on what evidence, with what confidence, and what would constitute evidence that it was wrong. Entries are never retroactively edited. Corrections use a narrowing pattern: a new entry that constrains or supersedes the prior one.

This matters for AI-assisted work specifically because AI outputs are fluent. A confidently-stated wrong answer looks identical to a confidently-stated right one. The confidence tagging system makes the epistemic status of every claim visible and trackable.

---

## Claim Governance — The Counter-Pass

The Counter-Pass is a structured adversarial session run against the outputs of a Mode 1 session. A separate AI context — isolated from the producer context — is given the output and asked to find every way it could be wrong.

Findings from Counter-Pass sessions are numbered, documented, and adjudicated. Each finding either:
- **Survives** — the original claim stands, narrowed if necessary
- **Does not survive** — the claim is revised or dropped

Nothing enters the canonical spec without clearing a Counter-Pass. Claims that are too speculative to survive adversarial pressure are explicitly stamped `SINGLE-CONTEXT — NOT PANELED` and cannot be cited as evidence for anything downstream.

A real example: during PC#8 design, Counter-Pass found that the original `boundType` vocabulary value `exposure-upper-bound` was dishonest as applied to an AT Protocol crossing — it implies a bound can be asserted when it cannot. The finding resulted in a new vocabulary value (`exposure-unbounded`) and a restatement of the architecture's honest limits. That is what Counter-Pass is for.

---

## Project Governance — The NI-5 Discipline

NI-5 is a named ceiling on generality claims. It holds that the Seam Stack architecture is local-first specific on current evidence, and that no claim of broader applicability can be made until a second substrate crossing has been demonstrated.

PC#8 provided that second crossing. The scope claim was then carefully advanced — to "demonstrated on two named substrates; additional substrates remain design intent" — with Counter-Pass on the advancement verdict before any public use of the new claim.

This is the practice precedes framework principle in action. The architecture does not claim more than the evidence supports. When the evidence changes, the claim changes — through a governed process, not by quietly updating a document.

---

## Project Governance — The UFO Lexicon

The project maintains a versioned controlled vocabulary (UFO Lexicon, currently v2.3). Every term with a specific architectural meaning is defined in it, with:
- What the term means in this context
- What it is not (collision prevention)
- Which register it belongs to
- What the plain-register alternative is for community-facing copy

Terms in PROPOSED status cannot appear in publication-track documents. This prevents a specific failure mode: a term gets used in a spec before its meaning is stable, and then the meaning shifts, and everything downstream is quietly wrong.

The Lexicon also carries rulings — decisions about specific term usage that arise from real collisions. Ruling R-1, for example, reserves "Substrate" for the Seam Stack's technical data layer only, and prohibits its use as a general synonym for "foundation" or "infrastructure." That ruling exists because the collision caused actual problems in early drafts.

---

## What the Methodology Is Not

It is not a process that prevents mistakes. Phase 0 of PC#8 produced a credential exposure incident. The methodology caught it, surfaced it, and produced a remediation path. That is what it is for.

It is not a process that slows everything down. Most of the individual instruments take minutes to run. The overhead is front-loaded into session opens and back-loaded into close compressions; the actual working time in between runs at normal speed.

It is not a process that makes AI output trustworthy by default. The output is trustworthy only after it has been through the Counter-Pass, cleared the Survival Ledger, and had its confidence level explicitly tagged. Before that, it is a draft.

---

## Why This Matters for AI-Assisted Builds Specifically

The strongest objection to AI-assisted technical work is not that the output is low quality — often it is not. The objection is that the builder cannot be accountable for decisions they did not make.

The governance methodology is a direct answer to that objection. Every significant architectural decision in this work is:
- Explicitly made by the operator (not delegated to the AI)
- Documented with its rationale and confidence level
- Adversarially tested before it becomes canonical
- Tracked in an append-only ledger

The AI is a build accelerator. The decisions, the constraints, the NI-5 ceiling, the Counter-Pass findings, the Survival Ledger — those are the operator's. That is what makes the work defensible.

---

## The Instruments at a Glance

| Instrument | Function |
|---|---|
| Session Harness v0.2 | Opens, governs, and closes working sessions |
| Counter-Pass | Adversarial claim validation in isolated context |
| Survival Ledger | Append-only decision log with confidence tags |
| UFO Lexicon v2.3 | Versioned controlled vocabulary with publication gates |
| GSEF | Governed Schema Evolution Framework — cross-layer schema change governance |
| Accuracy Ledger | Tracks factual claims and their verification status |
| NI-5 discipline | Generality claim ceiling tied to evidence base |

---

*Part of the [Pattern Commons series](./index.md). See also [PC#7](./pc7-employment-seam.md) and [PC#8](./pc8-substrate-crossing-seam.md).*
