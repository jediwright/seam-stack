# seam:CrossingRecord Base Shape
## crossing-record schema specification — amended 2026-08-22

The unified base shape that all governed-event records in the architecture
instantiate. The concrete schema artifact underlying the Evidence layer's claims.

IRI namespace: `https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1#`

---

## Four required field groups

### Identity group (required in every instance)

| Field | Type | Requirement | Notes |
|---|---|---|---|
| recordId | URI | Required | Globally addressable identifier. |
| recordType | Controlled vocabulary | Required | Values: gate-check / lineage / provenance / verification |
| emittedAt | ISO 8601 timestamp | Required | Author-declared under v0 default (trust-the-author-with-named-boundary). |
| emittedBy | DID | Required | The party responsible for emitting this record. |

---

### Provenance linkage group (required in every instance)

| Field | Type | Requirement | Notes |
|---|---|---|---|
| provenanceStatus | Controlled vocabulary | Required | Values: asserted / confirmed / contested / superseded |
| provenanceStatusBasis | Short text | Required when status ≠ asserted | One-sentence basis for the non-asserted status. Not required when status is asserted. |
| supersededBy | URI | Conditional | Required only when provenanceStatus: superseded. |

**v0 scope note — provenanceStatusBasis:** Provenance governance in v0 is exception-tracking, not
general-state governance. Records with `provenanceStatus: asserted` — the expected floor state for
the majority of records in any working deployment — require no basis field. A deployment can be fully
SHACL-compliant with every record self-asserted and no provenance accountability beyond schema
conformance. Public-facing provenance governance claims should be scoped to non-asserted records only
until this changes. v1 candidate: require a minimal basis field for `asserted` records (optional
free-text or controlled vocabulary noting the basis for self-assertion), promoting provenance
governance from exception-coverage to general-coverage.

**KL-SCHEMA-02:** Provenance governance is exception-coverage only in v0. The majority-state record
class (`provenanceStatus: asserted`) carries no required basis. General-state provenance governance
— reaching the floor state — is a v1 candidate item.

---

### Lineage anchoring group (required where chain participation is claimed)

| Field | Type | Requirement | Notes |
|---|---|---|---|
| chainReference | URI | Conditional — required for relay seam records | References the immediately upstream record. |
| chainDepth | Integer | Conditional — required when chainReference is present | Position in chain. |
| lineageAnchorType | Controlled vocabulary | Conditional — required when chainReference is present | Values: author-declared / witness-signed / timestamp-signed. |

**Note on lineageAnchorType:** `witness-signed` and `timestamp-signed` are locked pending
infrastructure. `author-declared` is the only currently available value in v0.

**Known Limit — KL-SCHEMA-01:** The dual-log tamper-visibility property — the property by which any
divergence between two parties' independently signed records referencing the same content hash is
itself the evidence of tampering — requires `witness-signed` or `timestamp-signed` lineage anchoring
to be cryptographically enforced. Under `author-declared` anchoring, a single actor can author both
dual-log records; no schema constraint prevents it. This property is the architecture's primary
evidence-layer design commitment for multi-party crossings and is currently aspirational rather than
enforced. It becomes operational when independent signing infrastructure is available and the locked
anchor types are unlocked. Version target: TBD.

---

### Evidence scope group (required in every instance)

| Field | Type | Requirement | Notes |
|---|---|---|---|
| governanceEvent | Controlled vocabulary | Required | Values: gate-check / schema-change / action-provenance / code-change-verification |
| boundType | Controlled vocabulary | Required | Values: exposure-upper-bound / confirmation / attestation |
| evidenceDecay | ISO 8601 date | Optional | Date after which the record's claims require re-verification. Optional in v0. |

**v0 scope note — evidenceDecay:** Decay tracking is a deployment governance choice in v0, not a
schema invariant. A deployment that omits `evidenceDecay` on all records is fully SHACL-compliant;
the omission is structurally indistinguishable from a deliberate "no decay applicable" determination.
Evidence-layer claims about systematic time-bounded validity are behavior-dependent until
`evidenceDecay` is either (a) made required for applicable record types or (b) documented as an
explicit opt-in with governance procedures ensuring consistent application across a deployment.
v1 candidate: require `evidenceDecay` for `recordType: gate-check` and `recordType: verification`
records where the architecture asserts re-verification obligations.

**KL-SCHEMA-03:** Systematic evidence decay tracking is behavior-dependent in v0. The `evidenceDecay`
field is optional; schema-level enforcement of time-bounded record validity is a v1 candidate item.
Evidence-layer assertions contingent on systematic decay tracking should be read as deployment
governance commitments, not schema invariants.

---

## SHACL enforcement shape (base)

```turtle
@prefix sh: <http://www.w3.org/ns/shacl#> .
@prefix seam: <https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

seam:CrossingRecordShape
  a sh:NodeShape ;
  sh:targetClass seam:CrossingRecord ;
  # Identity group
  sh:property [ sh:path seam:recordId ; sh:minCount 1 ; sh:maxCount 1 ; sh:nodeKind sh:IRI ] ;
  sh:property [ sh:path seam:recordType ; sh:minCount 1 ; sh:maxCount 1 ;
    sh:in ( "gate-check" "lineage" "provenance" "verification" ) ] ;
  sh:property [ sh:path seam:emittedAt ; sh:minCount 1 ; sh:maxCount 1 ; sh:datatype xsd:dateTime ] ;
  sh:property [ sh:path seam:emittedBy ; sh:minCount 1 ; sh:maxCount 1 ; sh:nodeKind sh:IRI ] ;
  # Provenance linkage group
  sh:property [ sh:path seam:provenanceStatus ; sh:minCount 1 ; sh:maxCount 1 ;
    sh:in ( "asserted" "confirmed" "contested" "superseded" ) ] ;
  # Evidence scope group
  sh:property [ sh:path seam:governanceEvent ; sh:minCount 1 ; sh:maxCount 1 ;
    sh:in ( "gate-check" "schema-change" "action-provenance" "code-change-verification" ) ] ;
  sh:property [ sh:path seam:boundType ; sh:minCount 1 ; sh:maxCount 1 ;
    sh:in ( "exposure-upper-bound" "confirmation" "attestation" ) ] .
```

---

## Conditional constraint shapes

Conditional constraints (chainReference required for relay records, supersededBy required when
provenanceStatus: superseded, etc.) are enforced in instance-specific SHACL shapes that extend
this base. Those shapes are not yet produced.

**Public-facing language qualifier — KL-SCHEMA-04:** Until instance-specific SHACL shapes are
produced, the following are design goals, not enforced schema constraints: chain integrity
(`chainReference` / `chainDepth` / `lineageAnchorType` consistency), supersession linkage
(`supersededBy` present and valid when `provenanceStatus: superseded`), relay-seam chain membership
validation, and composition-level level-boundary predicates. Architecture documentation and README
language describing chain properties, lineage continuity, and composition integrity should carry a
"design goal — conditional shapes pending" qualifier until these shapes ship. Version target: TBD
per instance-type formalization sessions.

**KL-SCHEMA-04:** Chain integrity, supersession linkage, and composition-level constraints are design
goals, not enforced invariants. Instance-specific SHACL shapes required to enforce these properties
are not yet produced. All chain-integrity and composition-integrity language in public documentation
is aspirational until those shapes exist. See also KL-THEORY-02.
