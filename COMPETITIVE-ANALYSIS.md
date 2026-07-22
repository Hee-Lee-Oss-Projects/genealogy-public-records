# Competitive & Improvement Analysis — genealogy-public-records

Analysis of the Hee-Lee Oss good-deed project `genealogy-public-records` (PLAN.md v0.2.0,
TASKS.md v0.2.0). The project builds an openly-licensed, structured, evidence-cited
family-history corpus (GEDCOM X / GEDCOM 7 / JSON-LD) from public-domain US records
(NARA census, naturalization, pension; LoC; Chronicling America), about **deceased
persons only**, with page/image-level provenance and a Genealogical Proof Standard (GPS)
evidence rationale on every assertion. Two gates dominate: **license/ToS compliance** and
**privacy (no living persons)**, plus a dignity/CARE track for enslavement-era and
Indigenous records.

Web sources are cited inline; the methodology is deliberately adversarial about the
"public records are actually free to reuse" assumption, which is the project's load-bearing
premise and its biggest risk.

---

## 1. Correctness & completeness review of PLAN.md

The plan is unusually rigorous and, on the two headline dimensions, largely correct. The
findings below are where it is strong, where it overclaims, and where it is silent.

**Strong / correct:**

- **The license/ToS boundary is right and well-grounded.** The plan's core insight — that a
  public-domain *record* can be re-wrapped behind a vendor's copyrighted *scan/index/compiled
  database* and ToS — is legally and operationally accurate. Find A Grave's ToS explicitly
  prohibit "Bots, crawlers, spiders, data miners, scraping" and "republication or resale"
  ([Legal Genealogist](https://www.legalgenealogist.com/2012/06/20/grave-terms-of-use/),
  [Find A Grave support](https://support.findagrave.com/s/privacy)). BillionGraves takes a
  sublicensable, transferable, resale-capable license to user submissions
  ([Legal Genealogist, FAG revisited](https://www.legalgenealogist.com/2013/09/16/find-a-grave-revisited/)).
  Even FamilySearch — the largest *free* site — restricts some census images to on-site viewing
  "due to contractual agreements"
  ([FamilySearch wiki, US Census](https://www.familysearch.org/en/wiki/United_States_Census)),
  proving the plan's point that "free to view" ≠ "free to reuse." The `serving_custodian`
  field and the four anti-laundering controls (page/image citation, public-custodian URL,
  contributor attestation, vendor-distinctive-field spot-checks) are the correct mechanical
  enforcement.

- **The privacy-window boundary is concrete and jurisdiction-aware for the US.** The 72-year
  census rule is correctly cited (Public Law 95-416; 1950 census opened 2022 —
  [NARA 1950 Census](https://www.archives.gov/research/census/1950),
  [Census Bureau](https://www.census.gov/library/stories/2022/04/national-archives-releases-1950-census-records.html)).
  The deceased-only invariant, the "100-year rule + positive death evidence" determination,
  the decision table (uncertainty → exclude; suppress possibly-living children named in a
  record), and the SSN/DMF/genetic schema-level exclusion are all concrete, not hand-waved.
  Making "living person published" a P0 incident with a ≤72h takedown SLA is appropriate for
  the identity-theft surface (DOB + birthplace + mother's maiden name) the plan explicitly
  models.

**Gaps / weaknesses (correctness findings):**

1. **The "100-year rule" is a heuristic, not a privacy-window guarantee, and the plan slightly
   conflates two different things.** The 72-year census rule is a *record-release* rule (when
   NARA publishes the page); the "100-year rule" is a genealogy-community *deceased-presumption*
   heuristic. A person enumerated as age 4 on the **1950** census (released 2022, fully in scope
   as a record) was born ~1946 and is very likely **alive in 2026**. The plan does handle this
   in the decision table (age-only record implying birth <100yr → exclude/suppress), but the
   prose occasionally treats "source is past its release window" as if it implied "subjects are
   deceased." These are independent gates and the plan should state explicitly that **passing
   the record-release window never by itself admits a person** — the deceased determination is a
   separate, per-person gate. This is the single most important privacy subtlety and it is only
   implicit.

2. **Vital-records embargo is jurisdiction-specific and the plan defers it correctly but provides
   no concrete encoding.** US state birth/marriage/death embargoes vary widely (e.g., ~72–125
   years for births, far shorter for deaths) and some are statutory, some administrative. The
   plan wisely pushes vital records behind census/naturalization/pension and flags per-jurisdiction
   determination, but there is no schema for *which* jurisdictions, *what* rule values, or who
   verifies them. Recommend an explicit `embargo_rules.yml` per-jurisdiction table with statute
   citations, treated like the allow-list.

3. **Indigenous data sovereignty / CARE is named but under-operationalized, and there is a
   genuine tension the plan does not resolve.** CARE (Collective benefit, Authority to control,
   Responsibility, Ethics) asserts Indigenous communities' *right to govern data about them even
   when that data is legally public/PD*
   ([CODATA Data Science Journal](https://datascience.codata.org/articles/10.5334/dsj-2020-043),
   [GIDA/Wikipedia](https://en.wikipedia.org/wiki/CARE_Principles_for_Indigenous_Data_Governance)).
   This directly **conflicts** with the project's open-data/CC0 default: a record can be legally
   PD and license-clean yet still be something a Tribal Nation asks not to be republished as bulk
   data. The plan says "non-publication on request" and requires a community advisor — correct in
   spirit — but (a) the advisor seat is unstaffed and gates the whole sensitive track, and (b) the
   default is publish-then-remove, whereas CARE's "Authority to control" implies **consult-before-
   publish** for flagged collections. This is a real ethical correctness gap, not just a staffing
   gap.

4. **Transcription accuracy: the threshold mechanism exists but the standard is undefined.** The
   plan mandates a "field-level acceptance threshold per method" and human transcription for
   handwritten sheets (correct — handwritten census/pension cannot be OCR'd reliably), but never
   states a target accuracy or a double-keying / reconciliation protocol. Genealogy transcription
   convention (transcribe-as-written, mark illegible, never "correct" or expand silently) should
   be a written rule; "extraction is assistive, never guessed" is the right principle but needs the
   concrete protocol.

5. **De-duplication / entity resolution is framed well as a privacy control** (a false merge
   conflates two real people = data error *and* privacy harm) with a precision-first,
   human-confirmed, reversible, zero-false-merge bar. This is correct. One gap: the plan does not
   address the **inverse** error (failing to merge → the same person appears as N records), which
   is acceptable for privacy but should be disclosed as a known data-quality limitation so
   downstream demographers don't over-count.

6. **"Not enabling stalking/harm" is addressed for living persons but not for the genealogy-
   specific harm of outing.** Historical records can reveal illegitimacy, adoption, name changes,
   passing, criminality, institutionalization, or hidden ancestry that affects *living* descendants
   even when every named subject is deceased. The plan's adoption/sensitive-records framing touches
   this but should name "harm to living descendants via deceased-subject data" as an explicit
   sensitivity category (alongside enslavement-era and Indigenous), with a takedown ground beyond
   "this person is alive."

7. **Provenance / GPS / conflict-faithfulness are excellent** and arguably the plan's
   differentiator (see §4). The 100%-provenance and 100%-evidence-rationale CI gates over a fixed
   "countable assertion" unit, plus retaining conflicting evidence rather than fabricating a single
   value, exceed what any competitor offers. No correction needed; this is a strength.

8. **Completeness:** all 17 required sections present, schema-mapped, honest about unsecured roles
   (steward, both reviewer seats, community advisor all TO BE SECURED; `verifiedNeed: false`). The
   honesty is a strength, but note the project **cannot exit M0** without two named reviewers and
   **cannot ship** without a steward — so the realistic status is "well-specified but blocked on
   people," which the plan admits.

---

## 2. Competitive landscape

| Project | What it is | Strengths | Weaknesses (vs. this project) |
| --- | --- | --- | --- |
| **FamilySearch** | Largest *free* genealogy site (LDS Church); billions of records, free index/images, GEDCOM X originator | Free; massive scale; runs the GEDCOM/GEDCOM X standards; huge indexing volunteer base | Not open *data* — reuse restricted; some images on-site-only "due to contractual agreements"; single collaborative tree, not evidence-cited microdata you can bulk-download under an open license ([terms](https://www.familysearch.org/en/legal/terms), [US Census wiki](https://www.familysearch.org/en/wiki/United_States_Census)) |
| **Ancestry / MyHeritage / FindMyPast / Fold3** | Commercial subscription databases | Best coverage, OCR, hints, scale | Paywalled; compiled-database copyright + ToS bar bulk reuse; *vendor-wrapped* PD records carry their restrictions; this is exactly what the project refuses ([Ancestry copyright policy](https://www.ancestry.com/c/legal/copyright-policy), [genealogy.com on compilation copyright](https://www.genealogy.com/articles/research/14_cpyrt.html)) |
| **Free UK Genealogy (FreeBMD / FreeCEN / FreeREG)** | Volunteer transcriptions of England & Wales BMD/census/parish | **Genuinely open: CC0**; 295M+ FreeBMD records; charity; open-data principles | Search-first, not a downloadable structured corpus yet (download feature "long-term plan"); UK-only; no GPS evidence layer; transcription-removal agreements complicate bulk reuse ([Free UK Genealogy open-data FAQ](https://www.freeukgenealogy.org.uk/about/opendata/open-data-for-volunteers/), [Wikipedia](https://en.wikipedia.org/wiki/Free_UK_Genealogy)) |
| **USGenWeb / volunteer transcriptions** | County-level volunteer PD transcriptions | Free, public-spirited, often PD | Inconsistent licensing, no schema, no provenance discipline, link-rot; quality varies wildly |
| **Find A Grave / BillionGraves** | Cemetery/memorial crowdsourcing (FAG owned by Ancestry) | Huge cemetery coverage, photos | **ToS prohibit scraping & republication**; BillionGraves takes resale/sublicense rights — both **out of scope** as sources ([FAG ToS](https://support.findagrave.com/s/privacy), [Legal Genealogist](https://www.legalgenealogist.com/2012/06/20/grave-terms-of-use/)) |
| **NARA (National Archives)** | Custodian of US federal census/naturalization/pension; Catalog API | **Authoritative PD source**; open Catalog API (metadata, OCR, bulk download; ~10k queries/mo/key); federal-gov works = PD | Provides *images/metadata*, not structured person/family microdata; no GPS/evidence layer — this project sits *downstream* of NARA, not in competition ([NARA Catalog API](https://www.archives.gov/research/catalog/help/api), [Catalog-API GitHub](https://github.com/usnationalarchives/Catalog-API)) |
| **DPLA** | Aggregator of US library/archive metadata | **Metadata is CC0**; public API + bulk download; standardized RightsStatements.org URIs; JSON-LD | Item-level *metadata* aggregator, not person-level genealogical microdata; rights vary per underlying item — aggregator ≠ PD ([DPLA API codex](https://pro.dp.la/developers/api-codex), [terms](https://dp.la/about/terms-conditions)) |
| **WikiTree** | Free collaborative single-tree, CC BY-SA | Free; privacy-serious (living profiles forced **Unlisted**, no public living people); strong community | CC BY-SA (share-alike — *contaminates* a CC0 corpus if ingested); single-tree conclusions, not source-cited evidence microdata; some site content copyrighted ([WikiTree Privacy](https://www.wikitree.com/wiki/Help:Privacy), [Ownership & Control](https://www.wikitree.com/wiki/Help:Ownership_and_Control)) |
| **GEDCOM X / GEDCOM 7 (gedcom.io)** | Open-ish interchange models | The interoperability lingua franca this project adopts (smart) | GEDCOM X is effectively a **FamilySearch-controlled** standard, not vendor-neutral; the project depends on it but doesn't own it ([Wikipedia GEDCOM](https://en.wikipedia.org/wiki/GEDCOM)) |
| **IIIF** | Image-delivery/annotation standard | **De facto standard** for digitized archives/manuscripts; cross-repository viewers, citation, annotation ([Wikipedia](https://en.wikipedia.org/wiki/International_Image_Interoperability_Framework), [Stanford](https://library.stanford.edu/iiif)) | Images, not data — complementary; the project should *consume* IIIF manifests for image-level provenance, not compete |

**Bottom line:** No existing player offers an **open-licensed, bulk-downloadable, GEDCOM-native,
evidence-cited (GPS), deceased-only, provenance-on-every-assertion** structured *dataset* of US
public-domain records. The closest in spirit (genuinely open) is Free UK Genealogy/CC0, but it is
UK-only, search-first, and has no evidence/conflict layer.

---

## 3. Gaps we can fill

1. **Open, license-clean, structured person/family microdata** from US PD records — the thing that
   today exists only inside paywalled compiled databases. NARA/DPLA give images and item metadata;
   nobody gives reusable Person/Fact/Relationship rows under CC0.
2. **Provenance + GPS evidence rationale on every assertion** — auditable, citable, conflict-faithful
   data. No free or paid competitor publishes the *reasoning*, only the conclusion.
3. **Deceased-only by construction with a takedown SLA** — a privacy posture stronger than the
   commercial products (which routinely surface living relatives) and operationalized in CI.
4. **A dignity/CARE-respecting track for enslavement-era and Indigenous records** — descendant
   communities are explicitly under-served by commercial products; doing this *right* (consult-first)
   is a fillable, high-value gap.
5. **Linked-data interoperability** (JSON-LD + Wikidata QIDs + GeoNames places + persistent w3id/PURL
   IRIs) so the corpus plugs into the open knowledge graph — neither FamilySearch nor Ancestry exposes
   reconcilable, dereferenceable identifiers openly.
6. **A clean, citable corpus for researchers/demographers/educators** who currently cannot legally
   reuse commercial data in publications or OER.

---

## 4. Differentiators to win

- **Evidence-cited, conflict-faithful provenance (the GPS layer).** This is the single strongest,
  hardest-to-copy differentiator: 100% provenance + 100% evidence-rationale as CI gates, retaining
  disagreeing sources rather than fabricating one value. It makes the corpus *trustworthy and
  auditable* in a field full of un-sourced, copied-around trees.
- **License-clean by construction** (public-custodian-served PD only; anti-laundering controls) —
  legally reusable where commercial data is not.
- **Privacy-first, deceased-only with takedown** — an ethical posture no competitor matches.
- **Open + interoperable** (GEDCOM 7 / GEDCOM X / JSON-LD, Wikidata reconciliation, persistent IRIs)
  — meets users in their existing tools instead of locking them in.
- **Dignity/CARE track** — credibility with descendant communities the incumbents ignore.

---

## 5. Claude API leverage

**Where Claude adds force-multiplier value (assistive, human-verified):**

1. **OCR-correction & transcription assist on machine-print PD text** (printed indexes,
   copyright-expired books, Chronicling America OCR): post-correct garbled OCR, segment columns,
   propose field boundaries — *never* as the source of truth for handwritten manuscripts (those stay
   human transcription per the plan).
2. **Structuring / indexing into GEDCOM X entities:** map a transcribed census line or pension
   deposition into Person/Name/Fact/Relationship/PlaceReference + a draft SourceDescription, with the
   page/image locator carried through — turning unstructured text into typed evidence at scale.
3. **Entity-resolution & reconciliation *candidate generation*:** propose Wikidata QID matches and
   within-corpus duplicate candidates with explained rationale and a confidence, feeding a human
   worklist — precision-first, human-confirms-every-merge.
4. **Drafting the GPS evidence rationale** (e.g., reconciling two disagreeing census ages) for human
   review, and flagging illegible/ambiguous fields for human attention rather than guessing.
5. **License/sensitivity *triage* assist:** flag a candidate source as likely vendor-wrapped, likely
   enslavement-era/Indigenous, or likely to name possibly-living individuals — as a *first-pass
   surfacing tool* that routes to the right human reviewer.

**Where Claude must NOT decide (hard human-verified boundaries):**

- **Privacy / living-person determinations** — the deceased classification and any suppression of a
  possibly-living relative must be human-verified; a model false-"deceased" is a P0 leak.
- **Sensitivity determinations** — enslavement-era / Indigenous flagging and especially any
  Indigenous-data-sovereignty (CARE) decision require the community advisor, not the model;
  consult-before-publish, model never overrides.
- **License / ToS approval** — the allow-list `approved` status and the public-custodian provenance
  are a human reviewer's call; Claude may triage but never grant.
- **No fabricated records or people** — gaps stay gaps; the model must never "fill in" an unknown
  ancestor, infer an undocumented relationship, or invent a citation. Citation-laundering detection
  depends on this being inviolable.
- **Transcription of handwritten manuscripts must be human-verified** against the image; model
  transcriptions of handwriting are not admissible as the provenance source.
- **Final merges** — human confirms; the zero-false-merge bar cannot be delegated.

(Per CLAUDE.md: donated lane, the human runs their own agent; any metered Claude run moves to
`packages/runner` with a hard `fundedBudgetUsd` cap.)

---

## 6. Ten concrete optimizations

1. **Separate the two gates explicitly in the schema:** a `record_release_ok` flag (window elapsed)
   AND a per-person `deceased_determination` field — and document that the former never admits a
   person. Closes correctness finding #1.
2. **Add `embargo_rules.yml`** — a per-jurisdiction vital-records embargo table with statute citations
   and a `verified_by` field, mirroring the allow-list gate (finding #2).
3. **Adopt consult-before-publish for CARE-flagged collections**, not publish-then-remove: a `sensitivity`
   value of `indigenous` blocks publication until advisor sign-off, distinct from the general takedown
   path (finding #3).
4. **Consume IIIF manifests for image-level provenance.** Where NARA/LoC expose IIIF, cite the canonical
   IIIF canvas/region as the page/image locator — stable, standard, and machine-verifiable.
5. **Pull directly from the NARA Catalog API + bulk download** (PD metadata/OCR, ~10k queries/mo/key) as
   the M1 ingestion path, and from DPLA's CC0 metadata API — both give license-clean, structured starting
   points and avoid any vendor copy.
6. **Define a written transcription protocol** (transcribe-as-written, mark illegible, no silent
   expansion/correction) and a target field-accuracy + double-key/reconcile rule per method (finding #4).
7. **Disclose the under-merge limitation** in the dataset card so demographers don't over-count distinct
   persons (finding #5).
8. **Add a "harm-to-living-descendants" sensitivity category and takedown ground** (adoption, illegitimacy,
   passing, institutionalization), independent of the living-person rule (finding #6).
9. **Ship a machine-readable dataset card / DCAT + per-statement license labeling** so CC0 vs. CC BY-SA
   (WikiTree/Wikipedia) vs. CC BY (GeoNames) vs. ODbL (OSM) provenance is queryable and CC BY-SA never
   silently contaminates the CC0 core.
10. **Reconcile to Wikidata QIDs from M1 with precision-only matching** and mint a persistent w3id/PURL
    IRI per Person immediately, so identifiers are stable and the corpus is linkable before a steward
    is secured.

---

## 7. Parallel & perpendicular spin-offs

- **loc-public-domain-engine (parallel):** a shared "is this specific copy PD / public-custodian-served?"
  rights-determination engine over LoC/NARA/Chronicling America. Genealogy needs it for its allow-list;
  factor it out as a reusable Hee-Lee Oss component both projects consume.
- **local-history-stubs (perpendicular):** deceased-person Facts (residence, occupation, place) feed
  notable-but-stub local-history figures; conversely local-history place context enriches PlaceReference
  normalization. Share the gazetteer.
- **historical-markers-index (perpendicular):** markers reference named historical persons/events; the
  genealogy corpus can resolve those names to cited Person records, and markers give the index
  evidence-linked biographies. Shared Wikidata reconciliation.
- **An open structured-records dataset (the core deliverable as a standalone reusable asset):** the
  CC0 GEDCOM X / JSON-LD corpus is itself the spin-off — a citable dataset other OER, digital-humanities,
  and library-linked-data projects reconcile against.
- **An MCP server (perpendicular product):** expose the corpus (read-only, deceased-only, source-linked)
  as an MCP server so any agent can query cited public-domain person/family facts with provenance — a
  clean way to let downstream tools consume the data *without* re-exposing living people or unsourced
  claims. Pairs naturally with the Claude leverage in §5.

---

## 8. Open questions

- **Steward & both reviewer seats:** who staffs License/ToS reviewer, Privacy reviewer (hard M0 exits),
  and the committed steward (blocks "shipped")? Realistically the project is blocked on people, not design.
- **Community advisor for the sensitive track:** unstaffed; gates the entire enslavement-era/Indigenous
  track, and CARE implies consult-before-publish — can the project ship M3 without it, or is the track
  deferred?
- **CARE vs. CC0 default:** how is the genuine tension between "legally PD/open" and "Indigenous authority
  to control" resolved as policy, not case-by-case?
- **Vital-records jurisdictions:** which states' embargo rules get encoded first, and is deferring vital
  records entirely behind census/naturalization/pension the right M1–M2 call (plan leans yes)?
- **Provenance mechanism:** named graphs + PROV-O vs. RDF-star — fixes the countable assertion unit
  (M0 decision).
- **Deceased-determination thresholds:** confirm the 100-year cutoff and the exact "positive death
  evidence" definition; and confirm record-release-window and deceased-determination are enforced as
  *independent* gates.
- **Default license:** CC0-only with CC BY-SA/CC BY/ODbL material segregated, or a single license?
- **DNA/genetic linkage:** keep as a permanent non-goal (current stance: yes).

---

### Sources

- FamilySearch [Terms](https://www.familysearch.org/en/legal/terms),
  [Developers/API](https://www.familysearch.org/en/developers/docs/api/resources),
  [US Census wiki (on-site-only restriction)](https://www.familysearch.org/en/wiki/United_States_Census)
- Free UK Genealogy [Open-data FAQ (CC0)](https://www.freeukgenealogy.org.uk/about/opendata/open-data-for-volunteers/),
  [Wikipedia](https://en.wikipedia.org/wiki/Free_UK_Genealogy)
- NARA [Catalog API](https://www.archives.gov/research/catalog/help/api),
  [Catalog-API GitHub](https://github.com/usnationalarchives/Catalog-API),
  [1950 Census / 72-yr rule](https://www.archives.gov/research/census/1950),
  [Census Bureau release](https://www.census.gov/library/stories/2022/04/national-archives-releases-1950-census-records.html)
- DPLA [API codex (CC0 metadata, bulk)](https://pro.dp.la/developers/api-codex),
  [Terms](https://dp.la/about/terms-conditions)
- WikiTree [Privacy (living = Unlisted)](https://www.wikitree.com/wiki/Help:Privacy),
  [Ownership & Control](https://www.wikitree.com/wiki/Help:Ownership_and_Control)
- Find A Grave [ToS/Privacy](https://support.findagrave.com/s/privacy),
  [Legal Genealogist on FAG ToS](https://www.legalgenealogist.com/2012/06/20/grave-terms-of-use/),
  [FAG revisited / BillionGraves license](https://www.legalgenealogist.com/2013/09/16/find-a-grave-revisited/)
- Compiled-database copyright: [genealogy.com](https://www.genealogy.com/articles/research/14_cpyrt.html),
  [Ancestry copyright policy](https://www.ancestry.com/c/legal/copyright-policy)
- GEDCOM / GEDCOM X (FamilySearch-controlled): [Wikipedia](https://en.wikipedia.org/wiki/GEDCOM)
- IIIF (de facto archive standard): [Wikipedia](https://en.wikipedia.org/wiki/International_Image_Interoperability_Framework),
  [Stanford Libraries](https://library.stanford.edu/iiif)
- CARE Principles: [CODATA Data Science Journal](https://datascience.codata.org/articles/10.5334/dsj-2020-043),
  [Wikipedia](https://en.wikipedia.org/wiki/CARE_Principles_for_Indigenous_Data_Governance)
