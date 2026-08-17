## Why the composition ladder has this shape

The architecture in this repository produces signed, verifiable records of
formal agreements between parties — here called **crossings** — and
composes in seven levels. These seven levels describe how records compose
within and across the architecture's four functional layers — Substrate,
Governance, Boundary, and Evidence — rather than replacing them. 

At the base sits an **invariant layer**: two parallel constraint families —
cryptographic invariants (signature suites, canonicalization, timestamp
anchoring) and vocabulary invariants (versioned namespaces, shapes,
controlled vocabularies) — that govern what can be said and what can be
proven, while asserting nothing about any particular crossing. Above it,
fields compose into separately signed units; signed units compose into
bonded multi-party structures; those structures organize into
purpose-bounded containers; the whole composes into a delivered bundle — a
standalone, verifiable artifact; bundles link into networks under shared
governance; and the patterns governing all of this organize into a corpus.

This document answers a question the README does not: why seven levels, in
this order? The answer is a cross-domain one. The ladder is an instance of
an organizational pattern studied in my companion research program, the
**Resonance Architecture** — a claim tested against a pre-registered
prediction, with the ways the claim had to be qualified or reduced in scope
recorded alongside it, not asserted after the fact.

Three findings from that test matter here.

**The ladder is two ladders.** Records compose upward along one axis:
fields into signed units, units into bonded structures, structures into a
bundle, bundles into linked networks. Specifications govern along a second
axis: a pattern specification governs bundles; a shared platform hosts
multiple such patterns under common governance; and a body of documented
patterns organizes them into a discipline. A bundle does not compose into
its governing pattern; the pattern governs bundles. The two axes meet but
do not merge — and the top level exists only on the specification axis.
Nothing on the record axis forms a governed whole; a participant's full
record set is an accumulation, not a totality. That asymmetry is a known
limit, recorded rather than repaired.

**Some evidentiary properties exist only in the bonded set.** When two
separately signed disclosures of a record are each required to reference
the same content hash, tampering becomes visible: any divergence between
them is itself the evidence. Dual logs required to agree on timestamps make
dispute visible the same way. No single signature has this property. It
appears only where the schema forces multiple signed units into a required
relationship — and it is enforced by schema constraint, not by discipline.

**No adjudication level exists, by design.** Some domains need a
structural level for events that cannot be authored in advance — a place
where the system itself decides what happened. Whether that level appears
is governed by one question: can the domain's events be fully specified
before they occur? Here they can. The conditions that trigger a crossing,
the stages of the signing process, and the terms of the agreement itself
are all fully defined when the arrangement is first created. Disagreement
is recorded as data — a contested status, preserved divergent accounts —
and residual dispute is exported to whatever institution the parties can
reach, never built into the structure. The pattern predicted this level's
absence for a domain shaped this way, and the prediction held, including
the hardest case: one party unilaterally and irreversibly cutting the
other off. The record's timestamps establish what happened first, so the
event is handled as evidence rather than by any built-in adjudication
level.

The relationship runs both ways. This architecture is the first
evidentiary, legal-stakes domain mapped against the pattern, and the first
whose level boundaries are machine-checkable. The pattern did not validate
the design — the architecture stands on its own grounds, and nothing in it
changed as a result of the mapping. What the mapping supplies is the missing 
explanation: the ladder looks this way because domains organized under these 
constraints keep producing this pattern.

---

## Part of a broader methodology

The Seam Stack sits inside a set of interlocking frameworks developed in
parallel under [Systems of Thought](https://www.systemsofthought.com).

**[Resonance Architecture](https://www.systemsofthought.com/about/)** is the
cross-domain research program that supplied the shape argument above and is a 
cross-domain synthesis that argues for structural isomorphism between content, 
matter, and consciousness across the same six organizational tiers first mapped 
in the Tiered Content Framework, and now extended into a much larger theoretical 
claim. It is the most speculative of the projects, and the one that, if it holds, 
reframes all the others. The seam domain is the first evidentiary, legal-stakes 
domain mapped against it.

**[Pattern Commons](https://github.com/jediwright/seam-stack)** is the
specification layer that sits on the second axis of the composition ladder.
Each Pattern Commons entry governs a class of crossing — defining the
boundary conditions, signing ceremony, and evidence requirements for a
specific domain. [PC#7](https://github.com/jediwright/employment-seam) is
the current entry for the employment seam.

**[Tiered Content Framework (TCF)](https://www.jediwright.com/content-strategy-framework)** is
a content governance framework developed alongside the Seam Stack that
governs how information is structured, composed, and made machine-readable
across tiers — from individual vocabulary terms up through full knowledge
corpora. The vocabulary invariants at the base of the composition ladder
reflect TCF's approach to controlled, resolvable namespaces.

**[Governed Pull Request Framework (GPRF)](https://github.com/jediwright/governed-pr-framework)**
is the review discipline that governs how changes to this architecture are
proposed and merged. It scales review rigor by blast radius — how far a
failure would spread — rather than by line count, and requires every claim
in a PR to be verified or honestly tagged as not yet verified. All
structural changes to the Seam Stack pass through it.
