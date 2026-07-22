# PLAN — genealogy-public-records

> Status: Draft · Version: 0.2.0 · Last updated: 2026-06-28 · Owner: TBD (maintainer) · Lane: donated

An open, citable corpus of **structured family-history data** — persons, families, life events,
relationships, and the documentary evidence that proves each claim — built **only** from
public-domain and openly-licensed records (NARA federal census, immigration/naturalization,
military service/pension files; Library of Congress; Chronicling America; openly-licensed
transcriptions), about **deceased persons only**, with **provenance and an evidence rationale on
every assertion**, modeled on the open **GEDCOM X** standard and disciplined by the
**Genealogical Proof Standard (GPS)**.

## Executive summary

Family history is one of the most popular research pursuits in the world, yet the structured,
machine-readable data that powers it lives almost entirely inside **proprietary, paywalled
databases** — Ancestry, MyHeritage, FindMyPast, Fold3, Find A Grave, Newspapers.com — whose
terms of use forbid bulk copying and whose compiled databases carry their own copyright. The
*underlying records* (US federal census, naturalization petitions, pension files, public-domain
newspapers) are largely public domain, but they are unstructured, un-linked, scattered across
custodians, and — once digitized by a vendor — frequently re-wrapped behind that vendor's terms.
The result is a paradox: the public's own records are hard for the public to use as data.

This project builds an **openly-licensed, structured family-history corpus** from public-domain
sources **only**, where **every assertion carries (a) a page/image-level provenance citation to a
specific public-domain document and (b) a short evidence rationale** consistent with the
Genealogical Proof Standard. It is expressed in the open **GEDCOM X** conceptual model (with
GEDCOM 7 and JSON-LD exports) so it interoperates with existing genealogy tools and the wider
linked-data web, and it reconciles persons/places against **Wikidata (CC0)** from the start.

Two constraints define this project's identity and dominate everything else:

1. **Licensing / terms-of-use.** **Scraping, harvesting, or bulk-copying any proprietary genealogy
   database (Ancestry, MyHeritage, FindMyPast, Fold3, Find A Grave, Newspapers.com, Geni, or
   similar) is OUT OF SCOPE and never acceptable** under Hee-Lee Oss guardrails — including any
   vendor-wrapped *scan or index* of an otherwise-public-domain record. Data is seeded exclusively
   from records served as public domain by their public custodian (primarily **NARA** and the
   **Library of Congress**) and from explicitly openly-licensed transcriptions.
2. **Privacy / living persons.** Genealogy is uniquely dangerous for privacy: a "mother's maiden
   name," date of birth, and birthplace are *literally* identity-verification secrets, and recent
   records routinely name living relatives. This project covers **deceased persons only**, applies a
   conservative **deceased-determination policy** (the "100-year rule" plus positive death
   evidence), **excludes Social Security Numbers and Death-Master-File data**, and provides a
   **takedown path** for any living person inadvertently included. Identity-theft enablement is a
   first-class abuse vector we design against (see §14).

A third, ethically serious dimension: many public-domain US records (1850/1860 census **slave
schedules**, Freedmen's Bureau records, Indigenous agency rolls) document **enslaved people and
Indigenous communities**. These are handled with dignity, descendant-community consultation, and
the **CARE principles** for Indigenous data governance — never as undifferentiated "rows" (see §7).

Risk tier: **medium** — driven by license/ToS compliance and privacy (living-person exclusion),
with a secondary genealogical-accuracy / citation-review requirement.

## Problem & beneficiaries

**The problem.** The records that document ordinary people's lives are public, but their *structured,
queryable form* is privately owned. Genealogists, historians, students, and the descendant
communities the records describe must either pay recurring subscriptions or hand-transcribe
scattered archives. There is no open, GEDCOM-native, evidence-cited corpus they can download,
build on, or audit. Even where the underlying record is public domain (e.g., a 1900 census page
held by NARA), the convenient digitized copy is often behind a vendor's terms, and the *data* —
names, ages, relationships, places normalized into fields — exists only inside proprietary systems.

**Beneficiaries.**
- **Family historians / genealogists** — especially those who cannot afford recurring subscriptions,
  or who need data they are *allowed* to reuse, cite, and redistribute.
- **Historians, demographers, and social scientists** needing queryable, citable, source-linked
  microdata about past populations.
- **Educators and students** (K-12 and university) wanting free, trustworthy primary-source history
  and a real-world dataset to learn evidence reasoning on.
- **Descendant communities** — including African-American and Indigenous researchers tracing
  ancestors who appear in enslavement-era and agency records — who are currently under-served by
  commercial products and for whom dignity and data sovereignty matter most.
- **Downstream open projects** (Wikidata, library/archive linked-data, digital-humanities and OER
  projects) that can reconcile against and reuse the corpus.

**Verified need.** The *gap* (no open, evidence-cited, GEDCOM-native family-history corpus exists)
is real and demonstrable. However, a **named partner organization or steward who will adopt, host,
cite, and last-mile the output is TO BE SECURED.** `verifiedNeed` is recorded **`false`** on tasks
until that partner is in hand — honest per Hee-Lee Oss, where the bar is *delivered, not merged*. Securing
a steward is a first-class M0/M1 objective and a precondition for declaring the project *shipped*.

**Partner org.** TO BE SECURED. Candidate stewards: a public library or genealogical society's
special-collections / linked-data team; a university digital-humanities center; a state archive or
historical society; the **FamilySearch / GEDCOM** standards community; an African-American or
Indigenous genealogy/heritage organization for the sensitive-records track. **No partner is
asserted that does not exist.**

**Minimum viable steward (so "shipped" is testable).** A qualifying steward is a named, non-profit
or public-interest organization that (1) publicly hosts or links the corpus, (2) commits to cite it
in at least one finding aid, lesson, exhibit, research output, or catalog, and (3) names a contact
who confirms the data met a real need. A single enthusiastic individual is *not* sufficient; a
proprietary/commercial reuser is *not* a qualifying steward.

**What success looks like for a beneficiary.** A descendant researcher downloads the GEDCOM 7 export,
finds a great-great-grandmother enumerated in the 1900 census, sees *exactly which page image*
attests her birth year and *why* the corpus reports 1868 rather than 1869 (two records disagree; the
rationale is recorded), reuses the data freely in their own openly-licensed tree, and never pays a
subscription to do so. That end-to-end experience — free, cited, auditable, reusable — is the deed.

## Goals and non-goals

**Goals**
- Publish an openly-licensed, queryable family-history corpus with **provenance + an evidence
  rationale on every assertion**, expressed in the open **GEDCOM X** model.
- Define a reusable, standards-aligned profile: Person, Name, Fact (life event), Relationship,
  PlaceReference, SourceDescription, and an Evidence/Conclusion layer mapping to **GPS**.
- Seed exclusively from public-domain sources, starting with one **NARA US federal census** batch,
  reconciling persons and places against **Wikidata (CC0)**.
- Enforce a **deceased-only** scope through an auditable deceased-determination policy, and provide
  a documented **living-person takedown** path.
- Represent **conflicting evidence faithfully** (two census ages that disagree) rather than
  flattening it into a single fabricated "fact."
- Provide **GEDCOM 7 + GEDCOM X JSON + JSON-LD** exports and a simple, no-account public explorer.
- Handle enslavement-era and Indigenous records with **dignity, consultation, and CARE principles**.
- Secure at least one educational/historical/heritage **steward** that adopts and cites the corpus.

**Non-goals**
- **Not** a scrape, mirror, or re-publication of Ancestry, MyHeritage, FindMyPast, Fold3, Find A
  Grave, Newspapers.com, Geni, or any proprietary genealogy database or vendor-wrapped scan/index.
- **Not** a database of **living people** — no living persons as data subjects, ever.
- **Not** a place to store Social Security Numbers, full SSNs, or Death Master File / SSDI data.
- **Not** an authority that adjudicates contested lineage; it records sourced claims with provenance
  and surfaces disagreement under GPS, leaving the conclusion auditable.
- **Not** a DNA / genetic-genealogy product — **no genetic data** is collected, stored, or linked.
- **Not** a general-purpose genealogy *platform*, user-tree hosting, account system, or for-profit
  product; **not** a "build your family tree" consumer app.
- **Not** original research that invents facts or AI-"fills" unknown ancestors; gaps stay gaps.

## Success metrics (outcomes)

Outcome-based and beneficiary-centric. Baselines are zero at project start unless noted. We
explicitly **do not** treat raw person count, PRs merged, or commits as success — a large corpus
with weak provenance or a single living-person leak is a **failure** under this plan.

| Metric | Baseline | Target (first 12 months) |
| --- | --- | --- |
| Person records published with ≥1 cited public-domain source | 0 | ≥ 5,000 (quality-gated, not a vanity target) |
| Share of published assertions carrying a resolvable page/image-level provenance citation | n/a | **100%** (hard CI gate; un-sourced assertions never publish) |
| Share of published assertions carrying a GPS-style evidence rationale | n/a | **100%** (every Conclusion records its reasoning + any conflict) |
| Confirmed living persons published | 0 | **0** (hard invariant; any single occurrence is a P0 incident) |
| SSN / Death-Master-File fields published | 0 | **0** (excluded by schema; CI rejects) |
| Distinct public-domain source collections integrated | 0 | ≥ 4 (e.g., census + naturalization + pension + Chronicling America obituaries) |
| Person records reconciled to a Wikidata QID (where a notable match exists) | 0 | tracked; no forced matches (precision over coverage) |
| Dedup/merge precision (false-merge rate in audit sample) | n/a | **zero confirmed false merges** in the audit sample |
| Conflict-faithful representation (contested facts retaining ≥2 sourced variants) | n/a | 100% — no single-winner conclusion ships without a sourced rationale |
| Citation-audit pass rate (stratified sample verifies against cited source) | n/a | ≥ 95% (sampling frame below) |
| Deceased-determination audit (sampled records correctly classified deceased) | n/a | **100%** correct in the audit sample (false "deceased" → living leak) |
| Educational/historical/heritage stewards adopting or citing the corpus | 0 | ≥ 1 committed steward; ≥ 1 documented citation/use |
| Takedown SLA (median time to remove a verified living-person report) | n/a | ≤ 72 hours from verified report |
| Reuse: external downloads / queries of exports | 0 | tracked from M2; growth trend reported quarterly |

**Citation-audit sampling frame.** The ≥95% target uses: a **minimum sample of 200 assertions per
release** (or the whole release if smaller); **stratified sampling** across strata defined by
*source batch* and *extraction method* (human transcription vs. OCR vs. structured import); and an
**auditor independent of the extractor** (no self-grading). The deceased-determination and
dedup-precision audits each draw their own stratified samples.

## Scope

**In scope**
- A GEDCOM X-aligned data profile + an Evidence/Conclusion (GPS) layer.
- A source allow-list with per-source license/PD **and** privacy-embargo determination recorded.
- Structured extraction of persons, names, life events, relationships, places, and source
  descriptions from public-domain records.
- A **deceased-determination** engine and a living-person **exclusion + takedown** workflow.
- Wikidata reconciliation, duplicate detection, and human-reviewed merges (precision-first).
- GEDCOM 7 + GEDCOM X JSON + JSON-LD exports and a simple no-account explorer.
- Sensitive-records handling track (enslavement-era, Indigenous) under dignity + CARE principles.
- Outreach for an adopting steward and for a sensitive-records community advisor.

**Out of scope (explicit)**
- **Any scraping, crawling, API harvesting, bulk download, or re-publication of any proprietary
  genealogy database, or of any vendor-wrapped scan/transcription/index of a public record.** Hard
  refusal under Hee-Lee Oss guardrails, not a deprioritized item.
- **Any data about living persons.** No exceptions, including "probably deceased" without evidence.
- **Social Security Numbers, Death Master File / SSDI data, and any genetic / DNA data.**
- Vital records (birth/marriage/death certificates) **still inside a jurisdiction's privacy
  embargo** — these are excluded until the per-jurisdiction embargo is verified to have elapsed.
- Adjudicating or "certifying" lineage; issuing eligibility determinations (DAR/SAR, citizenship).
- User trees, accounts, hosting of contributor family trees, or commercial packaging.
- AI-generated "fill-in" of unknown ancestors or speculative relationships.

## Solution approach & architecture

A **data/content pipeline** project (with supporting TypeScript tooling), not a hosted service.

**Pipeline stages**
1. **Source intake — licensing + privacy gate.** Each candidate source is logged in a
   machine-readable `sources/allowlist.yml` with: title, custodian, URL, format, license/PD basis,
   **the custodian that actually serves the copy** (must be the public custodian, not a vendor),
   **privacy-embargo determination** (e.g., census 72-year rule satisfied; vital-record embargo
   elapsed per jurisdiction), extraction-method policy, sensitivity flag (enslavement-era /
   Indigenous), and `status: approved | rejected | pending`. **Nothing is processed until
   `approved`** by the license/ToS reviewer **and** (where applicable) the privacy reviewer.
2. **Data-profile layer.** A documented GEDCOM X profile (`profile/`): Person, Name, Fact,
   Relationship, PlaceReference, SourceDescription, plus an **Evidence → Conclusion** layer encoding
   GPS reasoning, published as JSON Schema/SHACL + a human-readable spec, with explicit
   schema.org/Wikidata mappings.
3. **Extraction.** Per-source extractors turn records into typed evidence. A **per-source
   extraction-method policy** governs how each source is read: **human transcription** for
   handwritten manuscripts (census enumerator sheets, pension depositions); **OCR only for
   machine-print public-domain text** (printed indexes, copyright-expired books, Chronicling America
   newspaper OCR, which ships with its own OCR text). Each method is recorded into provenance. A
   documented **transcription-accuracy baseline** (field-level acceptance threshold per method) gates
   a batch before normalization. Extraction is **assistive**; ambiguous/illegible fields are flagged
   for human review, **never guessed**.
4. **Deceased determination + privacy filter.** Before any person is admitted, the
   deceased-determination engine classifies them; **anyone not provably deceased is excluded.** Any
   prohibited field (SSN, DMF) is stripped. (Policy in §7.)
5. **Normalization & provenance/evidence binding.** Records map to GEDCOM X entities; **every
   assertion is wrapped with (a) a provenance citation** (source document IRI + page/image locator +
   license + extraction method + confidence) **and (b) an evidence rationale** (the GPS Conclusion
   note), using a named-graph / reification pattern (decision in M0). The mechanism **defines the
   countable "assertion"** the CI gate measures.
6. **Reconciliation & dedup.** Candidate Wikidata matches and within-corpus duplicates are proposed
   via an explicit **blocking-key strategy** (e.g., normalized surname + birth-decade + birthplace
   region) and **confirmed by a human reviewer** before merge. Matcher tuned for **precision over
   recall**: the bar is **zero confirmed false merges** in audit; merges are reversible and retain
   both records' provenance. (Conflating two different real people is both a data error *and* a
   privacy risk.)
7. **Validation.** CI gates: schema/SHACL validation; **100% provenance** completeness; **100%
   evidence-rationale** presence; **no prohibited fields (SSN/DMF/genetic)**; **deceased-only**
   invariant; **allow-list-only sources**.
8. **Publish.** Versioned **GEDCOM 7 + GEDCOM X JSON + JSON-LD** dumps, a query surface, and a
   simple no-account explorer that shows every assertion's source and rationale.

**Tech stack**
- Tooling/extractors/validators: **TypeScript, ESM, pnpm** (Hee-Lee Oss conventions); `pnpm build && pnpm
  test && pnpm lint` must pass.
- Interchange: **GEDCOM X (JSON)** as the conceptual model; **GEDCOM 7** export for tool
  compatibility; **JSON-LD / RDF (Turtle)** for the linked-data web; identifiers as dereferenceable
  IRIs.
- Validation: **JSON Schema + SHACL** shapes + a custom provenance/evidence/privacy linter in CI.
- Reconciliation: Wikidata reconciliation API or a scripted matcher; human review via a worklist.
- Places: a gazetteer via **Wikidata (CC0)** / **GeoNames (CC BY 4.0, attribution)**; **if OpenStreetMap
  is used, ODbL share-alike + attribution obligations apply** and the derived place set is labeled
  accordingly (see §7).
- Explorer: a static-site explorer over the exports; **no accounts, no visitor PII**; targets
  **WCAG 2.1 AA** accessibility (keyboard navigable, semantic markup, sufficient contrast) so the
  public benefit is usable by people with disabilities.

**Engineering conventions (Hee-Lee Oss).** TypeScript/ESM, pnpm workspaces; `pnpm build && pnpm test &&
pnpm lint` must pass; commits signed off per the DCO (`git commit -s`); a changeset accompanies any
user-facing change. **Lane: donated** — the human runs their own agent; the CLI never runs headless
or authenticates an agent. Any future **funded** (metered) extraction run must move to
`packages/runner`, declare a hard per-task `fundedBudgetUsd` cap, and never exceed its escrow.

**Geographic scope boundary (deliberate).** M0–M3 are **US-record-centric** (NARA federal records,
LoC, Chronicling America), because their public-domain status and the 72-year census rule are
well-understood. Records from other countries have **different copyright terms, privacy regimes, and
embargo periods** and are **out of scope until separately license/privacy-reviewed** — explicitly
*not* assumed to follow US rules. International expansion is a post-M3 question, not a silent default.

**Data model (core GEDCOM X-aligned entities)**
- **Person** — a deceased historical individual (subject); aligned to `schema:Person` / Wikidata `Q5`.
- **Name** — name forms/parts with date/place context (supports name variants across records).
- **Fact** — a life event/attribute (birth, death, marriage, residence, occupation, census
  enumeration), each dated/placed where evidenced.
- **Relationship** — couple / parent-child, modeled as qualified relations to support
  source-conflicting kinship claims.
- **PlaceReference** — normalized place linked to a gazetteer (Wikidata/GeoNames).
- **SourceDescription** — the source record (census sheet, pension file, naturalization petition,
  obituary), carrying its license/PD status, custodian, and page/image locator.
- **Evidence / Conclusion** — the GPS layer: each Conclusion links the Evidence items that support
  it and records the analysis + any conflict resolution. This is what makes the corpus *citable*,
  not just *populated*.

**Key decisions (to ratify in M0)**
- **Provenance + evidence mechanism & assertion unit:** named graphs + PROV-O reification vs.
  RDF-star — pick one, apply uniformly; the choice **defines the countable assertion** the
  100%-provenance/evidence CI gates measure.
- **IRI scheme & persistence:** stable dereferenceable IRIs under a **host-independent persistent
  identifier** (w3id.org or a PURL), decoupled from the (unsecured) steward, redirecting to whatever
  host serves the corpus — so identifiers survive a steward change and no IRI is minted under a host
  we do not control.
- **Conflict representation:** every contested fact retains all sourced variants; no single "winner"
  without a sourced GPS rationale.
- **Deceased-determination thresholds:** the exact rule values (the "100-year" cutoff, what counts
  as positive death evidence) are ratified and versioned in M0 (§7).

## Data, licensing & compliance

**This is the headline gate for the project. Read this section before doing any data work.**

### Hard boundary — proprietary systems are out of scope
A corpus **scraped, harvested, or bulk-copied from any proprietary genealogy database** — including
**Ancestry, MyHeritage, FindMyPast, Fold3, Find A Grave, BillionGraves, Newspapers.com, Geni**, or
similar — would **violate their terms of use and the copyright in their compiled databases**, and is
**OUT OF SCOPE and never acceptable** under Hee-Lee Oss's license/privacy guardrails. This applies to:
- Automated scraping, crawling, or API harvesting of any such system.
- Re-publishing their record entries, indexes, transcriptions, or compiled-database structure.
- Using a **vendor-wrapped scan or index of a public-domain record** (e.g., a census image hosted
  under Fold3/Ancestry terms): the underlying record is PD, but *that copy* carries the vendor's
  restrictions. We use the copy served by the **public custodian** (NARA Catalog, LoC) instead.
- Laundering such data through an intermediary export, a "user-contributed" tree, or a third party.

A compiled database can carry copyright in its selection/arrangement even where the individual
underlying facts are public; ToS can independently bar copying. We assume both protections apply and
**do not touch these systems** absent an explicit written agreement.

**Closing the citation-laundering hole.** A contributor could copy a proprietary entry and back-fill
a plausible public-domain citation. Four controls make that detectable and unacceptable:
- **Page/image-level citations required** — a citation must point to the specific page or document
  image actually consulted at the public custodian, never to a collection or database as a whole.
  Collection-level and "Ancestry/FamilySearch record"-style citations are rejected at review.
- **Public-custodian provenance** — the citation must resolve to a public-custodian copy (NARA, LoC,
  a state archive, Chronicling America), not a vendor URL.
- **Contributor attestation of independent sourcing** — each batch carries a signed attestation that
  facts were read from the cited public-domain source directly, not from a proprietary product.
- **Spot-checks for vendor-distinctive fields** — reviewers sample for tell-tale artifacts of
  proprietary compilations (their distinctive normalizations, derived/editorial fields, index
  arrangement) that would not appear in the raw record; a hit triggers rejection and a flag.

### Approved sources (public domain / openly licensed only)
Every source must be entered in `sources/allowlist.yml` with a recorded license/PD basis, a
privacy-embargo determination, and `approved` status before use:
- **NARA — US federal census** (1790–1950). US federal government works → **public domain**; released
  under the **72-year rule** (the 1950 census opened in 2022). **Hard precondition:** use the copy
  served by NARA itself (e.g., the NARA Catalog / 1950census.archives.gov), **not** a Fold3/Ancestry
  copy. **Slave schedules (1850/1860)** are sensitive — see below.
- **NARA — immigration/naturalization** (passenger arrival lists, naturalization petitions) and
  **military service/pension/bounty-land** files — federal PD; verify the specific digitization's terms.
- **Library of Congress** — public-domain manuscript and print collections (verify per item).
- **Chronicling America (LoC/NEH)** — public-domain historic newspapers (obituaries, marriage/death
  notices); ships OCR text; verify per-title rights (some recent titles are not PD).
- **DPLA / state archives / state historical societies** — public-domain records; **rights vary per
  item** — verify each, do not assume aggregator = PD.
- **Openly-licensed transcription projects** (e.g., USGenWeb-style volunteer transcriptions, FreeBMD,
  WikiTree CC BY-SA) — **only where the project's license explicitly permits reuse/derivatives**;
  record the exact license; CC BY-SA triggers share-alike obligations.
- **Wikidata (CC0)** and **Wikipedia (CC BY-SA 4.0)** — for reconciliation, QIDs, place linking;
  prefer Wikidata (CC0); Wikipedia-derived text triggers CC BY-SA share-alike attribution.
- **GeoNames (CC BY 4.0)** for the place gazetteer (attribution). **OpenStreetMap (ODbL)** only if
  needed — its **share-alike** propagates to any derived place set, which is then labeled ODbL.

**Caveats we will not gloss over:** a public-domain underlying record can be wrapped by a vendor's
copyrighted scan, transcription, or index; "public domain" must be verified for the *specific copy
and edition* used, not assumed from the original's age. Each allow-list entry records this analysis.

### Provenance + evidence model
- **Every assertion** links to its SourceDescription IRI at **page/image-level** granularity and
  records license/PD basis, extraction method (transcription / OCR / structured import), and a
  confidence value — **and** carries a **GPS evidence rationale** (the Conclusion note: what supports
  it, and how any conflict was resolved).
- Un-sourced or un-reasoned assertions are **never published** — enforced as CI gates over the
  countable assertion unit fixed by the provenance-mechanism decision.
- Conflicting sources are retained side by side with their respective provenance.

**Worked example (one assertion = citation + evidence rationale).** For the Fact "Mary Doe born
1868":

```yaml
assertion: { subject: person:mary-doe, fact: Birth, value: "1868" }
provenance:
  source: source:1900-census-ED12-sheet4B-line7   # NARA-served image, page/image-level
  custodian: "U.S. National Archives (NARA Catalog)"
  license: "public-domain (US federal record; 72-year rule satisfied)"
  method: "human-transcription"
  confidence: 0.8
evidence_rationale: >
  1900 census gives age 32 (→ b. ~1868); 1880 census gives age 11 (→ b. ~1869).
  Census ages are approximate; the 1900 schedule asked month/year of birth directly,
  so it is weighted higher. Conflict retained: both source variants are published.
```

An assertion is publishable **only** if both the `provenance` and `evidence_rationale` blocks are
present and the `source` resolves to an `approved`, public-custodian-served record.

**Allow-list entry (machine-readable gate).** Example `sources/allowlist.yml` entry:

```yaml
- id: nara-1900-census
  title: "1900 U.S. Federal Census"
  custodian: "U.S. National Archives and Records Administration"
  serving_custodian: "NARA Catalog (catalog.archives.gov)"   # MUST be public, not a vendor
  url: "https://catalog.archives.gov/..."
  format: "digitized microfilm images (handwritten)"
  license_basis: "US federal government work — public domain"
  privacy_embargo: "72-year rule satisfied (released 1972)"
  extraction_method: "human-transcription"
  sensitivity: "none"                                         # or: enslavement-era | indigenous
  status: "approved"        # approved | rejected | pending — requires license AND privacy sign-off
  reviewed_by: { license: "TBD", privacy: "TBD" }
```

### Privacy / PII stance (genealogy-specific — this is as critical as licensing)
- **Subjects are deceased persons only.** A person is admitted **only** if the
  deceased-determination policy classifies them deceased; **uncertainty → exclusion.**
- **Deceased-determination policy (ratified/versioned in M0):** a person is treated as deceased if
  **(a)** there is positive death evidence (death record/obituary/grave within the corpus's PD
  sources), **or (b)** a documented birth date ≥ **100 years** before the present (the genealogy
  community "100-year rule"). A person known to be **born < 100 years ago without death evidence is
  treated as LIVING and excluded.** Records that merely *mention* living relatives (e.g., a child
  enumerated on a 1950 census who could be alive) have those individuals **suppressed**, not
  published.
- **Excluded fields, always:** Social Security Numbers, **Death Master File / SSDI** data, and any
  **genetic / DNA** data. The schema has no field for them; CI rejects their presence.
- **Identity-theft surface (designed against):** date of birth, birthplace, and **mother's maiden
  name** are common identity-verification secrets. Because the corpus is deceased-only and suppresses
  living relatives, it does not expose a *living* person's verification triple — but this risk is
  documented and reviewed (§14), and the takedown path exists precisely for edge-case leaks.
- **Deceased-determination decision table (edge cases made explicit):**

  | Situation | Classification | Action |
  | --- | --- | --- |
  | Positive death evidence in a PD source (death record / obituary / dated grave) | Deceased | Admit |
  | Documented birth ≥ 100 years before present, no death evidence | Deceased (presumed) | Admit |
  | Documented birth < 100 years ago, no death evidence | **Living (presumed)** | **Exclude** |
  | Age-only record (e.g., census age 4 in 1950) implying birth < 100 yrs ago | **Living (presumed)** | **Exclude / suppress** |
  | No birth or death evidence at all | **Unknown** | **Exclude (uncertainty → exclude)** |
  | Person clearly historical but record also names a possibly-living child | Mixed | Admit subject; **suppress the child** |

- **Living-person takedown:** a documented, low-friction report→verify→remove workflow with a **≤72h**
  median SLA; removals are logged (without re-publishing the PII) and the source batch re-audited. A
  confirmed living-person publication is a **P0 incident**: pull the affected export, root-cause the
  determination failure, re-audit the batch, and record the post-mortem before re-release.
- No collection of **contributor** PII beyond the standard open-source attribution a contributor
  chooses to provide.

### Sensitive records — enslaved persons & Indigenous communities (dignity + CARE)
US public-domain records include **1850/1860 census slave schedules**, **Freedmen's Bureau** records,
and **Indigenous agency/allotment rolls**. These name (or list) people who were enslaved or whose
communities have specific data-governance rights.
- Such sources are flagged `sensitive` in the allow-list and require a **community advisor** (a
  descendant-community or subject-matter reviewer) before extraction.
- Indigenous-related data follows the **CARE principles** (Collective benefit, Authority to control,
  Responsibility, Ethics); a community may request specific handling or non-publication.
- These records are presented with **dignity and historical context**, never as undifferentiated
  rows, and enslaved individuals are documented **as persons**, with their enslavement recorded as a
  historical fact about a system, sourced and contextualized — supporting descendant research rather
  than reproducing dehumanizing framings. **TO BE SECURED:** the community advisor seat.

### Attribution & output license
- **Code:** MIT. **Corpus/content:** **CC0-1.0** where derived purely from public-domain sources;
  **CC BY-SA 4.0** where any CC BY-SA material (Wikipedia, WikiTree) is incorporated; **CC BY 4.0**
  attribution where GeoNames is used; **ODbL** labeling where OpenStreetMap-derived place data is
  included. Mixed derivations are labeled at dataset and, where feasible, statement level.
- Required attributions (CC BY-SA, GeoNames, ODbL, repository acknowledgements) are recorded and
  surfaced.

## Quality, review & risk gates

**Risk tier: medium.** Three review dimensions, all required before a deed is "done":

1. **License/ToS review (primary gate).** Before any source is processed, a reviewer confirms the
   allow-list entry: source is PD/openly licensed, the *specific copy is served by a public
   custodian* (not a vendor wrap), and it is not derived from a proprietary database. Any task that
   proposes touching a proprietary system is **refused and flagged** per Hee-Lee Oss guardrails.
2. **Privacy review (co-primary gate).** A reviewer confirms the **deceased-only** invariant, the
   privacy-embargo determination, exclusion of SSN/DMF/genetic data, suppression of living relatives,
   and the sensitive-records handling. **Any living-person leak is a P0 incident.**
3. **Genealogical-accuracy / citation review.** A reviewer with relevant domain skill samples
   assertions against cited sources and checks the **GPS rationale** (reasonably exhaustive search,
   accurate citation, sound reasoning, conflict resolution); transcription errors, mis-citations,
   invented facts, or missing rationales block sign-off.

**Definition of Shipped (project level).** A published, openly-licensed, queryable family-history
corpus with: **100% provenance and 100% evidence-rationale** on assertions; **deceased-only** with a
clean deceased-determination audit and **zero confirmed living persons**; **no SSN/DMF/genetic**
fields; seeded from ≥4 approved public-domain sources; passing all CI gates; GEDCOM 7 + GEDCOM X JSON
+ JSON-LD exports published; a working **takedown** path (≤72h SLA); and **at least one steward that
has adopted or cited it**. Per Hee-Lee Oss, *delivered ≠ merged* — the data must be in a beneficiary's hands.

**Per-deed Definition of Done.** Acceptance criteria met + CI green (schema/SHACL/provenance/evidence/
privacy/allow-list) + license review passed + privacy review passed + accuracy/citation review passed
+ output published under the declared license. High-stakes sensitive-records tasks add community-advisor
sign-off (treated as a `high`-risk gate for those specific tasks).

## Roadmap & milestones

**M0 — Foundation, licensing & privacy spine (cold-start).**
Goal: establish rails so no data work can bypass the license, privacy, or evidence gates.
Exit criteria: (a) GEDCOM X data profile v0 published (core entities + Evidence/Conclusion layer)
mapped to schema.org/Wikidata; (b) `sources/allowlist.yml` schema defined (license **and** privacy
**and** sensitivity fields) with ≥3 sources analyzed and ≥1 `approved`; (c) provenance+evidence
mechanism ratified, **including the countable assertion unit**; (d) **deceased-determination policy
v1 ratified and versioned** (thresholds + positive-death-evidence definition); (e) CI gates
scaffolded — provenance, evidence, **no-SSN/DMF/genetic**, deceased-only, allow-list-only, SHACL;
(f) host-independent persistent IRI namespace (w3id.org/PURL) committed; (g) steward outreach started
and status logged; (h) a **qualified License/ToS reviewer AND a qualified Privacy reviewer named**
(hard exit; documented fallback if a seat is empty — M0 cannot exit and escalation begins);
(i) living-person **takedown workflow** documented.

**M1 — First sourced slice (proof of pipeline).**
Goal: end-to-end one batch from one approved NARA census source into the corpus, deceased-only, with
full provenance + evidence.
**Hard entry precondition:** the NARA access path for the batch is named and recorded as a PD-clear,
**NARA-served** digitization (not a Fold3/Ancestry copy), with the **72-year rule satisfied**, before
extraction starts.
Exit criteria: (a) ≥1 NARA census batch extracted into Person/Name/Fact/Relationship/Source nodes
(human transcription for handwritten sheets); (b) **deceased-determination applied** — sampled audit
100% correct, **zero living persons**; (c) 100% of new assertions carry provenance **and** evidence
rationale and pass CI; (d) **stratified** citation audit (min sample; by source-batch + method;
independent auditor) ≥95% verified; (e) GEDCOM 7 + GEDCOM X JSON + JSON-LD export produced under the
persistent-IRI namespace; (f) ≥1 candidate steward in conversation. Depends on M0.

**M2 — Evidence linking, reconciliation & explorer (usable surface).**
Goal: make the corpus discoverable, linked, browsable, and evidence-transparent.
Exit criteria: (a) Wikidata reconciliation pass with human-reviewed merges (precision-first, **zero
confirmed false merges** in audit); (b) duplicate-merge workflow operational; (c) public no-account
explorer live, showing **every assertion's source and rationale**, plus documented exports;
(d) takedown path operational and tested end-to-end; (e) reuse metrics tracked. Depends on M1.

**M3 — Scale, sensitive-records track & partner adoption (shipped).**
Goal: broaden sources (incl. the dignity-first sensitive-records track), and lock in real-world use.
Exit criteria: (a) ≥4 source collections integrated and ≥5,000 deceased-only sourced persons;
(b) sensitive-records track piloted **with a community advisor and CARE handling**; (c) ≥1 steward
has **adopted or cited** the corpus (Definition of Shipped met); (d) sustainability/maintenance plan
(incl. takedown + reviewer rotation) in effect. Depends on M2.

## Work breakdown

The itemized, schema-mapped backlog lives in [`TASKS.md`](./TASKS.md), organized by milestone
(M0–M3) plus a sized backlog. Each task maps to a Hee-Lee Oss Task JSON with type, size, risk tier,
deliverable, dependencies, and reviewer. M0 deliberately front-loads the **licensing, privacy, and
evidence** guardrails before any bulk extraction.

## Governance, roles & stakeholders

- **Maintainer / Owner:** TBD — accountable for scope, the licensing + privacy gates, and releases.
- **License/ToS reviewer:** TBD — must approve every allow-list entry; veto over any source. **Naming
  a qualified person is a hard M0 exit criterion.** Fallback if empty: no source advances past
  `pending`, no extraction begins, M0 cannot exit; maintainer escalates to Hee-Lee Oss governance/board
  (and may engage pro-bono counsel) before any data work proceeds.
- **Privacy reviewer:** TBD — owns the deceased-only invariant, embargo checks, SSN/DMF/genetic
  exclusion, and takedown. **Also a hard M0 exit criterion** (co-primary gate); same fallback.
- **Genealogical-accuracy reviewers (rotation):** experienced genealogists/historians who perform
  citation + GPS-rationale review. TO BE SECURED.
- **Sensitive-records community advisor:** descendant-community / subject-matter reviewer for
  enslavement-era and Indigenous records (CARE). **TO BE SECURED** — required before that track runs.
- **Steward (last-mile owner):** the educational/historical/heritage partner who adopts, hosts
  long-term, and cites the corpus. **TO BE SECURED** — required for "shipped."
- **Partner / requestor:** genealogists, historians, educators, descendant communities (diffuse
  beneficiary class); a named representative org is TO BE SECURED.
- **Hee-Lee Oss governance/board:** arbiter for edge cases under the published conflict-of-interest/veto
  checklist (e.g., a borderline source or a sensitive-records dispute).

## Dependencies & integrations

- **NARA** digitized census, immigration/naturalization, military/pension records (verify per-collection terms).
- **Library of Congress** and **Chronicling America** (public-domain print/newspaper sources).
- **DPLA / state archives** (public-domain records; per-item rights).
- **Wikidata / Wikipedia** (reconciliation; CC0 / CC BY-SA); **GeoNames** (CC BY); **OpenStreetMap**
  (ODbL, only if needed) — place gazetteer.
- **GEDCOM X / GEDCOM 7** specifications (interchange model + export); **schema.org / Wikidata**
  vocabularies (alignment).
- **JSON Schema + SHACL** tooling, an RDF/JSON-LD library, a reconciliation client (TypeScript/ESM).
- **Hee-Lee Oss pieces:** Task schema (`packages/schema`), CLI workspace/PR flow (`packages/cli`,
  `packages/core`), governance proposal/registry process. **Donated lane** — humans run their own agents.

## Risks & mitigations

| Risk | Likelihood | Impact | Mitigation | Owner |
| --- | --- | --- | --- | --- |
| Contributor scrapes/imports a proprietary DB (Ancestry/Fold3/Find A Grave), incl. laundered via a fake PD citation | Medium | Critical (legal/ToS, project-ending) | Hard out-of-scope rule; allow-list gate blocks un-approved sources; public-custodian-only provenance; license-reviewer veto; CI rejects non-`approved` sources; anti-laundering controls (page/image citations, public-custodian URLs, contributor attestation, vendor-field spot-checks); refusal + flag | License reviewer |
| **Living person published** (recent record, failed deceased determination, suppressed-relative leak) | Medium | **Critical (privacy)** | Deceased-only invariant; conservative deceased-determination (100-yr + positive death evidence; uncertainty→exclude); suppress living relatives; CI deceased-only gate; deceased-audit 100%; ≤72h takedown; P0 incident response | Privacy reviewer |
| "Public-domain" source actually wrapped in a copyrighted vendor scan/index | Medium | High | Per-copy/edition rights analysis; public-custodian-served copy required; verify the specific digitization, not the original's age | License reviewer |
| SSN / Death-Master-File / genetic data ingested | Low | Critical (privacy/legal) | No schema field for them; CI rejects their presence; excluded by policy; reviewer checklist | Privacy reviewer |
| Vital record used while still inside a jurisdiction's privacy embargo | Medium | High | Per-jurisdiction embargo determination recorded in allow-list; excluded until verified elapsed | Privacy reviewer |
| Enslavement-era / Indigenous records handled without dignity or community consent | Medium | High (ethical/harm) | Sensitivity flag; community advisor required before extraction; CARE principles; dignity-first presentation; non-publication on request | Community advisor |
| No steward/partner secured → "delivered ≠ merged" not met | High | High | Treat steward outreach as M0/M1 deliverable; log status honestly; pursue multiple candidates | Maintainer |
| Inaccurate transcription / mis-citation / invented facts / missing GPS rationale | Medium | High | Mandatory citation + GPS-rationale review; provenance + evidence CI gates; assistive (not autonomous) extraction; gaps stay gaps | Accuracy reviewer |
| Bad/false merges conflate two real people (data error **and** privacy risk) | Medium | High | Precision-over-recall matcher; explicit blocking keys; human-confirmed, reversible merges; zero-confirmed-false-merge audit | Accuracy reviewer |
| Reviewer capacity exhausted (license + privacy + citation review become a bottleneck) | Medium | High | Sampling-based review; reviewer rotation with response-time SLA; documented throughput ceiling throttles intake when backlog exceeds it | Maintainer |
| Identity-theft / re-identification misuse of aggregated data | Low–Medium | High | Deceased-only; suppress living relatives; no SSN/DOB-of-living triple; documented abuse model + takedown | Privacy reviewer |
| CC BY-SA (Wikipedia/WikiTree) or ODbL (OSM) contaminates a CC0 dataset | Medium | Medium | Prefer Wikidata (CC0)/GeoNames; label CC BY-SA/ODbL-derived data; segregate licenses; dataset-level honesty | Maintainer |
| IRI/host instability breaks dereferenceable links | Medium | Medium | Host-independent persistent identifier (w3id/PURL) committed M0; redirect strategy | Maintainer |
| Scope creep into a general genealogy platform / user trees | Medium | Medium | Explicit non-goals; governance review of scope changes | Maintainer |

## Security & privacy

- **Threat surface:** small infra-wise (data/content project, no accounts, no visitor PII), but the
  *data itself* is privacy-sensitive. Primary risks are **compliance** (illicit sourcing),
  **privacy** (living-person leakage, identity-theft enablement), and **data integrity**
  (false/unsourced assertions) — addressed by the license, privacy, and accuracy gates above.
- **Secrets handling:** no API keys/tokens are required for public-domain sources; any reconciliation
  credentials stay out of logs, receipts, and commits per Hee-Lee Oss rules. The donated lane never runs
  headless or authenticates an agent.
- **PII:** deceased persons only; SSN/DMF/genetic data excluded by schema and CI; living relatives
  suppressed; contributor attribution opt-in and minimal.
- **Abuse/misuse prevention:** the corpus is source-linked so every claim is auditable; the project
  **refuses and flags** any attempt to ingest proprietary-system data or to publish living persons.
  No surveillance, profiling, modern-person dossiers, or genetic linkage is supported. Aggregated-data
  re-identification is modeled and mitigated (deceased-only + relative suppression + takedown).

## Sustainability & maintenance

- **After delivery,** the maintainer plus the secured steward own ongoing curation; the steward
  provides long-term hosting, while the **canonical persistent identifier (w3id.org/PURL) is owned by
  the project, not the host** — so identifiers survive a steward change and the corpus is never
  orphaned.
- **Privacy maintenance is permanent:** the **takedown path stays open indefinitely** (≤72h SLA),
  and re-runs of the deceased-determination audit accompany each release (people pass the 100-year
  threshold over time, but new living-relative leaks must never appear).
- **Reviewer sustainability:** review runs on **sampling, not exhaustive re-checking**; reviewers
  work a **rotation with a response-time SLA**; a **documented throughput ceiling** throttles new
  extraction intake when the review backlog exceeds it, so quality/privacy gates never silently
  degrade under load.
- **Outcome tracking:** quarterly report on sourced-person growth, provenance + evidence completeness
  (must stay 100%), deceased-audit results, citation-audit pass rate, takedown SLA adherence, partner
  adoptions, and reuse/downloads.
- **Contributions** continue via the donated lane with the same license + privacy + citation gates.
- **Versioned releases** (with changelogs) keep downstream consumers stable.
- If no steward is secured, the project remains **maintained-but-not-shipped** and the gap is reported
  honestly rather than declared done.

## Open questions

- Who is the committed steward / adopting partner? (TO BE SECURED — blocks "shipped.")
- Who staffs the **Privacy reviewer** and **License/ToS reviewer** seats (both hard M0 exits)?
- Who is the **sensitive-records community advisor** for the enslavement-era / Indigenous track?
- Final provenance+evidence mechanism: named graphs + PROV-O vs. RDF-star? (Fixes the countable
  assertion unit — decided in M0.)
- Deceased-determination thresholds: confirm the **100-year** cutoff and the exact definition of
  "positive death evidence" (ratified/versioned in M0).
- Vital-records strategy: which jurisdictions' embargo rules will be encoded first, and is the corpus
  better served by deferring vital records entirely behind census/naturalization/pension in M1–M2?
- Default corpus license: CC0-only with CC BY-SA/CC BY/ODbL material segregated, or a single license?
- Should DNA/genetic linkage remain a permanent non-goal? (Current stance: yes.)

## References

- Project proposal: `governance/proposals/genealogy-public-records.md` (TO BE CREATED)
- Hee-Lee Oss work rules: `CLAUDE.md`
- Good Deed Definition & risk tiers: `docs/good-deed-definition.md`
- Task JSON schema: `packages/schema/src/schemas.ts`
- Portfolio roadmap: `planning/ROADMAP.md`
- Sibling plan (shared patterns): `planning/projects/revolutionary-patriots-kg/PLAN.md`
- U.S. National Archives (NARA) — federal census (72-year rule), immigration/naturalization,
  military service & pension files
- Library of Congress; Chronicling America (LoC/NEH) historic newspapers
- Wikidata (CC0); Wikipedia (CC BY-SA 4.0); GeoNames (CC BY 4.0); OpenStreetMap (ODbL)
- GEDCOM X and GEDCOM 7 specifications; schema.org vocabulary
- Genealogical Proof Standard (Board for Certification of Genealogists) — methodology referenced
  (the standard is cited as practice; no copyrighted text is reproduced)
- CARE Principles for Indigenous Data Governance (GIDA)
- W3C: RDF, JSON-LD, SHACL, PROV-O, RDF-star

## Glossary

- **GEDCOM X / GEDCOM 7** — open data models/formats for exchanging genealogical data; GEDCOM X is
  the richer conceptual model (Person/Fact/Source/Relationship), GEDCOM 7 the widely-supported file
  format. Used here so the corpus interoperates with existing tools.
- **GPS (Genealogical Proof Standard)** — the genealogy profession's standard for sound conclusions:
  reasonably exhaustive search, complete/accurate citations, analysis & correlation, resolution of
  conflicting evidence, and a soundly reasoned written conclusion. Referenced as *methodology*; no
  copyrighted text is reproduced.
- **72-year rule** — US federal census records are released to the public 72 years after the census
  (1950 census released 2022). The basis for census records being publicly available.
- **Death Master File (DMF) / SSDI** — the Social Security Death Index and the underlying Death Master
  File; access-restricted under the Bipartisan Budget Act of 2013. **Excluded entirely** here.
- **Slave schedules** — 1850/1860 US census schedules enumerating enslaved people (often by age/sex
  under an enslaver). Sensitive; handled under the dignity + CARE track with a community advisor.
- **CARE principles** — Collective benefit, Authority to control, Responsibility, Ethics — Indigenous
  data-governance principles applied to Indigenous-related records.
- **Provenance** — the resolvable, page/image-level citation to the specific public-custodian record
  that attests an assertion.
- **Assertion** — the atomic, countable unit (fixed by the M0 provenance-mechanism decision) over
  which the 100%-provenance and 100%-evidence CI gates run.

## Appendix A — Improvements applied

Twenty-five specific improvements made to the first draft, each **applied** in the sections noted.

1. **Distinct identity vs. the sibling KG.** Reframed away from a Revolution-patriot clone into a
   *general, GEDCOM-native, evidence-cited* family-history corpus (Exec summary, Goals).
2. **GEDCOM X / GEDCOM 7 adopted as the model + export.** Interoperates with real genealogy tools
   instead of inventing a bespoke ontology (Architecture, Exports, Glossary).
3. **Genealogical Proof Standard layer added.** Every Conclusion carries an evidence rationale, not
   just a citation — a 100%-evidence CI gate beside the 100%-provenance gate (Architecture, §7, Metrics).
4. **Deceased-only made a hard, mechanized invariant.** Deceased-determination engine + CI gate +
   audit metric, not just a stated policy (§6 stage 4, §7, M1, Metrics).
5. **Deceased-determination decision table.** Edge cases (age-only records, unknown, mixed records
   with living children) resolved explicitly, all defaulting to exclusion (§7).
6. **SSN / Death-Master-File / genetic data excluded by schema, not policy alone.** No field exists;
   CI rejects their presence (§5, §7, profile-001 AC).
7. **Identity-theft abuse model.** Named the mother's-maiden-name/DOB/birthplace verification-secret
   risk and designed against it (deceased-only + relative suppression) (§7, §14, Risks).
8. **Living-person takedown path with a ≤72h SLA + P0 incident runbook** (§7, Metrics, §15).
9. **Vendor-wrapped-scan hole closed.** "PD record behind Fold3/Ancestry terms" is rejected; the
   *public-custodian-served copy* is required (§7 hard boundary, anti-laundering control #2).
10. **Sensitive-records track (enslavement-era / Indigenous) added** with dignity framing, a community
    advisor, and CARE principles — marked TO BE SECURED (§7, M3, sensitive-001 task).
11. **Per-jurisdiction vital-records embargo handling.** Vital records excluded until the embargo is
    verified elapsed; matrix task in backlog (Scope, §7, vital-001).
12. **Worked single-assertion example** showing citation + evidence rationale + retained conflict (§7).
13. **Machine-readable allow-list entry example** with a `serving_custodian` field forcing public
    custody, plus dual license+privacy sign-off (§7).
14. **Co-primary Privacy reviewer seat** added as a hard M0 exit alongside the License reviewer, with
    a documented empty-seat fallback (§8, §11, M0 DoD).
15. **Precision-first reconciliation reframed as a privacy control** — conflating two real people is
    both a data error and a privacy risk; zero-confirmed-false-merge bar (§6 stage 6, Risks).
16. **Minimum-viable-steward definition** so "shipped" is testable and excludes a lone individual or a
    commercial reuser (§2).
17. **Beneficiary success narrative** grounding the abstract metrics in one descendant's experience (§2).
18. **WCAG 2.1 AA accessibility target** for the public explorer (Architecture).
19. **Explicit US-record-centric scope boundary**, with international records out of scope until
    separately reviewed (not silently assumed to follow US rules) (Architecture).
20. **OSM ODbL share-alike + GeoNames CC BY handled explicitly** for the place gazetteer, with license
    labeling/segregation (Architecture, §7, output licenses, TASKS licenses).
21. **No-DNA permanent non-goal** stated and surfaced as an open question to keep it deliberate (§3, §16).
22. **Hee-Lee Oss engineering conventions wired in** (DCO sign-off, changesets, pnpm gates) and a
    funded-lane budget-cap note for any future metered run (Architecture, TASKS mapping).
23. **Evidence + deceased audits separated from the citation audit** as their own M1 tasks/metrics with
    independent auditors (M1 qa-001/qa-002, Metrics).
24. **Reviewer-burnout sustainability controls** (sampling, rotation+SLA, throughput ceiling) carried
    through to a maintenance task (§15, Risks, sustain-001).
25. **Glossary added** so non-specialist reviewers and contributors can read the plan without prior
    genealogy/linked-data knowledge (Glossary).

## Review sign-off

**Completeness check (against PLAN_SPEC §FILE 1).** All 17 required H2 sections are present and in
order: Executive summary; Problem & beneficiaries; Goals and non-goals; Success metrics (outcomes);
Scope; Solution approach & architecture; Data, licensing & compliance; Quality, review & risk gates;
Roadmap & milestones; Work breakdown; Governance, roles & stakeholders; Dependencies & integrations;
Risks & mitigations (table); Security & privacy; Sustainability & maintenance; Open questions;
References. Plus Glossary, Appendix A, and this sign-off. Metadata header present.

**Correctness check.**
- *Licensing:* matches Hee-Lee Oss guardrails — proprietary-DB scraping (incl. vendor-wrapped PD scans) is a
  hard out-of-scope refusal; public-custodian-served PD only; OSM ODbL share-alike and GeoNames CC BY
  obligations are honored; CC0/CC-BY-SA segregation specified. Consistent with `CLAUDE.md`.
- *Privacy:* deceased-only is mechanized (engine + CI + audit + decision table); SSN/DMF/genetic
  excluded by schema; living relatives suppressed; ≤72h takedown + P0 runbook; identity-theft modeled.
  Exceeds the "aggregate/de-identified only" portfolio guardrail by being deceased-only with takedown.
- *Risk tier:* **medium** overall (license + privacy) is correct per `docs/good-deed-definition.md`;
  the sensitive-records track is correctly elevated to **high** with community-advisor sign-off.
- *Schema:* TASKS.md fields map to `packages/schema/src/schemas.ts`; the example Task JSON includes all
  required fields, valid enums (`type`, `lane`, `priority`, `riskTier`, `deliverable`, `tokenEstimate`,
  `status`), `verifiedNeed: false` (no partner secured), and a real `outputLicense`. Donated lane, so
  `fundedBudgetUsd` is correctly omitted; the funded-lane requirement is noted for any future run.
- *Honesty:* every unsecured role (steward, partner, both reviewer seats, community advisor) is marked
  TO BE SECURED; the proposal file is marked TO BE CREATED; no fictional partner is asserted.

**Issues found and fixed during review.**
- Added the `serving_custodian` field to the allow-list example so the public-custody requirement is
  mechanically enforceable, not just prose.
- Made the **Privacy reviewer** a *hard* M0 exit (co-equal with the License reviewer); the first draft
  implied it but did not gate M0 on it. M0 DoD and §8/§11 updated.
- Reconciled the success-metric table with M-level exit criteria (zero confirmed false merges; deceased
  audit 100%/zero living; takedown SLA) so metrics and gates do not drift.
- Flagged vital records as embargo-gated and pushed them behind census/naturalization/pension in
  sequencing, since their privacy rules are the least uniform (Scope, Open questions, vital-001).

**Residual risks accepted (documented, not resolved here):** no steward yet secured (blocks
"shipped"); both reviewer seats and the community advisor unstaffed (block M0 exit / the sensitive
track); final provenance mechanism and deceased-determination threshold values to be ratified in M0.
These are tracked in Open questions and are the human decisions this plan surfaces.

**Verdict:** Plan and backlog are internally consistent, schema-valid, and compliant with Hee-Lee Oss
guardrails. Ready for maintainer review and for staffing the two hard-gate reviewer seats.
</content>
</invoke>
