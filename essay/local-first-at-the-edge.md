## Local-First at the Edge

**Seven principles for what happens where the interior theory ends**

Local-first got the hard part right first: making software work on your own device. 

That hard work of the last decade went into making clients credible — CRDTs that converge, sync engines 
that survive partition, storage that holds when the network fails. Most recurring failure 
classes in the ecosystem since then trace to the boundary. Data copies leave without governed 
crossing records. Revocation closes future streams and cannot reach copies already crossed. 
Agents act on the data floor without accountability structures. Schema evolves, and old peers 
become illegible strangers. The seven ideals say nothing about any of this — not because they 
are incomplete, but because the boundary was never their focus.

---

## §1 — The Diagnostic Claim

Local-first has an interior theory. At the boundary (or exterior) it has point solutions — and no unifying theory.

The seven ideals Kleppmann, van Hardenberg, McGranaghan, and Wiggins published in 2019 are a complete and defensible theory of what a local-first system should be *inside*: data on your device, offline operation, real-time collaboration, long-term preservation. Every one of those ideals is a claim about interior behavior — how the system behaves when it is working correctly, in its own space.

None of them says what happens at the edge.

And yet the failures that interior improvement has not reduced are boundary failures. Data copies leave without governed crossing records. Revocation closes future streams and cannot reach copies already crossed. Agents act on the data floor, attributable at the op level, but are ungoverned as parties. Schema evolves, and old peers become illegible strangers.

The community has noticed. Adam Wiggins, reflecting on where local-first goes after the seven ideals, has pointed toward something more than the seven principles — toward governance that scales with the social and institutional complexity of real deployment. (Wiggins, "Politics and emotion at Local-First Conf," adamwiggins.com, Jul 2026: [https://adamwiggins.com/posts/politics-and-emotion-at-local-first-conf/](https://adamwiggins.com/posts/politics-and-emotion-at-local-first-conf/) — Wiggins frames the expansion as scaling local-first's data-ownership value from individual to company, nation, and society level; the governance-architecture inference is this essay's synthesis.) Martin Kleppmann, in conversation about sovereignty, reaches toward mechanisms for collective and portable control that the original ideals do not specify. (Kleppmann, "Local-first in an unstable world," keynote, Local-First Conference, Berlin, 12 Jul 2026: [https://martin.kleppmann.com/2026/07/12/local-first-conf.html](https://martin.kleppmann.com/2026/07/12/local-first-conf.html); and "Mitigating Geopolitical Risks with Local-First Software and atproto," QCon London, 17 Mar 2026: [https://martin.kleppmann.com/2026/03/17/qcon-keynote-geopolitical-risk.html](https://martin.kleppmann.com/2026/03/17/qcon-keynote-geopolitical-risk.html) — both talks argue for decentralisation and provider-portable control as strategic imperatives beyond the original seven ideals; the framing of "mechanisms for collective control" is this essay's synthesis of the documented position.)

These are gestures toward a boundary theory that does not yet exist.

The governed-crossing architecture names it explicitly: every interior discipline — whether organized around seven ideals, a different set of principles, or a set not yet written — generates a boundary shadow. The interior theory's jurisdiction ends somewhere. What happens at that end requires its own principles.

NI-2 — Boundary vs. Multi-Party Coordination: The diagnostic claim is not equivalent to "multi-party coordination failure." Multi-party coordination failures are typically addressed by improved protocols, conflict-resolution mechanisms, or consensus algorithms operating *within* a shared space. The boundary failure this architecture identifies is a different structural problem: it is the failure that occurs at the point where the interior theory's jurisdiction *ends*. The design consequence is not "coordinate better inside the system" — it is "declare and govern what happens when the interior meets the outside." A system that adds multi-party coordination mechanisms alone and leaves the boundary untheorized still has all the failures this architecture describes. The two problems are adjacent but not the same; the solutions are adjacent but not the same.

---

## §2 — Interior Discipline

Every interior theory generates a boundary shadow. This is a structural consequence, not a deficiency of any particular theory. An interior discipline that specifies what a system should be *inside* does not, by that specification, extend to what happens when content crosses to the outside. The boundary shadow is what remains ungoverned at the edge of any interior theory.

Understanding the boundary problem requires understanding what a well-formed interior discipline looks like: what it covers, where it is strong, and where it structurally stops.

The seven ideals Kleppmann, van Hardenberg, McGranaghan, and Wiggins published in 2019 are currently the best-worked example of a local-first interior discipline. They specify responsiveness, data residency across your devices, offline operation, collaborative merge, long-term preservation, security, and user control — seven claims that together are a complete account of what a local-first system should be when operating correctly in its own space. Their strength is coherence: each ideal is an interior claim; none overreaches into the boundary.

Their limit is the same limit any interior discipline faces: none of them addresses what happens when content crosses over outside. The boundary shadow exists for each ideal:

* An ideal that governs responsiveness makes no claim at the edge; its shadow is empty, and that emptiness is instructive.
* An ideal that governs data residency across your own devices cannot govern the copy that has left for a device that is not yours.  
* An ideal that governs offline operation cannot govern the deferred crossing it creates.  
* An ideal that governs collaboration among interior peers cannot govern non-human actors entering from an external context.  
* An ideal that governs long-term preservation cannot govern schema drift, commitment decay, or identity succession — because those are boundary phenomena.  
* An ideal that governs security cannot govern what happens after authorized decryption.  
* An ideal that governs individual sovereignty cannot govern the collective governance problem that arises when data is shared at scale.

These are not failures of the seven ideals. They are the predictable consequence of having a strong interior theory and no theory of the boundary — which is what other interior disciplines, including this one, faces before the boundary is theorized.

The seven ideals are one worked example of a local-first interior discipline — the most developed one currently available. The governed-crossing architecture was developed by studying this instantiation: identifying where the interior theory ceases to apply and where the boundary requires a different kind of governing claim. The architecture is the result of that study. It does not depend on the seven ideals as its organizing structure; it took the seven ideals as its subject of study.

This distinction matters for the architecture's scope and generalizability. The boundary principles in §4 have been demonstrated on two substrates — local-first replicated state (employment seam, PC\#7) and a federated substrate, AT Protocol (substrate-crossing seam, PC\#8). The principle statements carried across unchanged; the record architecture did not — §6 reports three load-bearing extensions the second substrate required that cannot be retrofitted to the first. A future local-first system organized around a different set of interior principles would produce a differently shaped set of boundary shadows, but the structural requirement — governed crossings, explicit crossing records, upper-bounded exposure claims, lineage-bearing schema changes, governed agent contacts, term-bearing longevity commitments, and collective governance composition — is expected to remain. What two substrates establish is that the boundary *requirements* recur and the *mechanisms* diverge. That is consistent with generalization; it does not establish it. Demonstration on additional substrates — in particular a second local-first substrate — remains future work. The seven ideals are the demonstration case, not the scope condition.

---

## §3 — Where the Interior Discipline Ends

The boundary shadow is not unique to local-first. It is the structural consequence of any interior discipline that specifies interior behavior without specifying what happens at the edge.

What makes the local-first case instructive is that its interior discipline is unusually well developed. The seven ideals are precise enough that the boundary shadows can be identified exactly. Each ideal's interior claim generates a specific, nameable gap at the boundary:

Data residency → ungoverned copies. The interior governs where data lives and who can access it locally. The copy that has crossed a boundary carries no governance record, no revocation semantics, no exposure bound. The interior discipline's strongest claim — that data is on your device and under your control — stops at the point the copy leaves.

Offline operation → deferred governance. The interior handles offline gracefully. A crossing that happens while the governance plane is offline defers its admissibility check to an unknown future moment against a governance state that may have changed in the interval. The gap between declaration and execution is ungoverned.

Collaborative merge → ungoverned agent contacts. The interior governs collaboration among human peers who share a replicated state. Non-human actors entering the data floor from an external context are functionally acting on the system without being governed as contacts. Any agent action taken without an accountability structure is the interior discipline's silence made concrete.

Long-term preservation → three compounding gaps. The interior commits to durability. Without governed schema lineage, that commitment means "the bytes exist" rather than "the bytes are intelligible." Without term-bearing longevity commitments, "durable" is an admissibility-uncoupled promise. Without an identity succession story, a document whose only persistent identity is a public key faces mortality when the key is lost, deprecated, or orphaned. Three distinct boundary shadows from a single interior ideal.

Security → post-decryption governance vacuum. The interior governs the channel and resting copies. End-to-end encryption terminates at authorized decryption. Everything after plaintext — what the authorized party does with it, under what obligations, with what forwarding and retention constraints — belongs to a governance plane the interior discipline does not specify.

Individual sovereignty → unclaimed copies and ungoverned collectives. The interior grounds control in data-on-device. At the boundary, revocation closes future streams; it cannot reach copies already held by external parties. At the collective scale, the governance of data that has been shared and relied upon in organizational decisions cannot be reduced to any single individual's absolute claim to control without undermining the governance of every other party's position. Two shadows: the exposure that has already happened, and the governance problem that individual sovereignty cannot solve.

Each of these gaps is a design input, not a critique. The interior discipline is not wrong; it is bounded. The governed-crossing architecture is designed to fill the gaps the interior discipline correctly and predictably leaves.

---

## §4 — Boundary Principles

These principles declare what is required at the boundary. They were derived from the study of local-first's boundary shadows — the specific gaps the seven ideals generate at their limits. They govern what the interior cannot reach.

All principles are PROPOSED pending Lexicon promotion. ⚑ SINGLE-CONTEXT — NOT PANELED.

---

### P8 — Every boundary crossing is explicit, minimal, and designed.

No ambient exfiltration. The surface at which a local-first system interacts with a server, external party, or agent is declared; its scope is the minimum required for the purpose; its failure states are enumerated; the local side loses nothing on failure. A crossing is a first-class design decision, not a side effect.

This principle addresses the ungoverned copy (data-residency shadow), the ungoverned agent contact (collaboration shadow), and the deferred governance gap (offline-operation shadow). The common thread is the same: a crossing that is not explicitly declared is ambient exfiltration under a different description.

---

### P9 — Exposure claims are upper bounds.

A system never claims more control than its architecture enforces. Revocation closes future streams; it does not erase copies already held by external parties. Exposure vocabulary is structural, not disclaimed in prose. The distance between a system's exposure claim and its architecturally enforced exposure bound is the measure of its honesty.

This principle addresses the unclaimed copy (data residency and individual sovereignty shadows) and the post-decryption governance vacuum. It also governs this architecture itself: any claim this architecture makes about what it governs is subject to P9. The architecture enforces the ladder's first two tiers; the third tier's ceiling is explicitly named.

The revocation limit itself is not this architecture's finding. Keyhive's design states it plainly: read revocation is forward-only and cannot un-share history. P9 takes that interior-side statement and makes it structural — a boundType vocabulary with an honest floor value, exposure-unbounded — so the limit is carried in the record rather than disclaimed in prose.

P9 applied reflexively: this architecture's own novelty claims are upper bounds. Where the IFC declassification literature establishes a structurally parallel ceiling (Sabelfeld & Sands 2009), this architecture's contribution is a transposition, rather than a new discovery. The un-mechanizable remainder is honestly stated as a transposition rather than a first-class novelty claim.

---

### P10 — Data shapes carry lineage and horizons.

Schema change is a governed decision. Every schema change carries a blast-radius classification, an append-only lineage record, and a policy-confirmed deprecation timeline. Old-version peers are permanent constituents of the governance context, not stragglers to be left behind. A schema version that has no governed deprecation path has made a longevity promise it may not keep.

This principle addresses the schema-drift shadow of long-term preservation. The GSEF (Governed Schema Evolution Framework) slots into the larger governed-crossing frame at P10: the GSEF's open questions about schema change governance are sub-questions of this principle's requirements.

The legibility half of this problem has an interior-side answer: Cambria (Ink & Switch, 2020) uses bidirectional lenses over an append-only lens graph so peers on different schema versions can still read each other. P10 sits above that translation layer. Lenses make an old version legible; P10 governs whether a change may ship, how far its blast radius reaches, and when an old version may be retired — the decision layer Cambria deliberately leaves to its users.

---

### P11 — Agents are governed parties, never authors of record.

Any non-human actor that touches the local-first data floor is a contact class with its own capability lifecycle, action context, and revocation semantics. Capabilities are checked at act time, not grant time. An agent granted access and later having that access revoked must not be able to act on a stale grant. An agent's actions are evidence artifacts — logged, attributable, revocation-state-aware.

This principle addresses the ungoverned-agent-contact shadow of the collaboration gap. An agent entering the data floor from an external context is not an interior collaborator; it is a governed party at a boundary, with its own contact class, capability lifecycle, and revocation semantics distinct from those of human collaborators sharing replicated state.

*The first known working example of this principle operating on a local-first data floor, on current evidence: the employment-seam prototype (github.com/jediwright/employment-seam), implementing contactClass: human | agent, AgentActionContext, and assertCapabilityCurrent() against Keyhive-managed replicated state. P11 as formalized in PC\#7 v0.5 as Principle 6: "Agents are governed parties, never authors of record." (\~, scoped absence claim: this scoping applies to local-first replicated data as the governed object, with contact-class taxonomy and act-time capability currency checks. No broader claim is made.)*

P11 and the 2026 agent-capability claims-race: The agent-capability governance space is active as of 2026\. The absence claim for P11 is scoped to the local-first data floor specifically — the first known worked example of this boundary principle implemented at the spec and prototype level on a local-first replicated substrate, on current evidence. Any broader phrasing would be falsifiable against the IBCT/AIP line and Vouchsafe (2026) work.

Nearest interior-side work, named so the absence claim is checkable: Keyhive's capability model already treats every principal — individual, group, or document — uniformly as a capability-bearing agent, and enforces currency at the access check; Patchwork (Ink & Switch) already puts agent edits on branches with attribution and review-before-merge; UCAN validates delegation chains at invocation time with expiry and revocation. What P11 adds over each is not the check but the record: every act-time capability check, pass or block, emits a governance-plane evidence record that carries the contact class and the revocation state as of the act. Keyhive's check admits or denies; P11's gate also testifies.

---

### P12 — Longevity commitments have terms.

"Supported," "maintained," and "compatible" are claims that carry evidence, dates, and decay. A system that asserts long-term preservation without specifying what it means by "long-term," what evidence backs that assertion, and what the deprecation path looks like when it ends has made an admissibility-uncoupled promise. Longevity commitments are admissibility- coupled: they can be checked against their stated terms, and they fail honestly when those terms expire.

This principle addresses the commitment-decay shadow of long-term preservation. It also applies to this architecture's own claimed stability: the architecture's longevity commitments are bounded by the evidence base and the demonstrated scope.

---

### P13 — Governance composes beyond the individual.

Boundary governance must have an account at the collective and institutional scales. Individual data sovereignty is real and architecturally groundable. At the boundary, data that has been shared, relied upon in organizational decisions, or embedded in an institutional context cannot be governed by any single person's absolute claim to control without undermining the governance of every other party's position.

Research on collective consent and the composition problem in data governance reaches this limit from the structural direction: individual consent frameworks do not compose into collective governance without additional machinery.

P13's machinery divides at the evidence/decision line. The evidence plane — amendment records, objection records, contest status derivable from the record set without a convergence mechanism — is built: four record types (SeamTermAmendmentRecord, ObjectionRecord, ConsentRecord, ResolutionRecord) implement the governance trail at the prototype level, composing with CR-1–CR-5 without modification. The bilateral case (n=2 seams) is closed: ConsentRecord derivation handles it without a decision-plane mechanism. The decision-plane equipping is partially built (D2 program, complete at the prototype scale): a standing registry (StandingRegistry), member-drawn panel rules on the existing ResolutionCapabilityRegistry infrastructure, a DelegationRecord type, a fork-lineage convention, and threshold derivation (ThresholdRule) have been implemented and tested. What remains unbuilt — n≥3 convergence machinery at non-bilateral scale and witness quorum for standing recognition at scale — is not an architectural gap this architecture must close internally; these are external functions the architecture equips by providing the evidence substrate and a capability-admissibility interface (ResolutionCapabilityRegistry) that an external resolver can satisfy.

The architecture's obligation is to make that equipping honest: the ResolutionRecord capability assignment interface is a porous point between the evidence and decision planes — named architecture, not a silent gap. An external authority satisfies the four-test admissibility line (no crossing-time liveness dependency; formation-time consent; fail-safe absence; substitutability) or no external authority is admitted.

P13 is the weakest current principle in the set — evidence plane built, decision plane partially built under equip-not-replace. It appears here as an honest statement of what the governance architecture requires and what it has built, rather than a claim of completion.

---

### P14 — Relay boundaries are governed crossings.

A relay — any infrastructure intermediary that forwards, transforms, or routes local-first data — is a governed party with acceptance conditions, not a passive delivery mechanism. The relay boundary requires: a distinct grant issued for the relay crossing (issuer class to be determined by the items-1+2 design program); its own revocation state model; and its own gate check record shape. A relay without a governance layer is an ungoverned crossing under a different name.

P14 is confirmed in-scope per the chained-crossing observation log (2026-08-08). From the test case: the relay boundary cannot be treated as an infrastructure concern separate from seam governance. The three open items from the chained-crossing test (OI-1: who issues the Seam 2 grant and what class; OI-2: linkage field between Seam 2 and Seam 1 grant records; OI-3: cross-seam revocation composition rule) are inputs to the items-1+2 design program, not resolved in this specification.

---

## §5 — Three-Tier Enforcement Ladder

The boundary principles require enforcement. This architecture specifies a three-tier enforcement ladder that moves from convention toward runtime enforcement, with an honestly stated ceiling.

Tier 1 — Convention Governance documents, crossing records, audit trails. The boundary is declared in writing; crossings are logged; the evidence is available for review and dispute resolution. This is the floor of enforcement: it requires no runtime enforcement, and it can be violated without immediate consequences. Its value is that it creates the evidence base on which stronger enforcement can act.

Tier 2 — Type-level / schema-level Governance shapes enforced at the schema level. Crossing records have required fields; schema changes require lineage fields; agent action contexts have mandatory structure. Violations are structurally impossible, not merely discouraged. Tier 2 enforcement is shipped: the employment-seam prototype implements SHACL shapes for seam:gateCheckRecord, seam:aiProvenance, and seam:AgentActionContext. This tier has been demonstrated at the prototype level.

Tier 3 — Membrane Runtime refusal: the system refuses crossings that do not satisfy admissibility conditions at act time. The employment-seam prototype's assertCapabilityCurrent() gate, checking act-time currency against Keyhive capability state, is an existence proof for one dimension of this at the data floor. The membrane tier composes Tier 2's structural enforcement with runtime revocation-state checking. The full membrane specification is unwritten; this is a Known Limit. The membrane framing is transposed from capability system design (E. Dean Tribble / Mark S. Miller membrane work) to the local-first boundary governance problem. The transposition is claimed openly: the structural parallel is genuine; the substrate (CRDT-based replicated state, governance-plane coupling) and the governance-plane requirements (terms, revocation, lineage) are novel relative to the original membrane concept.

The ceiling (P9 applied to the ladder itself): The runtime refuses what is checkable. The framework governs what is not checkable. The boundary between those two is not closed by any mechanism in this architecture, and none is claimed. No mechanism in the current architecture can enforce that a label correctly reflects the semantic content of the data it annotates — a limit consistent with enforcement constraints documented in the IFC declassification literature — specifically, the computability argument that type-based enforcement cannot satisfy semantic consistency for all programs, and the acknowledged difficulty of tracking information quantity through non-trivial loops (Sabelfeld & Sands 2009, §§3.1, 3.4); this architecture transposes this ceiling to governance frameworks over replicated state, where the ground shifts from computational undecidability to institutional economics. The transposition is non-trivial: the substrate (CRDT-style replicated state), the grounding (institutional economics vs. computability theory), and the governance-plane coupling (terms, revocation, lineage — absent from IFC) are all novel relative to the IFC result.

The ceiling is not a failure of the architecture. It is the architecture's honest account of its own limits, stated as a first-class design output.

---

## §6 — Known Limits

This architecture applies P9 to itself. Every claim in this specification is an upper bound. The following are the explicitly named limits of the current architecture.

Claim 7 — The ceiling (IFC transposition form):

The un-mechanizable remainder — the gap between a label's declared sensitivity class and the data's actual semantic content — is published as a first-class Known Limit. This ceiling is consistent with enforcement constraints documented in the IFC declassification literature — specifically, the computability argument that type-based enforcement cannot satisfy semantic consistency for all programs, and the acknowledged difficulty of tracking information quantity through non-trivial loops (Sabelfeld & Sands 2009, §§3.1, 3.4). This architecture transposes this ceiling to governance frameworks over replicated state, where the ground shifts from computational undecidability to institutional economics — labels cannot be mechanically verified to reflect semantic content under adversarial organizational pressure, incentive misalignment, and retrospective reinterpretation. The transposition is non-trivial: the substrate (CRDT-style replicated state), the grounding (institutional economics vs. computability theory), and the governance-plane coupling (terms, revocation, lineage — absent from IFC) are all novel relative to the IFC result.

*(IFC transposition, SL-0028 Outcome B. "No counterpart" framing retired. Transposition is non-trivial on three axes per IFC sweep findings.)*

Claim 4 — Runtime-refusable admissibility (composition form): Runtime-refusable admissibility exists for request authorization (Biscuit line) and for effect entitlement (ocap/Effekt line). This architecture's contribution is the coupling of refusal to governance-plane admissibility — evidence, terms, lineage, revocation state — over local-first replicated state. IFC is cited as prior art on label-based admissibility at the intra-program level; the structural gap (external obligations; replicated-state substrate) is genuine.

P13 machinery: The collective governance composition principle names the requirement and specifies the obligation: the architecture equips external governance; it does not replace it. The evidence plane is built (OI-P13-1): SeamTermAmendmentRecord, ObjectionRecord, ConsentRecord, ResolutionRecord — four record types, 38 tests, composing with CR-1–CR-5 without modification. The bilateral case is closed (ConsentRecord derivation, T7). The decision plane — convergence for n≥3, witness quorum, delegation beyond formation-time consent — remains the open frontier.

An external authority is admissible iff it satisfies all four conditions: (1) no crossing-time liveness dependency; (2) formation-time consent; (3) fail-safe absence (no admitted authority → contested, no gate blocked); (4) substitutability — a constraint on the integration, not the institution: the authority is admitted through the standard ResolutionRecord shape only, with no provider-specific schema extensions; authority identity is referenced solely via the registry's capability-slot reference, so that a successor authority can occupy the capability without record migration. An assignment whose integration-demand violates this is inadmissible. The constraint is checkable at the schema level (Tier 2).

The ResolutionCapabilityRegistry is a locally-replicated record structure within the seam's record set. Capability admissibility is checked at derivation time against locally held registry state — act-time, not grant-time, per P11's own discipline: a ResolutionRecord authored after the forum's capability was withdrawn is inadmissible. No derivation reads registry state that is not locally resident; a registry resident on any external substrate would reintroduce the state-read liveness dependency that closes chain-anchored governance.

The porous point between planes is named: ResolutionRecord capability assignment requires formation-time consent, introducing a single controlled surface where the evidence plane admits an external decision-plane function. This is named architecture.

Formation-time consent is a structural admissibility condition, not a substantive-validity guarantee. Under bilateral asymmetry, a resolution-capability assignment can be formally consented as a condition of seam formation — consent by adhesion. The architecture's claim is bounded accordingly (P9): it enforces that no capability is assigned without the recorded consent of all grant-chain parties; whether that consent was informed and uncoerced is an institutional-validity question that shifts to the resolution layer and external institutions, per the T5/T7 composition.

The residual Known Limit is: the decision-plane convergence mechanism for n≥3 is unbuilt. The four-test admissibility line and the porous point were Counter-Passed at E4 (2026-08-09) and survived under the operative availability reading with narrowing: registry residency and derivation-time enforcement made explicit, substitutability restated as an integration constraint, and the consent-by-adhesion bound named.

Identity succession (long-term preservation gap): Long-term preservation for documents whose only persistent identity is a public key faces a succession gap. The derivation set (item 5\) names this. No design solution exists in the current architecture. Admissible as a Known Limit in v0.

Cross-seam revocation propagation (chained crossing): Revocation state is currently local to a seam. A valid chained crossing requires that each seam's revocation state be independently checked (C2, C5), but the mechanism for propagating revocation across seam boundaries is not designed. The chained-crossing observation log (OI-3) carries this forward to the items-1+2 design program. Unanswerable at prototype scale.

Post-crossing revocation (offline deferred crossings): A crossing recorded while offline and executed later may be subject to revocation if the state changed in the interval. The architecture fails safe at crossing time; it has no mechanism to retroactively invalidate an offline-declared crossing if revocation state arrives later. Named gap, not a bug — this is the correct behavior statement for the current prototype.

Substrate scope: This architecture has been demonstrated on local-first systems (employment seam, PC\#7) and across a substrate boundary into AT Protocol, a federated substrate (substrate-crossing seam, PC\#8). The boundary principle statements (P8–P14) are substrate-agnostic; their record architectures are not; demonstration on additional substrates remains future work. NI-5 CLOSED (SL-0128, 2026-08-19): two-substrate demonstration confirmed. The second substrate required three load-bearing architectural extensions: (1) a symmetric ordering rule restructuring for the boundType vocabulary — from "overclaiming forbidden" to "both overclaiming and underclaiming are substitution errors" (CP-F1/CP-F3) — which introduced exposure-unbounded as the honest floor value for post-crossing exposure; (2) a crossingType conditional grant group requiring explicit declaration of epistemic-regime change before the crossing fires; (3) a two-record intent/completion pattern whose load-bearing element is the redesign of the intent record from outcome artifact to pre-act authorization anchor — a change driven by irreversibility, not merely by confirmation opacity. The non-retrofittable unit is the architectural cluster: pre-act authorization anchor design \+ exposure-unbounded boundType \+ irreversibility invariant. The two-record structure alone could travel to PC\#7 with appropriate vocabulary; the cluster cannot do so without a P9 violation. Counter-Pass complete (SL-0132, 2026-08-20); gate CLEARED.

A contemporaneous independent project (habitable, ChelseaKR/habitable, 2026\) applies structurally equivalent upper-bound and coordinator-free constraints to habitability evidence records in a tenant-union context — RFC 3161 timestamps as upper bounds on content existence (P9 structural parallel); no central authority over a union's records, keys, or revocation (P11 structural parallel) — providing independent evidence that the pattern class generalizes beyond the local-first substrate without constituting a demonstration of it. (\~ reconnaissance read, 2026-08-16; not a citation source; project is alpha, pre-pilot, and unvalidated by legal review.)

Membrane spec: The Tier 3 enforcement tier is specified but not fully designed. The membrane specification is a derivation-set item (item 7), not a completed component of this architecture.

Item 3 — Standing and multi-party governance: Who can declare the terms of a seam; what objection does; delegation beyond formation-time consent; convergence without a finality arbiter. The evidence plane for these questions is built (OI-P13-1): amendment proposals, objections, consent, and resolutions are record types in the architecture. What an objection *does* — its formal effect — is answered: it creates a record in the evidence set and shifts the derived AmendmentStatus to contested; it is not a veto and does not block a gate.

The D2 design session (Ostrom transposition, 2026-08-09) split what this item had previously been paired as: "Ostrom/DAO-class convergence machinery." DAO-class machinery — chain-anchored, global-ordering — is closed: it reintroduces a state-read liveness dependency that the finality-arbiter-free constraint excludes, and local replication of chain data does not cure it — canonicity is a liveness-dependent property of the network, not of the replica. A mechanism reduced to a formation-time-pinned finalized snapshot exits the global-ordering class and is adjudicated as ordinary record-set machinery. Ostrom-class machinery is formation-time institutional constitution plus local monitoring plus consented local resolution arenas, and it lands on the admissible side of the formation-time/contest-time line.

Five designed directions are in the program: formation-time standing constitution (requiring no quorum for recognition); counting over formation-time-constituted closed sets; member-drawn resolution panels via existing capability machinery; nested-seam decomposition with cross-sub-seam meet; and revocable delegation of consent-authoring capability. The T11 exit valve (fork-and-let-both-live) applies where a party can substitute its counterparty at tolerable cost; lock-in degeneracy holds at any party count. The employment-seam prototype — locked-in on the worker–employer axis — is outside T11's reach. Standing recognition reframes as a formation-time question (who is constituted as a party, not who a quorum attests to at contest time) and requires no quorum. Witness-quorum attestation — a lineage-anchoring question distinct from standing recognition — remains separately locked under Q6.

The adhesion bound is general to formation-time consent under lock-in, not particular to a surface: formation-time consent to capability assignment is satisfiable by adhesion; the same is true of delegation — a party with structural power may require delegation of consent-authoring capability to a designated steward as a condition of seam formation — and of any other formation-time-declared structure, counting thresholds included. All such surfaces carry the same posture (P9). The architecture cannot detect or prevent any form of coercion across these surfaces. It carries the evidence, structurally enforces revocability, and relocates claims of coercion to the institutional layer (T5/F2).

The residual is the set of seams where no formation-time constitution occurred and parties reach a contested amendment without a resolution path. This residual is a formation-time-omission floor: it shrinks as the design program provides machinery parties can elect at formation time, but it does not close — the finality-arbiter-free constraint makes it structurally unclosable. The architecture's posture for that case is the one it already takes: derive contested, hold status quo ante, carry the evidence, and relocate the contest to whatever institution the parties can reach. The remaining design program shrinks the set of seams that ever land in the residual; it does not claim to close it. Item 3 remains admissible as a Known Limit in v0; its disposition is now a designed direction with a named floor.

---

## §7 — NI-2: Boundary vs. Multi-Party Coordination

The diagnostic claim in §1 is not equivalent to the claim that local-first suffers from multi-party coordination failures. The distinction matters because it generates different design responses.

Multi-party coordination failure occurs *within* a shared context: parties who are, in principle, operating under the same system fail to reach agreement, handle conflicts, or maintain consistency. The canonical response is better coordination mechanisms—conflict-resolution protocols, consensus algorithms, and synchronization primitives. CRDTs themselves are a response to multi-party coordination failure. At maximum coordination strength, Byzantine fault-tolerant (BFT) protocols extend this further: BFT consensus tolerates adversarial and non-compliant participants, provided those participants remain enrolled in the protocol interaction — reachable, participating, subject to the consensus round.

Boundary failure is a failure that occurs at the point where the interior theory's *jurisdiction ends* — where the system's assumptions about shared state, common protocol, and governed behavior no longer hold because one of the parties is now external. The operative criterion is enrollability: coordination mechanisms — including BFT-class mechanisms — presuppose an enrollable counterpart within a shared protocol interaction. Boundary failure involves non-enrollable exteriors: parties and data that have exited the interaction entirely. The canonical response is not "coordinate better" but "declare and govern the crossing." A system that adds multi-party coordination mechanisms but leaves the crossing untheorized still has all of the failures this architecture describes, because those failures occur after the interior theory's reach ends.

§1's four failure classes — ungoverned copies on uncontrolled machines, unreachable post-crossing revocation, unaccountable agent action after an exterior handoff, schema-illegible data in the hands of departed recipients — all involve non-enrollable exteriors by definition. Their incidence is, by that definition, invariant to pure coordination strength: strengthening the coordination mechanism, up to and including BFT-class consensus, cannot reach a party that is not enrolled in the interaction. This invariant does not extend to the governed-crossing architecture itself — crossing records and capability registries are the enrollment mechanism, and the architecture reduces these failures precisely because it addresses enrollability, not because it adds coordination strength. The claim is narrower: adding coordination strength alone, without the governing layer, cannot reduce §1's failure classes.

Where an exterior party is willing to enroll — a counterpart organization agreeing to a crossing protocol, a federation peer accepting a shared schema — the governed-crossing architecture handles that enrollment through the crossing record and capability registry machinery. The boundary/coordination distinction survives in the non-enrollable complement: ungoverned copies already in transit, peers gone silent, departing recipients with no participation incentive. There, coordination is not merely weaker but structurally inapplicable — there is no protocol instance to enroll in.

There are mechanism classes that do reach a copy after it has left: remote attestation and trusted execution environments enroll the receiving device after the fact; sticky information-flow labels travel with the data and are enforced by whatever trusted runtime next touches it; time-bounded cryptography makes data unreadable on schedule without anyone's participation. This architecture declines all three for the same reason — each depends on the exterior cooperating: a trusted runtime on a machine the interior does not control, or a copy that was never decrypted. The non-enrollable case is defined by the absence of exactly that cooperation. Where such a runtime exists, these mechanisms are complementary, and a crossing record can require them; where it does not, the honest claim is P9's.

The two failure modes are adjacent. A boundary crossing often involves parties who are also in coordination. But the design response to boundary failure is structurally different: it requires naming the boundary as a first-class object, declaring which governance applies there, and specifying what happens when the governance conditions are not met — rather than improving the coordination mechanisms that operate on either side of the boundary.

This distinction is the load-bearing claim of §1. It has survived adversarial review — Panel Pass origin (SL-0028) and lighter confirmation pass (SL-0095, 2026-08-16) — and, in the pre-publication Counter-Pass on this essay (2026-08-20), as one of five claims put to an expert reader of the local-first literature, where it held with the amendments now applied — with one standing qualification: the full claim, as now stated with the enrollability criterion and its invariance consequence, has not yet been tested as a standalone proposition in a dedicated Counter-Pass. That test remains available.

---

## Closing

The governed-crossing architecture is not a critique of local-first. It is what happens when you take local-first's interior theory seriously enough to ask what it requires at its limits.

Every strong interior discipline generates a boundary shadow. The shadow is not a failure; it is the predictable consequence of a theory that knows what it is about. The seven ideals know what they are about. The boundary is where their account ends.

This architecture starts where the interior theory ends. The boundary principles (P8–P14) were derived by identifying the specific gaps the interior discipline generates at its limits. The enforcement ladder represents the current state of what has been built against those principles — convention, then structure, then runtime, with an honest ceiling. The Known Limits section is a first-class output, not a footnote.

The employment-seam prototype (github.com/jediwright/employment-seam) demonstrates P11 at the data floor — the first known worked example of an agent-as-governed-party principle, with contact-class taxonomy and act-time capability currency, operating on local-first replicated state at the spec and prototype level, on current evidence. Other boundary principles have interior-side precedents — Keyhive states P9's revocation limit, Cambria answers P10's legibility problem — and those are named where they apply. The rest of the principles are specified, some partially designed, some openly undesigned. That is the honest state of the work.

This architecture was developed on local-first because local-first provided the best-worked interior discipline available to study. The boundary problem it names has recurred on a second, federated substrate; the governing architecture had to change shape to meet it. That is what the evidence base supports: the requirements generalize, the mechanisms are substrate-specific, and the claim stops there. Local-first started something. The boundary is where that something goes next.

---

Developed with AI assistance; human authorial responsibility and intellectual direction held by Jedi Wright / Systems of Thought / UX Minds, LLC. The principles in this essay passed a governed adversarial review process before publication. Counter-Pass 2026-08-20, reconciliation SL-0138; amendments A1–A5 applied. \[CC license\]

---

*2026-08-20*  
