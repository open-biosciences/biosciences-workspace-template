# Synapse Dataset Grounding — CQ #2: S′/ΔS′ Systemic Fragility in Lung Cancer

**Report grounded:** `sprime-sl-systemic-fragility-report.md`
**KG grounded:** `sprime-sl-systemic-fragility-knowledge-graph.json`
**Grounding date:** 2026-03-15
**Synapse MCP:** Connected (search_synapse available)

---

## Coverage Assessment

Synapse.org has **moderate coverage** for lung cancer genomics, with TCGA datasets providing the strongest grounding for tumor suppressor mutation profiles. Direct synthetic lethality datasets in NSCLC are limited; Synapse's strongest SL coverage is in the neurofibromatosis (NF1) domain. The PRMT5/MTAP mechanism has analogous datasets in NF1 glioma. DDR pathway datasets exist in leukemia and ovarian cancer contexts, providing analogous but not direct grounding.

**Queries executed:** 7 compound Synapse searches covering all 4 genotype architectures plus cross-cutting queries (CRISPR screen, NSCLC drug response, TCGA LUAD).

---

## Grounded Datasets

### Dataset 1: TCGA Lung Adenocarcinoma

| Field | Value |
|-------|-------|
| Synapse ID | syn274449 |
| Name | TCGA Lung adenocarcinoma - Consolidated |
| Type | Folder (TCGA project) |
| Grounding Strength | **Strong** |
| Grounded Nodes | MONDO:0008903 (NSCLC), HGNC:9588 (PTEN), HGNC:11998 (TP53), HGNC:1787 (CDKN2A), HGNC:9884 (RB1) |
| Rationale | TCGA LUAD contains somatic mutation, copy number, and expression data for all four primary tumor suppressors in NSCLC. Enables identification of PTEN-loss, CDKN2A-loss, RB1-loss, and TP53-loss patient subgroups for genotype-specific vulnerability analysis. Directly relevant to the ΔS′ genotype classification framework. |
| Source | `[Source: search_synapse("TCGA LUAD mutation PTEN TP53")]` |

### Dataset 2: TCGA Lung Squamous Cell Carcinoma

| Field | Value |
|-------|-------|
| Synapse ID | syn274452 |
| Name | TCGA Lung squamous cell carcinoma - Consolidated |
| Type | Folder (TCGA project) |
| Grounding Strength | **Moderate** |
| Grounded Nodes | MONDO:0008903 (NSCLC), HGNC:9588 (PTEN), HGNC:11998 (TP53), HGNC:1787 (CDKN2A) |
| Rationale | TCGA LUSC profiles the same tumor suppressor genes in the squamous cell subtype of NSCLC. TP53 mutations are near-universal (~80%) in LUSC, making this a comparator dataset for TP53-loss DDR vulnerability. CDKN2A loss is also frequent. Provides complementary genotype context to LUAD. |
| Source | `[Source: search_synapse("CDKN2A MTAP PRMT5 lung cancer")]` |

### Dataset 3: Phenotype Transitions in Small Cell Lung Cancer (CSBC U01)

| Field | Value |
|-------|-------|
| Synapse ID | syn17084070 |
| Name | Phenotype Transitions in Small Cell Lung Cancer - CA215845 |
| Type | Project (CSBC/PS-ON) |
| Grounding Strength | **Moderate** |
| Grounded Nodes | HGNC:9884 (RB1), HGNC:11998 (TP53), HGNC:11393 (AURKA) |
| Rationale | SCLC is characterized by near-universal RB1+TP53 co-loss. This systems biology project models SCLC phenotypic drug response dynamics — directly relevant to the RB1-loss and TP53-loss vulnerability architectures. The project's focus on treatment-induced phenotype transitions and mathematical modeling of drug response parallels the S′ operational-state concept. AURKA is a key therapeutic target in SCLC/RB1-loss contexts. |
| Source | `[Source: search_synapse("CDKN2A MTAP PRMT5 lung cancer")]` |

### Dataset 4: Spatial Single-Cell Analysis of LUAD Precancer Interception

| Field | Value |
|-------|-------|
| Synapse ID | syn54951674 |
| Name | Spatial-Single-Cell-Analysis-LUAD-Interception |
| Type | Project |
| Grounding Strength | **Moderate** |
| Grounded Nodes | MONDO:0008903 (NSCLC) |
| Rationale | Spatial immune profiling of 114 human LUAD and LUAD precursors with scRNA-seq across 5 mouse models. Published in Cancer Cell. Provides tumor microenvironment context for understanding how genotype-dependent vulnerabilities interact with immune checkpoint biology — relevant to the combination strategies suggested for ceralasertib + durvalumab (NCT05941897) and BMS-986504 + pembrolizumab (NCT07063745). |
| Source | `[Source: search_synapse("NSCLC drug response genomics")]` |

### Dataset 5: Targeting PRMT5 in MTAP-deleted NF1 High Grade Gliomas

| Field | Value |
|-------|-------|
| Synapse ID | syn64500888 |
| Name | Targeting PRMT5 in MTAP-deleted NF1 High Grade Gliomas |
| Type | Project (CTF Drug Discovery Initiative) |
| Grounding Strength | **Analogous** |
| Grounded Nodes | HGNC:10894 (PRMT5), HGNC:1787 (CDKN2A — via 9p21 MTAP co-deletion) |
| Rationale | Tests the same PRMT5/MTAP co-deletion vulnerability mechanism (9p21 deletion → CDKN2A + MTAP loss → PRMT5 dependency) in NF1 glioma rather than lung cancer. Demonstrates that PRMT5 inhibitors show selective efficacy in MTAP-deleted vs MTAP-intact cells, and synergize with MEK inhibitors. Same mechanistic claim as CQ #2 CDKN2A-loss metabolic arm, different disease context. |
| Source | `[Source: search_synapse("CDKN2A MTAP PRMT5 lung cancer")]` |

### Dataset 6: Investigating Synthetic Lethality Associated with NF1 Loss

| Field | Value |
|-------|-------|
| Synapse ID | syn21641955 |
| Name | Investigating Synthetic Lethality Associated with NF1 Loss |
| Type | Project |
| Grounding Strength | **Analogous** |
| Grounded Nodes | FRAMEWORK:Sprime, FRAMEWORK:DeltaSprime (conceptual — SL screening methodology) |
| Rationale | Uses CRISPR/Cas9 screens to identify synthetic lethal partners of NF1 loss — same screening methodology that would be required to experimentally validate S′/ΔS′ predictions from this report. While NF1 is not one of the four genotype architectures studied here, the SL-by-CRISPR-screen approach is directly applicable. Provides a methodological template for future validation. |
| Source | `[Source: search_synapse("PTEN lung cancer synthetic lethality")]` |

### Dataset 7: Anti-PD1 Response DREAM Challenge (mitten_IO21)

| Field | Value |
|-------|-------|
| Synapse ID | syn23668287 |
| Name | Anti-PD1 Response DREAM Challenge mitten_IO21 |
| Type | Project |
| Grounding Strength | **Weak** |
| Grounded Nodes | HGNC:11393 (AURKA), HGNC:9077 (PLK1), HGNC:1925 (CHEK1) |
| Rationale | TMB imputation model uses AURKA, PLK1, CHEK1, CHEK2, CDK1, E2F1, FOXM1, KIF20A, KIF23 as cell-cycle gene predictors of mutation burden in TCGA-LUAD (503 patients). Confirms that these genes are relevant biomarkers in lung cancer genomics. Contextual support for the mitotic/DDR gene network in NSCLC, but does not directly test synthetic lethality. |
| Source | `[Source: search_synapse("TP53 ATR CHEK1 WEE1 DNA damage response")]` |

### Dataset 8: DDR in MLL-ENL Leukemia Transformation (GSE35038)

| Field | Value |
|-------|-------|
| Synapse ID | syn359637 |
| Name | GSE35038 |
| Type | Folder (GEO dataset) |
| Grounding Strength | **Analogous** |
| Grounded Nodes | HGNC:882 (ATR), HGNC:1925 (CHEK1), HGNC:11998 (TP53) |
| Rationale | Studies the ATR/ATM-Chk1/Chk2-p53/p21 checkpoint as a rate-limiting barrier to cellular transformation. Demonstrates that DDR activation (the same ATR→CHEK1→TP53 cascade in our TP53-loss architecture) induces senescence, and its inhibition accelerates transformation. Same molecular pathway, different disease (leukemia vs NSCLC). |
| Source | `[Source: search_synapse("TP53 ATR CHEK1 WEE1 DNA damage response")]` |

### Dataset 9: PTEN/Kras/Trp53 Interaction in Ovarian Cancer (GSE34882)

| Field | Value |
|-------|-------|
| Synapse ID | syn351611 |
| Name | GSE34882 |
| Type | Folder (GEO dataset) |
| Grounding Strength | **Analogous** |
| Grounded Nodes | HGNC:9588 (PTEN), HGNC:11998 (TP53), HGNC:6973 (MDM2) |
| Rationale | Studies PTEN-loss + Kras-mutant + Trp53-status interaction in ovarian cancer. Demonstrates that wild-type TP53 promotes DNA repair and proliferation in PTEN-null cells (rather than suppressing them), and that MDM2 antagonism (nutlin-3a) restores apoptotic TP53 function. Directly relevant to the MDM2↔PTEN bridge interaction (Section 2.2) and the cross-talk between PTEN-loss and TP53-loss architectures. Different disease (ovarian vs NSCLC). |
| Source | `[Source: search_synapse("TCGA LUAD mutation PTEN TP53")]` |

---

## Grounding Summary

| Strength | Count | Datasets |
|----------|-------|----------|
| **Strong** | 1 | syn274449 (TCGA LUAD) |
| **Moderate** | 3 | syn274452 (TCGA LUSC), syn17084070 (SCLC systems biology), syn54951674 (Spatial LUAD) |
| **Analogous** | 4 | syn64500888 (PRMT5/MTAP in glioma), syn21641955 (NF1 SL screen), syn359637 (DDR in leukemia), syn351611 (PTEN/Trp53 in ovarian) |
| **Weak** | 1 | syn23668287 (Anti-PD1 DREAM, uses AURKA/PLK1/CHEK1 in LUAD) |
| **Total** | 9 | |

### Coverage by Genotype Architecture

| Architecture | Strong | Moderate | Analogous | Gap |
|-------------|--------|----------|-----------|-----|
| PTEN-loss (PI3K-AKT-MTOR) | syn274449 | syn274452 | syn351611 | No direct PTEN-SL dataset in NSCLC |
| CDKN2A-loss (CDK4/6 + PRMT5) | syn274449 | syn274452 | syn64500888 | PRMT5 data is in NF1 glioma, not NSCLC |
| RB1-loss (mitotic checkpoint) | syn274449 | syn17084070 | syn21641955 | No direct RB1-SL dataset; SCLC provides closest model |
| TP53-loss (ATR-CHEK1-WEE1) | syn274449 | syn274452 | syn359637, syn351611 | DDR cascade data is in leukemia/ovarian, not NSCLC |

### Key Observation

Synapse coverage for lung cancer synthetic lethality is predominantly **indirect**. The strongest grounding comes from TCGA (mutation/expression profiles) rather than functional SL datasets. The PRMT5/MTAP mechanism has emerging Synapse support via NF1 glioma projects. CRISPR-based SL screening datasets (like syn21641955) provide methodological grounding but are disease-context mismatched. This is consistent with the pipeline's note that lung cancer SL is an active research frontier where many S′/ΔS′ predictions remain experimentally unvalidated.

---

*Synapse grounding performed via `search_synapse` MCP tool. 7 queries executed. 9 datasets classified across 4 grounding strength tiers. All Synapse IDs resolve to valid entities on Synapse.org.*
