# 05 · Running a Framework's Trust Rules for the First Time

*What Phase 0 of the Tiered Content Framework (TCF) runtime found*

*2026-09-11*

---

The Tiered Content Framework has existed as a governance document since the spring. It says how content should be structured across seven tiers, and (since v1.6) it says that every piece of content should declare how much it can be trusted: confirmed, inferred, unverified, or time-sensitive. It says the label is declared at the smallest unit, inherited upward, and that a composite is never more certain than its weakest member. A rule set, in prose.

Prose has a property that code doesn't: it can be internally inconsistent, and nobody notices until someone tries to run it. In August, a companion spec was written for a runtime — the machinery that would enforce the TCF's rules at write time. That spec was single-context and unpaneled. In September, I decided to build a small working version before opening the next framework revision, reasoning that the build would surface what the revision needed to rule on. This entry reports what the build surfaced.

---

## What was built

The scope was deliberately narrow: the write-time gate only, on fixtures, with the validation engine pinned, and every refusal path reached by a real run. Not a crossing gate. Not a real storage backend. Not a shape library. One engine, one language, one fixture set, one machine.

The gate does eight things in order. It loads a content-addressed library of constraint shapes through a store adapter; refuses the store outright if its digest doesn't match its contents, before any shape runs; validates the incoming write against the record shapes; enforces that a status can't be upgraded without a verification record and that a derived claim can't exceed its input's status; recomputes composite status bottom-up through the tiers; flags ancestors that are now stale without rejecting the write; records non-blocking violations; and commits the write set, the computed fields, and a per-node validation report as one change.

Twenty fixtures run against it. Nineteen produced the outcome the build plan predicted. One did not, and that one was left unmodified and logged. All six refusal codes the spec defines were reached by at least one fixture, on the operator's machine, with the engine version and lockfile recorded. The build met its own bar for "functional": every refusal path reached by a real run.

---

## What the spec got wrong

The build's more useful output was a list of places where the runtime spec did not execute as written. Six were logged. Three matter.

**The spec used a query form the standard forbids.** The rank comparison that lets the gate decide whether a composite's status is stale was written with an inline `VALUES` table inside a SHACL-SPARQL constraint. The W3C SHACL specification prohibits that construction. This is not an engine limitation — no conforming engine can run the constraint as the spec wrote it. The fix was to generate the same ordering as a nested conditional expression from the single source-of-truth rank table, which is semantically identical and legal. Recorded as a spec gap, not a build choice.

**The spec claimed silence and delivered a default.** The propagation rule says that if any member of a composite has no status, the composite gets no computed status — no binding, no value, silence. Under standard SPARQL semantics, an unbound variable is join-compatible with every row of a `VALUES` table. A member with no status therefore contributed the lowest rank instead of nothing, and the composite came out `unverified`. The query inserted a default, while the prose above it said it never would.

This one is worth pausing on. The entire point of the epistemic-status rule is that no content object gets a trust label it didn't earn. The machinery built to enforce that rule, following the spec's own text, was quietly assigning the weakest label to things that should have had none. It failed closed — `unverified` is the safest wrong answer — but a safe default is still a default, and the spec's claim about its own query was false. The guard now requires every member to carry a bound status before computing the composite.

**The spec's own test prediction was unreachable.** One fixture was written to trigger the upgrade-without-verification refusal. It never got there because an earlier step in the gate sequence rejects a `confirmed` status with no verification record before the upgrade rule runs. The prediction and the step order were inconsistent. The fixture was left as written and marked as a logged divergence; a supplementary fixture reaches the code by a path the step order actually permits, and the operator ruled that it counts.

The remaining three were smaller: a serialization detail where the spec's prefixed names needed escaping in Turtle; an engine-specific mis-evaluation in a focus-node shortcut that was worked around by validating the full graph; and a reference in the upgrade rule to a "previous status timestamp" that no record shape defines. The last is the one place the build chose leniency over refusal, and it is flagged for the framework revision rather than settled here.

---

## What the critic found that the fixtures didn't

After Phase 0 closed, the runtime spec was reissued as v0.2 under a siloed adversarial pass—a separate context that had the spec and the build record but not the build conversation —and run to convergence over three iterations.

It found a build defect. The silence guard described above had been applied at one level of composition and not the next: a cluster with a statusless particle would correctly get no status, but a zone above a statusless cluster would not. Nineteen green fixtures had not exercised that case. The critic did so by reading the code against the rule rather than running it. A fixture was added, the guard was extended, and the validation event passed with the new fixture green.

This is a method observation more than a TCF observation. The adversarial pass earned its cost in code, not just prose, by catching something a passing test suite couldn't have caught because the suite didn't include the case. That's the argument for the discipline stated as a result rather than as an intention.

---

## What changed about the claim

Before Phase 0, the honest statement was: the TCF's epistemic-status rules are specified in prose, and a runtime for them has been drafted.

After Phase 0, it is: a write-time gate enforcing the epistemic-status rules runs on a pinned engine, reaches every refusal path on real fixtures, and the spec it implements has been corrected at the four places where its text did not execute as written.

That is a narrower claim than "the TCF is executable." It covers the Machine-Legibility Layer's status governance and touches four of the seven tiers — three composite levels are untested, two of the three cross-cutting dimensions are untouched, and the field names the runtime uses are candidates pending the framework's next revision. Every computed status the build produces inherits that provisional standing. One long-open question—whether `time-sensitive` is a rank in the status order or an orthogonal flag—was not resolved; no fixture produced a case where the two readings would yield a different gate outcome, and that silence is stated as silence rather than read as a ruling.

Nothing here stops a real publish yet.

---

## What comes next

The framework revision the build was meant to feed can open now. Its inputs are the findings above: the missing timestamp field, the rank-versus-flag question, and the candidate field names that need a ruling before they stop being candidates.

Phase 1 of the runtime — a real storage adapter behind the existing interface, the reader that lets the publish-side crossing gate consume this runtime's outputs, and the integrity shapes the critic's narrowing pointed at — waits until the revision has ruled. Building against candidate names a second time would be building on the same uncertainty twice.

---

*This is the fifth entry in a notebook about building the Seam Stack in public. The runtime discussed here is at [jediwright/tcf-runtime](https://github.com/jediwright/tcf-runtime) · HEAD `fa7fded` · pyshacl 0.40.1 / rdflib 7.6.0 · 20 fixtures, 19 as predicted, 1 logged. The governing spec is v0.2, issued 2026-09-11; v0.1 is retained as the record of what Phase 0 implemented. This runtime implements the [Tiered Content Framework](https://www.jediwright.com/content-strategy-framework); this repository implements it and doesn't amend it. Where the build found problems, they go to the framework's next revision as inputs, not as changes.*

*Ledger: SL-0227 (build plan delta), SL-0228 (engine pin), SL-0229 (spec-text findings; closed by supersession at v0.2), SL-0230 (Phase 0 exit), SL-0231 (v0.2 issuance), SL-0232 (method: siloed critic caught a build defect the fixtures missed). IDs fixed against tail SL-0221, verified by direct read on 2026-09-11; append pending.*
