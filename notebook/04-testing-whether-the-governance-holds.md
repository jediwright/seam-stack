# 04 · Testing Whether the Governance Holds

*What Phase 3 of the employment-seam prototype found*

*2026-08-31*

---

Entry 03 of this notebook ended with an honest problem. The central claim being tested — that a local-first document can publish a cryptographically verified record to a public AT Protocol feed without the published content bypassing the local governance — had survived four sweeps for prior art. But surviving sweeps means the territory you covered came back empty, not that the territory you didn't cover doesn't exist. Three known territories remained unswept: The Update Framework, W3C Verifiable Credentials, and decentralized attestation systems like the Ethereum Attestation Service.

There was also the more fundamental question: claims about a mechanism are not the same as claims about how the mechanism behaves under pressure. Passing a test under ideal conditions proves it works. It doesn't tell you whether it holds when conditions are awkward — when time passes between authorization and action, or when the thing being authorized has the opportunity to change before it fires.

Phase 3 ran three scenarios designed to find out.

---

## Why three scenarios instead of one

A single crossing from a local-first document to a public AT Protocol post was established in Phase 2: the intent record gets written before the publish fires, the grant gets checked at gate time, the completion record gets written after publication, and the published post carries a back-pointer to the intent record in the local document. That's the mechanism working under normal conditions. It's the baseline.

Phase 3's question was whether the governance holds when conditions are less cooperative. Three scenarios were chosen, each introducing a different kind of awkwardness — not arbitrary difficulty, but the kind a real deployment would encounter.

**Scenario one** ran the baseline again, this time with content assembled from multiple granted source documents rather than a single one. This established how the seam handles a common real-world case: you want to publish something built from several governed inputs, not a single document, and each input is governed separately.

**Scenario two** added a not-before constraint. The document's owner sets an earliest authorized time on the intent record — the crossing won't fire until after that time. The question this creates is: does the governance apply at the moment you authorize the crossing, or at the moment it actually fires? The two are not the same. Time passes. The authorization happens, then you wait, then the fire happens. If governance runs only at authorization time, the wait introduces a window.

**Scenario three** tested content integrity directly. At the moment the crossing is authorized, a digest of the content being published is computed and locked into the intent record. At the moment the crossing fires, the seam re-reads the content and verifies it still matches that digest. The test: between authorization and fire, mutate the content. What happens?

---

## What each scenario found

**The multi-input baseline.** The seam creates an assembly document — a single seam-owned document that holds the combined content of all the granted inputs, assembled in a fixed order. Every crossing, whether built from one source document or several, goes through this document; there's one code path, not two. The intent record's digest, its back-pointer fields, and the completion record all point at the assembly document, not at the individual inputs. The inputs are traceable through a lineage field, but the binding is on the assembled output.

What was confirmed: the gate checked the grant on each input document, the assembly was deterministic (running it twice on the same inputs produced byte-identical output), and the published content matched what was authorized. This scenario doesn't test whether the content can change between assembly and fire. That's Scenario three.

**The delayed release.** An earliest-authorized time was set on the intent record, ninety seconds into the future. A before-horizon crossing attempt was made and blocked — no record was written, nothing was published. After the horizon passed, the crossing fired and completed. A third attempt was made with a new future horizon set — it blocked again, confirming that the gate re-evaluates on every attempt rather than caching the first pass.

One finding during the build is worth naming: the horizon check was initially placed in the wrong position in the gate sequence — after the assembly document was written rather than before it. That meant content was being written to the seam's own document before the gate had finished evaluating. The fix moved both horizon checks to a single step before any write. It was corrected before any observation was logged; the fixed behavior is what ran.

One open question the scenario surfaced but didn't answer: what happens when the grant authority expires before the release horizon passes? A not-before horizon delays the fire; a lapse of the underlying grant in that same window would mean the content was authorized before the delay, but the authorization has since expired by the time the fire is allowed. That scenario was not exercised. The grant persisted across all three legs of this run; the seam refused on the basis of the not-yet-reached horizon, not on a lapsed grant. That gap — lower-bound behavior confirmed, lapse behavior unknown — is the most interesting open question going forward.

The scenario produced one small observation worth noting about legibility: the published post's displayed date reflects the content timestamp, not the publication time. A delayed-release post shows a pre-embargo date on the surface of the feed. Horizon compliance is legible only from the intent record in the local document — both horizon values plus the fact that the actual publication time falls after them. The AppView doesn't surface this; the record does.

**The content integrity check.** The multi-input assembly was run through four legs in sequence: a determinism check (the assembly produces the same bytes twice), a pre-authorization tamper attempt (content modified before the intent record is written), a post-authorization tamper (content modified after the intent record is written but before the fire), and a clean positive run after the blocks.

The pre-authorization tamper was caught at the digest check before mint — nothing was written, nothing was published. The post-authorization tamper — the TOCTOU case, named for the class of vulnerability where the thing you checked isn't the thing you use — was caught at the re-verification step before the fire: the seam re-reads the assembly document immediately before publishing and compares what it finds against the digest it locked in at authorization time. Content mismatch: fire blocked, nothing published, the intent record remains in the document in an unconfirmed state. Clean content on the retry: passed.

The window between authorization and fire was measured in the positive run: under two milliseconds on each instrumented surface. That's the exploitable window the re-verification collapses the *content* half of the race to. It is not zero. The mechanism doesn't eliminate the window — it bounds it. A system that completely separated authorization from execution with an arbitrarily long delay between them would have a proportionally larger window; this one keeps the sequence in a single pipeline run. Bounding is not closing.

There's a second half of the race the re-verification doesn't touch: whether the grant is still current at the moment the fire actually happens, as opposed to the moment the gate checked it. A revocation landing in that gap is checked-then-crossed. That window cannot be closed without a coordinating party in the path — something this architecture deliberately doesn't require. The entry 03 falsifier discipline applies to both halves: the honest description is that the content window is as small as a single-process pipeline makes it, and the grant-currency window is an acknowledged structural fact, not a defect.

Both surfaces of the check are implemented. If the seam detects that the assembly document changed after mint, it blocks and logs the block — a gate-style outcome, recoverable by running the gate again from scratch. If the outgoing payload's content doesn't match the authorized digest, the seam faults before the publish fires — this is treated as the seam breaking its own invariant, not as an external condition. Surface two was exercised by tests, not by a live run; a live mismatched publish would mint bad evidence, which is exactly what the check is preventing.

---

## What changed about the claim

Three scenarios, each creating a different kind of pressure, and the governance held in all three.

"The mechanism works" is now "the mechanism works, and the governance holds, across a baseline multi-input scenario, a delayed-release scenario, and a content-integrity scenario, each of which could have failed differently." That's a narrower, more specific, and more useful claim than the one entry 03 started with.

Two things were found that further narrow it.

The content-integrity scenario surfaces a named structural limit. The TOCTOU window — the time between when the content is authorized and when the fire checks it — cannot be eliminated in any system that separates authorization from execution. You can bound it, and this implementation does. The restatement of the claim has to include: the seam re-verifies content before firing, the window is as small as a single-process pipeline makes it, and that is not zero.

The delayed-release scenario raises the most interesting open question. Grant-authority lapse — what happens when the authorization behind a crossing expires before the not-before horizon passes — was not exercised. The seam enforced the not-before constraint; the grant didn't lapse in any of the runs. If you're building something where a time-delayed crossing matters, that's the scenario to think about.

What didn't change: the three unswept territories listed in entry 03 remain unswept. The Update Framework, W3C Verifiable Credentials, and the Ethereum Attestation Service haven't been formally compared to this work. Phase 3 closed no collision-check territory. Those remain honest open items.

---

## What comes next

Two things on the horizon worth tracking.

The more interesting one is whether the same seam architecture holds on a second local-first substrate. The employment-seam prototype runs on Automerge with Keyhive for access control; the AT Protocol side is the target. What happens if you swap the local-first side for a different CRDT system — or the target side for a different public protocol? A second worked crossing on a different substrate would move the cross-substrate claim from a single-instance observation to something more like an argument. That work isn't built yet.

The unswept territories from entry 03 remain the honest frontier. The Update Framework, in particular, governs software update metadata with time-bounded validity assertions and a client-verifiable integrity anchor — the structural overlap with what this prototype does is sufficient that a proper comparison is warranted. Whether TUF, W3C Verifiable Credentials, or decentralized attestation systems occupy the same space, or leave this specific combination of constraints unoccupied, is genuinely unknown. One system in unswept territory exhibiting the same composition would break the claim. Whether that system exists is what further work is for.

---

*This is the fourth entry in a notebook about building the Seam Stack in public. The notebook lives alongside the [architecture documentation](https://github.com/jediwright/seam-stack/blob/main/README.md) in this repository. The prototype discussed here is at [jediwright/employment-seam](https://github.com/jediwright/employment-seam) · HEAD `fb05ea1` · 51/51 tests. The pattern specification is in [jediwright/local-first-series](https://github.com/jediwright/local-first-series/blob/main/pattern-commons/pattern-commons-08-substrate-crossing-seam.md).*
