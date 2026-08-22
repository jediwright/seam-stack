# LINEAGE.md — seam:CrossingRecord
## `vocab/crossing-record/0.1`

Governed under: GSEF v0.2 Scope Statement (`gsef-v0-2-scope-statement_2026-08-19.md`)  

Schema IRI namespace: `https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1#` 

Record type: append-only lineage register  

---

## Schema Version Declaration

**v0.1 — Initial declaration**

| Field | Value |
|---|---|
| Version | 0.1 |
| Status | SUPPORTED |
| Declared | 2026-08-22 |
| Deprecation horizon | Not yet set |
| Content-address reference | sha256:3080f2279c521c64d5cfad38bd3545656b3c3e6517c648edaa8a6fcaab09a7dd |
| Successor | None |
| Prior versions in namespace | None — initial version for this namespace |

This base shape generalizes across seam types. Instance-specific extensions live at their own namespaces. A peer reading a crossing record that declares schema version `0.1` in this namespace should resolve this LINEAGE.md to determine the governed-read status of that record.

---

## Lineage Entries

### L-1 — KL-SCHEMA-01 / `lineageAnchorType` disclosure

| Field | Value |
|---|---|
| entryId | L-1 |
| schemaVersion | 0.1 |
| changeDate | 2026-08-22 |
| changeClass | A |
| changeDriver | endogenous |
| changeDescription | KL-SCHEMA-01 Known Limit disclosure added to the `lineageAnchorType` field note. Names the dual-log tamper-visibility property as aspirational under v0 `author-declared`-only anchoring. States `witness-signed` and `timestamp-signed` as locked pending signing infrastructure. Adds version target note (TBD) for anchor-type unlock. |
| compatibilityBound | No deployment compatibility impact (upper bound). The `author-declared` constraint was already the only operative anchoring value in v0. No peer data behavior changes. Schema enforcement surface unchanged; claim is stated as upper bound per M2. |
| deprecationHorizon | null |
| blastRadiusClassification | Zero operational blast. Documentation consumers may update their understanding of the `lineageAnchorType` v0 limitation. No SHACL delta. No field delta. No vocabulary delta. |
| shaclDelta | false |
| fieldDelta | false |
| vocabDelta | false |
| sourceEvidenceBasis | Three-model adversarial evaluation (Gemini, ChatGPT, Claude) + producer reconciliation, 2026-08-22. Item 8 verdict: ACCEPTED. |
| recordId | `https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1/lineage#L-1` |

---

### L-2 — KL-SCHEMA-02 / `provenanceStatusBasis` scope note

| Field | Value |
|---|---|
| entryId | L-2 |
| schemaVersion | 0.1 |
| changeDate | 2026-08-22 |
| changeClass | A |
| changeDriver | endogenous |
| changeDescription | KL-SCHEMA-02 Known Limit disclosure and v0 scope note added below the `provenanceStatusBasis` field row. Characterizes v0 provenance governance as exception-tracking only. Confirms that the majority-state record class (`provenanceStatus: asserted`) requires no basis field. Names v1 candidate: general-state provenance governance requiring a minimal basis field for all statuses including `asserted`. |
| compatibilityBound | No deployment compatibility impact (upper bound). The absence of a basis field for `asserted` records was already structurally correct under the existing conditional requirement ("Required when status ≠ asserted"). Scope note confirms rather than changes existing deployment behavior. |
| deprecationHorizon | null |
| blastRadiusClassification | Zero operational blast. Deployments with `provenanceStatus: asserted` records and no basis field remain SHACL-compliant before and after. No SHACL delta. No field delta. No vocabulary delta. |
| shaclDelta | false |
| fieldDelta | false |
| vocabDelta | false |
| sourceEvidenceBasis | Three-model adversarial evaluation + producer reconciliation, 2026-08-22. Item 9 verdict: ACCEPTED. |
| recordId | `https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1/lineage#L-2` |

---

### L-3 — KL-SCHEMA-03 / `evidenceDecay` scope note

| Field | Value |
|---|---|
| entryId | L-3 |
| schemaVersion | 0.1 |
| changeDate | 2026-08-22 |
| changeClass | A |
| changeDriver | endogenous |
| changeDescription | KL-SCHEMA-03 Known Limit disclosure and v0 scope note added below the `evidenceDecay` field row. Characterizes decay tracking as a deployment governance choice, not a schema invariant. Clarifies that omission of `evidenceDecay` is SHACL-compliant and structurally indistinguishable from a deliberate "no decay applicable" determination. Names v1 candidate: require `evidenceDecay` for `recordType: gate-check` and `recordType: verification` records where re-verification obligations apply. |
| compatibilityBound | No deployment compatibility impact (upper bound). The Optional designation was already operative. Deployments omitting `evidenceDecay` remain SHACL-compliant. Evidence-layer assertions contingent on systematic decay tracking are deployment-dependency claims, not schema invariants — per this amendment. |
| deprecationHorizon | null |
| blastRadiusClassification | Zero operational blast. No peer data behavior changes. No SHACL delta. No field delta. No vocabulary delta. |
| shaclDelta | false |
| fieldDelta | false |
| vocabDelta | false |
| sourceEvidenceBasis | Three-model adversarial evaluation + producer reconciliation, 2026-08-22. Item 10 verdict: ACCEPTED. |
| recordId | `https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1/lineage#L-3` |

---

### L-4 — KL-SCHEMA-04 / Conditional shapes note (replacement + expansion)

| Field | Value |
|---|---|
| entryId | L-4 |
| schemaVersion | 0.1 |
| changeDate | 2026-08-22 |
| changeClass | B (governing class) / A (schema enforcement component) — see component breakdown |
| changeDriver | endogenous |
| changeDescription | Conditional shapes note replaced with expanded KL-SCHEMA-04 qualified version. Prior note acknowledged generic deferral of conditional constraint enforcement ("Those shapes are not yet produced"). New version names four property categories in the unenforced set: (1) chain integrity — `chainReference` / `chainDepth` / `lineageAnchorType` consistency; (2) supersession linkage — `supersededBy` present and valid when `provenanceStatus: superseded`; (3) relay-seam chain membership validation; (4) composition-level level-boundary predicates. Adds public-facing documentation qualifier requirement: architecture documentation and README language describing these properties must carry a "design goal — conditional shapes pending" qualifier until instance-specific SHACL shapes ship. Cross-references KL-THEORY-02. |
| componentBreakdown | Schema enforcement surface: Class A — SHACL shape unchanged; no field delta; no vocabulary delta; existing conformant records remain conformant. Documentation propagation surface: Class B — three downstream documents carry named propagation obligations (see propagationObligations). Governing class: B (highest component). Per Q-C §4 of GSEF v0.2 Scope Statement: one taxonomy application, highest component class governs. |
| compatibilityBound | Schema enforcement surface (Class A): no deployment compatibility impact (upper bound). SHACL unchanged; existing conformant records remain conformant. Documentation surface (Class B): compatibility claim bounded to three named downstream documents. Upper-bound claim: this record cannot confirm whether additional documentation surfaces describe chain properties without the qualifier; the three named documents are the known blast-radius surface. Documentation propagation completion is pending, not confirmed. |
| deprecationHorizon | null |
| blastRadiusClassification | Schema surface: zero operational blast. SHACL unchanged. No peer data behavior changes. Documentation surface (Class B): three downstream documents require qualifier amendment — (1) THEORY.md: chain property language and KL-THEORY-02 cross-reference; (2) seam-stack/README.md: chain integrity and composition integrity sections; (3) Artifact B r2.4: composition integrity claims. Propagation tracked via L-4a / L-4b / L-4c (append when applied). |
| propagationObligations | THEORY.md → L-4a (PENDING), seam-stack/README.md → L-4b (PENDING), Artifact-B-r2.4 → L-4c (PENDING) |
| shaclDelta | false |
| fieldDelta | false |
| vocabDelta | false |
| sourceEvidenceBasis | Three-model adversarial evaluation + producer reconciliation, 2026-08-22. Item 11 verdict: ACCEPTED. |
| recordId | `https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1/lineage#L-4` |

---

### L-4a — THEORY.md propagation apply

| Field | Value |
|---|---|
| entryId | L-4a |
| changeDate | 2026-08-22 |
| changeClass | A |
| changeDriver | endogenous |
| changeDescription | THEORY.md amended to carry KL-THEORY-02 and KL-THEORY-03 known limit blocks. KL-THEORY-02 names composition-level machine-checkability as a design goal pending instance-specific SHACL shapes. KL-THEORY-03 names the three crossing sub-classes outside the canonical pre-specifiable scope. Chain-property qualifier obligation from L-4 met. |
| propagationObligationClosed | L-4 → THEORY.md |
| recordId | `https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1/lineage#L-4a` |

---

### L-4b — seam-stack/README.md propagation apply

| Field | Value |
|---|---|
| entryId | L-4b |
| changeDate | 2026-08-22 |
| changeClass | A |
| changeDriver | endogenous |
| changeDescription | seam-stack/README.md amended with v0 enforcement scope disclosure (Item 1), relay-exit three-property distinction and KL-README-01 prose (Item 4), corrected ToIP prior-art acknowledgment (Item 6), and layer necessity conditionalization on local-first premise (Item 7). Chain-integrity and composition-integrity qualifier obligation from L-4 met. |
| propagationObligationClosed | L-4 → seam-stack/README.md |
| recordId | `https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1/lineage#L-4b` |

---

### L-4c — Artifact B (r2.4) propagation apply [PENDING]

Append when Artifact B composition integrity language is operator-amended.

| Field | Placeholder |
|---|---|
| entryId | L-4c |
| changeDate | [date of apply] |
| changeClass | A |
| changeDriver | endogenous |
| changeDescription | Artifact B r2.4 composition integrity claims amended to carry "design goal — conditional shapes pending" qualifier, per L-4 propagation obligation. |
| propagationObligationClosed | L-4 → Artifact B r2.4 |
| recordId | `https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1/lineage#L-4c` |

---

## Machine-Readable Block

```json
{
  "schemaNamespace": "https://jediwright.github.io/seam-stack/vocab/crossing-record/0.1#",
  "lineageFileVersion": "0.1",
  "governedBy": "gsef-v0-2-scope-statement_2026-08-19.md",
  "initializedDate": "2026-08-22",
  "currentStatus": "SUPPORTED",
  "deprecationHorizon": null,
  "entries": [
    {
      "id": "L-1",
      "changeClass": "A",
      "changeDriver": "endogenous",
      "changeDate": "2026-08-22",
      "kl": "KL-SCHEMA-01",
      "shaclDelta": false,
      "fieldDelta": false,
      "vocabDelta": false,
      "propagationPending": []
    },
    {
      "id": "L-2",
      "changeClass": "A",
      "changeDriver": "endogenous",
      "changeDate": "2026-08-22",
      "kl": "KL-SCHEMA-02",
      "shaclDelta": false,
      "fieldDelta": false,
      "vocabDelta": false,
      "propagationPending": []
    },
    {
      "id": "L-3",
      "changeClass": "A",
      "changeDriver": "endogenous",
      "changeDate": "2026-08-22",
      "kl": "KL-SCHEMA-03",
      "shaclDelta": false,
      "fieldDelta": false,
      "vocabDelta": false,
      "propagationPending": []
    },
    {
      "id": "L-4",
      "changeClass": "B",
      "changeClassComponent": "A",
      "changeDriver": "endogenous",
      "changeDate": "2026-08-22",
      "kl": "KL-SCHEMA-04",
      "shaclDelta": false,
      "fieldDelta": false,
      "vocabDelta": false,
      "propagationPending": [
        { "entryId": "L-4a", "target": "THEORY.md", "status": "CLOSED" },
        { "entryId": "L-4b", "target": "seam-stack/README.md", "status": "CLOSED" },
        { "entryId": "L-4c", "target": "Artifact-B-r2.4", "status": "PENDING" }
      ]
    }
  ]
}
```

---

*Append-only. Do not edit existing entries. Corrections and continuations are new entries referencing the entry being corrected or continued. Canonical copy: operator's machine.*
