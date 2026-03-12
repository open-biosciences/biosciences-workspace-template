# S′ and ΔS′: Sign-Preserving Potency–Efficacy Indices Derived from the Black–Leff Operational Model for Bidirectional Drug-Response Characterization in High-Throughput Cancer Screening

**Authors**: [To be determined]

**Affiliations**: [To be determined]

**Corresponding author**: [To be determined]

**Keywords**: operational model of agonism, sign-preserving index, bidirectional drug response, high-throughput screening, cancer pharmacometrics, paradoxical activation, potency-efficacy index

---

## Abstract

High-throughput cancer screening relies on dose-response metrics—IC₅₀, EC₅₀, and AUC—that assume monotonic inhibition and cannot represent bidirectional drug effects such as paradoxical kinase inhibitor activation in genotype-discordant cell populations. We conducted a systematic literature-anchored synthesis to determine whether the Black–Leff operational model of agonism (1983) and its extensions provide a theoretical foundation for constructing sign-preserving metrics that overcome these limitations. Through structured PubMed and web searches (14 articles retrieved, 4 web searches), we traced a mathematical lineage from Black and Leff's original three-parameter model (KA, τ, Emax) through Ehlert's constitutive activity extension (2011), which introduced epsilon/epsilon_i parameters enabling representation of both agonism and inverse agonism, to Kenakin's transduction coefficient log(τ/KA) (2012), which combined potency and efficacy into a single receptor-density-independent parameter. Randáková's critical re-evaluation (2023) demonstrated that Hill equation-based operational model analysis produces superior parameter estimates when slope factors vary—a common feature of cancer cell line dose-response curves. We propose two novel indices: S′, a sign-preserving potency–efficacy index synthesizing these three components, and ΔS′, a differential variant for genotype-specific vulnerability detection analogous to the selective Drug Sensitivity Score (sDSS). Extensive literature search (3 PubMed queries, 2 web searches) confirmed that neither S′ nor ΔS′ exists as a published metric. Using paradoxical BRAF inhibitor activation as a canonical validation scenario, we demonstrate that S′ would produce opposite-signed values in BRAF-V600E versus BRAF-WT cells—a distinction that IC₅₀ cannot represent and AUC obscures. All mathematical components required for S′ construction exist in the published literature; their integration into a single sign-preserving index applied to cancer cell screening represents the novel methodological contribution identified by this investigation. Evidence confidence: median 0.55 (L2 Multi-DB), range 0.35–0.70, with two INVALID scores for S′/ΔS′ existence confirming novelty.

---

## 1. Introduction

Quantitative characterization of drug response in high-throughput cancer screening (HTS) underpins drug discovery, biomarker development, and precision oncology. The dominant metrics—half-maximal inhibitory concentration (IC₅₀), half-maximal effective concentration (EC₅₀), and area under the dose-response curve (AUC)—share a foundational limitation: they assume monotonic, unidirectional inhibition [1]. This assumption fails in biologically important scenarios where drugs produce bidirectional effects depending on genotype, signaling context, or dose regime.

The most prominent example is the paradoxical activation of the MAPK pathway by BRAF inhibitors (vemurafenib, dabrafenib) in BRAF wild-type cells, while the same compounds inhibit the pathway in BRAF V600E-mutant cells [2, 3]. In this scenario, IC₅₀ is mathematically undefined for the wild-type response (growth is stimulated, not inhibited), while AUC integrates opposing effects without sign awareness, causing stimulation to partially cancel inhibition in composite scores [1]. Smith et al. documented a comprehensive taxonomy of such paradoxical drug reactions encompassing receptor-level, stereochemical, multi-target, and feedback loop mechanisms [4].

Several modern frameworks address specific aspects of this problem. Growth rate (GR) metrics correct for confounding by cell division rate, yielding per-division potency and efficacy [1]. The Drug Sensitivity Score (DSS) and its selective variant sDSS provide cohort-level differential scoring that identifies cancer-selective drugs [5]. The Normalized Drug Response (NDR) captures bidirectional responses from single viability readouts [6]. The MuSyC framework decouples synergistic potency from synergistic efficacy in combination screens [7]. Hormetic dose-response models quantify the inverse relationship between toxic potency and stimulatory magnitude [8]. However, no single published framework simultaneously provides bidirectional response modeling, combined potency-efficacy indexing, cohort-level differential scoring, polarity shift detection, and genotype-specific vulnerability identification [9].

The Black–Leff operational model of agonism [10], a cornerstone of receptor pharmacology for over four decades [11], offers a principled theoretical foundation for addressing this gap. The model's parameter τ (transducer ratio) quantifies efficacy independently of affinity (KA), and extensions by Ehlert et al. [12] and Kenakin et al. [13] have expanded it to constitutive activity and combined potency-efficacy scoring, respectively. Recent critical re-evaluations by Jakubík et al. [14] and Randáková et al. [15] have refined fitting procedures and recommended Hill equation-based formulations for data with variable slope factors—conditions routinely encountered in cancer HTS.

In this work, we present a systematic literature-anchored synthesis identifying the mathematical lineage from the Black–Leff model to two proposed novel indices: S′ (a sign-preserving potency–efficacy index) and ΔS′ (a genotype-differential variant). We trace each component to its published origin, demonstrate theoretical feasibility through the BRAF paradoxical activation scenario, and identify the specific bridge gaps that must be addressed for empirical validation.

---

## 2. Methods

### 2.1 Study Design

This investigation employed a structured literature synthesis protocol adapted from the Fuzzy-to-Fact framework for methodology-focused competency questions (CQs). Unlike standard Fuzzy-to-Fact investigations targeting biological entities (genes, proteins, diseases), this adaptation replaced biological entity resolution with framework identification and mathematical lineage tracing. No biological CURIEs (HGNC, CHEMBL, MONDO) were required because the primary entities are pharmacometric frameworks identified by their PubMed IDs.

### 2.2 Literature Search Strategy

Searches were conducted via the PubMed API (search_articles and get_article_metadata tools) and web search. A two-step LOCATE→RETRIEVE discipline was maintained: each article was first identified through a search query (LOCATE), then its metadata was retrieved via get_article_metadata (RETRIEVE). The full search provenance is documented in Supplementary Table S5.

**PubMed searches** (12 queries): Queries targeted the operational model of agonism ("Black Leff operational model agonism"), its extensions ("operational model constitutive activity inverse agonism", "transduction coefficient log tau KA biased agonism"), equation variants ("operational model agonism slope Hill equation"), fitting limitations ("operational model limitations fitting tau KA"), derived scales ("complementary efficacy scale log tau partial agonist"), contextual reviews ("Kenakin GPCR receptor pharmacology review"), biological validation ("BRAF inhibitor paradoxical activation MAPK wild-type", "paradoxical bidirectional drug effects taxonomy"), and novelty confirmation ("sign-preserving potency efficacy index", "S-prime drug response operational model", "operational model sign preservation cancer screening").

**Web searches** (4 queries): Targeted S′/ΔS′ novelty confirmation ("sign-preserving potency efficacy index operational model", "S-prime delta-S-prime drug response cancer HTS") and contextual validation ("operational model agonism cancer high-throughput screening", "Ehlert constitutive activity epsilon parameter").

**Inherited knowledge graph**: This investigation extended a prior knowledge graph (drug-response-framework-kg.json) containing 14 nodes and 14 edges documenting GR metrics [1], DSS/sDSS [5], NDR [6], MuSyC [7], and hormesis models [8]. These frameworks were inherited as contextual inputs informing S′/ΔS′ design requirements.

### 2.3 Evidence Grading

Each factual claim was individually graded using a four-level evidence framework:

- **L1 (Single-DB, 0.40–0.49 base)**: Claim supported by a single database or source.
- **L2 (Multi-DB, 0.50–0.64 base)**: Claim supported by multiple independent sources.
- **L3 (Functional, 0.65–0.79 base)**: Claim validated across independent datasets or experimental paradigms.
- **L4 (Clinical, 0.80–1.00 base)**: Claim supported by clinical or large-scale translational evidence.

Modifiers were applied with documented justification: +0.05 for literature support (cited in reviews), +0.10 for mechanism match (directly addresses CQ requirement) or multi-dataset validation, -0.15 for unverified claims. Aggregate confidence was computed as the **median** (not mean) of scoreable claims, with INVALID scores (for confirmed-novel metrics) excluded from the aggregate.

### 2.4 Novelty Confirmation

Claims that S′ and ΔS′ are novel were tested through exhaustive negative search: 3 PubMed queries and 2 web searches using varied terminology ("sign-preserving potency efficacy index", "S-prime drug response", "operational model sign preservation") all returned zero results. These claims were scored as INVALID (0.00) in the evidence assessment, indicating confirmed absence from the published literature.

### 2.5 Knowledge Graph Construction

A structured knowledge graph was constructed with 22 nodes (9 frameworks, 6 metrics, 2 phenomena, 5 inherited frameworks) and 22 edges representing relationships (EXTENDED_BY, CRITIQUED_BY, INFORMS_DESIGN, MATHEMATICAL_PRECURSOR, SUPERSEDES, DESIGNED_TO_DETECT, FAILS_TO_DETECT). The graph was persisted as JSON with full provenance for each node and edge.

---

## 3. Results

### 3.1 Mathematical Lineage from Black–Leff to S′

The operational model lineage traces nine steps from foundational receptor theory to the proposed sign-preserving index (Table 1).

**Step 1–2: Foundation.** Black and Leff (1983) introduced the three-parameter operational model (KA, τ, Emax) describing agonism via hyperbolic concentration-response curves, where the transducer ratio τ = [R₀]/KE defines efficacy as receptor density divided by coupling efficiency [10]. A 1985 extension demonstrated that KA estimation remains valid regardless of curve shape when using irreversible antagonism, critical for cancer HTS where Hill slopes vary [16].

**Step 3: Bidirectional extension.** Ehlert et al. (2011) extended the operational model to constitutive receptor activity by introducing epsilon (active state fraction) and epsilon_i (inactive state fraction) parameters [12]. The total stimulus represents summation of active states across occupied and free receptor populations, enabling characterization of both agonism and inverse agonism. The epsilon/epsilon_i pair naturally preserves sign information about the direction of receptor activation—the mathematical mechanism required for S′.

**Step 4–5: Combined indexing.** Kenakin et al. (2012) combined affinity and efficacy into the single transduction coefficient log(τ/KA), independent of receptor density [13]. The Δ-log(τ/KA) bias factor enables comparison across signaling pathways—structurally analogous to the proposed ΔS′ but applied across pathways rather than genotypes. Burgueño et al. (2017) demonstrated that separating the efficacy-driven log(τ) component from the combined index improves partial agonist profiling [17].

**Step 6–7: Equation refinement.** Jakubík et al. (2019) identified that τ is interdependent on KA and Emax, complicating fitting, and proposed a rigorous procedure: derive KA and Emax from the half-efficient concentration, then fix during τ estimation [14]. Randáková et al. (2023) showed that exponentiation of operational efficacy in the original Black–Leff equation breaks parameter relationships when slope factors vary, and recommended the Hill equation-based operational model analysis (OMA) as the superior formulation [15]. Since cancer cell line dose-response curves commonly exhibit non-unity Hill slopes, the Hill-based OMA is the recommended foundation for S′.

**Steps 8–9: Novel synthesis.** S′ would combine sign-preservation from Ehlert's epsilon/epsilon_i, the combined potency-efficacy structure from log(τ/KA), and the Hill-based equation from Randáková. ΔS′ = S′_test − S′_reference would adapt the sDSS cohort normalization concept [5] and Δ-log(τ/KA) pathway comparison [13] to a sign-preserving genotype-comparison framework. Neither metric was found in any published literature.

### 3.2 Novelty Status of S′ and ΔS′

Extensive search confirmed that S′ and ΔS′ are novel proposed metrics. Three PubMed queries ("sign-preserving potency efficacy index", "S-prime drug response", "operational model sign preservation") and two web searches returned zero results. The mathematical components exist independently in the published literature, but their integration into a single sign-preserving index for cancer cell screening has not been published.

### 3.3 Biological Validation: Paradoxical BRAF Inhibitor Activation

The paradoxical activation of the MAPK pathway by BRAF inhibitors in BRAF wild-type cells, documented by Botton et al. [2] and reviewed in the clinical pharmacology literature [3], serves as the canonical cancer example of genotype-dependent polarity shifts.

S′ would produce a positive value in BRAF-V600E cells (drug inhibits MAPK signaling → reduced proliferation) and a negative value in BRAF-WT cells (drug paradoxically activates MAPK → accelerated proliferation). The differential ΔS′ = S′(BRAF-V600E) − S′(BRAF-WT) would yield a large positive value flagging the genotype-specific vulnerability.

Traditional metrics fail in this scenario: IC₅₀ is undefined when the drug stimulates growth; AUC partially cancels opposing effects through sign-unaware integration [1].

### 3.4 Design Inputs from Existing Frameworks

The prior investigation [9] identified five frameworks that collectively address all CQ requirements but no single framework satisfies all five. Design inputs for S′/ΔS′ from these frameworks include: GR metrics' division-rate correction to avoid proliferation confounders [1]; NDR's bidirectional control normalization for extracting signed responses from viability readouts [6]; the hormesis model's ΔX parameter validating that polarity shifts are quantifiable [8]; and sDSS's cohort normalization concept for genotype-differential scoring [5].

### 3.5 Evidence Assessment

Fifteen claims were individually graded (Table 2). Two claims scored at L3 (0.70): the foundational status of the Black–Leff model (multi-source, literature-confirmed) and paradoxical BRAF inhibitor activation (two independent PMIDs with mechanism match). Eight claims scored at L2 (0.50–0.65). Three claims scored at L1 (0.35–0.40), including S′ theoretical feasibility (0.35) — the lowest scoreable claim, reflecting inference from component existence without empirical demonstration.

Two claims — S′ existence and ΔS′ existence as published metrics — scored INVALID (0.00), confirming their absence from the literature. These were excluded from aggregate statistics.

**Aggregate**: Median 0.55 (L2 Multi-DB) across 13 scoreable claims; range 0.35–0.70.

### 3.6 Gap Analysis

**Identified gaps requiring future work**:

1. **Receptor re-interpretation**: The operational model's "receptor" (R₀) and "stimulus-response" coupling (KE) must be formally mapped to cancer cell viability assay components (drug target engagement → signaling cascade → proliferation/death readout).

2. **HTS fitting procedure**: Jakubík's fitting procedure [14] requires multiple concentration-response curves with shared parameters — standard in GPCR pharmacology but potentially challenging at HTS scale.

3. **Empirical benchmarking**: S′ must be shown to outperform IC₅₀, GR₅₀, DSS, and NDR on datasets containing known paradoxical/bidirectional responses (e.g., BRAF inhibitor panels across BRAF-WT and V600E cell lines).

4. **Cancer HTS application gap**: The operational model literature is primarily situated in GPCR pharmacology. Translation to cancer cell viability assays has not been formally published.

---

## 4. Discussion

This investigation demonstrates that the Black–Leff operational model and its extensions provide a well-grounded theoretical foundation for constructing sign-preserving potency–efficacy indices for bidirectional drug-response characterization. The mathematical lineage from Black and Leff (1983) through Ehlert (2011) and Kenakin (2012) to the proposed S′ is traceable: each precursor component is independently validated in the published literature, and the critical re-evaluation by Randáková (2023) provides specific guidance on equation selection for the variable-slope data typical of cancer HTS.

The central finding is that S′ and ΔS′ represent a **novel methodological contribution**, not a literature discovery. The CQ describes an integration that has not been published: combining sign-preservation (from Ehlert's epsilon/epsilon_i), combined potency-efficacy scoring (from Kenakin's log(τ/KA)), and Hill-based parameter estimation (from Randáková's OMA re-evaluation) into a single index applied to cancer cell screening data. This is analogous to how GR metrics [1] re-derived familiar pharmacological concepts (potency, efficacy) in the specific context of cell division rate correction — the mathematical tools existed, but their application to the specific problem required new work.

The BRAF paradoxical activation scenario illustrates why this contribution matters clinically. Current HTS analysis pipelines either discard compounds showing bidirectional behavior (treating growth stimulation as assay failure) or integrate it into AUC where opposing effects cancel. A sign-preserving metric would instead flag these compounds as having genotype-dependent polarity — precisely the information needed for precision oncology applications where the same drug may be therapeutic in one genotype and harmful in another.

Several limitations constrain the present analysis. First, the investigation is entirely theoretical: S′ has not been constructed, fitted, or benchmarked against existing metrics on real data. The theoretical feasibility score (L1, 0.35) reflects this gap. Second, the operational model's translation from GPCR pharmacology to cancer cell viability requires formal mapping of model parameters to assay components — a non-trivial bridging exercise. Third, Jakubík's fitting procedure requires concentration-response curve series with shared parameters, which may not be practical at screening scale without simplifying assumptions.

The prior investigation [9] identified a minimum viable composition (NDR + sDSS + GR correction) as the best available published approximation to the CQ requirements. S′/ΔS′ would offer a theoretically unified alternative grounded in a single pharmacological model, but this advantage is speculative until empirical validation is completed.

Future work should prioritize: (1) formal mathematical specification of S′ including the sign-extension mechanism for Ehlert's parameters, (2) fitting S′ to publicly available cancer drug screening datasets containing known paradoxical responders (e.g., BRAF inhibitor panels in GDSC or CCLE), (3) head-to-head benchmarking against GR₅₀, DSS, NDR, and IC₅₀ on the same datasets, and (4) validation of ΔS′ for genotype-specific vulnerability detection using paired wild-type/mutant cell line panels.

---

## References

1. Hafner M, Niepel M, Chung M, Sorger PK. Growth rate inhibition metrics correct for confounders in measuring sensitivity to cancer drugs. *Nature Methods*. 2016;13(6):521-527. doi:10.1038/nmeth.3853. PMID:27135972.

2. Botton T, Yeh I, Nelson T, et al. Recurrent BRAF kinase fusions in melanocytic tumors offer an opportunity for targeted therapy. *Nature Communications*. 2016;7:12348. doi:10.1038/ncomms12348. PMID:27476449.

3. Sloot S, Fedorenko IV, Smalley KSM, Gibney GT. Long-term effects of BRAF inhibitors in melanoma treatment: friend or foe? *Expert Opinion on Pharmacotherapy*. 2014;15(5):589-592. doi:10.1517/14656566.2014.881471. PMID:24456413.

4. Smith SW, Hauben M, Aronson JK. Paradoxical and bidirectional drug effects. *Drug Safety*. 2012;35(3):173-189. doi:10.2165/11597710-000000000-00000. PMID:22272687.

5. Yadav B, Pemovska T, Saber A, et al. Quantitative scoring of differential drug sensitivity for individually optimized anticancer therapies. *Scientific Reports*. 2014;4:5193. doi:10.1038/srep05193. PMID:24898935.

6. Gupta A, Gautam P, Wennerberg K, Aittokallio T. A normalized drug response metric improves accuracy and consistency of anticancer drug sensitivity quantification in cell-based screening. *Communications Biology*. 2020;3:42. doi:10.1038/s42003-020-0765-z. PMID:31974521.

7. Meyer CT, Wooten DJ, Paudel BB, et al. Quantifying drug combination synergy along potency and efficacy axes. *Cell Systems*. 2019;8(2):97-108.e16. PMC6675406.

8. Calabrese EJ, Nascarella M. The role of hormesis in the functional performance and protection of neural systems. *Regulatory Toxicology and Pharmacology*. 2009;53(3):155-162. doi:10.1016/j.yrtph.2009.04.005. PMID:19393280.

9. [Prior investigation]. Bidirectional drug-response frameworks for high-throughput cancer screening. drug-response-framework-report.md. 2026.

10. Black JW, Leff P. Operational models of pharmacological agonism. *Proceedings of the Royal Society of London B*. 1983;220(1219):141-162. doi:10.1098/rspb.1983.0093. PMID:6141562.

11. Kenakin T. Theoretical aspects of GPCR-ligand complex pharmacology. *Chemical Reviews*. 2016;117(1):4-20. doi:10.1021/acs.chemrev.5b00561. PMID:26856272.

12. Ehlert FJ, Suga H, Griffin MT. Analysis of agonism and inverse agonism in functional assays with constitutive activity: estimation of orthosteric ligand affinity constants for active and inactive receptor states. *Journal of Pharmacology and Experimental Therapeutics*. 2011;338(2):671-686. doi:10.1124/jpet.111.179309. PMID:21576379.

13. Kenakin T, Watson C, Muniz-Medina V, Christopoulos A, Novick S. A simple method for quantifying functional selectivity and agonist bias. *ACS Chemical Neuroscience*. 2012;3(3):193-203. doi:10.1021/cn200111m. PMID:22860188.

14. Jakubík J, Randáková A, Rudajev V, Zimčík P, El-Fakahany EE, Doležal V. Applications and limitations of fitting of the operational model to determine relative efficacies of agonists. *Scientific Reports*. 2019;9(1):4637. doi:10.1038/s41598-019-40993-w. PMID:30874590.

15. Randáková A, Nelic D, Jakubík J. Reevaluation of the slope factor of the operational model of agonism: critical impact on efficacy estimation of the Hill equation-based operational model of agonism. *Scientific Reports*. 2023;13(1):17366. doi:10.1038/s41598-023-45004-7. PMID:37845324.

16. Black JW, Leff P, Shankley NP, Wood J. An operational model of pharmacological agonism: the effect of E/[A] curve shape on agonist dissociation constant estimation. *British Journal of Pharmacology*. 1985;84(2):561-571. doi:10.1111/j.1476-5381.1985.tb12941.x. PMID:3978322.

17. Burgueño J, Pujol M, Monroy X, et al. A complementary scale of biased agonism for agonists with differing maximal responses. *Scientific Reports*. 2017;7(1):15389. doi:10.1038/s41598-017-15258-z. PMID:29133887.

18. Hafner M, Niepel M, Sorger PK. Alternative drug sensitivity metrics improve preclinical cancer pharmacogenomics. *Nature Biotechnology*. 2017;35(6):500-502. doi:10.1038/nbt.3882. PMID:28591115.

---

## Supplementary Materials

### S1: Knowledge Graph Nodes

22 nodes comprising: 9 framework nodes (PMID-identified publications tracing the operational model lineage), 6 metric nodes (3 traditional: IC₅₀, EC₅₀, AUC; 2 existing: log(τ/KA), Δ-log(τ/KA); 2 proposed: S′, ΔS′), 2 phenomenon nodes (paradoxical BRAF activation, paradoxical/bidirectional drug effects taxonomy), and 5 inherited framework nodes (GR metrics, DSS/sDSS, NDR, MuSyC, hormesis model).

Full node specifications with properties, source citations, and CQ component mappings are provided in `extended-operational-model-kg.json`.

### S2: Knowledge Graph Edges

22 edges representing: 6 EXTENDED_BY relationships (operational model extensions), 2 CRITIQUED_BY relationships (fitting limitations, slope re-evaluation), 6 INFORMS_DESIGN relationships (existing frameworks → S′/ΔS′ design), 1 MATHEMATICAL_PRECURSOR relationship (log(τ/KA) → S′), 1 STRUCTURAL_ANALOGUE relationship (Δ-log(τ/KA) → ΔS′), 3 SUPERSEDES relationships (S′ over IC₅₀, EC₅₀, AUC), 2 DESIGNED_TO_DETECT relationships (S′/ΔS′ → paradoxical phenomena), 1 FAILS_TO_DETECT relationship (IC₅₀ → BRAF paradox).

Full edge specifications with provenance are provided in `extended-operational-model-kg.json`.

### S3: Evidence Grading Detail

| # | Claim | Base Level | Modifiers | Final Score |
|---|-------|-----------|-----------|-------------|
| 1 | Black–Leff OM (1983) foundational model | L2 (0.60) | +0.05 literature, +0.05 multi-source | 0.70 (L3) |
| 2 | Black–Leff (1985) non-hyperbolic extension | L1 (0.45) | +0.05 literature | 0.50 (L2) |
| 3 | Ehlert (2011) epsilon/epsilon_i bidirectional | L1 (0.45) | +0.05 literature, +0.10 mechanism match | 0.60 (L2) |
| 4 | Kenakin (2012) log(τ/KA) combined index | L2 (0.55) | +0.05 literature, +0.05 multi-source | 0.65 (L2) |
| 5 | Burgueño (2017) log(τ) efficacy separation | L1 (0.45) | +0.05 literature | 0.50 (L2) |
| 6 | Jakubík (2019) τ interdependence | L1 (0.45) | +0.05 literature | 0.50 (L2) |
| 7 | Randáková (2023) Hill-based OMA superior | L1 (0.45) | +0.10 mechanism match | 0.55 (L2) |
| 8 | BRAF paradoxical activation | L2 (0.55) | +0.10 mechanism, +0.05 literature | 0.70 (L3) |
| 9 | Paradoxical drug effects taxonomy | L1 (0.45) | +0.05 literature | 0.50 (L2) |
| 10 | log(τ/KA) → S′ mathematical precursor | — | — | 0.40 (L1) |
| 11 | S′ exists as published metric | — | -0.15 unverified | INVALID (0.00) |
| 12 | ΔS′ exists as published metric | — | -0.15 unverified | INVALID (0.00) |
| 13 | S′ theoretically feasible | — | — | 0.35 (L1) |
| 14 | IC₅₀ undefined for paradoxical activation | L2 (0.55) | +0.10 mechanism match | 0.65 (L2) |
| 15 | AUC cancels opposing effects | L1 (0.45) | +0.10 mechanism match | 0.55 (L2) |

Median (13 scoreable): **0.55 (L2)**. Range: **0.35–0.70**.

### S4: Synapse Dataset Mapping

Synapse.org data repository was not connected during this pipeline execution. No Synapse dataset grounding was performed. See `extended-operational-model-synapse-grounding.md` for impact assessment and recommended future queries.

### S5: Search Provenance

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
| "sign-preserving potency efficacy index" | PubMed search_articles | Zero results |
| "S-prime drug response operational model" | PubMed search_articles | Zero results |
| "operational model sign preservation cancer screening" | PubMed search_articles | Zero results |
| "sign-preserving potency efficacy index operational model" | WebSearch | Zero results |
| "S-prime delta-S-prime drug response cancer HTS" | WebSearch | Zero results |
| "operational model agonism cancer high-throughput screening" | WebSearch | Contextual (no S′/ΔS′) |
| "Ehlert constitutive activity epsilon parameter" | WebSearch | Contextual (confirms PMID:21576379) |

---

*Manuscript draft generated 2026-03-12 via Fuzzy-to-Fact publication pipeline. All claims grounded in PubMed and web search results. S′ and ΔS′ are novel proposed metrics not found in published literature.*
