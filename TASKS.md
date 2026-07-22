# TASKS — genealogy-public-records

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

Itemized backlog for the open, evidence-cited family-history corpus. See [`PLAN.md`](./PLAN.md) for
context, the licensing + privacy gates, and the roadmap (M0–M3).

## How these tasks map to Hee-Lee Oss

Each task below becomes a Hee-Lee Oss **Task JSON** validated against `packages/schema/src/schemas.ts`.
Field mapping:

- **id** — stable `genealogy-public-records-<area>-NNN` (the table ID).
- **title** — the table Title.
- **project** — `genealogy-public-records`.
- **type** — one of `code | research | writing | data | design-spec | maintenance` (table "Type").
- **lane** — `donated` for all tasks here. Any future metered run would be `funded` and must add
  `fundedBudgetUsd`.
- **priority** — `high | medium | low`.
- **domain** — e.g. `["genealogy","family-history","open-history","public-data","education"]`.
- **riskTier** — `low | medium | high`. License/extraction/privacy tasks are **medium**;
  scaffolding/docs are **low**; **sensitive-records (enslavement-era/Indigenous) tasks are `high`**
  (community-advisor sign-off required).
- **urgent** — boolean (default `false`).
- **deliverable** — `pr | dataset | document | translation` (table "Deliverable").
- **tokenEstimate** — `small | medium | large` (table "Size").
- **status** — `open | in-progress | review | delivered | done` (start `open`).
- **context / objective / acceptanceCriteria[] / resources[] / output** — per task.
- **requestor** — `jdev1977` / beneficiary class until a named partner is secured.
- **verifiedNeed** — **`false`** while no committed partner/steward is secured (honest; the *gap* is
  real, the last-mile beneficiary is TO BE SECURED).
- **outputLicense** — `MIT` (code), `CC0-1.0` (PD-derived data/docs), `CC-BY-SA-4.0` (where CC BY-SA
  material is incorporated), `CC-BY-4.0` (GeoNames-derived), or `ODbL-1.0` (OSM-derived place data).

> **Standing guardrails on every data/extraction task:**
> 1. No source may be touched until its `sources/allowlist.yml` entry is `approved` by the **license
>    reviewer AND privacy reviewer**. Any task proposing to scrape/harvest/import a proprietary
>    genealogy database (Ancestry, MyHeritage, FindMyPast, Fold3, Find A Grave, Newspapers.com,
>    Geni, …) — or a vendor-wrapped scan/index of a PD record — is **refused and flagged**.
> 2. **Deceased persons only.** Uncertainty → exclude. No SSN / Death-Master-File / genetic data.
> 3. **Sensitive-records (enslavement-era / Indigenous) tasks require a community advisor** and CARE
>    handling, and are `high` risk.

---

## Milestone M0 — Foundation, licensing & privacy spine

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| genealogy-public-records-profile-001 | Define GEDCOM X data profile + Evidence/Conclusion (GPS) layer | design-spec | medium | low | document | — | Maintainer + accuracy reviewer |
| genealogy-public-records-license-001 | Source allow-list schema (license + privacy + sensitivity) + gate policy | design-spec | small | medium | document | — | License + privacy reviewers |
| genealogy-public-records-privacy-001 | Deceased-determination policy v1 + living-person takedown workflow | design-spec | small | medium | document | — | Privacy reviewer |
| genealogy-public-records-license-002 | Analyze & record first 3 candidate sources (≥1 approved NARA census) | research | medium | medium | document | genealogy-public-records-license-001 | License + privacy reviewers |
| genealogy-public-records-prov-001 | Ratify provenance+evidence mechanism + define countable assertion unit | design-spec | small | low | document | genealogy-public-records-profile-001 | Maintainer |
| genealogy-public-records-iri-001 | Commit host-independent persistent IRI namespace (w3id.org/PURL) | design-spec | small | low | document | genealogy-public-records-profile-001 | Maintainer |
| genealogy-public-records-ci-001 | CI gates: SHACL + provenance + evidence + no-SSN/DMF/genetic + deceased-only + allow-list | code | medium | medium | pr | genealogy-public-records-profile-001, genealogy-public-records-prov-001, genealogy-public-records-privacy-001 | Maintainer + privacy reviewer |
| genealogy-public-records-partner-001 | Steward + sensitive-records community-advisor outreach | research | small | low | document | — | Maintainer |

**Acceptance criteria (key M0 tasks)**

- **genealogy-public-records-profile-001**
  - Core entities defined (Person, Name, Fact, Relationship, PlaceReference, SourceDescription) with
    properties, datatypes, cardinality, aligned to GEDCOM X.
  - An **Evidence → Conclusion** layer encodes GPS reasoning: each Conclusion links supporting
    Evidence and records analysis + conflict resolution.
  - Each entity/property mapped to a schema.org and/or Wikidata equivalent where one exists; gaps noted.
  - Relationship + conflicting-claim representation modeled (no single forced "truth").
  - **No field exists for SSN, Death-Master-File, or genetic/DNA data** (explicit by design).
  - Every assertion reserves a provenance slot (source IRI + page/image locator + license) and an
    evidence-rationale slot.
  - Published as a human-readable spec + machine artifact (JSON Schema/SHACL stub).
- **genealogy-public-records-license-001**
  - Allow-list schema captures: title, custodian, **serving-custodian (must be public, not vendor)**,
    URL, format, license/PD basis, **privacy-embargo determination**, extraction-method, **sensitivity
    flag**, and `status: approved|rejected|pending`.
  - Policy text states all proprietary genealogy DBs and vendor-wrapped scans/indexes are categorically
    `rejected`; anti-laundering controls documented (page/image citations, public-custodian URLs,
    contributor attestation, vendor-field spot-checks).
  - Per-copy/edition rights-verification requirement documented (PD original ≠ PD scan).
- **genealogy-public-records-privacy-001**
  - Deceased-determination rule v1 stated and **versioned**: positive death evidence **or** documented
    birth ≥100 years ago; **uncertainty → exclude**; living relatives suppressed.
  - SSN/DMF/genetic exclusion stated; vital-record embargo handling stated.
  - Takedown workflow documented: report → verify → remove (≤72h median SLA), with logging that does
    not re-publish PII.
- **genealogy-public-records-ci-001**
  - CI fails on any assertion lacking a provenance link **or** an evidence rationale.
  - CI rejects any record referencing a source not marked `approved`.
  - CI rejects any presence of SSN/DMF/genetic fields and any person not classified deceased.
  - CI runs SHACL validation against the profile shapes.

**M0 Definition of Done:** GEDCOM X profile v0 + Evidence/Conclusion layer published; allow-list
schema + policy merged with ≥3 sources analyzed and ≥1 approved (incl. a NARA census source);
provenance+evidence mechanism ratified **with the countable assertion unit defined**;
**deceased-determination policy v1 ratified/versioned**; CI gates (provenance, evidence, no-SSN/DMF/
genetic, deceased-only, allow-list, SHACL) live; **host-independent persistent IRI namespace
committed**; takedown workflow documented; steward + community-advisor outreach initiated with status
logged; **a qualified License/ToS reviewer AND a qualified Privacy reviewer named (hard exits; if
either seat is empty M0 cannot exit — escalate per PLAN.md fallback)**. `pnpm build && pnpm test &&
pnpm lint` green.

---

## Milestone M1 — First sourced slice

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| genealogy-public-records-extract-001 | Build NARA census extractor (assistive, flag-on-doubt, method-aware) | code | medium | medium | pr | genealogy-public-records-ci-001, genealogy-public-records-license-002 | Maintainer |
| genealogy-public-records-privacy-002 | Deceased-determination engine + living-relative suppression | code | medium | medium | pr | genealogy-public-records-extract-001, genealogy-public-records-privacy-001 | Privacy reviewer |
| genealogy-public-records-data-001 | Extract one approved NARA census batch → corpus (provenance + evidence) | data | large | medium | dataset | genealogy-public-records-extract-001, genealogy-public-records-privacy-002 | Accuracy + privacy reviewers |
| genealogy-public-records-qa-001 | Citation + GPS-rationale audit of M1 batch (≥95% verified) | research | small | medium | document | genealogy-public-records-data-001 | Accuracy reviewer |
| genealogy-public-records-qa-002 | Deceased-determination audit of M1 batch (100% correct; zero living) | research | small | medium | document | genealogy-public-records-data-001 | Privacy reviewer |
| genealogy-public-records-export-001 | GEDCOM 7 + GEDCOM X JSON + JSON-LD export tooling | code | medium | low | pr | genealogy-public-records-data-001 | Maintainer |
| genealogy-public-records-partner-002 | Identify & engage ≥1 candidate steward for adoption | research | small | low | document | genealogy-public-records-partner-001 | Maintainer |

**Acceptance criteria (key M1 tasks)**

- **genealogy-public-records-extract-001**
  - Honors a **per-source extraction-method policy**: human transcription for handwritten census
    sheets; OCR only for machine-print sources; method recorded into each assertion's provenance.
  - A **transcription-accuracy baseline** (per-method field-level threshold) gates a batch before
    normalization.
  - Ambiguous/illegible fields are flagged for review, **never guessed**; gaps remain gaps.
- **genealogy-public-records-privacy-002**
  - Implements the ratified deceased-determination rule; **anyone not provably deceased is excluded.**
  - Suppresses individuals on a record who may be living (e.g., children on a recent census).
  - Strips any prohibited field (SSN/DMF/genetic) before persistence; emits an auditable report.
- **genealogy-public-records-data-001**
  - Records from one approved NARA census batch mapped to Person/Name/Fact/Relationship/Source nodes.
  - **100%** of new assertions carry a resolvable page/image-level provenance link **and** a GPS
    evidence rationale.
  - Conflicting source statements (e.g., disagreeing ages) retained with separate provenance.
  - Passes all CI gates (SHACL + provenance + evidence + no-SSN/DMF/genetic + deceased-only +
    allow-list) before review.
- **genealogy-public-records-qa-002**
  - A stratified sample is checked: **100% correctly classified deceased; zero living persons.**
  - Auditor independent of the extractor; any misclassification is a blocking P0 until fixed and
    triggers a batch re-audit.

**M1 entry precondition (hard gate):** the NARA census access path is named and recorded in the
allow-list as a PD-clear, **NARA-served** digitization (not Fold3/Ancestry), with the **72-year rule
satisfied**, before any extraction begins.

**M1 Definition of Done:** end-to-end pipeline proven on one approved NARA census batch; deceased-only
with deceased-audit 100% / zero living; 100% provenance + evidence; stratified citation audit ≥95%;
GEDCOM 7 + GEDCOM X JSON + JSON-LD exports produced under the persistent-IRI namespace; ≥1 candidate
steward in conversation.

---

## Milestone M2 — Evidence linking, reconciliation & explorer

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| genealogy-public-records-recon-001 | Wikidata reconciliation pass (propose QID/place matches) | data | medium | medium | dataset | genealogy-public-records-data-001 | Accuracy reviewer |
| genealogy-public-records-recon-002 | Human-reviewed duplicate-merge workflow (precision-first) | code | medium | medium | pr | genealogy-public-records-recon-001 | Maintainer + accuracy reviewer |
| genealogy-public-records-explorer-001 | No-account public explorer (every assertion shows source + rationale) | code | medium | low | pr | genealogy-public-records-export-001 | Maintainer |
| genealogy-public-records-takedown-001 | Operationalize + test living-person takedown path end-to-end | maintenance | small | medium | document | genealogy-public-records-explorer-001, genealogy-public-records-privacy-001 | Privacy reviewer |
| genealogy-public-records-docs-001 | Data dictionary + citation/usage guide for consumers | writing | small | low | document | genealogy-public-records-explorer-001 | Maintainer |
| genealogy-public-records-metrics-001 | Reuse-metrics tracking (downloads/queries) | maintenance | small | low | document | genealogy-public-records-explorer-001 | Maintainer |

**Acceptance criteria (key M2 tasks)**

- **genealogy-public-records-recon-002**
  - No automatic merge ships without human confirmation.
  - Uses an explicit **blocking-key strategy** (e.g., normalized surname + birth-decade + birthplace
    region); matcher tuned for precision over recall.
  - **Zero confirmed false merges** in the dedup audit sample.
  - Merges preserve all source provenance + evidence from both records; reversible/auditable.
- **genealogy-public-records-explorer-001**
  - Browse a Person and see names, facts, relationships, places, and — for **every assertion** — its
    source citation **and** evidence rationale.
  - Static/no-account; collects **no visitor PII**; links to raw exports; shows license per dataset.
- **genealogy-public-records-takedown-001**
  - A test report of a (synthetic) living-person inclusion is processed end-to-end and removed within
    the **≤72h** SLA; removal is logged without re-publishing PII; the source batch is re-audited.

**M2 Definition of Done:** reconciliation pass complete with reviewed merges (zero confirmed false
merges); duplicate workflow operational; no-account explorer + documented exports live with
source+rationale visible per assertion; takedown path tested end-to-end; reuse metrics tracked.

---

## Milestone M3 — Scale, sensitive-records track & partner adoption (shipped)

| ID | Title | Type | Size | Risk | Deliverable | Depends on | Reviewer |
| --- | --- | --- | --- | --- | --- | --- | --- |
| genealogy-public-records-data-002 | Integrate ≥3 more approved sources; scale to ≥5,000 deceased persons | data | large | medium | dataset | genealogy-public-records-recon-002, genealogy-public-records-license-002 | Accuracy + license + privacy reviewers |
| genealogy-public-records-sensitive-001 | Pilot enslavement-era/Indigenous records (dignity + CARE + community advisor) | data | medium | high | dataset | genealogy-public-records-data-002, genealogy-public-records-partner-001 | Community advisor + privacy reviewer |
| genealogy-public-records-partner-003 | Secure steward adoption + ≥1 documented citation/use | research | medium | low | document | genealogy-public-records-partner-002 | Maintainer |
| genealogy-public-records-sustain-001 | Sustainability plan (hosting + permanent takedown + reviewer rotation/throughput) | writing | small | low | document | genealogy-public-records-partner-003 | Maintainer + privacy reviewer |

**Acceptance criteria (key M3 tasks)**

- **genealogy-public-records-data-002**
  - ≥4 total source collections integrated; ≥5,000 deceased-only sourced Person records published.
  - Each new source passed the license **and** privacy gate before extraction; 100% provenance +
    evidence maintained; a fresh citation audit still ≥95%; a fresh deceased audit still 100%/zero living.
- **genealogy-public-records-sensitive-001**
  - A **community advisor is engaged and signs off** before extraction; CARE principles applied.
  - Enslaved/Indigenous individuals documented **as persons** with dignity and historical context;
    enslavement recorded as a sourced historical fact, never as a dehumanizing label.
  - Non-publication or special handling honored where the community requests it.
- **genealogy-public-records-partner-003**
  - A named educational/historical/heritage steward commits to adopt/host and cite the corpus.
  - ≥1 concrete citation or downstream use is documented.

**M3 Definition of Done (project "shipped"):** ≥5,000 deceased-only sourced persons across ≥4 sources;
100% provenance + evidence; deceased-audit clean (zero living); sensitive-records track piloted with a
community advisor under CARE; ≥1 steward has adopted/cited the corpus; permanent takedown + sustainability
plan in effect.

---

## Backlog / future (sized, unscheduled)

| ID | Title | Type | Size | Risk | Deliverable | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| genealogy-public-records-data-003 | Integrate NARA naturalization/immigration batch | data | medium | medium | dataset | PD; verify NARA-served copy |
| genealogy-public-records-data-004 | Integrate Chronicling America obituaries (OCR, PD titles) | data | medium | medium | dataset | OCR text; verify per-title PD |
| genealogy-public-records-vital-001 | Vital-records embargo matrix (per-jurisdiction) + first cleared set | research | medium | high | document | Privacy-heavy; many jurisdictions restrict; default exclude |
| genealogy-public-records-gazetteer-001 | Historic place gazetteer (Wikidata CC0 / GeoNames CC BY) | data | medium | low | dataset | Label ODbL if OSM used |
| genealogy-public-records-quality-001 | Automated anomaly/outlier flagging for review | code | medium | low | pr | Assistive QA, human-confirmed |
| genealogy-public-records-i18n-001 | Multilingual person/place labels via Wikidata | data | small | low | dataset | CC0 labels |
| genealogy-public-records-sparql-001 | Optional hosted SPARQL endpoint | code | large | low | pr | Depends on steward hosting |

---

## Example task JSON

Schema-valid Task JSON for the first M0 task (`genealogy-public-records-profile-001`):

```json
{
  "id": "genealogy-public-records-profile-001",
  "title": "Define GEDCOM X data profile + Evidence/Conclusion (GPS) layer",
  "project": "genealogy-public-records",
  "type": "design-spec",
  "lane": "donated",
  "priority": "high",
  "domain": ["genealogy", "family-history", "open-history", "public-data", "education"],
  "riskTier": "low",
  "urgent": false,
  "deliverable": "document",
  "tokenEstimate": "medium",
  "status": "open",
  "context": "An open, evidence-cited family-history corpus needs a shared data profile before any data is ingested. Sources are public-domain records only (NARA federal census under the 72-year rule, immigration/naturalization, military/pension files; Library of Congress; Chronicling America; openly-licensed transcriptions) plus Wikidata/GeoNames for reconciliation. Subjects are deceased persons only; no SSN, Death-Master-File, or genetic data. Scraping any proprietary genealogy database (Ancestry, MyHeritage, FindMyPast, Fold3, Find A Grave, Newspapers.com) or any vendor-wrapped scan/index is out of scope. See PLAN.md.",
  "objective": "Define the GEDCOM X-aligned core entities (Person, Name, Fact, Relationship, PlaceReference, SourceDescription) plus an Evidence-to-Conclusion layer encoding Genealogical Proof Standard reasoning, reusing schema.org/Wikidata vocabulary where equivalents exist, modeling conflicting claims rather than flattening them, and deliberately providing NO field for SSN/DMF/genetic data.",
  "acceptanceCriteria": [
    "Core entities defined with properties, datatypes, and cardinality, aligned to GEDCOM X.",
    "Evidence-to-Conclusion (GPS) layer specified: each Conclusion links supporting Evidence and records analysis plus conflict resolution.",
    "Each entity/property mapped to a schema.org and/or Wikidata equivalent where one exists; gaps documented.",
    "Relationship modeled to support source-conflicting kinship claims; no single forced 'truth'.",
    "No field exists for SSN, Death-Master-File/SSDI, or genetic/DNA data (explicit by design).",
    "Every assertion reserves a provenance slot (source IRI + page/image locator + license + method) and an evidence-rationale slot.",
    "Published as a human-readable spec plus a machine artifact (JSON Schema/SHACL stub)."
  ],
  "resources": [
    "planning/projects/genealogy-public-records/PLAN.md",
    "https://www.archives.gov/research/census",
    "https://www.gedcom.org/",
    "https://schema.org/",
    "https://www.wikidata.org/"
  ],
  "output": "A data-profile specification document (profile/README.md) plus a JSON Schema/SHACL stub defining the GEDCOM X-aligned entities, the Evidence/Conclusion layer, and the schema.org/Wikidata mappings, committed via PR.",
  "requestor": "jdev1977",
  "verifiedNeed": false,
  "outputLicense": "CC0-1.0"
}
```

---

## Generated task index

> Auto-generated by the Hee-Lee Oss task-decomposition process on 2026-06-29.
> Every TASKS.md row now has a corresponding validated `tasks/<id>.json` file.
> Total task files: 32 (1 pre-existing seed + 31 generated).
> Validation: all 32 files passed `validate-tasks.mjs` with exit 0.

| Task file | Milestone | Type | Priority | Risk | Deliverable |
| --- | --- | --- | --- | --- | --- |
| tasks/genealogy-public-records-profile-001.json | M0 | design-spec | high | low | document |
| tasks/genealogy-public-records-license-001.json | M0 | design-spec | high | medium | document |
| tasks/genealogy-public-records-privacy-001.json | M0 | design-spec | high | medium | document |
| tasks/genealogy-public-records-license-002.json | M0 | research | high | medium | document |
| tasks/genealogy-public-records-prov-001.json | M0 | design-spec | high | low | document |
| tasks/genealogy-public-records-iri-001.json | M0 | design-spec | medium | low | document |
| tasks/genealogy-public-records-ci-001.json | M0 | code | high | medium | pr |
| tasks/genealogy-public-records-partner-001.json | M0 | research | medium | low | document |
| tasks/genealogy-public-records-extract-001.json | M1 | code | high | medium | pr |
| tasks/genealogy-public-records-privacy-002.json | M1 | code | high | medium | pr |
| tasks/genealogy-public-records-data-001.json | M1 | data | high | medium | dataset |
| tasks/genealogy-public-records-qa-001.json | M1 | research | high | medium | document |
| tasks/genealogy-public-records-qa-002.json | M1 | research | high | medium | document |
| tasks/genealogy-public-records-export-001.json | M1 | code | high | low | pr |
| tasks/genealogy-public-records-partner-002.json | M1 | research | medium | low | document |
| tasks/genealogy-public-records-recon-001.json | M2 | data | medium | medium | dataset |
| tasks/genealogy-public-records-recon-002.json | M2 | code | medium | medium | pr |
| tasks/genealogy-public-records-explorer-001.json | M2 | code | medium | low | pr |
| tasks/genealogy-public-records-takedown-001.json | M2 | maintenance | high | medium | document |
| tasks/genealogy-public-records-docs-001.json | M2 | writing | medium | low | document |
| tasks/genealogy-public-records-metrics-001.json | M2 | maintenance | low | low | document |
| tasks/genealogy-public-records-data-002.json | M3 | data | high | medium | dataset |
| tasks/genealogy-public-records-sensitive-001.json | M3 | data | medium | high | dataset |
| tasks/genealogy-public-records-partner-003.json | M3 | research | high | low | document |
| tasks/genealogy-public-records-sustain-001.json | M3 | writing | medium | low | document |
| tasks/genealogy-public-records-data-003.json | Backlog | data | low | medium | dataset |
| tasks/genealogy-public-records-data-004.json | Backlog | data | low | medium | dataset |
| tasks/genealogy-public-records-vital-001.json | Backlog | research | low | high | document |
| tasks/genealogy-public-records-gazetteer-001.json | Backlog | data | low | low | dataset |
| tasks/genealogy-public-records-quality-001.json | Backlog | code | low | low | pr |
| tasks/genealogy-public-records-i18n-001.json | Backlog | data | low | low | dataset |
| tasks/genealogy-public-records-sparql-001.json | Backlog | code | low | low | pr |

**Fan-out note:** No fan-out dimensions were applied. The TASKS.md does not explicitly enumerate items within any row (e.g., it does not name specific languages for i18n, specific states for vital-records, or specific census years for each data task beyond the NARA census). One JSON file was generated per backlog row per the fan-out policy.
</content>
