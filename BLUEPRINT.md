# The Seam Stack — Technical Architecture Blueprint

**Version:** v0.3.1 · **Date:** September 27, 2026
**Scope:** The architecture, repositories, and working method behind the Seam Stack portfolio (Systems of Thought · UX Minds, LLC)
**Audience:** Mixed technical and strategic readers. Part One is written for anyone; Part Two assumes comfort with distributed systems; Part Three is a lexicon split into architecture terms and method terms.
**Scope note:** This is the full accounting of the work, including the internal method and its vocabulary. Internal material is included by design and marked where it appears.
**Pattern reference:** [Pattern Commons #00, *The Governed Crossing*](https://github.com/jediwright/local-first-series/blob/main/pattern-commons/pattern-commons-00-the-governed-crossing.md), v0.3. Where this document and PC#00 differ, PC#00 governs.
**Companion documents:** [README](README.md) (what the Seam Stack is) · [THEORY.md](THEORY.md) (why the composition ladder has its shape)

---

## How to read this document

The work described here spans several repositories, a vocabulary of its own, and a governance method that is unusual enough to need explaining. To keep all three legible, each section follows the same order: the problem, the design answer, and the closest familiar system an engineer or strategist will already know. The familiar systems are **parallels, not equivalences**. Each one has a short "where it differs" note, because the differences are usually where the design choices live.

---

# Part One — The problem and the idea

## 1. The problem: the moment data leaves home

Over the past decade, local-first software solved a hard problem well. Tools like Automerge (a CRDT library: data structures that merge concurrent edits without a central server) made it credible for a person's own device to be the authoritative copy of their data. Sync works. Offline works. Conflicts resolve.

What that work left open is the **boundary**: the moment data has to leave a person's own system and enter something they don't control. That includes an employer's HR system, a payment processor, a public social network, a regulator, or an AI agent acting on someone's behalf. At that moment, most real systems quietly fall back to the old model. A server sits in the middle, sees everything, keeps a copy, and becomes the de facto owner of the relationship.

The portfolio's unifying claim is that **these boundary events are architecturally ungoverned**. They happen constantly, they carry legal and personal consequences, and almost nothing in current infrastructure makes them explicit, authorized, checked, and provable.

## 2. The idea: design the seam, not the server

The **Seam Stack** is an architecture that treats the boundary (the "seam") as the primary design surface. The name comes from tailoring: a seam is where two pieces of fabric meet, and it's where a garment holds or fails.

The central unit is the **governed crossing**: any event where data or authority passes across a trust boundary. A crossing is governed only if it has all four of these properties:

1. **A declared scope.** What crosses is named, minimal, and explicit. What does not cross is also named.
2. **A grant.** An accountable party authorized this specific crossing.
3. **A gate.** At the moment of crossing, the system checks the *current* state of that authorization, never a cached token or a time-to-live. If anything is unclear, it refuses (fails closed): an unconfirmed revocation blocks, and silence blocks.
4. **A record.** Every check, whether it passes or blocks, produces a piece of evidence naming the grant, the party, the capability checked, the time, and the result. The record is a first-class output, not an audit log added afterward.

A crossing is valid if and only if all four properties hold at crossing time. Missing any one makes it, in PC#00's phrase, an ungoverned exposure with a name attached.

Crossings aren't only exits. They fire whenever the status of a relationship changes: joining, changing role, gaining or losing a capability, leaving, or a new intermediary entering the chain.

> **Parallel: a building's badge-access system, done properly.** A badge reader checks your access at the door, not when the badge was issued. It denies by default, and it logs every swipe, including the denied ones.
> **Where it differs:** in a building, the landlord keeps the log. In a governed crossing, the evidence lives with the parties themselves.

**What the record binds.** Two further principles, first observed in the substrate-crossing prototype (Pattern Commons #8) and stated at the pattern level in PC#00 v0.2 onward, govern how the record relates to what actually crossed:

- **A seam publishes only what its own intent record describes.** Whatever crosses is built from the exact content the gate checked, never from a separately constructed copy or a re-read taken after the check. The record's account of what crossed and the bytes that crossed are the same thing.
- **A block is not a fault.** A *block* is the gate correctly refusing an external condition (content changed after the check, a time window not yet open or already closed, a hash mismatch). It is logged, and nothing is released. A *fault* is the system breaking its own rule on data it just wrote. That is raised as an error, because logging it as an ordinary block would corrupt the very record the seam exists to produce.

Both are registered for the substrate-crossing seam. At the general-pattern level they remain proposed, with PC#00 as the gate for their general reading.

**No permission is needed to implement the pattern.** PC#00 v0.1.2 onward states this directly: any system that satisfies the four properties and conforms to the shared crossing-record shape produces a fully valid governed crossing, with or without a numbered Pattern Commons entry. Numbered entries are an evidence-gated documentation layer for cross-domain claims, not a gate on building.

> **Parallel: an open standard versus a certification program.** Anyone may implement HTTP; certification (where it exists) is a separate claim about evidence, not a license to build.

## 3. The inversion

Conventional platforms sit in the middle of relationships and accumulate them: the social graph, the employment history, the transaction record. The governed crossing inverts that arrangement. **The platform facilitates the crossing and then exits.** The relationship and its records live with the parties. Any intermediary is a relay, not an owner.

> **Parallel: a notary or an escrow agent.** They witness or hold something for the duration of a transaction, and then they step out. They don't own the house.
> **Where it differs:** the Seam Stack makes one kind of exit *technical* rather than a matter of trust. With end-to-end encryption, a relay can be structurally unable to read what it carries. Other kinds of exit are not architectural (see §5, Boundary).

## 4. Why this matters beyond engineering

The strategic argument, developed in the *Full Personhood* essay, is that AI accelerates the problem. Agents act on people's behalf across many systems, and every one of those actions is a boundary event. Without governed crossings, accountability collapses into whoever runs the server. With them, a person carries a verifiable record of what was authorized, by whom, and what actually happened, and that record doesn't depend on any one platform surviving.

---

# Part Two — The architecture

## 5. The four layers

The Seam Stack has four layers. They are not interchangeable. Given the local-first commitment, omitting any one doesn't remove the trust problem; it just moves it somewhere less visible. The claim is scoped: the architecture doesn't assert that local-first is the only approach to the seam problem, only that the seam problem has a local-first solution and that it needs all four layers.

| Layer | Question it answers | In this portfolio |
|---|---|---|
| **Substrate** | Where does the data live, who owns it, and what cryptographic guarantees does it carry? | Automerge (CRDT documents) + Keyhive (end-to-end encryption and capability-based access control) |
| **Governance** | How is meaning structured, classified, made machine-legible, and constrained, and who may act on it, in what role, with what authority? | Controlled vocabularies and schema shapes (SHACL, the crossing-record vocabulary); grants, roles, delegation, and consent records. AI agents are governed parties with their own capability lifecycle. |
| **Boundary** | What happens at the exact moment of crossing? | Gates that check live authorization, bind the exact content being released, and fail closed |
| **Evidence** | What makes the record contemporaneous, tamper-evident, and legible to a later party without the platform's help? | Signed crossing records with content hashes and timestamps, held by the parties |

**An open question about this layer.** The project's own sources describe the Governance layer in two ways. PC#00 defines it by *meaning*: how information is structured, classified, made machine-legible, and constrained (the territory of the Tiered Content Framework and the controlled vocabularies). This repository's README defines it by *authority*: who may participate, in what role, with what standing. The work uses both, so the table states both. Whether one framing is primary, or the layer is formally both, has not yet been decided, and the next revision of either source is expected to settle it.

**Substrate.** This is not a commodity choice. The substrate's properties propagate upward. Whether a relay can read the data, whether access can be revoked, and whether data is private by default are all decided here. Keyhive, developed at the research lab Ink & Switch, adds encrypted, capability-based groups to Automerge. That produces different guarantees at the seam than a plain database would.

> **Parallel: the difference between a shared Google Drive folder and a Signal group.** Both let people collaborate, but only one makes it structurally impossible for the host to read the contents.

**Governance.** Rules are only as strong as their enforcement. The design preference is cryptographic enforcement (binding regardless of anyone's behavior) over policy enforcement (binding only if everyone behaves).

> **Parallel: capability-based security, such as UCAN or object capabilities.** Authority is a transferable, attenuable token rather than an entry on an access-control list.

> **Nearest prior art: Trust over IP and W3C Verifiable Credentials.** Layered architectures for governed trust between systems are established territory. The Trust over IP technology architecture (four layers: Trust Support, Trust Spanning, Trust Tasks, Trust Applications), the W3C Verifiable Credentials data model, and DIF Presentation Exchange together cover cryptographic substrate, governance rules, disclosure at the boundary, and credential evidence. The Seam Stack builds on that field.
> **Where it differs:** three commitments. The local CRDT client is the canonical site of state, not a carrier for server-issued credentials. Crossings are finality-arbiter-free with a fail-closed gate. And crossing records are built to be read by a later party without re-querying the originating platform.
> **Where it differs:** the Seam Stack adds the *legal* anchor. Every grant traces back to an accountable issuer, not just a valid key.

**Boundary.** The characteristic failure at this layer is the **relay**: a server that passes data between parties and quietly becomes a point of trust. The design goal is that relays facilitate and exit.

"Exit" means three different things, and the architecture delivers only one of them:

- **Content-opacity exit:** the relay cannot read the payload. End-to-end encryption makes this structural. The architecture delivers this.
- **Architectural exit:** the relay is no longer a required component of the crossing. This depends on what the domain's counterparties require.
- **Institutional exit:** the relay is no longer legally or contractually obliged to take part. This is a matter of jurisdiction and contract, not architecture.

In regulated domains (payment settlement rails, HIPAA business-associate relationships, anti-money-laundering and know-your-customer workflows), the relay's continued institutional presence is required by law or contract however opaque the payload is. These are recorded as known boundary conditions of the relay-exit principle, not problems awaiting a cryptographic fix.

> **Parallel: an open mail relay.** Early email servers forwarded anything for anyone, and the internet spent decades cleaning up the consequences. The Seam Stack treats every relay as its own governed crossing, with its own grant, gate, and record.

**Evidence.** Evidence here is a first-class output, not an audit log bolted on afterward. It is designed for an **adversarial reader**: someone who arrives later, is not cooperative, and wants to know exactly what the record proves.

**Evidence is not the same as the substrate's own log.** An append-only log in the substrate is a platform-side record of state changes. The Evidence layer is about who holds the legally meaningful record after the seam closes: the party, under their own cryptographic anchor, retrievable without the platform. Where the substrate is end-to-end encrypted, the platform can't produce that evidence from its logs at all, because it holds state it cannot read. That custody distinction is the point.

> **Parallel: Certificate Transparency.** CT makes every web certificate issuance publicly logged, so that mis-issuance can be detected after the fact.
> **Where it differs:** CT relies on central public logs. Seam Stack evidence lives on the parties' own systems and is verifiable without asking anyone's permission.

## 6. How a crossing actually runs

The concrete flow, as built in the employment-seam and substrate-crossing prototypes:

1. **Grant.** An accountable party issues authority for a specific crossing.
2. **Intent record.** *Before* anything leaves, the system writes a `crossing-intent` record. It names who is authorizing, the target identity, a hash of the exact content authorized to cross, and a deadline. This record must exist before the publish call can fire, and the code enforces that.
3. **Gate.** At the moment of the act, the gate re-checks live authorization, re-verifies that the content about to be published matches the authorized hash, and checks time windows. Any mismatch **blocks**: nothing is published, and the block itself is logged.
4. **Crossing.** The data is published (for example, to Bluesky via AT Protocol).
5. **Completion record.** Afterward, a `crossing-completion` record captures the published item's address and content hash and links back to the intent record.

If an intent has no matching completion, that absence is itself evidence of a failed or abandoned crossing.

> **Parallel: two-phase commit, or a TLS handshake.** Both require agreement to be established before data flows, and both have a defined shape for failure.
> **Where it differs:** there is no coordinator. The design is deliberately *finality-arbiter-free*: no central party has to confirm anything for the record to be valid. That matters in local-first systems, where no single server exists to play referee.

> **Parallel: Git's content addressing.** A commit hash commits to exact content, and changing one byte changes the hash. The crossing record's content digest works the same way: the authorization covers *this* content and nothing else.

## 7. Honesty about what can be proven

Much of the architecture is built around one discipline: **never claim more control than the system actually enforces.** (Internally this is principle P9, "exposure claims are upper bounds.")

Once data is published to a public, relay-distributed network, it cannot be architecturally recalled. Revoking access afterward is a *request*, not an enforcement. So every crossing record declares how strong a claim it is making, on an ordered scale:

| Claim level | What the record honestly says |
|---|---|
| `exposure-unbounded` | Control ended at this crossing. After this point, no upper bound on copies can be asserted (for example, publishing to a public network). |
| `exposure-upper-bound` | This is what was checked and declared. No claim is made that no copy escaped. This is the normal local-first condition. |
| `confirmation` | A human reviewer confirmed this at review time. |
| `attestation` | A named reviewer explicitly attested that a named criterion was met. |

Overclaiming is treated as invalid. Underclaiming is also treated as an error, because the claim should match reality in both directions.

> **Parallel: financial reporting's distinction between audited, reviewed, and unaudited statements.** The label tells the reader how much weight the numbers can bear.

## 8. How records compose: the seven-level ladder

Records don't exist alone. The architecture describes how they build up, in seven levels:

1. **Invariants.** Cryptographic rules (signature schemes, canonical encoding, timestamps) and vocabulary rules (versioned namespaces, controlled terms). These constrain what can be said and proven, but say nothing about any particular crossing.
2. **Fields** compose into **separately signed units** (one party's signed record).
3. Signed units compose into **bonded multi-party structures**: records from different parties that the schema *requires* to agree.
4. Those structures organize into **purpose-bound containers**: a set of records scoped to one relationship or purpose.
5. The whole assembles into a **delivered bundle**: a standalone artifact anyone can verify.
6. Bundles link into **networks** under shared governance.
7. The patterns governing all of this organize into a **corpus**: the Pattern Commons.

Two design findings matter most.

**Tamper-evidence comes from required agreement, not from any single signature.** When two parties each sign a disclosure that must reference the same content hash, any divergence between them is itself the evidence of tampering. This is the target design. In the current version (v0), the schema enforces field presence, vocabulary, identifier format, and timestamp typing; multi-party bonding, chain integrity, timestamp ordering, and supersession are still behavior-dependent, with cryptographic enforcement on the development horizon (see §12).

> **Parallel: double-entry bookkeeping.** Every transaction appears twice and the books must balance. An imbalance doesn't tell you who is lying, but it proves that something is wrong.

**There is deliberately no built-in dispute resolution.** This is a design choice, not an impossibility: the architecture takes the recording role, not the adjudicating role. Disagreements are recorded as data (a contested status, preserved divergent accounts) and exported to whatever outside institution the parties can reach, such as a court, an arbitrator, or a regulator.

The choice is cleanest where a domain's crossings can be fully specified in advance, as in the employment seam's entry and exit events: the triggers, the signing stages, and the terms are all defined when the arrangement is created. Three kinds of crossing fall outside that clean scope and may still need adjudication in their own domains:

- **Timestamp-ordering conflicts.** The event is pre-specified, but if two parties record conflicting times, deciding which came first depends on behavior until independent timestamp anchoring exists.
- **Emergent-content crossings.** The terms aren't fully known when the arrangement starts (for example, reporting obligations that arise mid-procedure in clinical care).
- **Procedural-fact crossings.** Required facts can't be known before the crossing begins (for example, biometric checks that return a confidence level rather than a yes or no).

> **Parallel: a well-kept contract file.** It doesn't settle the dispute, but it is exactly what the judge will want to read.

**Two axes.** Records compose upward along one axis (fields → units → bundles → networks). Specifications govern along a second axis (a Pattern Commons entry governs a class of bundles). The two meet but do not merge. A person's full set of records is an accumulation, not a single governed whole, and that is recorded as a known limit rather than papered over.

## 9. The repositories: where each idea lives in code

| Repository | What it is | Stack | Layer(s) it exercises |
|---|---|---|---|
| `jediwright/employment-seam` | Reference implementation. Governed publishing from a Keyhive-protected Automerge document to Bluesky, with intent and completion records and revocation as a first-class event. | TypeScript · `@automerge/automerge-repo-keyhive` · `@atproto/api` · Vitest | All four |
| `jediwright/local-first-social-native` | The same pattern as a native device app. Grant, revoke, and cold-key recovery have been demonstrated on device. | Rust core · SwiftUI (iOS) · Kotlin/Compose (Android) · Automerge + Keyhive · AT Protocol for identity | Substrate, Governance, Boundary |
| `jediwright/selvage` | A small language for writing seam rules. A parser for seam grammars. | TypeScript, zero dependencies, recursive-descent parser | Governance (expressing rules) |
| `jediwright/tcf-runtime` | Makes the Tiered Content Framework's trust rules executable as write-time gates. | Python · pySHACL · rdflib (RDF/SHACL validation) | Boundary and Evidence for content |
| `jediwright/governedcrossing` | A neutral, public lexicon for crossing records on AT Protocol, under the `org.governedcrossing` namespace. The access-change design is posted as a non-normative draft for community feedback (open until 26 Oct 2026); normative access-change text waits on UFO Lexicon v2.7 registration. | AT Protocol Lexicon (schema) | Evidence (interoperable record format) |
| `jediwright/seam-stack` | The architecture itself: README, theory, and crossing-record vocabulary | Markdown · RDF vocabulary | All (specification) |
| `jediwright/local-first-series` | The Pattern Commons: specifications for each class of crossing | Markdown | Specification axis |
| `jediwright/governed-pr-framework` | Governance for code changes: how changes to this architecture are proposed and merged | Markdown | Governance (applied to development) |

> **Parallel for `tcf-runtime`: Kubernetes admission controllers.** They validate a write against policy *before* it is persisted, and they reject writes that don't conform. The TCF gate does the same thing for content and its trust labels.

> **Parallel for `governedcrossing`: how schema.org or ActivityStreams gave the web a shared vocabulary.** A neutral namespace lets different apps read each other's crossing records without depending on any one project.

## 10. Pattern Commons: one pattern, many domains

The **Pattern Commons** is a numbered series of specifications. **#00, the Governed Crossing,** is the root entry: it names the pattern the domain entries had been demonstrating without naming it. The domain entries are its instances:

| Entry | Domain | What triggers the seam | Status and stakes |
|---|---|---|---|
| #1 — Checkout seam | Commerce | Transaction start and completion | Financial; one seam per transaction |
| #2 — High-stakes seam | Healthcare | Patient intake and record handoff | Clinical and regulatory; one seam per intake |
| #3 — Profile map as local CRM | Social | Connection formation | Relational; one seam per connection |
| #4 — attachArrayObserver | Infrastructure | CRDT state synchronization | Supporting machinery, not an instance of the pattern (no grant event of its own) |
| #5 — Distributed seam | Social networking | Peer handshake; the relay exits after sync | The "relay exits" posture demonstrated |
| #6 — CRDT as trust graph | Social | Trust assignment at connection | Trust held as local-first state |
| #7 — Employment seam | Labor / legal | Any state change in a working relationship | The most demanding instance: all four layers required, full participant model, deepest failure taxonomy |
| #8 — Substrate-crossing seam | Public networks | Publishing from a private local-first store to a globally indexed network | Prototype-verified: Phases 0–3 complete, eight governed runs against a live AT Protocol server |
| #9 — Governed content production crossing | Content production | Content crosses to a public surface at a declared compliance threshold | Design intent, no prototype; the gate checks the content's own governed state, not only who is crossing |

The employment seam (#7) is where the pattern became fully visible. It is the proof that the class exists and that the architecture can handle the hardest case, not the definition of the class.

**How far the claim reaches.** The series does not claim governed crossings are universal. It claims the pattern applies wherever the structural condition holds: contextual knowledge at a boundary, legal or relational stakes at a change of state, and no current architecture governing the event. The generality claim is bounded to the two kinds of substrate actually built against: a local-first substrate, and (through #8) a globally indexed public network. A third kind is unclaimed.

> **Parallel: design patterns (Gang of Four) or IETF RFCs.** These are named, reusable solutions with defined scope, known limits, and a numbered home. You can implement one and then point to what you implemented.

## 11. Application notes: the pattern in new settings

PC#00 v0.3 added two **application notes**. Each applies the pattern to a kind of system the domain entries did not cover, without adding a new property, layer, trigger, or vocabulary. Both began as candidates for their own entries and narrowed, under review, *toward* the existing crossing-record vocabulary. That narrowing is why they are applications of the pattern rather than siblings to it.

Both notes put the same **four seam questions** to a system. These are the Seam Stack's four layers asked from the point of view of someone who wasn't there:

1. **State:** where does the shared state live, and who owns it?
2. **Standing:** who was allowed to act on it, and was that permission current when they acted?
3. **Crossing:** what exactly moved from one party's hands into the shared work, and on what basis?
4. **Evidence:** what survives that someone who wasn't there could check?

**Note A: shared workspaces written by teams of agents.** This covers any workspace where several non-human actors write to state that someone will later depend on: a shared repository, a results pool, a task queue. The 2026 cases the note reads (an agent-team compiler build on plain git, a public math-results arena, a plug-in agent harness) show coordination working without an orchestrator or a permission model, but with weak answers to the four questions. In the compiler build, for example, all sixteen agents committed under one author string, so no later misbehavior can be traced to an individual. The note proposes eight principles:

- **GC-0.** Govern the crossings, not the keystrokes.
- **GC-1.** Shared state has one declared home.
- **GC-2.** Every actor is a party with standing, checked when it acts.
- **GC-3.** Claiming a task is a crossing and produces a record.
- **GC-4.** Verification is a claim with a basis ("these checks ran; these were known but not run; these were unknown"), never a summary impression like "tests pass."
- **GC-5.** Borrowed ideas carry lineage.
- **GC-6.** The record outlives the platform.
- **GC-7.** Governance itself is a protected surface.

They are adoptable as a **ladder** in order of cost. The first two rungs (a protected-surface list, and verdicts that record their basis and a `not_run` state) work on a plain git repository today. Rung 3 is where the employment seam's agent-party machinery transposes. Rungs 4 and 5 need the crossing-record vocabulary.

> **Parallel: branch protection and CODEOWNERS on GitHub.** They mark some files as needing extra authority to change. GC-7 goes further: the rules themselves are the most protected surface, and no agent may change them at its own discretion.

**Note B: convergent shared state.** This covers systems where shared state merges automatically with no final referee, several parties write to it, and someone later needs the merged result to be *correct*, not just *consistent*. The motivating case is Ink & Switch's Livelymerge, a live programming environment that keeps its whole program as Automerge data. Its own lab notes report two clients concurrently editing a linked list: Automerge merged both edits exactly as promised, and the list came out corrupted. The merge replayed effects, not intents, and the program's invariants were never written anywhere the CRDT could see. The note calls this the **invariant gap**.

Asked the four questions, the failure sits precisely at **Crossing**: one client's writes were computed against a state that no longer existed after the merge, and the *basis* of those writes was never recorded. That places the problem at the Boundary layer, not only the Substrate layer. The note finds that refusing a bad merge is already built elsewhere (for example, in the Coln system and in constructions such as ECROs and the Kleppmann move operation). What no reference system carries, as read, is:

- **L1, repair standing:** who is entitled to repair a refused merge, and under what grant.
- **L2, refusal evidence:** a persisted record of a refusal that names which invariants were checked, which failed, and which never ran.
- **L3, a deferred-party record:** a record of the verdict itself that is readable without the store, not just state from which someone with a checker could re-derive it.

The pattern's contribution is the Standing and Evidence half of the invariant gap. The two notes cross-reference each other: L1 is GC-2 with the repairing party named, L2 is GC-4 applied per merge, and L3 is GC-6.

> **Parallel: database constraints versus replication.** A replicated database can faithfully copy a row that violates a business rule the schema never encoded. Replication is working; the rule was simply never written where the machinery could check it.

## 12. Known limits

The pattern records its own limits rather than papering over them. From PC#00 v0.3:

- **Revocation doesn't reach backward.** Revoking a grant does not invalidate a crossing that happened before the revocation. This is a structural limit, named in the record's claim-strength vocabulary.
- **Multi-party governance at scale is only begun.** The first built machinery for governance beyond the individual party (records for proposing, objecting to, consenting to, and resolving amendments to a seam's terms) is the start of a design, not a solution.
- **Witness and signed-timestamp anchoring are locked.** The vocabulary defines `witness-signed` and `timestamp-signed` anchors, but they are unavailable until the supporting infrastructure exists. Author-declared anchoring is the current default.
- **There is no general failure taxonomy.** Each domain entry carries its own failure states. A general taxonomy has not been attempted.
- **Records don't yet compose across domains.** A healthcare seam and a housing seam both produce valid crossings, but their schemas don't automatically compose. Cross-domain composition under a shared identity anchor is the next unspecified tier.

Further limits are recorded in this repository's README and `THEORY.md`:

- **Current enforcement is schema-level.** In v0, the schema checks field presence and cardinality, vocabulary conformance, identifier format, and timestamp typing. Timestamp ordering, chain integrity, multi-party bonding, decay tracking, and supersession are behavior-dependent. Cryptographic enforcement of cross-party integrity is a development goal, not a current property.
- **Timestamp precedence is a commitment, not yet a delivered property.** "The record's timestamps establish what happened first" holds as a design commitment. It becomes a delivered property when witness-signed or timestamp-signed anchoring is available.
- **Composition-level checks are a goal.** The base schema enforces fields. Checks at the boundaries between ladder levels need instance-specific shapes that aren't produced yet.
- **Some crossings sit outside the no-adjudication scope.** See the three kinds in §8.
- **Relay institutional exit is not architectural.** See §5, Boundary.
- **A person's full record set is an accumulation, not a single governed whole** (§8).

## 13. The working method: governance applied to the work itself

The same principles that govern data crossings also govern how the code and specs get built. That symmetry is intentional. A project that argues for explicit authority, verification before assertion, and durable evidence should be built that way too.

In practice, that means:

- **Independent adversarial review.** Important specifications go to a separate AI session that has no shared memory or context. It acts as a critic, and the producer has to reconcile each objection. Exit conditions are fixed before the review starts, so the goalposts can't move.
- **Verify before asserting.** Claims about external systems (a library's behavior, a protocol's guarantees) need primary-source verification before they enter a governed document.
- **Hash-verified files.** Every governing document is identified by its content hash before it is edited, and diffs prove that only the intended lines changed.
- **Pre-registered success criteria.** Build sessions declare what would count as failure before the build runs.
- **Append-only decision log.** Verdict-bearing decisions go into a numbered, append-only ledger. Corrections are new entries that point back to the old ones; history is never rewritten.
- **Two registers.** Precise internal vocabulary for working sessions, and plain language for anything public, with the boundary between them explicitly marked.

> **Parallel: aviation's safety culture, or reproducible-research practice.** Checklists, pre-registration, independent review, and blameless logs exist because memory and good intentions fail under load.
> **Where it differs:** the reviewers here are separate AI sessions, deliberately isolated from the producer's context, which makes the independence of the review auditable.

---

# Part Three — Lexicon

Terms are grouped into two sets. **Part A** is the architecture vocabulary used in the specifications and repositories. **Part B** is the method vocabulary: how the work itself is run. Part B is marked **INTERNAL** because in the project's own practice those terms stay in working records rather than public copy; they are included here because this document is the full accounting. Where a term's status is still provisional within the project's own governance, it is marked *(proposed)*. Definitions follow UFO Lexicon v2.6, the current governing version, and PC#00 v0.3.

## Part A — Architecture terms (public)

**Access change** *(pending registration)* — A planned crossing-record type for changes to who can read something (for example, a space becoming readable by anyone). Its values (`access-change` as a record type; `access-change` and `access-change-observed` as events) and supporting fields are designed and in public feedback, but they do not conform until UFO Lexicon v2.7 registers them.

**Adversarial-grade evidence** — Evidence designed to hold up for a hostile reader who arrives later and isn't cooperating. It serves a friendly auditor too, but the reverse isn't true. *Closest familiar idea: evidence prepared for litigation rather than for an internal review.*

**Agent (as a governed party)** — An AI or other non-human actor treated as a party with its own grants, checks, and revocation, never as a tool that acts invisibly under someone else's authority. Agents are never the "author of record" for a decision. *Closest familiar idea: a contractor with a scoped work order, as opposed to an employee's borrowed login.*

**Application note** — PC#00 applied to a kind of system the domain entries did not cover. It adds no property, layer, trigger, or vocabulary of its own. PC#00 v0.3 has two: shared workspaces written by teams of agents (A), and convergent shared state (B).

**Assembly document** — The single document a crossing is built from. Content from one or more authorized sources is assembled in a fixed order, and the authorization hash covers the assembled result. A document is never partly authorized; content with different authorizations belongs in separate documents.

**AT Protocol** — The open protocol behind Bluesky: portable identity (DIDs), signed personal data repositories, and schemas called Lexicons. It is used here for identity and as the public side of a crossing.

**Automerge** — An open-source CRDT library. Documents merge concurrent edits from many devices without a central server.

**Block vs. fault** — A *block* is the gate correctly refusing an external condition (expired authorization, content mismatch, a time window not yet open). It is logged, and nothing is released. A *fault* is the system breaking its own rules on data it just wrote. That throws an error; it is not logged as a normal outcome. *Closest familiar idea: an HTTP 403 versus a 500.*

**Boundary (layer)** — The third Seam Stack layer: the designed moment where a system meets something it does not control.

**Boundary principles** *(proposed at the general level)* — The numbered rules the pattern expresses, set out in full in Artifact B. The ones PC#00 names are: **P8** every crossing is explicit, minimal, and designed; **P9** exposure claims are upper bounds; **P11** agents are governed parties, never authors of record; **P14** relay boundaries are governed crossings.

**Bonded structure** — Records signed separately by different parties that the schema requires to agree (for example, on the same content hash). Divergence is itself evidence. *Closest familiar idea: double-entry bookkeeping.*

**Bound type (`boundType`)** — The field on every crossing record that declares how strong a claim it makes: `exposure-unbounded`, `exposure-upper-bound`, `confirmation`, or `attestation` (see §7).

**CID / content digest** — A content address: a hash that uniquely identifies exact bytes. An authorization bound to a digest covers only that content.

**Capability (capability-based access)** — Authority represented as a grantable, revocable, delegable token or group membership, rather than as an entry on a permissions list.

**Completion record (`crossing-completion`)** — The record written after a successful crossing. It captures where the data went and its hash, and it links back to the intent record.

**Composition rules (CR-1 to CR-5)** — The rules for when a chain of seams (a relay holding one crossing while bridging to another) forms a valid composed crossing. Each seam in the chain keeps its own grant, gate, and record.

**Conformance vs. canon** — *Conformance* means a system satisfies the four properties and the crossing-record base shape; it needs no one's permission. *Canon* means a numbered Pattern Commons entry, which requires prototype evidence and review because it supports cross-domain claims.

**Crossing record (`seam:CrossingRecord`)** — The shared base shape every governed-event record extends (gate checks, lineage, AI provenance, code-change verification, multi-party governance), so records of different types can be chained and read uniformly. Four required field groups: *identity* (record ID, type, emission time, emitting party's DID); *provenance linkage* (epistemic status and its basis, supersession); *lineage anchoring* (chain reference, depth, anchor type); and *evidence scope* (event class, claim strength, optional evidence-decay date). Namespace: `https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1#`.

**CRDT** — Conflict-free replicated data type. A data structure that lets independent copies merge without conflicts or a coordinating server.

**Deferred party** — Anyone who arrives after a crossing and needs to know what happened: an auditor, a court, a future employer, the person themselves years later. The Evidence layer is designed for this reader.

**DID** — Decentralized identifier. A globally unique identity that isn't issued by a single platform. It is used here to name grantors and targets.

**Evidence (layer)** — The fourth Seam Stack layer: what persists after the crossing, who holds it, and how it can be verified.

**Exposure upper bound** — The honest default claim for local-first systems: "this is what was checked and declared." It makes no promise that no copy escaped.

**Fail closed** — When authorization is uncertain, the gate refuses. Silence blocks. An unconfirmed revocation blocks.

**Finality-arbiter-free** — No field in any record requires a central party to confirm it at the time it is written. This is essential where there is no central server. Its clean scope is crossings that can be fully specified in advance (§8).

**Gate** — The check that runs at the moment of crossing, against *live* authorization state, never a cached token.

**GC-0 to GC-7 (governed coordination principles)** *(application note A)* — Eight principles for shared workspaces written by teams of agents, from "govern the crossings, not the keystrokes" to "governance itself is a protected surface" (see §11). Adoptable as a five-rung ladder in order of cost.

**Governance (layer)** — The second Seam Stack layer. PC#00 frames it as how meaning is structured, classified, made machine-legible, and constrained; the seam-stack README frames it as who may act, in what role, with what authority. Both are in use (see §5).

**Governed crossing** — A boundary event with a declared scope, a grant, a gate, and a record. Missing any one of the four makes it an ungoverned exposure.

**Grant** — The authorization for a crossing, issued by an accountable party. It is the legal-responsibility anchor.

**Horizon** — A time bound on a crossing. A *not-before* horizon means the crossing cannot fire early. A *timeout* horizon means that if the crossing hasn't completed by then, that failure is itself recorded.

**Identity custody class** — Who holds the keys to an identity: the person themselves, a provider, or a mix. It caps how strongly attribution can be claimed. A provider-held identity can't be claimed as confirmed.

**Intent record (`crossing-intent`)** — The record written *before* a crossing fires. It names the authorizer, the target, the content hash, and the deadline. The publish call cannot run without it.

**Inversion, the** — The core commitment: the platform facilitates the crossing and exits, and the relationship and its records live with the parties.

**Invariant gap** *(application note B; working label)* — The failure where automatic merging produces state that is consistent but wrong, because the invariants the program relied on were never written anywhere the merge machinery could check. Located at the Crossing question: the basis of a write was never recorded.

**Invariants** — The base level of the composition ladder: the cryptographic and vocabulary rules everything else must obey.

**Keyhive** — Ink & Switch's encryption and capability-based access-control layer for Automerge.

**Lexicon (AT Protocol sense)** — A schema defining a record type on AT Protocol. Not to be confused with the project's internal vocabulary document (see Part B).

**NSID** — Namespaced identifier for an AT Protocol Lexicon, written in reverse-domain form (for example, `org.governedcrossing`). Owning the domain establishes authority over the namespace.

**P13 evidence plane** — The first built machinery for multi-party governance of a seam's own terms: records for proposing an amendment, objecting, consenting, resolving, and delegating. An amendment is operative only when every party in the grant chain holds an unrevoked consent. Objections mark an amendment as contested but never block a gate.

**Pattern Commons** — The numbered series of domain specifications for governed crossings (#00, #7, #8, #9, …).

**Payload-provenance rule** — What gets published must be built from exactly the content the gate hash-checked, never from a separately constructed copy. *(Seam-scoped; its general form is proposed.)*

**Recall semantics** — A declaration, at crossing time, of what "revoke" will actually mean in the destination system. On a public network, it is a request, not an enforcement.

**Relay** — An intermediary that carries data between parties. The design goal is that it facilitates and exits. The architecture delivers *content-opacity exit* (the relay can't read the payload); *architectural* and *institutional* exit depend on the domain and jurisdiction.

**Revocation** — Withdrawing a grant. In this architecture it is a first-class, recorded event.

**Seam** — The boundary where a local-first system meets something outside its control. The primary design surface.

**Seam questions (State, Standing, Crossing, Evidence)** — The four layers asked from the viewpoint of someone who wasn't there: where does the state live, who was allowed to act and was that permission current, what moved and on what basis, and what survives that an outsider could check.

**Seam Stack** — The four-layer architecture as a whole. (Not "the substrate," which is one layer.)

**SHACL / RDF** — W3C standards for describing data as linked statements (RDF) and validating it against shapes (SHACL). Used in `tcf-runtime` for write-time gates.

**Substrate (layer)** — The first Seam Stack layer: where data lives and what cryptographic properties it carries. Used *only* for this layer.

**Substrate crossing** — A crossing between two systems with different guarantees, such as a private encrypted store and a public network.

**Tiered Content Framework (TCF)** — A content-governance framework that governs how information is structured and labeled across tiers, from individual terms to whole corpora. `tcf-runtime` makes its trust rules executable.

## Part B — Method and governance terms (INTERNAL)

These terms describe how the work is run. In the project's practice they live in session records, handoffs, and specs, and stay out of public-facing copy. They are part of the accounting because the method is itself one of the portfolio's claims (§13).

**Anti-retroactivity** — Exit conditions for a review loop are declared before the loop opens, so success can't be redefined after seeing the results.

**Artifact B / Form C** — Artifact B is the specification that sets out the full set of boundary principles. Form C is the architectural form they describe: governed boundary crossing on local-first systems.

**Confidence tags (✓ ~ ?)** — Markers on every assumption and ledger entry: ✓ confirmed by direct evidence; ~ inferred or single-context; ? open or assumed. Every ~ claim needs a named review target or a named consequence.

**Converged / narrowed** — Outcomes of a Counter-Pass. *Converged* means producer and critic agree. *Narrowed* means the claim survives but in a smaller scope than proposed.

**Counter-Pass** — An adversarial review cycle. A siloed critic session attacks a draft, and the producer reconciles each finding. There are at most three iterations; non-convergence is logged as a finding rather than forced.

**Delivery-not-application** — Sessions produce files; the operator applies them to the canonical copies on the operator's own machine. A hosted surface is never treated as the record.

**Env-3 / siloed session** — A fresh session with memory off and no project knowledge, given only the prompt and the attached inputs. The isolation tier used for critics.

**Known limit (KL-n) / open item (OI-n)** — Numbered, tracked gaps. A known limit is a recognized boundary of what the design or build demonstrates. An open item is a pending decision or task.

**Lexicon v2.7 (pending)** — The next UFO Lexicon version, in preparation; v2.6 remains current. Its scope is set as access change only: the access-change values and fields, a scoped restatement of `exposure-unbounded`, making `gateCheck` required on the gate-check type, and a one-line check of how PC#7's gate results map onto `pass` / `blocked`. Everything else queued (the `seam` / `actor` / `principal` registrations, the Selvage coinage cluster, review watch terms) moves to v2.8.

**Lightweight Mode** — An abbreviated session form for maintenance, appends, and scoped checks: a three-line open, the task, and a one-line close.

**Master Playbook** — The consolidated operating guide for the method: rules, command-line discipline, and lessons learned.

**Modes (1–4)** — Session tiers under the Session Harness, from standard governed work (Mode 1) through the higher-rigor forms: multi-context review, adversarial Counter-Pass, and staged queued runs.

**On/Off (product register)** — The product-layer vocabulary (authorization grammar, governed action, boundary primitive) used for the console and employment products. It carries its register tag whenever it appears elsewhere.

**Panel Pass** — The same question put to two or three independent contexts, with the answers reconciled. Retires a single-context stamp.

**Pending-edit rule** — A pending edit to a document is tracked only in the ledger entry or note that created it and discharged in that document's own change log. Session records are never rewritten to renumber or retarget.

**PPG (Pre-Draft Primary-Source Gate)** — Before a claim enters a draft, it must be *Verified* (primary source read), *Scoped-absence* (verified that it isn't there), or *Deferred* (explicitly held). Characterizations inherited from earlier sessions don't pass on their own.

**Pre-registered falsifier** — A statement, written before a build or test, of what result would count as failure.

**Proposed vs. registered** — Vocabulary status. A *registered* term is governed and usable as defined. A *proposed* term is in a queue with a named gate condition and doesn't govern output until promoted in a governed session.

**Queue-don't-reopen** — Locked specifications are changed only in their designated amendment session. New issues are queued, not patched in passing.

**Register (plain / contextual / governed-internal)** — The language level a piece of output must use. Required plainness rises with the audience's distance from working sessions.

**Resonance Architecture (RA)** — The companion cross-domain research program. It maps organizational structures against a pre-registered grammar and records where predictions hold, narrow, or fail. It supplied the explanation for the seven-level ladder's shape.

**Scaffold / seam distinction** — Governance apparatus is applied where a wrong decision would propagate silently (seams), not everywhere (scaffolding). Apparatus vocabulary stays out of publication copy.

**Session Harness** — The protocol for governed working sessions. It has an open block (what was loaded, assumptions, stamps), a mid-session checkpoint, and a close block (compression, handoff, ledger deltas).

**SHA-gated file identity** — Every governing file is identified by its content hash before editing. The operator's hash governs over any sandbox copy.

**SINGLE-CONTEXT — NOT PANELED (stamp)** — A mechanical marker on any output produced in one context and not yet independently reviewed. It travels with the deliverable until a Panel Pass or named review retires it.

**Survival Ledger (SL-nnnn)** — The append-only log of verdict-bearing claims, each with an ID and confidence tag. Its tail is verified by direct read before any new ID is assigned. Entries are never edited after the session that appended them; corrections are new entries that name the one they correct. IDs that were reserved and not used are retired as permanent gaps (for example, SL-0222), never back-filled.

**UFO altitude / UFO Lexicon** — "UFO altitude" is the portfolio-wide level of discussion, above any single project. The UFO Lexicon is the governing vocabulary document for that level: registered terms, proposed terms, register rules, and collision-prevention rules. The layer-only use of "substrate" is its Ruling R-1.

---

## About this document

**Review status.** This blueprint was drafted in a single working context and has not yet had an independent review. It summarizes documents that have been reviewed (PC#00, `THEORY.md`, the lexicon), and where it and those documents differ, they govern.

**What's still open.** Two things are named in the text rather than resolved: which framing of the Governance layer is primary (§5), and the access-change vocabulary, which is designed but not yet registered (Part A, *Access change*). Repository stack details reflect each repository as of this version and will drift as the builds move.

**Sources.** Pattern Commons #00 v0.3; `THEORY.md` and the README in this repository, as of September 27, 2026; UFO Lexicon v2.6.

## Changelog

**v0.3.1 (September 27, 2026).** Wording: gender-neutral language throughout.

**v0.3 (September 27, 2026).** Aligned to the current README and `THEORY.md`. Added the three kinds of relay exit and the regulated-domain boundary condition, the scoping of the four-layer claim to the local-first commitment, the custody distinction between evidence and the substrate's own log, the nearest prior art (Trust over IP, W3C Verifiable Credentials), the reframing of no-adjudication as a design choice with its three out-of-scope crossing kinds, and the v0 enforcement scope. Expanded the known limits accordingly.

**v0.2 (September 27, 2026).** Aligned to Pattern Commons #00 v0.3. Added what the record binds, conformance without permission, the full table of domain entries and the limit on the generality claim, the two application notes (§11), and the pattern's known limits (§12). Stated both framings of the Governance layer. Updated the `governedcrossing` status. Expanded the lexicon, and recorded Lexicon v2.7 as pending. The method vocabulary (Part B) is kept as part of the full accounting.

**v0.1 (September 27, 2026).** Initial draft.

---

*MIT License · Built with AI-collaborative methods · Intellectual direction and authorial responsibility: Jedi Wright · Systems of Thought · UX Minds, LLC*
