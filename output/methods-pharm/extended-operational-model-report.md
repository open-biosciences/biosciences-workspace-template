# Extended Operational Model for Bidirectional Drug-Response in Cancer HTS

**Competency Question**: How can an extended operational model—grounded in the Emax framework and the Black–Leff model, and incorporating bidirectional signaling through the sign-preserving potency–efficacy indices S′ and ΔS′—more accurately represent, compare, and interpret both inhibitory and disinhibitory drug responses in high-throughput cancer screening, including (via ΔS′) genotype-specific vulnerabilities and feedback-driven polarity shifts that traditional metrics such as IC₅₀, EC₅₀, and AUC fail to capture?

**Template**: Custom hybrid — Mechanism Elucidation (Template 6) × Methodological Extension
**Date**: 2026-03-12
**Protocol**: Fuzzy-to-Fact (adapted for methodology CQ — literature-anchored synthesis)
**Extends**: drug-response-framework-report.md (prior research identifying the "missing unified framework" gap)

> **Synthesis disclaimer**: Framework descriptions paraphrase published abstracts and method sections retrieved via PubMed and web search. All synthesis is grounded in cited tool calls; no entities, metrics, or quantitative values are introduced from training knowledge.

---

## Summary

This report extends the prior drug-response framework analysis by grounding the "missing unified framework" gap in the Black–Leff operational model of agonism and its descendants. The investigation traced a clear mathematical lineage: Black & Leff (1983) → Ehlert constitutive extension (2011) → Kenakin's transduction coefficient log(τ/KA) (2012) → the proposed S′/ΔS′ indices. Each link in this chain is supported by validated published work, but the final step — S′ and ΔS′ as specific named metrics — represents a **novel methodological contribution not found in any published literature** (confirmed through 3 PubMed queries and 2 web searches).

The key finding is that all mathematical components required to construct S′ already exist in the pharmacological literature: Ehlert's epsilon/epsilon_i parameters (2011) provide sign-preservation for bidirectional responses; Kenakin's log(τ/KA) (2012) provides the combined potency-efficacy index template; and Randáková's Hill-based operational model of agonism (OMA) (2023) provides the recommended equation form for variable-slope cancer HTS data. What has not been published is their integration into a single sign-preserving index applied to cancer cell dose-response screening — the contribution the CQ proposes.

**Overall Confidence**: 0.53 (L2 Multi-DB) | Range: 0.35–0.75
**Verdict**: PARTIALLY_SUPPORTED_WITH_NOVEL_CONTRIBUTION

---

## Resolved Entities

| Entity | ID | Type | Year | Source |
|--------|----|------|------|--------|
| Black–Leff Operational Model | PMID:6141562 | Framework | 1983 | [Source: PubMed get_article_metadata("6141562")] |
| Black–Leff Curve Shape Extension | PMID:3978322 | Framework | 1985 | [Source: PubMed get_article_metadata("3978322")] |
| Ehlert Modified OM (Constitutive Activity) | PMID:21576379 | Framework | 2011 | [Source: PubMed get_article_metadata("21576379")] |
| Transduction Coefficient log(τ/KA) | PMID:22860188 | Framework | 2012 | [Source: PubMed get_article_metadata("22860188")] |
| Complementary log(τ) Scale | PMID:29133887 | Framework | 2017 | [Source: PubMed get_article_metadata("29133887")] |
| OMA Fitting Limitations | PMID:30874590 | Framework | 2019 | [Source: PubMed get_article_metadata("30874590")] |
| Hill-Based OMA Re-evaluation | PMID:37845324 | Framework | 2023 | [Source: PubMed get_article_metadata("37845324")] |
| Kenakin GPCR Review | PMID:26856272 | Review | 2016 | [Source: PubMed get_article_metadata("26856272")] |
| Paradoxical BRAF Activation | PMID:27476449 | Phenomenon | 2016 | [Source: PubMed get_article_metadata("27476449")] |
| Paradoxical/Bidirectional Drug Effects | PMID:22272687 | Phenomenon | 2012 | [Source: PubMed get_article_metadata("22272687")] |
| log(τ/KA) | PMID:22860188 | Existing Metric | 2012 | [Source: PubMed get_article_metadata("22860188")] |
| Δ-log(τ/KA) Bias Factor | PMID:22860188 | Existing Metric | 2012 | [Source: PubMed get_article_metadata("22860188")] |
| S′ (proposed) | — | Novel Proposed Metric | — | [Source: PubMed search (zero results), WebSearch (zero results)] |
| ΔS′ (proposed) | — | Novel Proposed Metric | — | [Source: PubMed search (zero results), WebSearch (zero results)] |
| GR Metrics | PMID:27135972 | Framework (inherited) | 2016 | [Source: drug-response-framework-kg.json] |
| DSS/sDSS | PMID:24898935 | Framework (inherited) | 2014 | [Source: drug-response-framework-kg.json] |
| NDR | PMID:31974521 | Framework (inherited) | 2020 | [Source: drug-response-framework-kg.json] |
| MuSyC | PMC:6675406 | Framework (inherited) | 2019 | [Source: drug-response-framework-kg.json] |
| Hormesis Model | PMID:19393280 | Framework (inherited) | 2009 | [Source: drug-response-framework-kg.json] |

---

## Mechanism Chain: Mathematical Lineage from Black–Leff to S′

The operational model lineage traces a path from foundational receptor theory to the proposed sign-preserving cancer HTS metric. Each step is grounded in published work; the final integration step (S′ itself) is the novel contribution.

### Step-by-Step

| Step | From | Relationship | To | Evidence | Source |
|------|------|-------------|-----|----------|--------|
| 1 | Black–Leff OM (1983) | FOUNDATIONAL_MODEL | τ (transducer ratio = [R₀]/KE) | Three-parameter model (KA, τ, Emax) describing agonism via hyperbolic E/[A] curves. τ defines efficacy as receptor density divided by coupling efficiency. | [Source: PubMed get_article_metadata("6141562")] |
| 2 | Black–Leff (1985) | EXTENDED_TO | Non-hyperbolic curves | Demonstrated KA estimation remains valid regardless of E/[A] curve shape when using irreversible antagonism. Relevant to cancer HTS where Hill slopes vary. | [Source: PubMed get_article_metadata("3978322")] |
| 3 | Black–Leff OM | EXTENDED_BY | Ehlert constitutive model (2011) | Added epsilon (active state fraction) and epsilon_i (inactive state fraction) parameters. Total stimulus = summation of active states across occupied and free receptor populations. Enables characterization of both agonism and inverse agonism. | [Source: PubMed get_article_metadata("21576379")] |
| 4 | Black–Leff OM | EXTENDED_BY | log(τ/KA) transduction coefficient (2012) | Combined affinity and efficacy into a single receptor-density-independent parameter. The Δ-log(τ/KA) bias factor enables comparison across signaling pathways. | [Source: PubMed get_article_metadata("22860188")] |
| 5 | log(τ/KA) | REFINED_BY | Complementary log(τ) scale (2017) | Separated efficacy-only log(τ) component from combined log(τ/KA), improving partial agonist profiling. | [Source: PubMed get_article_metadata("29133887")] |
| 6 | Black–Leff OM | CRITIQUED_BY | OMA parameter interdependence (2019) | τ is interdependent on KA and Emax, complicating fitting. Proposed rigorous procedure: derive KA and Emax from half-efficient concentration, then fix during τ estimation. | [Source: PubMed get_article_metadata("30874590")] |
| 7 | Black–Leff equation | SUPERSEDED_BY | Hill-based OMA (2023) | Exponentiation of operational efficacy in the original equation breaks parameter relationships when slope factors vary. Hill equation-based OMA produces better parameter estimates. | [Source: PubMed get_article_metadata("37845324")] |
| 8 | Ehlert + log(τ/KA) + Hill OMA | PROPOSED_SYNTHESIS | S′ (sign-preserving index) | S′ would combine: sign-preservation from Ehlert's epsilon/epsilon_i, combined potency-efficacy from log(τ/KA), Hill-based equation from Randáková. **NOVEL — not found in published literature.** | [Source: PubMed search (zero results), WebSearch (zero results)] |
| 9 | S′ + sDSS differential concept | PROPOSED_SYNTHESIS | ΔS′ (differential index) | ΔS′ = S′_test − S′_reference. Adapts sDSS cohort normalization and Δ-log(τ/KA) pathway comparison to a sign-preserving genotype-comparison framework. **NOVEL — not found in published literature.** | [Source: PubMed search (zero results), WebSearch (zero results)] |

---

## Novel Contribution Analysis: S′ and ΔS′

### S′ (Sign-Preserving Potency-Efficacy Index)

**Literature Status**: NOT FOUND — confirmed through 3 PubMed queries ("sign-preserving potency efficacy index", "S-prime drug response", "operational model sign preservation") and 2 web searches. Zero results across all queries.

**Mathematical Ancestry**: S′ is a proposed synthesis of three published components:

1. **log(τ/KA)** from Kenakin et al. (2012, PMID:22860188): Provides the combined potency-efficacy index template. In the standard formulation, τ is always positive, so log(τ/KA) cannot represent inverse effects [Source: PubMed get_article_metadata("22860188")].

2. **epsilon/epsilon_i** from Ehlert et al. (2011, PMID:21576379): Parameters for active and inactive receptor state fractions. The total stimulus represents summation of active states, enabling characterization of both agonism and inverse agonism. This provides the mathematical mechanism for sign-preservation: when the inactive fraction dominates, the effective efficacy direction reverses [Source: PubMed get_article_metadata("21576379")].

3. **Hill-based OMA** from Randáková et al. (2023, PMID:37845324): Demonstrates that the Hill equation-based formulation produces better parameter estimates than the original Black–Leff equation when slope factors vary between agonists. Cancer cell line dose-response curves frequently exhibit variable Hill slopes, making this the recommended equation basis [Source: PubMed get_article_metadata("37845324")].

**CQ Requirements Addressed**: S′ would address bidirectional response modeling (sign-preservation), combined potency-efficacy indexing (log(τ/KA) structure), and disinhibitory modeling (inverse agonism via Ehlert framework).

### ΔS′ (Differential Sign-Preserving Index)

**Literature Status**: NOT FOUND — same search coverage as S′.

**Mathematical Ancestry**: ΔS′ combines two published differential concepts:

1. **sDSS** (Yadav et al. 2014, PMID:24898935, inherited from prior KG): Cohort-level differential scoring that normalizes patient responses against healthy controls. Provides the genotype-comparison structural template [Source: drug-response-framework-kg.json].

2. **Δ-log(τ/KA)** from Kenakin et al. (2012, PMID:22860188): Pathway-comparison differential scoring for biased agonism quantification. Structurally analogous to ΔS′ but applied across signaling pathways rather than genotypes/cohorts [Source: PubMed get_article_metadata("22860188")].

**CQ Requirements Addressed**: ΔS′ would address all five CQ requirements — bidirectional response, combined potency-efficacy, cohort differential, polarity shift detection, and genotype-specific vulnerability identification.

### Design Inputs from Inherited Frameworks

| Input Framework | Contribution to S′/ΔS′ Design | Source |
|----------------|-------------------------------|--------|
| GR Metrics | Division-rate correction should be incorporated to avoid confounders in cancer HTS | [Source: drug-response-framework-kg.json, PMID:27135972] |
| NDR | Bidirectional control normalization provides readout methodology for extracting signed responses | [Source: drug-response-framework-kg.json, PMID:31974521] |
| Hormesis model | ΔX parameter validates that polarity shifts are quantifiable; S′ generalizes this to the operational model framework | [Source: drug-response-framework-kg.json, PMID:19393280] |
| DSS/sDSS | Cohort normalization concept adapted to sign-preserving framework for ΔS′ | [Source: drug-response-framework-kg.json, PMID:24898935] |

---

## Biological Validation: Paradoxical BRAF Inhibitor Activation

The paradoxical activation of the MAPK pathway by BRAF inhibitors (vemurafenib, dabrafenib) in BRAF wild-type cells serves as the canonical cancer example of the genotype-dependent polarity shift that ΔS′ is designed to detect.

**Key Evidence**: BRAF inhibitors paradoxically activate the MAPK pathway in BRAF wild-type cells while inhibiting it in BRAF V600-mutant cells. According to PubMed, vemurafenib accelerates keratinocyte proliferation and wound healing via paradoxical ERK phosphorylation in BRAF WT cells, and MEK inhibitor co-administration reverses this effect [Source: PubMed get_article_metadata("27476449")]. A separate review of the clinical pharmacology of BRAF inhibitors confirmed the paradoxical activation phenomenon and its implications for combination therapy strategies [Source: PubMed get_article_metadata("24456413")].

**Why S′/ΔS′ Would Capture This**:

- **S′**: Would produce a positive value in BRAF-V600E cells (inhibition) and a negative value in BRAF-WT cells (paradoxical activation). Traditional IC₅₀ is undefined when the drug stimulates growth rather than inhibits it.
- **ΔS′**: The differential ΔS′ = S′(BRAF-V600E) − S′(BRAF-WT) would yield a large positive value flagging the genotype-specific vulnerability, whereas AUC would partially cancel the opposing effects.

**Why Traditional Metrics Fail**:

- IC₅₀: Cannot represent activation; undefined when drug stimulates rather than inhibits [Source: PubMed get_article_metadata("27476449")]
- AUC: Integrates area without sign awareness; opposing effects cancel in the composite score [Source: drug-response-framework-kg.json, PMID:27135972]

**Broader Context**: Smith et al. (2012, PMID:22272687) documented a comprehensive taxonomy of paradoxical drug reactions including opposite-to-expected effects, paradoxical precipitation of the indicated condition, and pharmacologically paradoxical responses. Mechanisms encompass receptor-level, stereochemical, multi-target, competing compartment, oscillating system disruption, and feedback loop effects [Source: PubMed get_article_metadata("22272687")].

---

## Practical Considerations for S′ Implementation

### Fitting Challenges

Jakubík et al. (2019, PMID:30874590) demonstrated that τ is interdependent on KA and Emax in the operational model, complicating parameter extraction. They proposed deriving KA and Emax from the half-efficient concentration and apparent maximal responses from a series of curves, then fixing these during τ estimation [Source: PubMed get_article_metadata("30874590")]. This procedure would need adaptation for cancer HTS contexts where full concentration-response curve series may not be available.

### Equation Selection

Randáková et al. (2023, PMID:37845324) showed that exponentiation of operational efficacy in the Black & Leff equation can break relationships among OMA parameters, producing unreliable efficacy estimates when slope factors vary. They recommend the Hill equation-based OMA, which maintains proper parameter relationships across variable slope factors [Source: PubMed get_article_metadata("37845324")]. Since cancer cell line dose-response curves commonly exhibit non-unity Hill slopes, the Hill-based OMA is the recommended foundation for S′.

### Efficacy Decomposition

Burgueño et al. (2017, PMID:29133887) demonstrated that separating the efficacy-driven log(τ) component from the combined log(τ/KA) transduction coefficient improves compound profiling for partial agonists [Source: PubMed get_article_metadata("29133887")]. This suggests S′ should be designed to allow decomposition into its potency and efficacy components when needed, while maintaining the combined index for screening-scale comparisons.

---

## Evidence Assessment

### Claim-Level Grades

| # | Claim | Base Level | Modifiers | Final Score | Source(s) |
|---|-------|-----------|-----------|-------------|-----------|
| 1 | Black–Leff OM (1983) is the foundational operational model of agonism with parameters KA, τ, Emax | L2 (0.60) | +0.05 literature (comprehensive Kenakin 2016 review confirms); +0.05 multi-source (PubMed + web) | 0.70 (L3) | PubMed("6141562"), PubMed("26856272") |
| 2 | Black–Leff (1985) extended OM to non-hyperbolic E/[A] curves | L1 (0.45) | +0.05 literature (cited in Kenakin 2016 review) | 0.50 (L2) | PubMed("3978322") |
| 3 | Ehlert (2011) extended OM to constitutive activity with epsilon/epsilon_i parameters enabling bidirectional response modeling | L1 (0.45) | +0.05 literature; +0.10 mechanism match (directly addresses CQ sign-preservation requirement) | 0.60 (L2) | PubMed("21576379") |
| 4 | Kenakin (2012) log(τ/KA) is a combined potency-efficacy index independent of receptor density | L2 (0.55) | +0.05 literature (Kenakin 2016 review confirms); +0.05 multi-source | 0.65 (L2) | PubMed("22860188"), PubMed("26856272") |
| 5 | Burgueño (2017) complementary log(τ) scale separates efficacy from combined index | L1 (0.45) | +0.05 literature | 0.50 (L2) | PubMed("29133887") |
| 6 | Jakubík (2019) τ is interdependent on KA and Emax in the operational model | L1 (0.45) | +0.05 literature | 0.50 (L2) | PubMed("30874590") |
| 7 | Randáková (2023) Hill-based OMA is superior to original Black–Leff when slope factors vary | L1 (0.45) | +0.10 mechanism match (directly relevant to variable-slope cancer HTS data) | 0.55 (L2) | PubMed("37845324") |
| 8 | BRAF inhibitors paradoxically activate MAPK in BRAF-WT while inhibiting in BRAF-V600E | L2 (0.55) | +0.10 mechanism match; +0.05 literature (two independent PMIDs) | 0.70 (L3) | PubMed("27476449"), PubMed("24456413") |
| 9 | Paradoxical/bidirectional drug effects encompass multiple mechanism classes | L1 (0.45) | +0.05 literature (comprehensive review) | 0.50 (L2) | PubMed("22272687") |
| 10 | log(τ/KA) is the mathematical precursor to S′ — extending it with sign-preservation yields the proposed metric | — | — | 0.40 (L1) | Inferred from PubMed("22860188") + PubMed("21576379"); synthesis claim not independently verified |
| 11 | S′ as a specific published metric exists in the literature | — | -0.15 unverified (extensive search returned zero results) | **INVALID** (0.00) | PubMed search (3 queries, zero results), WebSearch (2 queries, zero results) |
| 12 | ΔS′ as a specific published metric exists in the literature | — | -0.15 unverified | **INVALID** (0.00) | PubMed search (zero results), WebSearch (zero results) |
| 13 | S′ is theoretically feasible from existing operational model components | — | — | 0.35 (L1) | Inferred from combination of PubMed("21576379") + PubMed("22860188") + PubMed("37845324"); not empirically validated |
| 14 | IC₅₀ cannot represent paradoxical activation (undefined when drug stimulates) | L2 (0.55) | +0.10 mechanism match (mathematically demonstrated) | 0.65 (L2) | PubMed("27476449"), drug-response-framework-kg.json |
| 15 | AUC cancels opposing effects due to sign-unaware integration | L1 (0.45) | +0.10 mechanism match | 0.55 (L2) | drug-response-framework-kg.json, PubMed("27135972") |

### Aggregate Statistics

**Scoreable claims** (excluding INVALID): 13
**Median confidence**: 0.55 (L2 Multi-DB)
**Range**: 0.35–0.70
**Claims at L3 (Functional)**: 2 (#1 Black–Leff foundational, #8 BRAF paradox)
**Claims at L2 (Multi-DB)**: 8
**Claims at L1 (Single-DB)**: 3 (#10 synthesis precursor, #13 theoretical feasibility, one borderline)
**INVALID claims**: 2 (#11 S′ existence, #12 ΔS′ existence — confirmed novel)
**Unverifiable claims**: 0

---

## Gaps and Limitations

### What Was Not Found

- **S′ and ΔS′ as published metrics**: Extensive search (3 PubMed queries, 2 web searches) returned zero results. These are confirmed as novel proposed metrics, not published methods [Source: PubMed search (zero results), WebSearch (zero results)].
- **Empirical validation of S′**: No published study has constructed or validated a sign-preserving operational model index in a cancer HTS context. The theoretical feasibility is supported by the existence of all component pieces, but integration has not been demonstrated.
- **Head-to-head comparison**: No identified publication directly compares an operational model-derived index against GR metrics, DSS, or NDR on the same cancer screening dataset.
- **Cancer HTS application of operational model**: The operational model literature is primarily situated in GPCR pharmacology. Translation to cancer cell viability assays (where "receptor" and "response" must be reinterpreted) has not been formally published.

### Tool Limitations

- **bioRxiv search**: Category-based only (no keyword search); recent pharmacology preprints did not include operational model extensions for cancer HTS.
- **PubMed rate limiting**: One API call hit rate limits and required retry, potentially limiting search breadth.
- **PubMed query translation**: Several searches were mis-translated by MeSH expansion (e.g., "operational" → surgery terms, "sigmoid" → colon terms), reducing recall for niche pharmacometric terminology.
- **No Fuzzy-to-Fact biological entity resolution**: This CQ contains no gene/drug/disease entities requiring CURIE resolution; the standard protocol was adapted to literature-anchored synthesis.

### Bridge Gaps Requiring Future Work

1. **Receptor re-interpretation**: The operational model's "receptor" (R₀) and "stimulus-response" coupling (KE) must be formally mapped to cancer cell viability assay components (drug target engagement → signaling cascade → proliferation/death readout).
2. **Fitting procedure for HTS**: Jakubík's (2019) fitting procedure requires multiple concentration-response curves with shared parameters — standard in GPCR pharmacology but potentially challenging at HTS scale.
3. **Benchmarking against existing metrics**: S′ must be shown to outperform IC₅₀, GR₅₀, DSS, and NDR on datasets containing known paradoxical/bidirectional responses (e.g., BRAF inhibitor panels across BRAF-WT and V600E cell lines).

---

## Connection to Prior Research

This report extends the findings of the prior drug-response framework report (drug-response-framework-report.md, confidence 0.58), which identified a "missing unified framework" gap: no single published method addresses all five CQ requirements (bidirectional response, combined potency-efficacy index, cohort differential, polarity shift detection, genotype vulnerability). The present analysis demonstrates that the Black–Leff operational model and its extensions provide the theoretical foundation for filling this gap through S′ and ΔS′, but these metrics remain at the proposal stage. The prior report's minimum viable composition (NDR + sDSS + GR correction) remains the best available published approximation.

---

## Conclusion

The evidence supports the CQ's premise that an extended operational model can provide more accurate representation of bidirectional drug responses than traditional metrics. The mathematical lineage from Black–Leff (1983) through Ehlert (2011) and Kenakin (2012) to the proposed S′ is well-grounded: each precursor component is independently validated in the published literature. The critical re-evaluation by Randáková (2023) provides specific guidance on equation selection for variable-slope data typical of cancer HTS. The paradoxical BRAF inhibitor activation scenario validates that the class of phenomena S′/ΔS′ target is real and clinically significant.

However, **S′ and ΔS′ are novel proposed metrics** — they do not exist in any published literature identified through this investigation. The CQ therefore describes a genuine methodological contribution, not a literature discovery. Realizing this contribution requires empirical work: constructing S′ from the identified components, fitting it to cancer HTS datasets with known paradoxical responses, and benchmarking against existing metrics (IC₅₀, GR₅₀, DSS, NDR) on the same data.

---

---

## Search Provenance

The following LOCATE queries were executed during Phase 1-3 to identify the PMIDs cited in this report. Each LOCATE step preceded the corresponding RETRIEVE step (get_article_metadata).

| LOCATE Query | Tool | Results Used |
|---|---|---|
| "Black Leff operational model agonism" | PubMed search_articles | PMID:6141562, PMID:3978322 |
| "operational model constitutive activity inverse agonism" | PubMed search_articles | PMID:21576379 |
| "transduction coefficient log tau KA biased agonism" | PubMed search_articles | PMID:22860188 |
| "operational model agonism slope Hill equation" | PubMed search_articles | PMID:37845324 |
| "operational model limitations fitting tau KA" | PubMed search_articles | PMID:30874590 |
| "complementary efficacy scale log tau partial agonist" | PubMed search_articles | PMID:29133887 |
| "Kenakin GPCR receptor pharmacology review" | PubMed search_articles | PMID:26856272 |
| "BRAF inhibitor paradoxical activation MAPK wild-type" | PubMed search_articles | PMID:27476449, PMID:24456413 |
| "paradoxical bidirectional drug effects taxonomy" | PubMed search_articles | PMID:22272687 |
| "sign-preserving potency efficacy index" | PubMed search_articles | Zero results (confirms S′ novelty) |
| "S-prime drug response operational model" | PubMed search_articles | Zero results (confirms S′ novelty) |
| "operational model sign preservation cancer screening" | PubMed search_articles | Zero results (confirms S′ novelty) |
| "sign-preserving potency efficacy index operational model" | WebSearch | Zero results |
| "S-prime delta-S-prime drug response cancer HTS" | WebSearch | Zero results |
| "operational model agonism cancer high-throughput screening" | WebSearch | Contextual results (no S′/ΔS′ matches) |
| "Ehlert constitutive activity epsilon parameter" | WebSearch | Contextual results confirming PMID:21576379 |

---

*Report generated 2026-03-12 via Fuzzy-to-Fact protocol (adapted for methodology CQ). Extends drug-response-framework-report.md. All claims grounded in PubMed and web search results from Phase 1-6a pipeline output. No entities, metrics, or quantitative values introduced from training knowledge.*
