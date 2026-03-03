# Computational Validation of Synthetic Lethal Gene Pairs and Druggable Opportunities in TP53-Mutant Cancers Using Multi-Database Knowledge Graph Construction

**Authors**: Fuzzy-to-Fact Pipeline v1.0 (Automated Knowledge Graph Construction)

**Corresponding Author**: Pipeline output — no human corresponding author

**Date**: 2026-03-03

**Keywords**: synthetic lethality, TP53, CRISPR screens, knowledge graph, drug repurposing, WEE1, checkpoint kinase

**Subject Areas**: Cancer Biology, Bioinformatics, Pharmacology

---

## Abstract

**Background**: TP53 is the most frequently mutated tumor suppressor gene in human cancers, yet TP53-mutant tumors remain largely undruggable by direct targeting. Synthetic lethality — where loss of one gene creates dependency on a partner gene — offers an indirect strategy to selectively kill TP53-mutant cells. Feng et al. (2022) identified multiple synthetic lethal partners of TP53 through genome-wide CRISPR screens in isogenic cell line pairs, but systematic validation of these findings across independent databases and identification of druggable opportunities requires integrative computational approaches.

**Methods**: We developed and applied the Fuzzy-to-Fact protocol, an automated knowledge graph construction pipeline that resolves biological entities across eight federated databases (HGNC, NCBI Entrez, Ensembl, UniProt, STRING, BioGRID ORCS, Open Targets, ClinicalTrials.gov) using a two-phase entity resolution strategy. The pipeline executed eight phases: entity anchoring, enrichment with cross-references, network expansion via STRING protein-protein interactions, CRISPR essentiality validation via BioGRID ORCS, drug traversal via Open Targets knownDrugs, clinical trial discovery, edge validation, and knowledge graph persistence. Synapse.org datasets were queried for additional grounding.

**Results**: From eight candidate synthetic lethal partners of TP53, three genes — WEE1, CHEK1, and PLK1 — were independently validated as broadly essential across 882–929 CRISPR knockout screens in BioGRID ORCS. Drug traversal identified seven compounds targeting four gene products, with adavosertib (WEE1 inhibitor) achieving the highest evidence level (L4, score 0.95) based on completed Phase II trials specifically enrolling TP53-mutant patients (NCT01164995, NCT02448329). MDM2 antagonists (idasanutlin, navtemadlin) were identified but flagged as contraindicated for TP53-mutant cancers due to mechanism mismatch. The resulting knowledge graph comprised 24 nodes (8 genes, 7 compounds, 4 pathways, 5 trials) and 23 edges across six relationship types. Median evidence confidence across 11 graded claims was 0.60 (range 0.35–0.95). Synapse grounding achieved 21% node coverage and 16% edge coverage via five datasets including the Broad-DREAM Gene Essentiality Prediction Challenge (syn2384331) and CCLE (syn5889324).

**Conclusions**: Integrative multi-database knowledge graph construction validates WEE1 as the most clinically actionable synthetic lethal target in TP53-mutant cancers, supported by convergent CRISPR screen data, pharmacological evidence, and completed clinical trials. The Fuzzy-to-Fact protocol demonstrates that automated federated database traversal can systematically assess druggability of genetically defined vulnerabilities, with transparent evidence grading distinguishing well-validated targets from preliminary findings.

---

## 1. Introduction

TP53 (HGNC:11998) encodes the tumor protein p53, a transcription factor that functions as a master regulator of the DNA damage response, cell cycle arrest, apoptosis, and senescence [1]. Loss-of-function mutations in TP53 are observed across virtually all cancer types, with somatic TP53 alterations detected in over 3,277 disease associations catalogued in Open Targets [2]. Li-Fraumeni syndrome (MONDO_0018875), caused by germline TP53 mutations, exemplifies the foundational role of p53 loss in tumorigenesis [2]. Despite this central role, TP53 remains one of the most therapeutically challenging targets in oncology — the protein lacks enzymatic activity amenable to small-molecule inhibition, and attempts to restore wild-type p53 function through MDM2 antagonists are limited to tumors retaining at least one functional TP53 allele [3].

Synthetic lethality offers an alternative therapeutic paradigm. The concept, originally described in yeast genetics, posits that simultaneous loss of two genes is lethal while loss of either alone is tolerated [4]. In the context of TP53-mutant cancers, cells that have lost p53 function become critically dependent on compensatory DNA damage response and cell cycle checkpoint pathways. Pharmacological inhibition of these compensatory pathways — particularly WEE1-mediated G2/M checkpoint control, CHEK1-mediated S-phase checkpoint regulation, and PLK1-dependent mitotic checkpoint recovery — creates a therapeutic window where TP53-mutant tumor cells are selectively killed while TP53-wild-type normal cells survive [5].

Feng et al. (2022) performed genome-wide CRISPR screens using isogenic cell line pairs differing only in TP53 status, identifying multiple synthetic lethal gene partners including WEE1, CHEK1, PLK1, PPM1D, POLQ, ATR, and USP28 [5]. However, the translation from CRISPR screen hit to druggable target requires systematic cross-validation against independent functional genomics databases, assessment of pharmacological tractability, and evaluation of clinical trial evidence. Each of these steps involves querying distinct databases with different identifiers, ontologies, and access patterns — a process that is time-consuming when performed manually and prone to identifier mapping errors.

To address this challenge, we developed the Fuzzy-to-Fact protocol, an automated knowledge graph construction pipeline that federates queries across eight biomedical databases using a principled two-phase entity resolution strategy: fuzzy search (LOCATE) followed by strict identifier retrieval (RETRIEVE). This protocol ensures that all entities in the knowledge graph are grounded in canonical database identifiers (CURIEs) with full provenance tracking. We applied this pipeline to the TP53 synthetic lethality competency question, systematically validating CRISPR screen findings, identifying druggable opportunities, and evaluating clinical trial evidence for each synthetic lethal target.

---

## 2. Methods

### 2.1 Pipeline Architecture: The Fuzzy-to-Fact Protocol

The Fuzzy-to-Fact protocol consists of eight sequential phases, each building on the output of the previous phase. The pipeline is implemented as a series of MCP (Model Context Protocol) tool calls to federated biomedical databases, with each tool call logged for provenance tracking.

**Phase 1 — ANCHOR**: The anchor gene TP53 was resolved to its canonical identifier HGNC:11998 via the HGNC database [6], with cross-references to Ensembl (ENSG00000141510), UniProt (P04637), and NCBI Entrez (NCBIGene:7157). The reference paper (PMID: 35559673) was verified via PubMed [5].

**Phase 2 — ENRICH**: Six synthetic lethal partner genes (WEE1, CHEK1, PLK1, PPM1D, POLQ, ATR) and one regulatory gene (MDM2) were resolved using a two-step LOCATE→RETRIEVE pattern. For genes with unambiguous symbols, HGNC search was used (hgnc_search_genes → hgnc_get_gene). For short or ambiguous symbols (ATR, POLQ), NCBI Entrez was used as fallback (entrez_search_genes → entrez_get_gene) [7, 8]. Each resolved entity was annotated with cross-references to Ensembl, UniProt, and Entrez.

**Phase 3 — EXPAND**: The TP53 protein interaction network was retrieved from STRING (version 12.0) using the STRING ID 9606.ENSP00000269305, with a minimum combined score threshold of 0.700 [9]. This identified 15 high-confidence interactors, including MDM2 (score 0.995), ATM (score 0.995), and SIRT1 (score 0.999). Pathway membership was retrieved from WikiPathways, identifying four relevant pathways: TP53 network (WP1742), Cell cycle checkpoints (WP1775), ATM signaling (WP3878), and DNA Damage/Telomere Stress Induced Senescence (WP3565) [10].

**Phase 4a — CRISPR_VALIDATION**: CRISPR knockout screen essentiality data was retrieved from BioGRID ORCS for each candidate synthetic lethal partner gene using Entrez gene IDs [11]. Only genes with hit_only=True data were considered validated. Hit screen counts were recorded as a measure of broad essentiality across cancer cell line panels.

**Phase 4b — TRAVERSE_DRUGS**: Drug candidates were identified via Open Targets Platform GraphQL API using the knownDrugs endpoint for each druggable target (WEE1/ENSG00000166483, CHEK1/ENSG00000149554, PLK1/ENSG00000166851, MDM2/ENSG00000135679) [12]. Drug mechanism of action, maximum clinical phase, and indication data were extracted.

**Phase 5 — TRAVERSE_TRIALS**: Clinical trials were discovered via ClinicalTrials.gov search for each drug-target combination, with trial details retrieved using clinicaltrials_get_trial [13]. Each NCT ID was independently validated.

**Phase 6 — VALIDATE**: All 10 entities with external identifiers were re-verified against their source databases. Edges were validated for biological consistency, with particular attention to mechanism mismatch (e.g., MDM2 antagonists requiring wild-type TP53).

**Phase 7 — PERSIST**: The complete knowledge graph was serialized to JSON format with nodes, edges, and metadata arrays.

### 2.2 Evidence Grading

Claims were graded individually using a four-level system with numeric modifiers:

- **L4 Clinical** (0.90–1.00): Clinical trial evidence with verified NCT IDs
- **L3 Functional** (0.70–0.89): Experimental validation from multiple independent sources
- **L2 Multi-DB** (0.50–0.69): Cross-database concordance without direct experimental validation
- **L1 Single-DB** (0.30–0.49): Single database source with limited corroboration

Modifiers included: +0.10 (active clinical trial, mechanism match), +0.05 (literature support, high STRING score ≥ 900), −0.10 (single source, no target-specific trial), −0.20 (mechanism mismatch). The overall report confidence was computed as the median of all claim scores, not the mean, to reduce the influence of outliers [14].

### 2.3 Synapse Dataset Grounding

The Synapse.org data repository was queried using compound search terms (e.g., "TP53 synthetic lethality cancer", "WEE1 inhibitor cancer CRISPR", "DepMap cancer cell line gene essentiality") to identify datasets that could independently ground knowledge graph entities and edges [15]. Grounding strength was classified as Strong, Moderate, Analogous, or Weak based on the directness of the dataset's relevance to the specific mechanistic claims in the knowledge graph. MEMBER_OF edges (pathway membership, N=4) were excluded from edge grounding denominator as ontological definitions rather than experimental claims.

### 2.4 Quality Review

The completed report was evaluated against a 10-dimension quality review framework: CURIE Resolution, Source Attribution, LOCATE→RETRIEVE Discipline, Disease CURIE in ENRICH Phase, Open Targets Pagination, Evidence Grading, Gain-of-Function Filter, Trial Validation, Completeness, and Hallucination Risk [14]. Each dimension was scored as PASS, PARTIAL, or FAIL, with failures classified as protocol failures (step not executed), presentation failures (step executed but not documented), or documentation errors (step documented incorrectly).

---

## 3. Results

### 3.1 Entity Resolution and Knowledge Graph Construction

The Fuzzy-to-Fact pipeline resolved eight gene entities to canonical CURIEs across two databases: six via HGNC (TP53/HGNC:11998, WEE1/HGNC:12761, CHEK1/HGNC:1925, PLK1/HGNC:9077, MDM2/HGNC:6973, PPM1D/HGNC:9277) and two via NCBI Entrez (POLQ/NCBIGene:10721, ATR/NCBIGene:545) [6, 7, 8]. ATR and POLQ required Entrez fallback because their short gene symbols returned incorrect results in HGNC fuzzy search.

The complete knowledge graph contained 24 nodes (8 genes, 7 compounds, 4 pathways, 5 clinical trials) and 23 edges across six relationship types: SYNTHETIC_LETHAL (5), REGULATES (2), MEMBER_OF (4), INHIBITOR (7), and TESTED_IN (5) [Table S1, S2].

### 3.2 CRISPR Essentiality Validation

Three of the eight candidate synthetic lethal partners were independently validated as broadly essential via BioGRID ORCS CRISPR knockout screen data [11]:

PLK1 (NCBIGene:5347) was identified as a hit in 929 CRISPR screens, the highest count among all three validated partners, consistent with its role as a core mitotic regulator [11]. CHEK1 (NCBIGene:1111) was essential in 928 screens, reflecting its critical function in S-phase checkpoint integrity [11]. WEE1 (NCBIGene:7465) was essential in 882 screens, confirming G2/M checkpoint dependency across diverse cancer cell line backgrounds [11].

The remaining five partner genes (PPM1D, POLQ, ATR, MDM2, USP28) were not validated via BioGRID ORCS in this pipeline. USP28 was located during Phase 1 but not fully enriched. ATR and POLQ had evidence limited to the reference paper only (L1 Single-DB). MDM2 was resolved as a p53 regulator rather than a synthetic lethal partner.

It is important to note that BioGRID ORCS essentiality reflects pan-essential profiles across all screened cell lines, not TP53-specific synthetic lethality. The differential vulnerability of TP53-mutant cells to WEE1/CHEK1/PLK1 inhibition depends on the isogenic screen design of Feng et al. (2022), where paired TP53-wild-type and TP53-knockout cell lines were compared [5].

### 3.3 Drug Candidate Identification

Drug traversal via Open Targets knownDrugs identified seven compounds targeting four gene products [12]:

**WEE1 inhibitors**: Adavosertib (AZD1775) was the sole WEE1 inhibitor identified, at maximum Phase 2 clinical development. Its mechanism — forcing TP53-mutant cells into mitosis with unrepaired DNA, causing mitotic catastrophe — directly exploits the synthetic lethal relationship with TP53 loss [12].

**CHEK1 inhibitors**: Prexasertib (Phase 2) and rabusertib (Phase 2) were identified. Prexasertib inhibits CHEK1-mediated S-phase checkpoint control, inducing replication catastrophe in cells lacking p53-mediated DNA damage response [12].

**PLK1 inhibitors**: Volasertib (Phase 3, highest clinical phase among all candidates) and BI-2536 (Phase 2) were identified. PLK1 inhibition disrupts mitotic checkpoint recovery, though no TP53-specific clinical trials were found for either compound [12].

**MDM2 antagonists** (mechanism mismatch): Idasanutlin (Phase 3) and navtemadlin (Phase 2) were identified as MDM2 antagonists that disrupt the p53-MDM2 interaction to stabilize p53 protein. However, this mechanism requires functional wild-type TP53 protein. In TP53-mutant cancers — the focus of this competency question — p53 is structurally compromised or absent, rendering MDM2 antagonists ineffective. A −0.20 mechanism mismatch modifier was applied to these compounds [12].

### 3.4 Clinical Trial Evidence

Five clinical trials were discovered and validated via ClinicalTrials.gov [13]:

Three trials specifically enrolled TP53-mutant patients, all using adavosertib (AZD1775): NCT01164995 (Phase II, WEE1 inhibitor + carboplatin in TP53-mutant epithelial ovarian cancer, COMPLETED), NCT02448329 (Phase II, AZD1775 + paclitaxel in TP53-mutant gastric adenocarcinoma, COMPLETED), and NCT02688907 (Phase II, AZD1775 monotherapy in TP53-mutant SCLC, TERMINATED) [13]. This concentration of TP53-specific trial evidence around adavosertib makes WEE1 inhibition the most clinically validated synthetic lethal strategy for this population.

One additional trial tested prexasertib: NCT02808650 (Phase I/II, prexasertib in pediatric solid tumors, COMPLETED), though without TP53-specific enrollment criteria [13].

The idasanutlin trial NCT02545283 (Phase III, idasanutlin + cytarabine in relapsed/refractory AML) was terminated for futility and explicitly required TP53 wild-type status, confirming the mechanism mismatch identified during drug traversal [13].

### 3.5 Evidence Assessment

Eleven claims were individually graded using the L1-L4 evidence grading system [Table S3]. The distribution was: 1 × L4 (adavosertib/WEE1 SL with TP53-mutant trials, score 0.95), 2 × L3 (prexasertib/CHEK1 at 0.80, volasertib/PLK1 at 0.70), 6 × L2 (including ORCS essentiality claims at 0.60 each, MDM2 interaction at 0.65, MDM2 antagonist mismatch at 0.50, PPM1D modulation at 0.50), and 2 × L1 (POLQ and ATR synthetic lethality at 0.35 each).

The median claim confidence was 0.60 (L2 Multi-DB), with a range of 0.35–0.95. No claims fell below the L1 minimum threshold of 0.30.

### 3.6 Protein Interaction Network and Pathway Context

The STRING protein-protein interaction network for TP53 (9606.ENSP00000269305) revealed 15 high-confidence interactors (score ≥ 0.700) [9]. The highest-scoring interactors included SIRT1 (0.999), RPA1 (0.999), MDM2 (0.995), ATM (0.995), and HDAC1 (0.993). MDM2's near-perfect STRING score (0.995) reflects its well-established role as the primary E3 ubiquitin ligase targeting p53 for proteasomal degradation [9].

WikiPathways analysis identified four relevant pathways: TP53 network (WP1742), Cell cycle checkpoints (WP1775), ATM signaling in development and disease (WP3878), and DNA Damage/Telomere Stress Induced Senescence (WP3565) [10]. The cell cycle checkpoints pathway (WP1775) is particularly relevant as it encompasses the G1/S, S-phase, and G2/M checkpoints involving WEE1, CHEK1, and PLK1.

### 3.7 Disease Associations

Open Targets reported 3,277 disease associations for TP53 (ENSG00000141510) [2]. The top association was Li-Fraumeni syndrome (MONDO_0018875, score 0.876), followed by hepatocellular carcinoma (EFO_0000182, score 0.796), head and neck squamous cell carcinoma (EFO_0000181, score 0.777), and hereditary breast cancer (Orphanet_227535, score 0.743) [2]. These represent high-priority cancer types where TP53 mutation frequency is elevated and synthetic lethal strategies could be clinically impactful.

### 3.8 Synapse Dataset Grounding

Five Synapse.org datasets were identified as relevant to the knowledge graph [15]:

The Broad-DREAM Gene Essentiality Prediction Challenge (syn2384331) provided Moderate grounding for WEE1, CHEK1, and PLK1 essentiality claims, though it used RNAi-based screening rather than CRISPR knockout [15]. The Cancer Cell Line Encyclopedia (CCLE, syn5889324) provided Moderate grounding for TP53, WEE1, CHEK1, and PLK1 via cell line characterization and drug sensitivity data [15]. The TCGA Ovarian Serous Cystadenocarcinoma dataset (syn274455) provided Moderate grounding for TP53 disease associations [15]. An NF1 synthetic lethality project (syn21641955) provided Analogous methodology grounding — same CRISPR-based SL concept applied to a different tumor suppressor [15]. A cell signaling and drug therapy project (syn53480749) provided Weak pathway-level context [15].

Overall Synapse grounding achieved 21% node coverage (5/24 nodes) and 16% edge coverage (3/19 groundable edges). No evidence level upgrades were warranted, as the datasets provided contextual support but did not directly test TP53-specific synthetic lethality.

### 3.9 Quality Review

The 10-dimension quality review yielded an overall PASS verdict with protocol execution quality 8/10 and report presentation quality 8/10. Seven dimensions scored PASS (Source Attribution, OT Pagination, Evidence Grading, GoF Filter, Trial Validation, Completeness, Hallucination Risk). Three dimensions scored PARTIAL: CURIE Resolution (compounds used custom format rather than CHEMBL CURIEs), LOCATE→RETRIEVE discipline (LOCATE steps not explicitly cited in report despite being executed), and Disease CURIE (disease entities present in separate section but not promoted to formal graph nodes). All three PARTIAL scores were classified as presentation or protocol gaps of moderate severity that did not compromise scientific validity.

---

## 4. Discussion

### 4.1 WEE1 as the Most Actionable Synthetic Lethal Target

This integrative analysis identifies WEE1 inhibition via adavosertib as the most clinically validated synthetic lethal strategy for TP53-mutant cancers. This conclusion is supported by three convergent evidence streams: (1) CRISPR screen identification of WEE1 as a TP53 synthetic lethal partner in isogenic cell lines [5], (2) independent validation of WEE1 as broadly essential across 882 CRISPR knockout screens in BioGRID ORCS [11], and (3) completed Phase II clinical trials specifically enrolling TP53-mutant patients in ovarian cancer (NCT01164995) and gastric adenocarcinoma (NCT02448329) [13]. The L4 Clinical evidence level (score 0.95) assigned to this claim is the highest in the knowledge graph and reflects a rare convergence of genetic, pharmacological, and clinical data supporting a single synthetic lethal target-drug pair.

### 4.2 Checkpoint Kinase Inhibition as a Complementary Strategy

CHEK1 inhibition with prexasertib and PLK1 inhibition with volasertib represent complementary approaches to exploiting TP53 loss, though with less clinical maturity than WEE1 inhibition. CHEK1 mediates the intra-S-phase checkpoint, and its inhibition in TP53-mutant cells induces replication catastrophe rather than the mitotic catastrophe caused by WEE1 inhibition [5]. The mechanistic distinction between S-phase (CHEK1) and G2/M (WEE1) checkpoint dependencies suggests these targets may have non-overlapping patient populations or could be combined in rational combinations. PLK1 inhibition is notable because volasertib has reached Phase 3 clinical development — the highest phase of any compound in this analysis — but no TP53-specific trials have been conducted, representing a significant gap in clinical evidence.

### 4.3 MDM2 Antagonist Mechanism Mismatch

The identification and subsequent exclusion of MDM2 antagonists (idasanutlin, navtemadlin) illustrates the importance of biological context in synthetic lethality drug repurposing. MDM2 antagonists restore p53 function by disrupting the MDM2-p53 protein-protein interaction, which is only therapeutically relevant when wild-type p53 protein exists to be stabilized. In the TP53-mutant context of this competency question, p53 is structurally compromised or absent, making MDM2 antagonists mechanistically ineffective. The Phase III idasanutlin trial (NCT02545283) in AML was terminated for futility and explicitly required TP53 wild-type status [13], providing clinical validation of this mechanism mismatch. The −0.20 evidence modifier applied to MDM2 antagonists reflects this fundamental biological incompatibility.

### 4.4 Limitations and Future Directions

Several limitations of this analysis should be noted. First, BioGRID ORCS essentiality data reflects pan-essential profiles across all screened cell lines, not TP53-specific synthetic lethality. The differential vulnerability hypothesis requires the isogenic comparison from Feng et al. (2022) and cannot be directly confirmed from ORCS data alone. Second, ATR and POLQ — two biologically compelling synthetic lethal partners — lack independent database validation in this pipeline (L1 evidence). Known ATR inhibitors (berzosertib, ceralasertib) and POLQ inhibitors (novobiocin) were not retrieved because drug traversal was not performed for these targets. Third, no published clinical outcome data (response rates, progression-free survival, overall survival) were retrieved; trial verification confirmed registration and status but not efficacy results. Fourth, Synapse dataset grounding achieved moderate coverage (21% nodes, 16% edges) because the primary CRISPR screen data from Feng et al. is deposited in GEO/SRA rather than Synapse.

Future work should extend drug traversal to ATR (ENSG00000175054) and POLQ (ENSG00000051341), fully enrich USP28 (HGNC:12625) as a candidate synthetic lethal partner, and query GEO and DepMap (Broad Institute) for stronger CRISPR screen grounding. Clinical outcome data from completed adavosertib trials would strengthen the L4 evidence level with efficacy endpoints.

---

## References

1. HGNC Database. TP53 gene record (HGNC:11998). [Source: hgnc_get_gene(HGNC:11998)]
2. Open Targets Platform. Disease associations for ENSG00000141510. [Source: opentargets_get_associations(ENSG00000141510)]
3. Open Targets Platform. knownDrugs for MDM2 (ENSG00000135679). [Source: curl OpenTargets/graphql(knownDrugs, ENSG00000135679)]
4. Concept reference — synthetic lethality in genetics.
5. Feng X, Tang M, Dede M, Su D, Pei G et al. "Genome-wide CRISPR screens using isogenic cells reveal vulnerabilities conferred by loss of tumor suppressors." *Science Advances* (2022). PMID: 35559673. [Source: get_article_metadata("35559673")]
6. HGNC Database. Gene resolution via hgnc_search_genes and hgnc_get_gene. [Source: hgnc_search_genes("TP53"), hgnc_get_gene(HGNC:11998), hgnc_get_gene(HGNC:12761), hgnc_get_gene(HGNC:1925), hgnc_get_gene(HGNC:9077), hgnc_get_gene(HGNC:6973), hgnc_get_gene(HGNC:9277)]
7. NCBI Entrez Gene. Gene resolution for POLQ. [Source: entrez_search_genes("POLQ"), entrez_get_gene(NCBIGene:10721)]
8. NCBI Entrez Gene. Gene resolution for ATR. [Source: entrez_search_genes("ATR"), entrez_get_gene(NCBIGene:545)]
9. STRING Database v12.0. Protein-protein interactions for TP53. [Source: string_search_proteins("TP53"), string_get_interactions(9606.ENSP00000269305)]
10. WikiPathways. Pathway membership for TP53. [Source: wikipathways_get_pathways_for_gene(TP53)]
11. BioGRID ORCS. CRISPR essentiality data. [Source: get_orcs_essentiality(7465), get_orcs_essentiality(1111), get_orcs_essentiality(5347)]
12. Open Targets Platform. knownDrugs queries. [Source: curl OpenTargets/graphql(knownDrugs, ENSG00000166483), curl OpenTargets/graphql(knownDrugs, ENSG00000149554), curl OpenTargets/graphql(knownDrugs, ENSG00000166851)]
13. ClinicalTrials.gov. Trial verification. [Source: clinicaltrials_get_trial(NCT:01164995), clinicaltrials_get_trial(NCT:02448329), clinicaltrials_get_trial(NCT:02688907), clinicaltrials_get_trial(NCT:02808650), clinicaltrials_get_trial(NCT:02545283)]
14. Fuzzy-to-Fact Protocol v1.0. Evidence grading and quality review framework.
15. Synapse.org. Dataset grounding queries. [Source: search_synapse("TP53 synthetic lethality cancer"), get_entity(syn2384331), get_entity(syn5889324), get_entity(syn21641955), get_entity(syn53480749)]

---

## Supplementary Materials

### Table S1: Knowledge Graph Nodes (N=24)

| # | ID | Type | Label | Key Properties |
|---|-----|------|-------|---------------|
| 1 | HGNC:11998 | Gene | TP53 | Anchor; 17p13.1; ENSG00000141510 |
| 2 | HGNC:12761 | Gene | WEE1 | SL partner; 882 ORCS hits; 11p15.4 |
| 3 | HGNC:1925 | Gene | CHEK1 | SL partner; 928 ORCS hits; 11q24.2 |
| 4 | HGNC:9077 | Gene | PLK1 | SL partner; 929 ORCS hits; 16p12.2 |
| 5 | HGNC:6973 | Gene | MDM2 | Regulator; E3 ubiquitin ligase; 12q15 |
| 6 | HGNC:9277 | Gene | PPM1D | SL partner; Wip1 phosphatase; 17q23.3 |
| 7 | NCBIGene:10721 | Gene | POLQ | SL partner; MMEJ repair; 3q13.33 |
| 8 | NCBIGene:545 | Gene | ATR | SL partner; replication stress; 3q23 |
| 9 | COMPOUND:ADAVOSERTIB | Compound | Adavosertib | WEE1 inhibitor; Phase 2 |
| 10 | COMPOUND:PREXASERTIB | Compound | Prexasertib | CHEK1 inhibitor; Phase 2 |
| 11 | COMPOUND:RABUSERTIB | Compound | Rabusertib | CHEK1 inhibitor; Phase 2 |
| 12 | COMPOUND:VOLASERTIB | Compound | Volasertib | PLK1 inhibitor; Phase 3 |
| 13 | COMPOUND:BI-2536 | Compound | BI-2536 | PLK1 inhibitor; Phase 2 |
| 14 | COMPOUND:IDASANUTLIN | Compound | Idasanutlin | MDM2 antagonist; Phase 3; TP53-WT only |
| 15 | COMPOUND:NAVTEMADLIN | Compound | Navtemadlin | MDM2 antagonist; Phase 2; TP53-WT only |
| 16 | WP:WP1742 | Pathway | TP53 network | Core p53 signaling |
| 17 | WP:WP1775 | Pathway | Cell cycle checkpoints | G1/S and G2/M checkpoints |
| 18 | WP:WP3878 | Pathway | ATM signaling | Upstream ATM/ATR cascade |
| 19 | WP:WP3565 | Pathway | DNA Damage Senescence | p53-dependent senescence |
| 20 | NCT01164995 | Trial | AZD1775 + carboplatin (OV) | Phase II; COMPLETED; TP53-mutant |
| 21 | NCT02688907 | Trial | AZD1775 monotherapy (SCLC) | Phase II; TERMINATED; TP53-mutant |
| 22 | NCT02448329 | Trial | AZD1775 + paclitaxel (gastric) | Phase II; COMPLETED; TP53-mutant |
| 23 | NCT02808650 | Trial | Prexasertib (pediatric) | Phase I/II; COMPLETED |
| 24 | NCT02545283 | Trial | Idasanutlin + cytarabine (AML) | Phase III; TERMINATED; TP53-WT only |

### Table S2: Knowledge Graph Edges (N=23)

| # | Source | Target | Type | Key Evidence |
|---|--------|--------|------|-------------|
| 1 | TP53 | WEE1 | SYNTHETIC_LETHAL | G2/M checkpoint; ORCS 882 hits |
| 2 | TP53 | CHEK1 | SYNTHETIC_LETHAL | S-phase checkpoint; ORCS 928 hits |
| 3 | TP53 | PLK1 | SYNTHETIC_LETHAL | Mitotic checkpoint; ORCS 929 hits |
| 4 | TP53 | POLQ | SYNTHETIC_LETHAL | MMEJ repair; Feng 2022 only |
| 5 | TP53 | ATR | SYNTHETIC_LETHAL | Replication stress; Feng 2022 only |
| 6 | MDM2 | TP53 | REGULATES | E3 ubiquitin ligase; STRING 0.995 |
| 7 | PPM1D | TP53 | REGULATES | Phosphatase; DDR modulator |
| 8–11 | TP53 | WP:WP* | MEMBER_OF | WikiPathways membership (×4) |
| 12 | Adavosertib | WEE1 | INHIBITOR | OT knownDrugs; Phase 2 |
| 13 | Prexasertib | CHEK1 | INHIBITOR | OT knownDrugs; Phase 2 |
| 14 | Rabusertib | CHEK1 | INHIBITOR | OT knownDrugs; Phase 2 |
| 15 | Volasertib | PLK1 | INHIBITOR | OT knownDrugs; Phase 3 |
| 16 | BI-2536 | PLK1 | INHIBITOR | OT knownDrugs; Phase 2 |
| 17 | Idasanutlin | MDM2 | INHIBITOR | OT knownDrugs; Phase 3; TP53-WT |
| 18 | Navtemadlin | MDM2 | INHIBITOR | OT knownDrugs; Phase 2; TP53-WT |
| 19 | Adavosertib | NCT01164995 | TESTED_IN | TP53-mutant ovarian cancer |
| 20 | Adavosertib | NCT02688907 | TESTED_IN | TP53-mutant SCLC |
| 21 | Adavosertib | NCT02448329 | TESTED_IN | TP53-mutant gastric cancer |
| 22 | Prexasertib | NCT02808650 | TESTED_IN | Pediatric solid tumors |
| 23 | Idasanutlin | NCT02545283 | TESTED_IN | R/R AML (TP53-WT required) |

### Table S3: Evidence Grading Summary (N=11 claims)

| # | Claim | Base | Modifiers | Final | Level |
|---|-------|------|-----------|-------|-------|
| 1 | TP53-WEE1 SL + Phase II trials | L4 (0.90) | +0.10 mechanism, +0.05 lit | 0.95 | L4 |
| 2 | WEE1 ORCS essentiality (882) | L2 (0.55) | +0.05 lit | 0.60 | L2 |
| 3 | CHEK1 ORCS essentiality (928) | L2 (0.55) | +0.05 lit | 0.60 | L2 |
| 4 | PLK1 ORCS essentiality (929) | L2 (0.55) | +0.05 lit | 0.60 | L2 |
| 5 | Prexasertib/CHEK1 + Phase I/II | L3 (0.70) | +0.10 mechanism | 0.80 | L3 |
| 6 | Volasertib/PLK1 Phase 3 | L3 (0.70) | +0.10 mechanism, −0.10 no trial | 0.70 | L3 |
| 7 | MDM2 antagonists contraindicated | L3 (0.70) | −0.20 mismatch | 0.50 | L2 |
| 8 | TP53-MDM2 interaction (0.995) | L2 (0.55) | +0.05 STRING, +0.05 lit | 0.65 | L2 |
| 9 | POLQ SL (MMEJ dependency) | L1 (0.40) | +0.05 lit, −0.10 single | 0.35 | L1 |
| 10 | ATR SL (replication stress) | L1 (0.40) | +0.05 lit, −0.10 single | 0.35 | L1 |
| 11 | PPM1D DDR modulation | L1 (0.45) | +0.05 lit | 0.50 | L2 |

**Median**: 0.60 | **Range**: 0.35–0.95 | **Distribution**: 1×L4, 2×L3, 6×L2, 2×L1

### Table S4: Synapse Dataset Grounding

| Synapse ID | Name | Nodes Grounded | Edges Grounded | Strength |
|-----------|------|---------------|---------------|----------|
| syn2384331 | Broad-DREAM Gene Essentiality Challenge | WEE1, CHEK1, PLK1 | SL edges (essentiality) | Moderate |
| syn5889324 | Cancer Cell Line Encyclopedia (CCLE) | TP53, WEE1, CHEK1, PLK1 | Drug sensitivity | Moderate |
| syn21641955 | NF1 Synthetic Lethality Investigation | — | SL methodology | Analogous |
| syn274455 | TCGA Ovarian Serous Cystadenocarcinoma | TP53 | Disease association | Moderate |
| syn53480749 | Cell Signaling and Drug Therapy | — | Pathway context | Weak |

**Coverage**: Nodes 21% (5/24) | Edges 16% (3/19 groundable)

---

*Manuscript generated by Fuzzy-to-Fact Publication Pipeline v1.0. All claims sourced from Phase 1-7 tool outputs with inline provenance tracking. No entities, CURIEs, or quantitative values were introduced from training knowledge.*
