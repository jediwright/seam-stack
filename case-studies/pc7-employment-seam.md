# PC#7 — The Employment Seam

**Pattern Commons #7 · Reference Implementation**
*Local-first governed data crossing at employment transitions*

---

## The Problem in Plain Language

When someone leaves a job, the record of what they did there — their knowledge, their contributions, the context of how the relationship ended — belongs almost entirely to the employer. The worker walks away with whatever they can remember or informally document. If the departure was contested, the employer's account is the one that persists in any formal system.

This is an architectural problem before it is a legal or political one. The information asymmetry is baked into how employment record systems are built.

The Employment Seam is a governed architectural pattern for that moment: a designed boundary crossing where both sides of a transition can record their account, contemporaneously, with cryptographic anchoring, without either party being able to alter the other's record after the fact.

---

## What Was Built

A reference implementation of PC#7 on two foundational technologies:

- **Automerge** — a CRDT (conflict-free replicated data type) library that makes the local client the canonical site of state, not the server
- **Keyhive** (`@automerge/automerge-repo-keyhive@0.5.0-alpha.1`) — a capability-based access control layer that governs who can read and write what, without requiring a central authority to enforce it

The implementation covers the full four-layer Seam Stack architecture:

| Layer | What it does |
|---|---|
| **Substrate** | Automerge + Keyhive — local-first, cryptographically governed |
| **Governance** | Who has standing to do what, enforced at the schema level |
| **Boundary** | The seam itself — the designed moment of crossing |
| **Evidence** | What persists after the seam closes, and who has a copy |

---

## The Build Arc

This was not a single-session build. It went through five major prototype iterations and a full governance pass on each.

**What the iterations resolved:**

The early prototypes established that the "relay should facilitate and exit" principle — a foundational constraint of the architecture — is not just a policy aspiration. It can be made structurally true: when the substrate uses end-to-end encryption with capability-based access control, the relay is cryptographically incapable of reading the content it carries. The exit is architectural, not aspirational.

Later iterations formalized the multi-perspective record: every seam firing captures accounts from all participant perspectives, with role-conditioned access enforced at the schema level. A worker's account and an employer's account are both present, neither is privileged, and neither party can alter the other's entry.

The final iteration produced a reference implementation checker (`check-governed-read.mjs`) — a tool that verifies a given crossing record meets the governed-read requirements. That artifact is the difference between a working prototype and a replicable pattern.

**Result:** 27/27 tests passing. Vocabulary namespaces published at resolvable IRIs. Schema version declaration block in place per the Governed Schema Evolution Framework (GSEF).

---

## Key Technical Decisions (and Why)

**Why CRDT + capability-based access, not a traditional database?**
A traditional database requires the server to be trusted. The whole point of this pattern is that trust cannot be delegated to a server that one party controls. CRDTs let the client be authoritative; Keyhive lets access rules be enforced without a central enforcer.

**Why verifiable credentials for the legal record layer?**
W3C Verifiable Credentials Data Model 2.0 with `eddsa-rdfc-2022` cryptosuite. RFC 3161 timestamps from two independent qualified TSAs. OpenTimestamps for long-term tamper-evidence. This is not over-engineering — an employment record may need to be readable and legally defensible decades after the system that produced it has been deprecated. The format choices were made for longevity, not convenience.

**Why role-conditioned access at the schema level, not enforced by application logic?**
Application logic can be changed, misconfigured, or bypassed. Schema-level access conditions — enforced via SHACL shapes — mean the constraint is in the data model itself. A record that a party isn't entitled to see cannot be retrieved from the schema; it is not a matter of the application choosing not to show it.

---

## What the Governance Layer Did

This build ran under a formal governance methodology (see [The Governance Methodology](./methodology.md)). A few concrete examples of what that meant in practice:

- **Counter-Pass sessions** challenged every major architectural claim from an adversarial position before it was written into the spec. Findings that didn't survive Counter-Pass were revised or dropped.
- **The Survival Ledger** tracked every significant decision with its confidence level and what would constitute evidence that it was wrong. Claims that hadn't cleared the ledger were explicitly marked provisional in the output documents.
- **NI-5 discipline** — a ceiling on generality claims — meant the spec only asserted what the evidence actually supported. The employment seam is local-first specific on current evidence; the spec says so rather than overclaiming.

The Pattern Commons #7 spec is currently at v0.5. The vocabulary namespace resolves at `https://jediwright.github.io/seam-stack/vocab/employment-seam/0.5`.

---

## What It Demonstrated

The employment seam is the first worked demonstration that the Seam Stack architecture — four layers, governed crossing, finality-arbiter-free — can be implemented in production-grade code on a real substrate, for a real use case with real legal and evidentiary stakes.

That demonstration is the foundation everything else in the Pattern Commons series builds on.

---

## Relevant Artifacts

- `jediwright/employment-seam` — reference implementation repository
- `pattern-commons-07-employment-seam-v0-5.md` — canonical spec
- `check-governed-read.mjs` — reference implementation checker (v0.2)
- `LINEAGE.md`, `HORIZONS.md` — GSEF minimal artifact set for the employment-seam vocabulary

---

*Part of the [Pattern Commons series](./index.md). See also [PC#8](./pc8-substrate-crossing-seam.md) and [The Governance Methodology](./methodology.md).*
