# 02 · Governance, Designed to Compose

*2026-08-21*

---

My [Tiered Content Framework](https://www.jediwright.com/content-strategy-framework) is a framework for governing content, meaning, and more. It describes how content is structured, classified, and made legible to external systems — search engines, AI retrievers, the person who reads a fragment stripped of its original context three years after it was written. It says nothing about where the data lives, who owns it, or how it moves between parties. Those concerns belong to adjacent layers. The TCF composes with them; it doesn't absorb them.

That claim has been in the framework since v1.6. What it didn't have, until recently, was a concrete implementation to point at.

---

Hexane is the storage engine underneath the Automerge document format. Its README describes it as "columnar compression you can edit in place." The byte format it operates on is the Automerge document format — and that format is frozen by specification, not by convention. The test suite pins it with golden fixtures: byte-exact expected encodings for every codec, captured against the original reference implementation. A change that trips one of those tests isn't a test failure. It's a compatibility break with every existing document.

That distinction matters more than it might look. A format frozen by convention can drift when a maintainer changes their mind, or when a new contributor doesn't know the convention exists, or when a dependency update shifts behavior in a way no one noticed until it was too late. A format frozen by a test suite that treats compatibility breaks as build failures is a different kind of guarantee. The governance context embedded in a document at that layer cannot be silently changed by an upstream version bump. It's there, in the format, and the format doesn't move.

This is the storage-level answer to a problem the TCF names at the semantic level: the AI Search Fragment Problem. When an AI retrieval system extracts a Particle — a field, a sentence, a data point — stripped of the document structure that gave it meaning, the governance context goes with it. The TCF's response is to specify that governance decisions about structure, classification, and relational context should be made at the same time and by the same team as the content decisions they describe. The Automerge format's response is to make that contemporaneity durable. The crossing-intent record, the authorization chain, the declaration that this content is intended for unbounded exposure — none of that is decoration bolted on after the fact. It lives in the same frozen format the fragment itself lives in.

---

The second property is about what happens when content changes.

Hexane allows insert, delete, and splice directly on compressed bytes — at logarithmic time cost relative to the document size, plus the bytes affected. A million identical values cost three bytes at rest. Rarely-used columns cost near-zero on edit. Speculative columns — fields you're carrying because you might need them — cost a handful of bytes in memory and zero on disk.

The governance layer has an equivalent concern. At the field level, it means constraints, terminology guardrails, and tone parameters. At the document level, it means governing how content assembles dynamically — when an AI system inserts a new section, updates an intent record, or splices a revised component into an existing structure — so that the result maintains coherence and intent alignment even when no human authored it. The TCF calls this the Intelligence Layer. It's a meaningful governance commitment. It's also a commitment that gets expensive fast if the storage layer charges a decompression tax on every assembly event.

Hexane doesn't. Dynamic assembly operates directly on compressed bytes. The governance layer doesn't have to fight the storage layer to do its job. They're designed to compose.

---

The full picture, mapped across the four layers this work is organized around:

| Layer | Concern | Concrete implementation |
|---|---|---|
| Storage | Where data lives; who owns it | Automerge binary format (frozen); Hexane columnar storage; Keyhive authorization |
| Governance | How meaning is structured and made machine-legible | TCF tier structure; Intelligence, Taxonomy, and Machine-Legibility dimensions |
| Boundary | Transition discipline | PC#8 crossing-seam; crossing-intent and crossing-completion record pattern |
| Evidence | Tamper-evidence; deferred-party legibility | Content digest binding; back-pointer; AT Protocol PDS record |

The governance row has been in the framework for a while. The storage row now has something to point at.

---

What this doesn't settle is whether the composition holds under pressure — when the Hexane Rust implementation stabilizes and the TypeScript layer it's replacing either freezes or gets deprecated, when the crossing-record schema meets an edge case the current spec didn't anticipate, when a third party reads a fragment and the governance context fails to travel with it in the way the format was supposed to guarantee.

The point of building the prototype is partly to find out. The structural relationship between the governance layer and the storage layer is designed to compose. Whether it actually composes under the specific conditions this work creates is what the build is for.

That question has a designed answer. Whether the design holds is what the build is for.

---

*This is the second entry in a notebook about building the Seam Stack in public. The notebook lives alongside the [essay](../essay/local-first-at-the-edge.md) and the [architecture documentation](../README.md) in this repository.*
