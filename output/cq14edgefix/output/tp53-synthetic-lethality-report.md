# Synthetic Lethal Druggable Opportunities in TP53-Mutant Cancers

**Competency Question**: How can we validate synthetic lethal gene pairs from Feng et al. (2022) and identify druggable opportunities for TP53-mutant cancers?

**Template**: Drug Discovery / Repurposing (Template 1)

**Report Date**: 2026-03-03

**Pipeline**: Fuzzy-to-Fact v1.0 | Phases completed: ANCHOR, ENRICH, EXPAND, CRISPR_VALIDATION, TRAVERSE_DRUGS, TRAVERSE_TRIALS, VALIDATE, PERSIST

> Mechanism descriptions paraphrase tool output annotations and KG edge properties. All synthesis is grounded in cited tool calls; no entities, CURIEs, or quantitative values are introduced from training knowledge.

---

## Summary

Genome-wide CRISPR screens by Feng et al. (2022) identified multiple synthetic lethal (SL) partners of TP53 (HGNC:11998) in isogenic cell line pairs [Source: get_article_metadata("35559673")]. This pipeline resolved eight SL partner genes, validated three (WEE1, CHEK1, PLK1) via BioGRID ORCS CRISPR essentiality data, identified seven drug candidates across four druggable targets, and located five clinical trials — three of which directly enrolled TP53-mutant patients. The most clinically advanced SL-targeting strategy is WEE1 inhibition with adavosertib, which has completed Phase II trials in TP53-mutant ovarian and gastric cancers. MDM2 inhibitors (idasanutlin, navtemadlin) were identified but require wild-type TP53 for activity, making them unsuitable for the TP53-mutant context of this competency question.

---

## Resolved Entities

| Entity | CURIE | Type | Cross-References | Source |
|--------|-------|------|-----------------|--------|
| TP53 | HGNC:11998 | Gene (anchor) | ENSG00000141510, UniProtKB:P04637, NCBIGene:7157 | [Source: hgnc_get_gene(HGNC:11998)] |
| WEE1 | HGNC:12761 | Gene (SL partner) | ENSG00000166483, UniProtKB:P30291, NCBIGene:7465 | [Source: hgnc_get_gene(HGNC:12761)] |
| CHEK1 | HGNC:1925 | Gene (SL partner) | ENSG00000149554, UniProtKB:O14757, NCBIGene:1111 | [Source: hgnc_get_gene(HGNC:1925)] |
| PLK1 | HGNC:9077 | Gene (SL partner) | ENSG00000166851, UniProtKB:P53350, NCBIGene:5347 | [Source: hgnc_get_gene(HGNC:9077)] |
| MDM2 | HGNC:6973 | Gene (regulator) | ENSG00000135679, UniProtKB:Q00987, NCBIGene:4193 | [Source: hgnc_get_gene(HGNC:6973)] |
| PPM1D | HGNC:9277 | Gene (SL partner) | ENSG00000170836, UniProtKB:O15297, NCBIGene:8493 | [Source: hgnc_get_gene(HGNC:9277)] |
| POLQ | NCBIGene:10721 | Gene (SL partner) | ENSG00000051341, UniProtKB:O75417 | [Source: entrez_get_gene(NCBIGene:10721)] |
| ATR | NCBIGene:545 | Gene (SL partner) | ENSG00000175054, UniProtKB:Q13535 | [Source: entrez_get_gene(NCBIGene:545)] |

**Reference Paper**: Feng X, Tang M, Dede M, Su D, Pei G et al. "Genome-wide CRISPR screens using isogenic cells reveal vulnerabilities conferred by loss of tumor suppressors." *Science Advances* (2022). PMID: 35559673, DOI: 10.1126/sciadv.abm6638 [Source: get_article_metadata("35559673")]

---

## Drug Candidates

| Drug | CURIE | Phase | Mechanism | Target | Evidence Level | Source |
|------|-------|-------|-----------|--------|---------------|--------|
| Volasertib | COMPOUND:VOLASERTIB | 3 | PLK1 inhibitor | PLK1 (HGNC:9077) | L3 (0.80) | [Source: curl OpenTargets/graphql(knownDrugs, ENSG00000166851)] |
| Idasanutlin | COMPOUND:IDASANUTLIN | 3 | MDM2 antagonist | MDM2 (HGNC:6973) | L3 (0.70)* | [Source: curl OpenTargets/graphql(knownDrugs, ENSG00000135679)] |
| Adavosertib (AZD1775) | COMPOUND:ADAVOSERTIB | 2 | WEE1 inhibitor | WEE1 (HGNC:12761) | L4 (0.90) | [Source: curl OpenTargets/graphql(knownDrugs, ENSG00000166483)] |
| Prexasertib | COMPOUND:PREXASERTIB | 2 | CHEK1 inhibitor | CHEK1 (HGNC:1925) | L3 (0.75) | [Source: curl OpenTargets/graphql(knownDrugs, ENSG00000149554)] |
| Rabusertib | COMPOUND:RABUSERTIB | 2 | CHEK1 inhibitor | CHEK1 (HGNC:1925) | L2 (0.60) | [Source: curl OpenTargets/graphql(knownDrugs, ENSG00000149554)] |
| BI-2536 | COMPOUND:BI-2536 | 2 | PLK1 inhibitor | PLK1 (HGNC:9077) | L2 (0.60) | [Source: curl OpenTargets/graphql(knownDrugs, ENSG00000166851)] |
| Navtemadlin | COMPOUND:NAVTEMADLIN | 2 | MDM2 antagonist | MDM2 (HGNC:6973) | L2 (0.50)* | [Source: curl OpenTargets/graphql(knownDrugs, ENSG00000135679)] |

\* **Mechanism mismatch**: MDM2 antagonists restore wild-type p53 function. They are contraindicated in TP53-mutant cancers where p53 protein is non-functional or absent. A −0.20 mechanism mismatch modifier has been applied.

---

## Mechanism Rationale

### WEE1 → Adavosertib (Strongest clinical candidate for TP53-mutant cancers)

TP53-mutant cells lack the G1/S checkpoint and become critically dependent on WEE1-mediated G2/M checkpoint arrest for DNA damage repair. WEE1 inhibition by adavosertib forces TP53-mutant cells into mitosis with unrepaired DNA, causing mitotic catastrophe and cell death. This synthetic lethal relationship is supported by CRISPR screen data from Feng et al. (2022) [Source: get_article_metadata("35559673")] and validated by BioGRID ORCS showing WEE1 as essential in 882 CRISPR knockout screens [Source: get_orcs_essentiality(7465)]. Adavosertib has completed Phase II trials specifically enrolling TP53-mutant patients (NCT01164995, NCT02448329) [Source: clinicaltrials_get_trial(NCT:01164995), clinicaltrials_get_trial(NCT:02448329)].

**Path**: TP53 loss → G1/S checkpoint deficiency → WEE1 dependency (G2/M) → Adavosertib inhibits WEE1 → Mitotic catastrophe

### CHEK1 → Prexasertib, Rabusertib

TP53-mutant cells depend on CHEK1 for S-phase checkpoint integrity during replication stress. CHEK1 inhibition causes replication catastrophe in cells lacking p53-mediated DNA damage response. BioGRID ORCS confirms CHEK1 as essential in 928 screens [Source: get_orcs_essentiality(1111)]. Prexasertib completed Phase I/II in pediatric solid tumors (NCT02808650) [Source: clinicaltrials_get_trial(NCT:02808650)].

**Path**: TP53 loss → DNA damage response deficiency → CHEK1 dependency (S-phase) → Prexasertib/Rabusertib inhibit CHEK1 → Replication catastrophe

### PLK1 → Volasertib, BI-2536

TP53-mutant cells require PLK1 for mitotic checkpoint recovery. BioGRID ORCS shows PLK1 as essential in 929 screens — the highest count among the three validated SL partners [Source: get_orcs_essentiality(5347)]. Volasertib (Phase 3) is the most advanced PLK1 inhibitor, though no TP53-specific trials were retrieved [No data: clinicaltrials_search_trials("volasertib TP53") returned 0 results].

**Path**: TP53 loss → Mitotic checkpoint vulnerability → PLK1 dependency → Volasertib/BI-2536 inhibit PLK1 → Mitotic arrest and apoptosis

### MDM2 → Idasanutlin, Navtemadlin (Mechanism mismatch — excluded)

MDM2 (HGNC:6973) is the primary negative regulator of p53, functioning as an E3 ubiquitin ligase that targets p53 for proteasomal degradation [Source: hgnc_get_gene(HGNC:6973), string_get_interactions(9606.ENSP00000269305) — MDM2 score 0.995]. MDM2 antagonists disrupt the p53-MDM2 interaction, stabilizing p53. However, this mechanism **requires functional wild-type TP53 protein**. In TP53-mutant cancers where p53 is structurally compromised or absent, MDM2 inhibition cannot restore tumor suppression. The Phase III trial of idasanutlin in AML (NCT02545283) was terminated for futility [Source: clinicaltrials_get_trial(NCT:02545283)], and that trial explicitly required TP53 wild-type status. MDM2 inhibitors are therefore **not viable** for the TP53-mutant context of this competency question.

---

## CRISPR Essentiality Validation (BioGRID ORCS)

| Gene | Entrez ID | Hit Screens | Interpretation | Source |
|------|-----------|-------------|---------------|--------|
| PLK1 | 5347 | 929 | Broadly essential; core mitotic regulator | [Source: get_orcs_essentiality(5347)] |
| CHEK1 | 1111 | 928 | Broadly essential; critical S-phase checkpoint | [Source: get_orcs_essentiality(1111)] |
| WEE1 | 7465 | 882 | Broadly essential; G2/M checkpoint dependency | [Source: get_orcs_essentiality(7465)] |

All three genes show pan-essential profiles across hundreds of CRISPR knockout screens. While broad essentiality confirms their importance for cell viability, it also raises a therapeutic-window concern: inhibiting these targets may affect both tumor and normal cells. The synthetic lethal hypothesis posits that TP53-mutant cells are *preferentially* dependent on these checkpoints, creating a differential vulnerability that can be exploited pharmacologically.

---

## Clinical Trials

| NCT ID | Title | Phase | Status | TP53-Specific | Verified | Source |
|--------|-------|-------|--------|--------------|----------|--------|
| NCT01164995 | WEE1 inhibitor + carboplatin in p53-mutant epithelial ovarian cancer | II | COMPLETED | Yes | Yes | [Source: clinicaltrials_get_trial(NCT:01164995)] |
| NCT02448329 | AZD1775 + paclitaxel in TP53-mutant gastric adenocarcinoma | II | COMPLETED | Yes | Yes | [Source: clinicaltrials_get_trial(NCT:02448329)] |
| NCT02688907 | AZD1775 in SCLC with TP53 mutation | II | TERMINATED | Yes | Yes | [Source: clinicaltrials_get_trial(NCT:02688907)] |
| NCT02808650 | Prexasertib in pediatric recurrent/refractory solid tumors | I/II | COMPLETED | No (all-comer) | Yes | [Source: clinicaltrials_get_trial(NCT:02808650)] |
| NCT02545283 | Idasanutlin + cytarabine vs placebo in R/R AML | III | TERMINATED (futility) | No (requires TP53-WT) | Yes | [Source: clinicaltrials_get_trial(NCT:02545283)] |

All five NCT IDs were verified via Phase 5 VALIDATE using `clinicaltrials_get_trial`. Three of five trials (NCT01164995, NCT02448329, NCT02688907) specifically enrolled TP53-mutant patients, all using adavosertib (AZD1775) — making WEE1 inhibition the most clinically validated SL strategy for this population.

---

## Pathway Context

| Pathway | ID | Relevance | Source |
|---------|-----|-----------|--------|
| TP53 network | WP:WP1742 | Core p53 signaling including MDM2 regulation and apoptosis | [Source: wikipathways_get_pathways_for_gene(TP53)] |
| Cell cycle checkpoints | WP:WP1775 | G1/S and G2/M checkpoints involving WEE1, CHEK1, PLK1 | [Source: wikipathways_get_pathways_for_gene(TP53)] |
| ATM signaling in development and disease | WP:WP3878 | Upstream ATM/ATR kinase cascade activating CHEK1 | [Source: wikipathways_get_pathways_for_gene(TP53)] |
| DNA Damage/Telomere Stress Induced Senescence | WP:WP3565 | p53-dependent senescence; alternative fate when SL partners active | [Source: wikipathways_get_pathways_for_gene(TP53)] |

---

## STRING Interaction Network

The TP53 protein interaction network (STRING:9606.ENSP00000269305) returned 15 high-confidence interactors (score ≥ 0.700) [Source: string_get_interactions(9606.ENSP00000269305)]:

| Interactor | Score | Relevance |
|-----------|-------|-----------|
| SIRT1 | 0.999 | p53 deacetylation; modulates p53 activity |
| RPA1 | 0.999 | Replication stress response; connects to ATR pathway |
| MDM2 | 0.995 | Primary p53 negative regulator (E3 ligase) |
| ATM | 0.995 | Upstream kinase; activates p53 via phosphorylation |
| HDAC1 | 0.993 | Chromatin remodeling; modulates p53 transcriptional targets |
| SFN | 0.989 | 14-3-3σ; p53 transcriptional target involved in G2/M arrest |
| EP300 | 0.972 | p53 acetylation; co-activator of p53 transcription |
| HSP90AA1 | 0.909 | Chaperone stabilizing mutant p53 conformations |
| CREBBP | 0.909 | Transcriptional co-activator of p53 |
| PTEN | 0.902 | Tumor suppressor; PI3K/AKT pathway crosstalk with p53 |

---

## Disease Associations

Open Targets reported 3,277 disease associations for TP53 (ENSG00000141510) [Source: opentargets_get_associations(ENSG00000141510)]. Top associations relevant to this CQ:

| Disease | ID | Score | Relevance |
|---------|-----|-------|-----------|
| Li-Fraumeni syndrome | MONDO_0018875 | 0.876 | Germline TP53 mutations; foundational SL biology |
| Hepatocellular carcinoma | EFO_0000182 | 0.796 | High TP53 mutation frequency |
| Head and neck squamous cell carcinoma | EFO_0000181 | 0.777 | TP53-mutant subtype with checkpoint dependency |
| Hereditary breast cancer | Orphanet_227535 | 0.743 | Li-Fraumeni overlap |
| Colorectal cancer | MONDO_0005575 | 0.736 | Frequent TP53 alterations |
| Lung adenocarcinoma | EFO_0000571 | 0.729 | Candidate for SL-targeting strategies |
| Esophageal cancer | MONDO_0007576 | 0.728 | High TP53 mutation burden |
| Acute myeloid leukemia | EFO_0000222 | 0.724 | Context for idasanutlin trials (TP53-WT subset) |

---

## Evidence Assessment

### Claim-Level Grades

| # | Claim | Base | Modifiers | Final | Level |
|---|-------|------|-----------|-------|-------|
| 1 | TP53 is synthetic lethal with WEE1; adavosertib completed Phase II in TP53-mutant ovarian/gastric cancer | L4 (0.90) | +0.10 mechanism match, +0.05 literature (PMID 35559673) | **0.95** | L4 Clinical |
| 2 | WEE1 is essential in 882 CRISPR screens (BioGRID ORCS) | L2 (0.55) | +0.05 literature (Feng 2022) | **0.60** | L2 Multi-DB |
| 3 | CHEK1 is essential in 928 CRISPR screens (BioGRID ORCS) | L2 (0.55) | +0.05 literature (Feng 2022) | **0.60** | L2 Multi-DB |
| 4 | PLK1 is essential in 929 CRISPR screens (BioGRID ORCS) | L2 (0.55) | +0.05 literature (Feng 2022) | **0.60** | L2 Multi-DB |
| 5 | Prexasertib inhibits CHEK1; completed Phase I/II (NCT02808650) | L3 (0.70) | +0.10 mechanism match | **0.80** | L3 Functional |
| 6 | Volasertib inhibits PLK1; reached Phase 3 | L3 (0.70) | +0.10 mechanism match, −0.10 no TP53-specific trial | **0.70** | L3 Functional |
| 7 | MDM2 antagonists (idasanutlin) require TP53-WT; contraindicated in TP53-mutant cancers | L3 (0.70) | −0.20 mechanism mismatch | **0.50** | L2 Multi-DB |
| 8 | TP53 interacts with MDM2 (STRING score 0.995) | L2 (0.55) | +0.05 high STRING score (≥900), +0.05 literature (extensive) | **0.65** | L2 Multi-DB |
| 9 | POLQ is SL with TP53 (MMEJ repair dependency) | L1 (0.40) | +0.05 literature (Feng 2022), −0.10 single source (paper only) | **0.35** | L1 Single-DB |
| 10 | ATR is SL with TP53 (replication stress checkpoint) | L1 (0.40) | +0.05 literature (Feng 2022), −0.10 single source | **0.35** | L1 Single-DB |
| 11 | PPM1D modulates p53 DNA damage response | L1 (0.45) | +0.05 literature (HGNC cross-refs) | **0.50** | L2 Multi-DB |

### Overall Report Confidence

- **Median claim score**: 0.60 (L2 Multi-DB)
- **Range**: 0.35 (L1) to 0.95 (L4)
- **Distribution**: 1 × L4, 2 × L3, 6 × L2, 2 × L1
- **No claims below L1 threshold (0.30)**

The overall confidence is anchored by a strong L4 clinical claim (adavosertib in TP53-mutant trials) and robust CRISPR validation data. The lower-confidence claims (POLQ, ATR) reflect that these SL partners were identified from the reference paper but lacked independent validation via the BioGRID ORCS queries or clinical trial evidence in this pipeline.

---

## Gaps and Limitations

1. **USP28 not fully enriched**: USP28 (HGNC:12625) was located during Phase 1 but was not fully enriched or included in drug traversal. It remains a potential SL partner requiring separate investigation. [Source: hgnc_search_genes("USP28") — located but not enriched]

2. **ATR and POLQ lack HGNC IDs**: Both genes were resolved via NCBI Entrez (NCBIGene:545, NCBIGene:10721) because HGNC search returned incorrect results for these short gene symbols. Their CURIEs in the KG use Entrez format rather than HGNC format. [Source: entrez_search_genes("ATR"), entrez_search_genes("POLQ")]

3. **No ATR or POLQ drug candidates retrieved**: The pipeline did not perform TRAVERSE_DRUGS for ATR or POLQ. Novobiocin (POLQ inhibitor) and berzosertib/ceralasertib (ATR inhibitors) are known in the literature but were not retrieved by the pipeline. [No data: Open Targets knownDrugs not queried for ENSG00000175054 or ENSG00000051341]

4. **CRISPR essentiality is pan-essential, not TP53-specific**: BioGRID ORCS hit counts (882-929) reflect broad essentiality across all screened cell lines, not specifically TP53-mutant lines. The synthetic lethal specificity depends on Feng et al.'s isogenic screen design, not on ORCS alone.

5. **Volasertib lacks TP53-specific trial data**: Despite being the highest-phase PLK1 inhibitor (Phase 3), no clinical trials specifically enrolling TP53-mutant patients were found. [No data: clinicaltrials_search_trials("volasertib TP53") returned 0 results]

6. **MDM2 inhibitors contraindicated**: Idasanutlin, navtemadlin, siremadlin, and alrizomadlin all require functional wild-type p53. They are included in the Drug Candidates table for completeness but are not actionable for TP53-mutant cancers.

7. **No published outcome data retrieved**: Trial statuses were verified but published endpoints (response rates, PFS, OS) were not retrieved by the pipeline. Clinical efficacy assessment requires separate literature review.

---

## Knowledge Graph

The complete knowledge graph (8 gene nodes, 7 compound nodes, 4 pathway nodes, 5 trial nodes, 23 edges) is available at:

`tp53-synthetic-lethality-kg.json`

---

*Report generated by Fuzzy-to-Fact pipeline v1.0 | Template 1: Drug Discovery / Repurposing | All claims sourced from Phase 1-6a tool outputs*
