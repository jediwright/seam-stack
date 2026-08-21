# The Seam Stack

A four-layer architectural pattern for local-first systems where the seam, not the server, is the primary build and design surface.

---

### The problem

Local-first got the hard part right first: making software work on your own device.

That hard work of the last decade went into making clients credible: CRDTs that converge correctly, sync engines that survive partition, storage that holds when the network doesn't. That work was necessary, and it is mature enough to build on now. What it left unfinished is what happens at the boundaries — where a local-first system must interact with money, regulated work, the legal substrate, and parties outside the trust circle.

Those boundaries are the seams. And in production systems, the seam is where everything actually happens. The first payment from a new client. The hiring decision at a new job. The clinical intake at a care relationship. The labor record at a transition. The seam is where trust is established, transferred, and — when necessary — terminated on record. The Seam Stack is a pattern for deliberately designing those moments.

---

### The four layers

The architecture comprises four layers, each with a distinct responsibility. The layers are not optional substitutions for each other. A system that omits any one of them is not local-first at the seam; it has merely relocated the trust assumption elsewhere.

**1. Substrate.** The local-first foundation: CRDTs, IndexedDB, sync. The substrate is what makes the client credible as the canonical site of state.

The substrate is not a commodity choice. The authorization and encryption properties of the substrate layer propagate upward into the Boundary and Evidence layers. Whether the relay can read the handoff bundle, whether access can be revoked after the fact, and whether the data is private to the worker by default are substrate questions, not governance questions. A substrate that uses end-to-end encryption with capability-based access control (such as Automerge + Keyhive) produces different architectural guarantees at the seam than one that does not. The Seam Stack documents substrate choices as they affect seam behavior.

**2. Governance.** The rules that determine what counts as a legitimate operation at the seam: who can participate, in what role, with what authority. This includes AI agents — which the Seam Stack treats as governed parties with their own capability lifecycle, not as tools acting on behalf of a party. Governance is where the system encodes the social and legal facts that the substrate alone cannot represent. Trust tiers, role assignments, witness requirements, and who has standing to do a given thing.

Governance rules are only as strict as their enforcement layer. Where governance is enforced by policy and procedure, compliance depends on the parties' trustworthiness. When enforced cryptographically at the substrate level, it is binding regardless of party behavior. The distinction matters at the seam.

**3. Boundary.** The seam itself: the explicit, designed moment where the local-first system meets something it does not control. Payment processors, regulated counterparties, identity verification ceremonies, and legal record handoffs. The boundary layer is where the architecture has to answer for itself — to the parties on both sides of the crossing, and to anyone who arrives later asking what the record shows. It is also where production systems most often cede the architecture's premise.

The boundary layer has a characteristic failure mode: the relay. A system that passes data through a server to reach a counterparty has introduced a trust dependency at the boundary, regardless of how local-first the substrate is. The relay should facilitate and exit. Where the substrate supports end-to-end encryption, the relay can be made structurally incapable of reading the content it carries; the exit is cryptographic rather than aspirational.

**4. Evidence.** What persists after the seam closes? Who has a copy of what, in what format, with what cryptographic anchor, retrievable under what circumstances? The evidence layer is what makes the system answerable: to itself, to its participants, and to anyone who arrives later asking what happened.

The four layers compose. Omitting any of them relocates the trust assumption rather than eliminating it.

---

### Building with the pattern

**The Seam Stack is an open architectural pattern, not a permissioned registry.**

Any system that satisfies the four constitutive properties of a governed crossing and conforms to the `seam:CrossingRecord` base shape is a fully valid governed crossing — no canonical entry number, no upstream approval, and no repository access required. Build against the pattern freely.

A governed crossing has four constitutive properties. A boundary event missing any one of them is not a governed crossing; it is an ungoverned exposure with a name attached:

1. **A declared scope.** The crossing surface is explicit and minimal. What crosses is declared; what does not cross is also declared.
2. **A grant.** Authority to cross is issued by an eligible grantor to an eligible crossing party. The grant is the legal-responsibility anchor; the crossing is attributable to it.
3. **A gate.** Every crossing attempt is checked against the current capability state of the grant at act time. The gate fails closed: unconfirmed revocation blocks; silence blocks.
4. **A record.** Every gate-check invocation — pass or block — emits a `seam:CrossingRecord`. The record is a first-class output of the crossing, not an audit log appended afterward.

**Pattern conformance vs. canonical entry.** The Pattern Commons (see below) maintains a numbered series of canonical domain entries — PC#0 through PC#N — each representing a governed crossing class with peer-reviewed evidence and formal blast-radius review. Canonical numbering is a documentation and taxonomy layer above the pattern, not a gate below it. A working healthcare intake seam, a housing transition seam, or a financial record crossing built against this architecture is a valid governed crossing whether or not it ever becomes a canonical entry. The canon exists to make the generalization claim; your implementation does not need to make that claim to be real.

| Dimension | Implementing the pattern | Contributing to the canon (PC#N) |
|---|---|---|
| Scope | Domain-specific boundary crossings in your own system | Reusable, cross-cutting entries in the core taxonomy |
| Requirements | Four constitutive properties + `seam:CrossingRecord` conformance | Prototype evidence, operational proof, formal blast-radius review |
| Validation | Schema conformance and runtime execution at your seam | Evidence-gated integration into the taxonomy index |
| Permission gate | None. Build today without approval. | Governance-gated via the evidence review framework |

The `seam:CrossingRecord` vocabulary is the schema contract. The four layers are the architectural contract. Both are available now.

---

### What this repository contains

**`vocab/`** — Vocabulary namespaces for the Seam Stack architecture. Stable, resolvable IRIs for implementers.

| Namespace | IRI | Status |
|---|---|---|
| Crossing record (base shape) | [`vocab/crossing-record/0.1`](https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1/) | v0.1 — active development |
| Employment seam | [`vocab/employment-seam/0.5`](https://jediwright.github.io/seam-stack/vocab/employment-seam/0.5/) | v0.5 — active development |
| Canonical assurance scale | [`vocab/assurance`](https://jediwright.github.io/seam-stack/vocab/assurance/) | v0.1 — active development |

[THEORY.md](https://github.com/jediwright/seam-stack/blob/main/THEORY.md) — Why the four layers are composed in this order, and the cross-domain research that grounds the structural argument.

**Specifications and governing documents** will be published here as the architecture matures.

---

### Current development

**[Keyhive employment seam](https://github.com/jediwright/employment-seam)** is the primary development track — the first instance of the pattern built on an authorization-backed substrate (Automerge + Keyhive), and the first to include revocation as a first-class architectural event.

**[Pattern Commons #7 v0.5](https://github.com/jediwright/local-first-series/blob/main/pattern-commons/pattern-commons-07-employment-seam.md)** is the current specification for the employment seam, documenting the boundary layer for the employment relationship: entry seam, exit seam, gate-check records, agent capability grants, and revocation discipline.

**[Pattern Commons #0 — The Governed Crossing](https://github.com/jediwright/local-first-series/blob/main/pattern-commons/pattern-commons-00-the-governed-crossing.md)** is the abstract pattern the Seam Stack formalizes: the boundary event at which a party crosses into or out of a structured relationship under a capability grant, with four invariant properties — declared scope, grant, gate, record — across all domain instantiations.

**Pattern Commons #8 — The Substrate-Crossing Seam** governs crossings from local-first substrates into publicly indexed, relay-distributed regimes, with AT Protocol as the reference implementation. Phases 0, 1, and 2 are complete: 27/27 tests passing, live AT Protocol crossings documented, and `seamCrossingRef` back-pointer verified at the PDS layer.

---

### The Pattern Commons

The Pattern Commons is the canonical taxonomy of governed crossing classes. Each entry specifies a domain instantiation of the governed crossing pattern — naming the seam trigger, the participant model, the grant structure, the gate conditions, the record schema, and the failure taxonomy for that class.

**[PC#0 — The Governed Crossing](https://github.com/jediwright/local-first-series/blob/main/pattern-commons/pattern-commons-00-the-governed-crossing.md)** is the root entry: the abstract pattern that all domain entries instantiate. Read this first.

Current canonical entries:

| Entry | Domain | Status |
|---|---|---|
| [PC#0](https://github.com/jediwright/local-first-series/blob/main/pattern-commons/pattern-commons-00-the-governed-crossing.md) | The Governed Crossing — root pattern | v0.1.2 — Counter-Pass complete |
| [PC#7](https://github.com/jediwright/local-first-series/blob/main/pattern-commons/pattern-commons-07-employment-seam.md) | Employment Seam | v0.5 — reference implementation |
| [PC#8](https://github.com/jediwright/local-first-series/blob/main/pattern-commons/pattern-commons-08-substrate-crossing-seam.md) | Substrate-Crossing Seam (AT Protocol) | v0.1.3 — Phases 0–2 complete; 27/27 tests passing |

Entries #1–#6 document earlier prototype-phase work in commerce, healthcare, social networking, and infrastructure. The series is not exhaustive and is not intended to be: the pattern can be instantiated in any domain where a governed crossing makes sense. Canonical entries represent the classes where the architecture has been stress-tested and formalized. Pattern-conformant implementations that have not been submitted for canonical review are valid governed crossings; they have simply not entered the taxonomy index.

---

## Where this is going

**The institution keeps its copy. So do you.**
Person-side infrastructure for the records a life produces.

This repository holds the first working case: the employment seam — the records generated when a job begins, changes, or ends. Employment came first because it's the hardest ordinary case: the most parties, the most
legal weight, and the sharpest version of a pattern that shows up
everywhere — when a relationship with an institution ends, the
institution keeps the history, and the person starts over.

The pattern isn't specific to work. A life produces records across
health, commerce, finances, government, family, and creative practice —
and in many of those domains, people already hold legal rights to copies
of their records (data portability, right of access, open banking). What
doesn't exist is the receiving side: person-owned infrastructure that
can accept, hold, verify, and re-present those records. That's the gap
this work targets.

**Current coverage, honestly stated.** Working probes exist in four
domains: labor (this repo), health (fhir-seam), commerce
(checkout-seam), and personal communication (local-first social —
where the goal is private storage and relay, not formal records; not
every part of a life needs ceremony). What's unproven is composition:
records from different domains coexisting in one person-owned store
under a common schema. That is the roadmap — deliberately, instead of
new verticals.

**Near-term direction:**

- A unified record schema, so boundary events from different life
  domains compose in one store
- Graduated ceremony — heavyweight verification where stakes are high
  (a job ending), near-zero friction where they aren't (a receipt)
- Delegation for AI agents: permissions scoped per life domain,
  revocable, with a record of what was done
- No fifth vertical until composition is demonstrated

Spec-first, evidence-backed: claims here are meant to be checkable
against the running prototype and its test suite.

### Further reading

- [Local-First at the Edge](https://github.com/jediwright/seam-stack/blob/main/essay/local-first-at-the-edge.md) — seven boundary principles for what happens where the interior theory ends; the theoretical grounding for the pattern
- [The Seam Stack](https://www.systemsofthought.com/seam-stack/) — architecture overview and design principles
- [Nine Days, Four Prototypes, One Framework](https://www.systemsofthought.com/nine-days-four-prototypes-one-ai-development-governance-framework/) — the essay that names the pattern across four domains
- [Full Personhood: The Governance Model AI Requires and Capitalism Never Built](https://www.systemsofthought.com/full-personhood-the-governance-model-ai-requires-and-capitalism-never-built/) — the governance argument the architecture serves

---

*The Seam Stack is a project of [Systems of Thought](https://www.systemsofthought.com), a research lab and publication operated by [UX Minds, LLC](https://www.uxminds.org). Developed in part with AI assistance, disclosed where named.*

*Maintainer: [Jedi Wright](https://www.jediwright.com) / [Systems of Thought](https://www.systemsofthought.com) / [UX Minds, LLC](https://www.uxminds.org)*
