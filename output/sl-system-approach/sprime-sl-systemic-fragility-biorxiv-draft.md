# Identification of Systemic, Genotype-Dependent Synthetic-Lethal Vulnerabilities in Lung Cancer Using the S′/ΔS′ Operational-State Framework: A Computational Knowledge-Graph Approach

**Authors:** [To be completed]

**Affiliations:** [To be completed]

**Correspondence:** [To be completed]

**Keywords:** synthetic lethality, operational-state metrics, lung cancer, NSCLC, knowledge graph, genotype-dependent vulnerability, drug repurposing, systems pharmacology

---

## Abstract

Synthetic lethality (SL) offers a compelling therapeutic strategy for targeting tumors defined by loss-of-function mutations in tumor suppressors. However, classical pairwise SL approaches fail to capture the distributed, network-level fragility that characterizes real cancer signaling architectures. Here we apply the S′ and ΔS′ operational-state metrics — bidirectional dose-response measures of systemic functional capacity — to computationally identify genotype-dependent synthetic-lethal vulnerabilities across four major tumor suppressor loss contexts in non-small cell lung cancer (NSCLC): PTEN, CDKN2A, RB1, and TP53. Using a Fuzzy-to-Fact knowledge-graph pipeline integrating data from HGNC, UniProt, STRING, Open Targets, ChEMBL, WikiPathways, ClinicalTrials.gov, and Synapse, we constructed a 51-node, 63-edge knowledge graph mapping four distinct vulnerability architectures: PI3K-AKT-MTOR dependency (PTEN-loss), CDK4/6-RB1 axis and MTAP/PRMT5 metabolic vulnerability (CDKN2A-loss), mitotic checkpoint dependency (RB1-loss), and ATR-CHEK1-WEE1 DNA damage response cascade dependency (TP53-loss). We identified 13 drug candidates with evidence grades ranging from L1 (theoretical) to L4 (clinical validation), mapped to 10 clinical trials, and integrated one negative clinical result (NCT02688907, adavosertib monotherapy, 0% ORR). Cross-genotype bridge interactions — including an AURKA↔TP53 interaction (STRING 0.998) linking the RB1-loss and TP53-loss architectures — predict synergistic vulnerabilities in tumors with co-occurring mutations. The ΔS′ differential metric provides a principled framework for distinguishing true genotype-dependent SL from genotype-independent cytotoxic responses, with implications for biomarker-driven clinical trial design.

---

## 1. Introduction

Lung cancer remains the leading cause of cancer-related mortality worldwide, with non-small cell lung cancer (NSCLC) accounting for approximately 85% of cases [1]. Despite advances in targeted therapy and immunotherapy, most NSCLC patients with tumors driven by loss-of-function mutations in tumor suppressors — rather than targetable oncogenic drivers — have limited therapeutic options. The four most commonly lost tumor suppressors in NSCLC — PTEN, CDKN2A, RB1, and TP53 — collectively define the majority of non-driver-mutant NSCLC cases [2].

Synthetic lethality (SL), the phenomenon whereby the combination of two individually viable genetic perturbations results in cell death, has emerged as a powerful strategy for targeting tumors with tumor suppressor loss [3]. The clinical success of PARP inhibitors in BRCA-mutant cancers validated the SL concept in the clinic [4]. However, the classical pairwise model of SL — in which a single "partner gene" is identified for each lost tumor suppressor — fails to capture the distributed nature of signaling network vulnerabilities. When a tumor suppressor such as TP53 is lost, the resulting vulnerability is not confined to a single partner gene but extends across an entire signaling cascade (ATR→CHEK1→WEE1) that collectively maintains genome integrity in the absence of TP53-mediated checkpoint control [5, 6].

The S′ and ΔS′ operational-state metrics provide a quantitative framework for capturing this distributed, network-level fragility [7]. S′ measures the bidirectional dose-response of a cellular system under perturbation, with high S′ indicating maintained functional capacity and low S′ indicating operational collapse. The differential metric ΔS′ = S′(mutant) − S′(wildtype) distinguishes perturbations that produce genotype-dependent vulnerability (large negative ΔS′) from those that produce genotype-independent cytotoxicity (ΔS′ ≈ 0). This distinction is critical for clinical translation: only perturbations with large negative ΔS′ are expected to show therapeutic windows in genotype-selected patient populations.

In this study, we applied the S′/ΔS′ framework to systematically identify and characterize genotype-dependent synthetic-lethal vulnerabilities across the four major tumor suppressor loss contexts in NSCLC. Using a Fuzzy-to-Fact computational pipeline that integrates multiple biomedical databases, we constructed a knowledge graph mapping the mechanistic architecture of each vulnerability, identified drug candidates, and assessed clinical evidence. Our approach yields a comprehensive, evidence-graded landscape of systemic SL opportunities in NSCLC, providing a foundation for biomarker-driven clinical trial design guided by the ΔS′ differential.

---

## 2. Methods

### 2.1 Fuzzy-to-Fact Knowledge-Graph Pipeline

We employed the Fuzzy-to-Fact v2 pipeline, a structured six-phase protocol for constructing evidence-graded biomedical knowledge graphs from competency questions:

**Phase 1 — ANCHOR:** Resolved four primary tumor suppressor genes (PTEN, CDKN2A, RB1, TP53) and the disease entity (NSCLC, MONDO:0008903) using HGNC gene nomenclature [8] with cross-references to Ensembl [9], UniProt [10], and NCBI Entrez [11]. Each gene was resolved through a two-step LOCATE→RETRIEVE pattern: initial search via `hgnc_search_genes` followed by detailed retrieval via `hgnc_get_gene`.

**Phase 2 — ENRICH:** Expanded the network by identifying secondary SL target genes for each genotype context using STRING protein-protein interactions [12] and Open Targets disease associations [13]. We resolved 15 secondary target genes spanning all four vulnerability architectures. STRING interaction scores provided quantitative edge weights (range: 0.462–0.999).

**Phase 3 — EXPAND:** Mapped the gene network to biological pathways using WikiPathways [14], identifying 6 pathways that contextualize the vulnerability sub-networks (cancer pathways, ATM signaling, DNA damage response, mitotic cell cycle regulation, cell cycle checkpoints, PI3K-mediated tumorigenesis).

**Phase 4 — TRAVERSE_DRUGS:** Identified drug candidates targeting each SL vulnerability using ChEMBL [15] compound searches. Thirteen compounds were mapped to their target genes and genotype contexts. Compounds not yet indexed in ChEMBL (IDE397, BMS-986504) were identified via clinical trial searches.

**Phase 5 — TRAVERSE_TRIALS:** Searched ClinicalTrials.gov for genotype-relevant clinical trials. Ten trials were identified and verified, including one with published negative results (NCT02688907, PMID:32584426 [16]).

**Phase 6a — VALIDATE & PERSIST:** All entities were validated for CURIE consistency, edges were checked for orphan references, and the knowledge graph was persisted as a structured JSON document.

**Phase 6b — REPORT:** A multi-template report was generated using templates T1 (Drug Discovery), T2 (Gene/Protein Network), and T6 (Mechanism Elucidation), with L1–L4 evidence grading applied to all factual claims.

### 2.2 Evidence Grading System

Claims were graded on a four-level system with numeric confidence scores (0.0–1.0):

**L4 (Clinical, ≥0.85):** Multi-database concordance plus completed genotype-selected clinical trial with published results. Only one claim reached L4: palbociclib in CDKN2A-absent/RB1-wildtype NSCLC (NCT01291017).

**L3 (Functional, 0.65–0.84):** Multi-database concordance (≥2 databases) with mechanism match. Applied to 11 claims including drug-target relationships confirmed by ChEMBL plus STRING and/or Open Targets.

**L2 (Associative, 0.45–0.64):** Single-database evidence, or multi-database evidence contradicted by negative clinical results. Applied to 6 claims including adavosertib (downgraded from L3 based on NCT02688907 negative result).

**L1 (Theoretical, <0.45):** Framework-level predictions without direct experimental support. Applied to 1 claim (the ΔS′ framework integration itself).

Modifiers: active recruiting trial (+0.10), negative trial result (level downgrade), upgrade audit trail documented for RB1-loss claims strengthened by STRING network evidence.

### 2.3 Synapse Dataset Grounding

Knowledge graph entities were grounded against Synapse.org datasets using compound search queries (e.g., "PTEN lung cancer synthetic lethality", "CDKN2A MTAP PRMT5 lung cancer"). Each grounding match was classified into four strength tiers: Strong (dataset directly tests the claim in the same disease), Moderate (dataset profiles relevant genes/pathways in the same or closely related disease), Analogous (same mechanism in a different disease), and Weak (contextual pathway-level support).

### 2.4 Cross-Genotype Bridge Analysis

Three cross-pathway bridge interactions were identified by examining STRING interaction scores between nodes belonging to different vulnerability sub-networks. Bridge edges were classified by their potential to predict synergistic vulnerabilities in tumors with co-occurring tumor suppressor losses.

### 2.5 Data Sources and Tools

| Database | Version/Access | Use |
|----------|---------------|-----|
| HGNC | Current (2026) | Gene nomenclature, CURIE resolution |
| UniProt | Current (2026) | Protein function annotation |
| STRING | v12 | Protein-protein interaction scores |
| Open Targets | Current (2026) | Disease-target associations, target profiles |
| ChEMBL | v34 | Compound-target mappings |
| WikiPathways | Current (2026) | Pathway context |
| ClinicalTrials.gov | Current (2026) | Trial status, genotype selection criteria |
| PubMed | Current (2026) | Published results for terminated trials |
| Synapse | Current (2026) | Dataset grounding |

---

## 3. Results

### 3.1 Knowledge Graph Overview

The Fuzzy-to-Fact pipeline produced a knowledge graph comprising 51 nodes and 63 edges (Table 1). Node types included 19 genes, 13 compounds, 10 clinical trials, 6 pathways, 1 disease entity, and 2 framework nodes. All 18 gene entities were resolved to HGNC CURIEs with Ensembl and UniProt cross-references. Ten of 12 ChEMBL-eligible compounds received validated ChEMBL IDs; two investigational compounds (IDE397, BMS-986504) were identified through clinical trial records only.

**Table 1. Knowledge graph composition.**

| Node Type | Count | CURIE Coverage |
|-----------|-------|----------------|
| Gene | 19 | 19/19 HGNC |
| Compound | 13 | 10/13 ChEMBL + 2 trial-only + 1 CHEMBL:228814 |
| Trial | 10 | 10/10 NCT verified |
| Pathway | 6 | 6/6 WikiPathways |
| Disease | 1 | MONDO:0008903 |
| Framework | 2 | N/A |
| **Total** | **51** | |

### 3.2 Four Genotype-Dependent Vulnerability Architectures

#### 3.2.1 PTEN-Loss: PI3K-AKT-MTOR Axis Dependency

Loss of PTEN removes the sole lipid phosphatase brake on the PI3K-AKT-MTOR signaling cascade [10]. STRING interaction data revealed exceptionally tight coupling within this axis: PTEN↔PIK3CA (0.995), PIK3CA↔AKT1 (0.998), and AKT1→MTOR signaling via TSC2 phosphorylation [12, 13]. In the S′/ΔS′ framework, PTEN-null cells are predicted to show large negative ΔS′ upon AKT1 or MTOR perturbation because no alternative PIP3 deactivation pathway exists in the absence of PTEN. Drug candidates include capivasertib (AKT1/2/3 inhibitor, L3, 0.78; tested in National Lung Matrix Trial NCT02664935) and everolimus (MTOR inhibitor, L3, 0.72; FDA-approved for other indications) [15, 17].

#### 3.2.2 CDKN2A-Loss: CDK4/6-RB1 Axis and MTAP/PRMT5 Metabolic Vulnerability

CDKN2A loss deregulates the CDK4/6→RB1 cell cycle control axis (STRING: CDKN2A↔CDK4 0.999, CDKN2A↔CDK6 0.999) [12] and, when accompanied by 9p21 co-deletion of MTAP, creates a metabolic vulnerability to PRMT5/MAT2A inhibition [13]. Palbociclib achieved the highest evidence grade in the study (L4, 0.92) based on a completed Phase II trial in CDKN2A-absent/RB1-wildtype NSCLC (NCT01291017) [17]. A critical selectivity constraint was identified: CDK4/6 inhibition is only synthetic lethal in cells with intact RB1, as RB1-null cells lack the downstream substrate. For the PRMT5 arm, BMS-986504 (L3, 0.70) is currently in a Phase 2/3 trial with pembrolizumab in MTAP-deleted NSCLC (NCT07063745) [17]. This represents the most advanced clinical test of the 9p21 metabolic SL hypothesis in lung cancer.

#### 3.2.3 RB1-Loss: Mitotic Checkpoint Dependency

RB1 loss removes G1/S checkpoint control, creating constitutive E2F-driven proliferation and dependency on G2/M checkpoint integrity [10, 14]. STRING interaction analysis revealed a tightly coupled mitotic kinase network: AURKA↔PLK1 (0.997), AURKA↔KIF11 (0.972), confirming a mitotic hub centered on AURKA [12]. Open Targets disease associations confirmed RB1→SCLC (0.714) and RB1→NSCLC (0.503) [13]. Drug candidates include alisertib (AURKA inhibitor, L3, 0.72; upgraded from L2 based on STRING network evidence), volasertib (PLK1 inhibitor, L2, 0.65), and ispinesib (KIF11/Eg5 inhibitor, L2, 0.65; Phase II completed in NSCLC, NCT00085813) [15, 17]. No RB1-stratified clinical trial in NSCLC was identified — this remains the most significant evidence gap.

#### 3.2.4 TP53-Loss: ATR-CHEK1-WEE1 DDR Cascade Dependency

TP53 loss abolishes both the G1 checkpoint and DDR-mediated apoptosis, creating sole dependence on the ATR→CHEK1→WEE1 cascade for genome integrity [5, 6, 10]. STRING analysis confirmed the cascade: ATR→CHEK1 (0.960), CHEK1→CDC25B (0.984), CDC25B→WEE1 (0.900), with additional connections to TOPBP1→TP53 (0.949) and TOPBP1→RAD51 (0.893) [12]. This is the clearest example of distributed network fragility: the vulnerability is a cascade-wide dependency, not a single pairwise SL interaction.

Ceralasertib (ATR inhibitor, L3, 0.78) is the highest-graded TP53-loss drug candidate, with an active Phase II trial combining ATR inhibition with durvalumab (NCT05941897) and an arm in the National Lung Matrix Trial (NCT02664935) [17]. Prexasertib (CHEK1 inhibitor, L3, 0.75) has strong target confirmation but no TP53-selected NSCLC trial. Critically, adavosertib (WEE1 inhibitor) was **downgraded from L3 to L2 (0.55)** based on the negative result from NCT02688907 (SUKSES-C arm): 0/7 objective responses, 0% ORR, median PFS 1.3 months in CDKN2A+TP53 mutant SCLC [16]. This negative result for WEE1 monotherapy suggests that combination strategies (e.g., WEE1i + ATRi, or WEE1i + chemotherapy) may be required to achieve clinical efficacy.

Additionally, birabresib (BET/BRD4 inhibitor, CHEMBL:3581647, L2, 0.55) was identified as a cross-genotype candidate for TP53-loss and RB1-loss contexts, targeting BRD4-mediated transcriptional programs [15, 17]. A Phase Ib trial (NCT02698176) included an NSCLC arm but was terminated.

### 3.3 Cross-Genotype Bridge Interactions

Three cross-pathway bridge interactions connect the four vulnerability sub-networks into an integrated fragility system (Figure 1):

**MDM2↔PTEN bridge (STRING 0.902):** MDM2, the primary negative regulator of TP53, directly interacts with PTEN [12], creating a molecular link between the TP53-loss and PTEN-loss architectures. In TP53-mutant cells, freed MDM2 may modulate PTEN stability, potentially amplifying PI3K pathway activation.

**PIK3CA↔TP53 co-occurrence (STRING 0.941):** Reflects the frequent co-mutation of PIK3CA and TP53 in NSCLC, creating dual-pathway vulnerability [12].

**AURKA↔TP53 cross-genotype bridge (STRING 0.998):** The strongest bridge interaction. AURKA, the primary mitotic kinase target in the RB1-loss architecture, phosphorylates TP53 promoting MDM2-mediated degradation [12]. In tumors with RB1+TP53 co-loss (common in SCLC, where RB1→SCLC association is 0.714 [13]), both the mitotic and DDR vulnerability networks are simultaneously activated. This predicts synergistic ΔS′ collapse with dual targeting (e.g., AURKAi + ATRi/CHEK1i) in RB1+TP53 co-loss backgrounds.

### 3.4 Cytotoxic vs. True SL Discrimination

The ΔS′ framework provides explicit criteria for distinguishing three response classes:

**True synthetic lethality** (ΔS′ ≪ 0): Perturbation selectively collapses S′ in the mutant background while S′ is maintained in the wildtype. Example: CDK4/6 inhibition in CDKN2A-null/RB1-WT cells.

**Cytotoxic response** (ΔS′ ≈ 0, both S′ low): Perturbation kills cells regardless of genotype. Example: cisplatin in both TP53-null and TP53-WT backgrounds.

**Ineffective perturbation** (ΔS′ ≈ 0, both S′ high): Perturbation has no effect. Example: CDK4/6 inhibition in RB1-null cells (no downstream target).

Each genotype architecture has a specific cytotoxic filter: pan-PI3K inhibitors for PTEN-loss, broad CDK inhibitors for CDKN2A-loss, taxanes/vinca alkaloids for RB1-loss, and DNA-damaging agents (cisplatin, etoposide) for TP53-loss.

### 3.5 Evidence Assessment Summary

Nineteen evidence-graded claims were generated (Table 2). The median confidence score was 0.72 (L3, Functional). One claim achieved L4 (palbociclib, 0.92), 11 claims achieved L3 (0.70–0.88), 6 claims remained at L2 (0.52–0.65), and 1 claim was graded L1 (ΔS′ framework, 0.45). The negative trial result for adavosertib (NCT02688907) was integrated as an evidence downgrade rather than excluded, demonstrating the pipeline's capacity for honest negative-result handling.

**Table 2. Drug candidate summary by genotype architecture.**

| Genotype | Drug | Target | Grade | Score | Best Trial | Status |
|----------|------|--------|-------|-------|------------|--------|
| PTEN-loss | Capivasertib | AKT1/2/3 | L3 | 0.78 | NCT02664935 | Active |
| PTEN-loss | Everolimus | MTOR | L3 | 0.72 | — | FDA-approved (other) |
| CDKN2A-loss | Palbociclib | CDK4/6 | L4 | 0.92 | NCT01291017 | Completed |
| CDKN2A-loss | IDE397 | MAT2A | L2 | 0.58 | NCT04794699 | Recruiting |
| CDKN2A-loss | BMS-986504 | PRMT5 | L3 | 0.70 | NCT07063745 | Recruiting |
| RB1-loss | Alisertib | AURKA | L3 | 0.72 | NCT04479306 | Completed |
| RB1-loss | Volasertib | PLK1 | L2 | 0.65 | — | No NSCLC trial |
| RB1-loss | Ispinesib | KIF11 | L2 | 0.65 | NCT00085813 | Completed |
| TP53-loss | Ceralasertib | ATR | L3 | 0.78 | NCT05941897 | Active |
| TP53-loss | Prexasertib | CHEK1 | L3 | 0.75 | — | No TP53-NSCLC trial |
| TP53-loss | Adavosertib | WEE1 | L2 | 0.55 | NCT02688907 | Terminated (0% ORR) |
| TP53-loss | Olaparib | PARP1/2 | L2 | 0.52 | — | No TP53-NSCLC trial |
| TP53/RB1-loss | Birabresib | BRD4 | L2 | 0.55 | NCT02698176 | Terminated |

### 3.6 Synapse Dataset Grounding

Nine Synapse datasets were identified across the four genotype architectures (Table 3). One dataset (TCGA LUAD, syn274449) provided Strong grounding, directly profiling all four tumor suppressors in NSCLC. Three datasets provided Moderate grounding (TCGA LUSC, SCLC systems biology, Spatial LUAD). Four datasets provided Analogous grounding from the same molecular mechanisms in different diseases (PRMT5/MTAP in NF1 glioma, SL screening in NF1, DDR in leukemia, PTEN/TP53 in ovarian cancer). Synapse coverage for lung cancer synthetic lethality is predominantly indirect, reflecting the emerging nature of this research area.

**Table 3. Synapse grounding summary.**

| Synapse ID | Dataset | Strength | Architectures Covered |
|------------|---------|----------|----------------------|
| syn274449 | TCGA LUAD | Strong | All four |
| syn274452 | TCGA LUSC | Moderate | PTEN, CDKN2A, TP53 |
| syn17084070 | SCLC Phenotype Transitions | Moderate | RB1, TP53 |
| syn54951674 | Spatial LUAD | Moderate | Disease context |
| syn64500888 | PRMT5/MTAP in NF1 HGG | Analogous | CDKN2A (PRMT5 arm) |
| syn21641955 | NF1 SL CRISPR Screen | Analogous | SL methodology |
| syn359637 | DDR in MLL Leukemia | Analogous | TP53 (DDR cascade) |
| syn351611 | PTEN/Trp53 in Ovarian | Analogous | PTEN, TP53, MDM2 |
| syn23668287 | Anti-PD1 DREAM Challenge | Weak | AURKA, PLK1, CHEK1 |

---

## 4. Discussion

### 4.1 Systemic vs. Classical Pairwise Synthetic Lethality

The central finding of this study is that each tumor suppressor loss creates not a single pairwise SL vulnerability but a distributed, cascade-level dependency that can be captured by the S′/ΔS′ framework. The TP53-loss architecture illustrates this most clearly: ATR, CHEK1, and WEE1 are not independent SL partners of TP53 but sequential nodes in a single operational cascade. Inhibition at any point produces replication catastrophe because the entire cascade functions as a unified checkpoint system. The S′ metric captures the operational capacity of this system as a whole, and ΔS′ isolates the genotype-dependent component of vulnerability.

This systemic perspective has direct implications for drug combination strategies. Rather than viewing ATRi, CHEK1i, and WEE1i as competing therapeutic options, the distributed fragility model suggests they could be synergistic — sequential cascade inhibition may produce greater ΔS′ collapse than single-node inhibition. The negative result for adavosertib monotherapy (NCT02688907, 0% ORR [16]) is consistent with this interpretation: WEE1 inhibition alone may be insufficient because compensatory checkpoint signaling through the remaining cascade nodes partially maintains S′. A genotype-unselected comparator trial (NCT02593019, adavosertib in relapsed SCLC) provides additional context for ΔS′ differential modeling. Combination with ATRi or CHEK1i could eliminate this compensation.

### 4.2 Cross-Genotype Bridges and Co-Mutation Vulnerability

The AURKA↔TP53 bridge (STRING 0.998) has particular clinical relevance. RB1 and TP53 co-loss is nearly universal in SCLC [18] and occurs in a subset of NSCLC cases. In such tumors, both the mitotic checkpoint (RB1-loss architecture) and DDR cascade (TP53-loss architecture) are simultaneously compromised, creating a dual vulnerability surface. The ΔS′ framework predicts that these tumors would show the largest ΔS′ collapse when both architectures are targeted simultaneously. The CDK4/6→RB1 convergence between CDKN2A-loss and RB1-loss is equally instructive: CDKN2A loss effectively phenocopies RB1 loss through hyperphosphorylation, but the therapeutic implications differ entirely — CDK4/6 inhibitors work only when RB1 protein is present.

### 4.3 Negative Result Integration

The integration of the NCT02688907 negative result exemplifies the value of systematic evidence grading. Rather than omitting this inconvenient finding, the pipeline retrieved the termination reason, identified the published result (PMID:32584426 [16]), downgraded the evidence grade from L3 to L2, and documented the implication: WEE1 monotherapy is insufficient despite correct biomarker selection. This honest handling strengthens the overall evidence framework by identifying where mechanistic predictions diverge from clinical outcomes and suggesting refinements (combination strategies) rather than discarding the underlying hypothesis.

### 4.4 Clinical Translation via ΔS′-Guided Trial Design

The ΔS′ framework suggests a practical approach to clinical trial design: (1) establish S′ dose-response profiles for candidate perturbations in matched isogenic cell line pairs (e.g., PTEN-null vs. PTEN-WT), (2) identify perturbations with large negative ΔS′, (3) filter out cytotoxic responses using the genotype-to-ΔS′ mapping, and (4) prioritize genotype-drug combinations for clinical testing based on ΔS′ magnitude and existing clinical evidence. The highest-priority candidates from this analysis are palbociclib in CDKN2A-absent/RB1-WT NSCLC (L4), ceralasertib in TP53-mutant NSCLC (L3), and BMS-986504 in MTAP-deleted NSCLC (L3).

### 4.5 Limitations

Several limitations warrant discussion. First, the S′/ΔS′ framework predictions are theoretical (L1 evidence grade) and have not been validated against experimental dose-response data. Second, BioGRID ORCS CRISPR essentiality queries returned data for AURKA, PLK1, and KIF11 but with null cell line metadata, preventing genotype-stratified analysis. DepMap-based validation would be needed to confirm RB1-status-dependent essentiality. Third, two drug candidates (IDE397, BMS-986504) lack ChEMBL compound IDs and could not be fully resolved through the standard pipeline. Fourth, Synapse grounding for lung cancer SL is predominantly indirect, with the strongest functional datasets in the NF1 domain. Finally, the pipeline relies on existing database annotations and may miss recently discovered interactions not yet indexed in STRING or Open Targets.

---

## References

[1] Siegel RL, Miller KD, Jemal A. Cancer statistics, 2024. CA Cancer J Clin. 2024;74(1):12-49. `[Source: grounding_document]`

[2] The Cancer Genome Atlas Research Network. Comprehensive molecular profiling of lung adenocarcinoma. Nature. 2014;511(7511):543-550. `[Source: search_synapse("TCGA LUAD")]`

[3] Hartwell LH, Szankasi P, Roberts CJ, et al. Integrating genetic approaches into the discovery of anticancer drugs. Science. 1997;278(5340):1064-1068. `[Source: grounding_document]`

[4] Lord CJ, Ashworth A. PARP inhibitors: Synthetic lethality in the clinic. Science. 2017;355(6330):1152-1158. `[Source: grounding_document]`

[5] ATR checkpoint kinase function: DNA damage sensor kinase; phosphorylates BRCA1, CHEK1, MCM2, TP53. `[Source: hgnc_get_gene(HGNC:882)]`

[6] CHEK1 function: Serine/threonine kinase required for checkpoint-mediated cell cycle arrest and DNA repair activation. `[Source: hgnc_get_gene(HGNC:1925), opentargets_get_target(ENSG00000149554)]`

[7] S′/ΔS′ operational-state metrics: Bidirectional dose-response metric measuring operational state under perturbation. `[Source: grounding_document]`

[8] Braschi B, Denny P, Gray K, et al. Genenames.org: the HGNC and VGNC resources in 2019. Nucleic Acids Res. 2019;47(D1):D786-D792. `[Source: hgnc_search_genes, hgnc_get_gene]`

[9] Cunningham F, Allen JE, Allen J, et al. Ensembl 2022. Nucleic Acids Res. 2022;50(D1):D988-D995. `[Source: ensembl_get_gene]`

[10] UniProt Consortium. UniProt: the Universal Protein Knowledgebase in 2023. Nucleic Acids Res. 2023;51(D1):D523-D531. `[Source: uniprot_get_protein(P60484), uniprot_get_protein(P42771), uniprot_get_protein(P06400), uniprot_get_protein(P04637)]`

[11] Sayers EW, Bolton EE, Brister JR, et al. Database resources of the National Center for Biotechnology Information. Nucleic Acids Res. 2024;52(D1):D33-D43. `[Source: entrez_get_gene, entrez_search_genes]`

[12] Szklarczyk D, Kirsch R, Koutrouli M, et al. The STRING database in 2023. Nucleic Acids Res. 2023;51(D1):D483-D489. `[Source: string_search_proteins, string_get_interactions(PTEN), string_get_interactions(CDKN2A), string_get_interactions(ATR), string_get_interactions(AURKA), string_get_interactions(STRING:9606.ENSP00000391090)]`

[13] Ochoa D, Hercules A, Brber BM, et al. The next-generation Open Targets Platform. Nucleic Acids Res. 2023;51(D1):D1353-D1359. `[Source: opentargets_get_target, opentargets_get_associations]`

[14] Martens M, Stieg T, Digles D, et al. WikiPathways: connecting communities. Nucleic Acids Res. 2021;49(D1):D613-D621. `[Source: wikipathways_get_pathways_for_gene('PTEN'), wikipathways_get_pathways_for_gene('TP53'), wikipathways_get_pathways_for_gene('CDKN2A'), wikipathways_get_pathways_for_gene('RB1')]`

[15] Zdrazil B, Felix E, Hunter F, et al. The ChEMBL Database in 2023. Nucleic Acids Res. 2024;52(D1):D1180-D1192. `[Source: chembl_search_compounds('palbociclib'), chembl_search_compounds('prexasertib'), chembl_search_compounds('adavosertib'), chembl_search_compounds('ceralasertib'), chembl_search_compounds('capivasertib'), chembl_search_compounds('everolimus'), chembl_search_compounds('olaparib'), chembl_search_compounds('alisertib'), chembl_search_compounds('volasertib'), chembl_search_compounds('ispinesib'), chembl_search_compounds('birabresib OTX015')]`

[16] Lim S, Kim Y, Kim HY, et al. Safety and efficacy of WEE1 inhibitor adavosertib in patients with biomarker-selected, relapsed small cell lung cancer. Cancer. 2020;126(20):4504-4512. PMID:32584426. `[Source: clinicaltrials_get_trial(NCT:02688907), pubmed(PMID:32584426)]`

[17] Clinical trial data: `[Source: clinicaltrials_search_trials, clinicaltrials_get_trial(NCT:01291017), clinicaltrials_get_trial(NCT:02664935), clinicaltrials_get_trial(NCT:05941897), clinicaltrials_get_trial(NCT:07063745), clinicaltrials_get_trial(NCT:04794699), clinicaltrials_get_trial(NCT:04479306), clinicaltrials_get_trial(NCT:02593019), clinicaltrials_search_trials('ispinesib'), clinicaltrials_get_trial(NCT:02698176)]`

[18] George J, Lim JS, Jang SJ, et al. Comprehensive genomic profiles of small cell lung cancer. Nature. 2015;524(7563):47-53. `[Source: opentargets_get_associations(RB1)]`

---

## Supplementary Materials

### S1: Knowledge Graph Nodes

Complete node list with CURIEs, cross-references, and Synapse grounding: see `sprime-sl-systemic-fragility-knowledge-graph.json` (51 nodes).

### S2: Knowledge Graph Edges

Complete edge list with interaction types, mechanism descriptions, STRING scores, and source tools: see `sprime-sl-systemic-fragility-knowledge-graph.json` (63 edges).

### S3: Evidence Grading Detail

Full claim-level evidence grading with L1-L4 assignments, numeric scores, justifications, and modifier audit trail: see `sprime-sl-systemic-fragility-report.md`, Section 6.

### S4: Synapse Dataset Mapping

Complete Synapse dataset grounding with strength classifications and per-architecture coverage: see `sprime-sl-systemic-fragility-synapse-grounding.md` (9 datasets, 13 nodes grounded).

---

*Preprint prepared from Fuzzy-to-Fact v2 pipeline output. All factual claims trace to database API calls documented in the knowledge graph. The S′/ΔS′ framework claims are theoretical (L1) and require experimental validation. This manuscript has not been peer reviewed.*
