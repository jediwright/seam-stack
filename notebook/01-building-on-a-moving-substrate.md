# 01 · Building on a Moving Substrate

*2026-08-21*

---

On July 31, Ink & Switch published [their latest entry](https://www.inkandswitch.com/keyhive/notebook/06/) in the Keyhive notebook series — a formal security analysis of BeeKEM, co-authored with Derek Yen, a PhD cryptographer from NYU Courant, accompanied by [a peer-reviewed paper](https://eprint.iacr.org/2026/1434.pdf) on ePrint. The closing line: *"This is the last entry for now."*

I pulled it today, with Phase 1 of my [substrate-crossing prototype](https://github.com/jediwright/employment-seam/tree/main/substrate-crossing) complete and Phase 2 being scoped.

The substrate-crossing prototype — the piece of this work that publishes a governed record from a local-first document to a federated network — is pinned to `@automerge/automerge-repo-keyhive@0.5.0-alpha.1`. The Keyhive TypeScript package is the current implementation of the key agreement protocol BeeKEM formalizes. And BeeKEM's Rust implementation (Hexane) is on an alpha cadence, with a stable release estimated for Q4 2026–Q1 2027. Which means the TypeScript surface I'm building against is mid-transition to a replacement.

---

Ink & Switch follows a legible arc. Deep research. Lab notebook entries as the findings accumulate. Formal validation when the design is settled enough to stake academic credibility on it. A synthesizing essay. Then the handoff — the project moves to the community, the ecosystem, or wherever the ecosystem currently is.

Automerge went through this cycle before Keyhive did. It landed better than most handoffs because there were enough production users with enough stake in it to sustain real maintenance — but it still took years, and the adoption base still seems narrow relative to the idea's scale. Watching that transition is part of why the swappable-layer decision got made early this time. Keyhive on TypeScript is now at the notebook-close stage of the same arc, with Hexane, the successor implementation, already in motion. How that resolves is still open.

None of this is a criticism. It's a research lab pattern, and it's a reasonable one. The lab produces rigorous foundations. The notebook closing on a formal security proof likely means the protocol design is settled — which is useful information if you're building on it. The question is what it means for the build.

---

What it means, practically, is that the TypeScript sync surface may be a migration cliff waiting to land. Teams that built tightly against it — importing sync primitives directly, coupling test infrastructure to specific API shapes, assuming the package version is stable — will have meaningful work to do when Hexane stabilizes, and the TypeScript implementation either freezes or gets deprecated. That window is probably six to twelve months out. It's not a crisis. It's a design input.

The response I chose before Phase 1 opened was to treat the sync layer as explicitly swappable. The crossing-record schema — the thing that actually carries governance meaning across the boundary — is specified at the pattern level, not the package level. It describes what a crossing record must contain and why. The implementation wires it to the current packages; the spec survives a package replacement. When Hexane lands, the migration touches one surface. The governance layer doesn't move.

This wasn't a complicated architectural decision. It was a direct response to having watched the pattern play out once before with Automerge's own TypeScript-to-Rust transition history, and to choosing not to be surprised by it.

---

Whether the posture is sufficient won't be known until the transition actually happens. The prototype exists partly to find out — to build enough surface area against the current stack that I understand exactly what's coupled and what isn't before the coupling is tested by a real upstream change. That's what the build is for, among other things.

The Keyhive notebook closing is a maturation signal, not an abandonment signal. The protocol is formally grounded. The Rust implementation is active. The community is small, but the foundational work is sound. Building here is still the right call. The question was never whether to build here. It was how to build here in a way that doesn't require redoing the governance layer every time the implementation layer moves.

That question has an answer. It's just not fully tested yet.

---

*This is the first entry in a notebook about building the Seam Stack in public. The notebook lives alongside the [essay](../essay/local-first-at-the-edge.md) and the [architecture documentation](../README.md) in this repository.*
