# 06 · Taking Stock of the Whole Build

*Where each part of the Seam Stack stands, as of the blueprint*

*2026-09-28*

---

The first five entries in this notebook each followed one thread: a substrate that moved under the build, a governance design meant to compose, a claim tested before publishing, a prototype put under pressure, and a framework's trust rules run for the first time. Each thread was reported where it was, when it was. The work has since spread across eight repositories, and no single entry says how they fit together.

On September 27 that changed. [BLUEPRINT.md](https://github.com/jediwright/seam-stack/blob/main/BLUEPRINT.md) now sits at the root of this repository beside the README and `THEORY.md`. It is the full accounting: the problem, the four-layer architecture, how a crossing runs, what can and can't be proven, every repository and the layer it exercises, the pattern series, the known limits, the working method, and a lexicon. It includes the internal method vocabulary on purpose, marked where it appears. It was drafted in a single working context and hasn't yet had an independent review. Where it differs from the pattern specification or `THEORY.md`, those govern.

With the map written, this entry records the current position of each part against it before the next thread starts.

---

## The idea, in one paragraph

Local-first software made a person's own device a credible home for their data. What it left open is the moment data has to leave that home and enter something the person doesn't control, such as an employer's system, a public network, or an AI agent acting for them. The Seam Stack treats that boundary as the thing to design. A crossing counts as governed only if it has a declared scope, a grant from an accountable party, a gate that checks live authorization at the moment of crossing and refuses when anything is unclear, and a record held by the parties themselves. The blueprint's Part One says this at more length and for any reader.

---

## Where each part stands

**The pattern.** The specifications live in [local-first-series](https://github.com/jediwright/local-first-series). The root entry, *The Governed Crossing* (Pattern Commons #00, v0.3), names the pattern that the domain entries had been demonstrating without naming it. It also carries two application notes, one for shared workspaces written by teams of agents and one for shared state that merges automatically. A [plain-language version](https://github.com/jediwright/systems-of-thought) is the easiest way in. The generality claim is bounded to the two kinds of substrate actually built against. A third kind is unclaimed.

**The reference prototype.** [employment-seam](https://github.com/jediwright/employment-seam) is unchanged since entry 04: Phases 0 through 3 complete, 51 of 51 tests passing, eight governed runs against a live AT Protocol server. Governed publishing from an encrypted local-first document to Bluesky works, with an intent record written before the publish and a completion record after. The Automerge team's [August 2026 update](https://automerge.org/blog/2026-august/) described it as an early prototype and the first thing they'd seen combining Keyhive with AT Protocol, and noted that its implementation notes had turned up rough edges in their packaging. Nothing has been added to it since August 30. It is finished as a prototype rather than paused.

**The native app.** [local-first-social-native](https://github.com/jediwright/local-first-social-native) is the newest build and the most active one. It is a small iOS and Android app with a shared Rust core, running on the same encrypted document layer as the prototype. It has run a real identity ceremony on device, granted and revoked membership in a group through an app-side rule that refuses grants below admin standing, and recovered an identity from a cold key after the device key was lost. It currently runs on an iPhone and an Android emulator. Sync between devices hasn't been built. Recovery has only been shown within the same store, not onto a new phone. What happens to a lost device's standing permissions after recovery is undecided. A published evidence note measures what the app stores, written for the maintainers of the library it depends on. The full story of the recovery work is the next entry.

**The content gate.** [tcf-runtime](https://github.com/jediwright/tcf-runtime) is where entry 05 left it. A write-time gate enforcing the Tiered Content Framework's trust labels runs on a pinned engine and reaches every refusal path on fixtures. Its spec was corrected where the text didn't execute as written. Phase 1 waits on the framework's next revision, which hasn't opened. Building against candidate field names a second time would repeat the same uncertainty.

**The rule language.** [selvage](https://github.com/jediwright/selvage) is a small language for writing boundary rules and generating schemas from them. It covers one boundary shape, where a record leaves, is checked, and arrives. On one real instance its output matches the prototype's schemas field for field. The second shape, a boundary that makes a decision rather than passing a record through, is scoped and needs roughly nine language features that don't exist yet.

**The shared record format.** [governedcrossing](https://github.com/jediwright/governedcrossing) is the youngest repository. It holds draft AT Protocol lexicons, in a temporary namespace, so that different apps can read each other's crossing records without depending on this project. A draft for recording changes to who can read something is out for community feedback on the Atmosphere forum until October 26. Its values are designed but not yet registered in the vocabulary, so nothing conforms to them yet.

**Governance for the code itself.** [governed-pr-framework](https://github.com/jediwright/governed-pr-framework) scales pull-request review by how far a failure would spread rather than by line count. It is at v0.3, in use on the prototype repository.

**The essay.** [*Full Personhood*](https://www.systemsofthought.com/full-personhood/) is the strategic argument behind the architecture: AI agents act on people's behalf across many systems, every one of those actions is a boundary event, and without governed crossings accountability collapses into whoever runs the server. It is published and has been revised several times as the builds have moved.

---

## What's open across all of it

Three questions cut across the repositories rather than belonging to one.

The first concerns what the Governance layer is. The pattern specification defines it by meaning: how information is structured, classified, and constrained. This repository's README defines it by authority: who may act, in what role. The work uses both, and the blueprint states both rather than choosing. The next revision of either source is expected to settle it.

The second concerns the second substrate. Entry 04 said a worked crossing on a different local-first system would turn a single observation into something closer to an argument. The native app doesn't do that. It is the same document and encryption layer on a different platform and in a different language. That is useful evidence about portability and not evidence about substrates. The cross-substrate claim still rests on one instance.

The third is prior art. Entry 03 named territory that hadn't been swept. The blueprint now names Trust over IP and W3C Verifiable Credentials as the nearest prior art and says where the design differs. A full published comparison, including The Update Framework and decentralized attestation systems, still doesn't exist.

---

## What comes next

The next entry returns to a single thread: recovering an identity on the native app without a server, and what that exposed about the library underneath. After that, sync between devices is the native app's next phase, and the content gate waits on its framework revision.

The blueprint will drift as the builds move. Entries like this one are where the drift gets recorded.

---

*This is the sixth entry in a notebook about building the Seam Stack in public. It reports status and introduces no new findings. The map it follows is [BLUEPRINT.md](https://github.com/jediwright/seam-stack/blob/main/BLUEPRINT.md) v0.3.1 (commit `2e6f618`). Repository positions as of 2026-09-28: employment-seam `fb05ea1` · local-first-social-native `c803377` · tcf-runtime `827bcbe` · selvage `0bcb6e3` · governedcrossing `cef92b4` · governed-pr-framework `7ed691e` · local-first-series `ee52a0b`.*
