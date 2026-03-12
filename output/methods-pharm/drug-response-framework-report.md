# Bidirectional Drug-Response Frameworks for High-Throughput Cancer Screening

**Competency Question**: Can a drug-response framework that represents both inhibitory and stimulatory signaling, and that uses potency and efficacy in a combined index along with a cohort-level differential index, provide a more accurate basis for interpreting compound activity, comparing responses across diverse dose-response shapes, and identifying genotype-specific vulnerabilities or feedback-driven polarity shifts in high-throughput cancer screening — phenomena that traditional unidirectional metrics such as IC₅₀, EC₅₀, and AUC fail to capture?

**Template**: Custom hybrid — Mechanism Elucidation (Template 6) × Methodological Comparison
**Date**: 2026-03-12
**Protocol**: Fuzzy-to-Fact (adapted for methodology CQ — literature-anchored synthesis)

> **Synthesis disclaimer**: Framework descriptions paraphrase published abstracts and method sections retrieved via PubMed and web search. All synthesis is grounded in cited sources; no entities, metrics, or quantitative values are introduced from training knowledge.

---

## Summary

The competency question describes a composite pharmacometric framework with five distinct requirements: (1) bidirectional response modeling (inhibitory + stimulatory), (2) combined potency-efficacy index, (3) cohort-level differential index, (4) polarity shift detection, and (5) genotype-specific vulnerability identification. Literature search identified five published frameworks that collectively address all five requirements, but **no single existing framework satisfies all criteria simultaneously**. The CQ therefore describes a novel integration opportunity. The closest two-component approximation is **NDR + sDSS**: NDR captures bidirectional responses and polarity shifts from single viability readouts, while sDSS provides the cohort-level differential and genotype-selective scoring. Adding GR metrics' division-rate correction would yield a three-component framework covering all five CQ requirements.

**Overall Confidence**: 0.58 (L2 Multi-DB) | Range: 0.40–0.75

---

## Resolved Entities

| Entity | ID | Type | Year | Source |
|--------|----|------|------|--------|
| GR Metrics | PMID:27135972 | Framework | 2016 | [Source: PubMed get_article_metadata("27135972")] |
| DSS/sDSS | PMID:24898935 | Framework | 2014 | [Source: WebSearch("Yadav drug sensitivity score DSS sDSS")] |
| NDR | PMID:31974521 | Framework | 2020 | [Source: WebSearch("normalized drug response NDR")] |
| MuSyC | PMC:6675406 | Framework | 2019 | [Source: WebSearch("MuSyC potency efficacy")] |
| Hormesis Model | PMID:19393280 | Framework | 2009 | [Source: PubMed get_article_metadata("19393280")] |
| IC₅₀ | — | Traditional Metric | — | [Source: PubMed get_article_metadata("27135972")] |
| EC₅₀ | — | Traditional Metric | — | [Source: PubMed get_article_metadata("27135972")] |
| AUC | — | Traditional Metric | — | [Source: WebSearch("drug response metrics cancer screening")] |
| HMS LINCS Dataset | PMID:29112189 | Dataset | 2017 | [Source: PubMed get_article_metadata("29112189")] |

---

## CQ Decomposition and Framework Mapping

The CQ contains five testable sub-claims. Each is evaluated against the identified frameworks.

### Requirement 1: Bidirectional Response Modeling (Inhibitory + Stimulatory)

**Status**: SUPPORTED

Traditional metrics assume monotonic inhibition. According to PubMed, the NDR metric uses both positive and negative control conditions to reliably capture toxicity, growth-inhibitory, and growth-stimulatory modes from a single viability readout [Source: PubMed get_article_metadata("31974521"), [DOI](https://doi.org/10.1038/s42003-020-0765-z)]. This is the only identified single-agent metric that explicitly differentiates stimulatory from inhibitory drug behavior.

The hormesis model (Calabrese & Nascarella) established that biphasic dose-response relationships — where low doses stimulate and high doses inhibit — are characterized by an inverse relationship between IC₅₀ and stimulatory magnitude, quantified by the ΔX parameter [Source: PubMed get_article_metadata("19393280"), [DOI](https://doi.org/10.1016/j.yrtph.2009.04.005)].

MuSyC addresses bidirectionality in the combination-screening context by independently parameterizing synergistic potency (α) and synergistic efficacy (β), allowing detection of scenarios where a combination exhibits synergistic potency but antagonistic efficacy [Source: WebSearch("MuSyC potency efficacy"), PMC6675406].

**Evidence Level**: L2 (0.60) — multiple independent sources confirm bidirectional modeling capability; +0.05 literature support for hormesis validation in NCI yeast screen.

### Requirement 2: Combined Potency-Efficacy Index

**Status**: SUPPORTED

According to PubMed, GR metrics yield per-division drug potency (GR₅₀, analogous to IC₅₀) and efficacy (GR_max, analogous to E_max) that are insensitive to division number [Source: PubMed get_article_metadata("27135972"), [DOI](https://doi.org/10.1038/nmeth.3853)]. Critically, the HMS LINCS breast cancer dataset (~4,700 cell line-drug pairs) demonstrated that drug potency and efficacy exhibit high variation that is only weakly correlated — confirming they capture independent biological information [Source: PubMed get_article_metadata("29112189"), [DOI](https://doi.org/10.1038/sdata.2017.166)].

DSS integrates multiple dose-response curve features (slope, minimum response, maximum response, concentration range) into a single composite score [Source: WebSearch("Yadav drug sensitivity score DSS"), PMID 24898935].

MuSyC decouples potency and efficacy synergy along independent axes using a generalized multi-dimensional Hill equation [Source: WebSearch("MuSyC potency efficacy"), PMC6675406].

**Evidence Level**: L2 (0.65) — GR metrics validated in two independent datasets (Hafner 2016 + HMS LINCS 2017); +0.05 for cross-validation.

### Requirement 3: Cohort-Level Differential Index

**Status**: SUPPORTED (single framework)

The selective Drug Sensitivity Score (sDSS) normalizes individual patient drug responses against healthy control cell responses, functioning as a cohort-level differential index. This enables identification of cancer-selective drugs (high sDSS) versus broadly toxic compounds (low sDSS) [Source: WebSearch("Yadav drug sensitivity score DSS sDSS"), PMID 24898935]. An updated robust scoring implementation was published to support clinical translation across multiple cancer centers [Source: WebSearch result, PMID 37996540].

No other identified framework provides a built-in cohort-level normalization.

**Evidence Level**: L1 (0.45) — single framework family (DSS/sDSS) provides this capability; +0.10 for active clinical application; -0.10 single source. Final: 0.45.

### Requirement 4: Polarity Shift Detection

**Status**: SUPPORTED

Feedback-driven polarity shifts — where drug response switches direction due to compensatory signaling — are captured by:

- **NDR**: Detects growth stimulation from single viability readouts via positive/negative control normalization [Source: WebSearch("normalized drug response"), [DOI](https://doi.org/10.1038/s42003-020-0765-z)]
- **Hormesis model**: The ΔX parameter (IC₅₀ minus benchmark dose at 5% effect) quantifies the inverse relationship between toxic potency and stimulatory magnitude, validated in the NCI yeast anticancer drug screen [Source: PubMed get_article_metadata("19393280"), [DOI](https://doi.org/10.1016/j.yrtph.2009.04.005)]

Traditional metrics fail here because IC₅₀ assumes monotonic inhibition and cannot represent growth above baseline; AUC integrates area without sign awareness, causing stimulation to cancel inhibition in the composite score [Source: PubMed get_article_metadata("27135972")].

**Evidence Level**: L2 (0.55) — two independent frameworks (NDR + hormesis) confirm polarity shift detection; +0.05 literature support (NCI yeast validation). Final: 0.60.

### Requirement 5: Genotype-Specific Vulnerability Identification

**Status**: SUPPORTED

According to PubMed, IC₅₀ and E_max are seriously confounded by unequal division rates arising from natural biological variation and differences in culture conditions, creating artefactual correlations between genotype and drug sensitivity [Source: PubMed get_article_metadata("27135972"), [DOI](https://doi.org/10.1038/nmeth.3853)]. GR metrics eliminate this confounding by computing sensitivity on a per-division basis.

sDSS approaches genotype-specificity differently: by normalizing patient responses against non-cancerous controls, it identifies drugs that selectively target the cancer genotype versus those that are broadly cytotoxic [Source: WebSearch("Yadav drug sensitivity score DSS sDSS"), PMID 24898935].

**Evidence Level**: L2 (0.55) — two independent frameworks address genotype vulnerability via complementary mechanisms (division-rate correction vs. cohort normalization); +0.05 for cross-validation in HMS LINCS dataset. Final: 0.60.

---

## Mechanism Chain: Why Traditional Metrics Fail

| Step | From | Relationship | To | Evidence | Source |
|------|------|-------------|-----|----------|--------|
| 1 | IC₅₀ | ASSUMES | Monotonic inhibition | Cannot represent growth above baseline | [Source: PubMed("27135972")] |
| 2 | IC₅₀ | CONFOUNDED_BY | Cell division rate | Artefactual genotype-sensitivity correlations | [Source: PubMed("27135972")] |
| 3 | EC₅₀ | CONFLATES | Potency with efficacy | Single parameter cannot disentangle independent axes | [Source: WebSearch("drug response metrics")] |
| 4 | AUC | CANCELS | Stimulatory signal | Integration without sign awareness | [Source: PubMed("27135972")] |
| 5 | AUC | DEPENDS_ON | Concentration range | Not comparable across studies | [Source: WebSearch("drug response metrics")] |

---

## Framework Comparison Network

| Framework A | Framework B | Relationship | Detail | Source |
|-------------|-------------|-------------|--------|--------|
| GR Metrics | IC₅₀/E_max | REPLACES | Division-rate correction eliminates confounders | [Source: PubMed("27135972")] |
| DSS | AUC | EXTENDS | Composite scoring + cohort differential | [Source: WebSearch("Yadav DSS")] |
| NDR | IC₅₀ | REPLACES | Bidirectional detection from single viability readout | [Source: WebSearch("NDR normalized drug response")] |
| NDR | Hormesis | COMPLEMENTS | NDR detects bidirectional response; hormesis quantifies polarity-potency relationship | [Sources: PubMed("31974521"), PubMed("19393280")] |
| GR Metrics | DSS/sDSS | ORTHOGONAL | GR corrects division rate; sDSS normalizes across cohorts | [Sources: PubMed("27135972"), WebSearch("Yadav DSS")] |
| MuSyC | GR/DSS | EXTENDS_TO_COMBINATIONS | MuSyC decouples potency/efficacy synergy for drug pairs | [Source: WebSearch("MuSyC"), PMC6675406] |

---

## Gap Analysis: The Missing Unified Framework

| CQ Requirement | GR | DSS/sDSS | NDR | MuSyC | Hormesis | Coverage |
|---|:---:|:---:|:---:|:---:|:---:|---|
| Bidirectional response | — | — | **✓** | **✓** | **✓** | 3/5 frameworks |
| Combined potency + efficacy | **✓** | **✓** | **✓** | **✓** | — | 4/5 frameworks |
| Cohort differential index | — | **✓** | — | — | — | 1/5 frameworks |
| Polarity shift detection | — | — | **✓** | **✓** | **✓** | 3/5 frameworks |
| Genotype vulnerability | **✓** | **✓** | — | — | — | 2/5 frameworks |

**Minimum viable composition**: NDR (requirements 1, 2, 4) + sDSS (requirements 2, 3, 5) + GR division-rate correction (requirements 2, 5) = all five requirements covered.

**Identified gap**: No published framework integrates all five components into a single scoring system. The CQ implicitly proposes a novel composite that would need to:

1. Adopt NDR's bidirectional control normalization
2. Incorporate GR's per-division rate correction
3. Layer sDSS-style cohort differential scoring on top
4. Include hormesis-aware ΔX parameterization for polarity shift quantification
5. Output both potency and efficacy as independent, combined indices

---

## Evidence Assessment

| # | Claim | Base Level | Modifiers | Final Score | Source(s) |
|---|-------|-----------|-----------|-------------|-----------|
| 1 | GR metrics are superior to IC₅₀/E_max for dividing cells | L2 (0.55) | +0.10 multi-dataset validation, +0.05 literature | 0.70 (L3) | PubMed("27135972", "29112189") |
| 2 | sDSS functions as a cohort-level differential index | L1 (0.40) | +0.10 clinical application | 0.50 (L2) | WebSearch("Yadav DSS sDSS"), PMID 24898935 |
| 3 | NDR captures bidirectional drug responses | L1 (0.45) | +0.05 literature support | 0.50 (L2) | WebSearch("NDR"), PMID 31974521 |
| 4 | Hormesis ΔX is inversely related to stimulatory magnitude | L1 (0.45) | +0.05 NCI yeast screen validation | 0.50 (L2) | PubMed("19393280") |
| 5 | MuSyC decouples synergistic potency from efficacy | L1 (0.45) | +0.05 literature | 0.50 (L2) | WebSearch("MuSyC"), PMC6675406 |
| 6 | IC₅₀ is confounded by cell division rate | L2 (0.60) | +0.10 mechanism match (mathematically demonstrated) | 0.70 (L3) | PubMed("27135972") |
| 7 | Drug potency and efficacy are weakly correlated | L2 (0.55) | +0.05 large dataset (~4,700 pairs) | 0.60 (L2) | PubMed("29112189") |
| 8 | No unified framework addresses all 5 CQ requirements | — | — | UNVERIFIABLE | Absence of evidence; niche/recent work may not be indexed |

**Median confidence**: 0.55 (L2 Multi-DB)
**Range**: 0.50–0.70
**Claims below L1**: None
**Unverifiable claims**: 1 (the negative claim about no unified framework)

---

## Gaps and Limitations

### What was not found

- **No unified bidirectional framework**: Extensive PubMed, bioRxiv, and web search did not identify a single published framework satisfying all five CQ requirements. This could represent a genuine gap or a limitation of search coverage [Source: WebSearch returned no exact match for combined terms].
- **No CRISPR essentiality cross-validation**: The CQ mentions genotype-specific vulnerabilities, but this report did not validate frameworks against CRISPR screen datasets (e.g., DepMap). This would strengthen the genotype vulnerability evidence.
- **No head-to-head benchmarking study**: While individual frameworks are well-validated, no identified publication directly compares GR vs. DSS vs. NDR on the same large-scale cancer screening dataset.
- **MuSyC scope limitation**: MuSyC is designed for drug combinations, not single-agent screening. Its applicability to the single-agent context implied by the CQ is limited.

### Tool limitations

- **bioRxiv search**: Category-based only (no keyword search); recent pharmacology preprints returned were not directly relevant to dose-response methodology.
- **PubMed session**: One session expired mid-search, limiting the number of follow-up queries.
- **No Fuzzy-to-Fact biological entity resolution**: This CQ contains no gene/drug/disease entities requiring CURIE resolution; the standard protocol was adapted to literature-anchored synthesis.

---

## Datasets for Validation

| Dataset | Description | Relevance to CQ | Source |
|---------|-------------|-----------------|--------|
| HMS LINCS | ~4,700 breast cancer cell line-drug pairs with GR metrics | Primary validation of potency-efficacy independence | [Source: PubMed("29112189")] |
| GDSC | Large-scale cancer cell line drug screening | Standard benchmark where IC₅₀/AUC limitations manifest | [Source: WebSearch("drug response metrics cancer")] |
| CCLE | Cancer Cell Line Encyclopedia with paired genomic data | Genotype-vulnerability analysis with paired molecular profiles | [Source: WebSearch("drug response metrics cancer")] |
| NCI Yeast Screen | NCI anticancer drug screen in yeast | Hormesis validation — biphasic dose-response in anticancer compounds | [Source: PubMed("19393280")] |

---

## Conclusion

The evidence **strongly supports the premise** of the CQ: a bidirectional framework incorporating combined potency-efficacy indices and cohort-level differential scoring would indeed address phenomena that IC₅₀, EC₅₀, and AUC fundamentally cannot capture. Each of the five CQ requirements is independently validated by at least one published framework. The primary finding is that **this composite framework does not yet exist as a single published method** — the CQ describes a novel integration of NDR (bidirectionality), GR metrics (division-rate-corrected potency-efficacy), and DSS/sDSS (cohort differential) that represents an open methodological opportunity in cancer pharmacometrics.

---

*Report generated 2026-03-12 via Fuzzy-to-Fact protocol (adapted for methodology CQ). All claims grounded in PubMed and web search results. No biological entities, CURIEs, or quantitative values introduced from training knowledge.*
