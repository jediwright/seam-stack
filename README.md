# The Seam Stack

A four-layer architectural pattern for local-first systems where the seam, not the server, is the primary design surface.

---

### The problem

Local-first architecture might have solved the wrong half of the problem first.

The hard work of the last decade went into making clients credible: CRDTs that converge correctly, sync engines that survive partition, storage that holds when the network doesn't. That work was necessary, and it is mature enough to build on now. What it left unfinished is what happens at the boundaries — where a local-first system must interact with money, regulated work, the legal substrate, and parties outside the trust circle.

Those boundaries are the seams. And in production systems, the seam is where everything actually happens. The first payment from a new client. The hiring decision at a new job. The clinical intake at a care relationship. The labor record at a transition. The seam is where trust is established, transferred, and — when necessary — terminated on record. The Seam Stack is a pattern for deliberately designing those moments.

The Seam Stack is a pattern for deliberately designing those moments.

---

### The four layers

**1. Substrate.**
The local-first foundation: CRDTs, IndexedDB, sync. The substrate is what makes the client credible as the canonical site of state. Authorization and encryption properties at this layer propagate upward into the Boundary and Evidence layers.

**2. Governance.**
The rules that determine what counts as a legitimate operation at the seam: who can participate, in what role, with what authority. Where governance is enforced cryptographically at the substrate level, it is binding regardless of party behavior.

**3. Boundary.**
The rules that determine what counts as a legitimate operation at the seam: who can participate, in what role, with what authority. This includes AI agents — which the Seam Stack treats as governed parties with their own capability lifecycle, not as tools acting on behalf of a party. Where governance is enforced cryptographically at the substrate level, it is binding regardless of party behavior.

**4. Evidence.**
What persists after the seam closes? Who has a copy of what, in what format, with what cryptographic anchor, retrievable under what circumstances? The evidence layer is what makes the system answerable: to itself, to its participants, and to anyone who arrives later asking what happened.

The four layers compose. Omitting any of them relocates the trust assumption rather than eliminating it.

---

### What this repository contains

**`vocab/`** — Vocabulary namespaces for the Seam Stack architecture. Stable, resolvable IRIs for implementers.

| Namespace | IRI | Status |
|---|---|---|
| Crossing record (base shape) | [`vocab/crossing-record/0.1`](https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1/) | v0.1 — active development |
| Employment seam | [`vocab/employment-seam/0.5`](https://jediwright.github.io/seam-stack/vocab/employment-seam/0.5/) | v0.5 — active development |
| Canonical assurance scale | [`vocab/assurance`](https://jediwright.github.io/seam-stack/vocab/assurance/) | v0.1 — active development |

**Specifications and governing documents** will be published here as the architecture matures.

---

### Current development

**[Keyhive employment seam](https://github.com/jediwright/employment-seam)** is the primary development track — the first instance of the pattern built on an authorization-backed substrate (Automerge + Keyhive), and the first to include revocation as a first-class architectural event.

**[Pattern Commons #7 v0.5](https://github.com/jediwright/employment-seam)** is the current specification for the employment seam, documenting the boundary layer for the employment relationship: entry seam, exit seam, gate-check records, agent capability grants, and revocation discipline.

---

### Further reading

- [The Seam Stack](https://www.systemsofthought.com/seam-stack/) — architecture overview and design principles
- [Nine Days, Four Prototypes, One Framework](https://www.systemsofthought.com/nine-days-four-prototypes-one-ai-development-governance-framework/) — the essay that names the pattern across four domains
- [Full Personhood: The Governance Model AI Requires and Capitalism Never Built](https://www.systemsofthought.com/full-personhood/) — the governance argument the architecture serves

---

*The Seam Stack is a project of [Systems of Thought](https://www.systemsofthought.com), a research lab and publication operated by [UX Minds, LLC](https://www.uxminds.org). Developed in part with AI assistance, disclosed where named.*

*Maintainer: [Jedi Wright](https://www.jediwright.com) / [Systems of Thought](https://www.systemsofthought.com) / [UX Minds, LLC](https://www.uxminds.org)*
