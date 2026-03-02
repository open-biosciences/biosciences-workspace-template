# Synapse Dataset Grounding: EGFR NSCLC First-Generation TKIs

**Generated:** 2026-03-02
**KG Artifact:** `egfr-nsclc-first-gen-tki-knowledge-graph.json`
**Data Repository:** Synapse.org (connected via MCP)

---

## Grounding Summary

| Metric | Value |
|--------|-------|
| Synapse searches executed | 3 |
| Datasets evaluated | 30 (10 per query) |
| Datasets grounded | 6 |
| Strong grounding | 2 |
| Moderate grounding | 2 |
| Analogous grounding | 1 |
| Weak grounding | 1 |

---

## Grounded Datasets

### 1. GSE31852 — EGFR-Mutated NSCLC Gene Expression

| Field | Value |
|-------|-------|
| **Synapse ID** | syn232866 |
| **Type** | Folder (GEO dataset) |
| **Description** | EGFR-mutated NSCLC gene expression profiling — hallmarks including sensitivity to EGFR inhibitors, low proliferation, and increased MET |
| **Grounding Strength** | **Strong** |
| **KG Nodes Grounded** | HGNC:3236 (EGFR), EFO:0003060 (NSCLC), CHEMBL:939 (gefitinib), CHEMBL:553 (erlotinib) |
| **KG Edges Grounded** | EGFR→NSCLC (associated_with), variant:L858R→CHEMBL:939 (sensitizes_to) |
| **Rationale** | Directly profiles EGFR-mutated NSCLC tumors and EGFR inhibitor sensitivity — the core disease-target-drug axis of this CQ |
| **Source** | [Source: synapse_search("EGFR non-small cell lung cancer")] |

### 2. GSE31625 — NSCLC Cell Line Response to EGFR Inhibition

| Field | Value |
|-------|-------|
| **Synapse ID** | syn235691 |
| **Type** | Folder (GEO dataset) |
| **Description** | Gene expression from cultured NSCLC cell lines used to predict response to EGFR inhibition |
| **Grounding Strength** | **Strong** |
| **KG Nodes Grounded** | HGNC:3236 (EGFR), EFO:0003060 (NSCLC), CHEMBL:939 (gefitinib), CHEMBL:553 (erlotinib) |
| **KG Edges Grounded** | CHEMBL:939→HGNC:3236 (inhibits), CHEMBL:553→HGNC:3236 (inhibits) |
| **Rationale** | Directly tests EGFR inhibitor response in NSCLC cell lines — functional validation of the drug-target relationship |
| **Source** | [Source: synapse_search("EGFR non-small cell lung cancer")] |

### 3. TCGA Lung Adenocarcinoma — Consolidated

| Field | Value |
|-------|-------|
| **Synapse ID** | syn274449 |
| **Type** | Folder (TCGA dataset) |
| **Description** | Comprehensive genomic profiling of lung adenocarcinoma (LUAD), the NSCLC subtype most commonly associated with EGFR mutations |
| **Grounding Strength** | **Moderate** |
| **KG Nodes Grounded** | EFO:0003060 (NSCLC), HGNC:3236 (EGFR), variant:L858R, variant:T790M |
| **KG Edges Grounded** | HGNC:3236→EFO:0003060 (associated_with) |
| **Rationale** | TCGA LUAD contains EGFR mutation profiling (including L858R, T790M) in the primary NSCLC subtype; provides genomic context for the disease-target association |
| **Source** | [Source: synapse_search("gefitinib erlotinib lung cancer")] |

### 4. TCGA Lung Squamous Cell Carcinoma — Consolidated

| Field | Value |
|-------|-------|
| **Synapse ID** | syn274452 |
| **Type** | Folder (TCGA dataset) |
| **Description** | Comprehensive genomic profiling of lung squamous cell carcinoma (LUSC), a major NSCLC subtype |
| **Grounding Strength** | **Moderate** |
| **KG Nodes Grounded** | EFO:0003060 (NSCLC) |
| **KG Edges Grounded** | — |
| **Rationale** | LUSC is an NSCLC subtype; while EGFR mutations are less common in squamous histology, this provides disease-level context for the CQ |
| **Source** | [Source: synapse_search("gefitinib erlotinib lung cancer")] |

### 5. EGFRi67 — EGFR Inhibition Sensitivity Signature

| Field | Value |
|-------|-------|
| **Synapse ID** | syn12035066 |
| **Type** | Project |
| **Description** | 67-gene expression signature predicting sensitivity to EGFR inhibition, derived from cetuximab response in colorectal cancer, applied to bladder cancer |
| **Grounding Strength** | **Analogous** |
| **KG Nodes Grounded** | HGNC:3236 (EGFR) |
| **KG Edges Grounded** | CHEMBL:939→HGNC:3236 (inhibits) — analogous mechanism |
| **Rationale** | Same target (EGFR) and mechanism (EGFR inhibition), but different cancer type (bladder/colorectal rather than NSCLC). Demonstrates cross-cancer applicability of EGFR inhibitor sensitivity prediction |
| **Source** | [Source: synapse_search("EGFR non-small cell lung cancer")] |

### 6. GSE32975 — EGFR Pathway Activation and Cetuximab Resistance

| Field | Value |
|-------|-------|
| **Synapse ID** | syn360898 |
| **Type** | Folder (GEO dataset) |
| **Description** | EGFR pathway activation signatures using CoGAPS algorithm; cetuximab resistance in HNSCC; includes PI3K and MEK inhibition signatures |
| **Grounding Strength** | **Weak** |
| **KG Nodes Grounded** | HGNC:3236 (EGFR), HGNC:8975 (PIK3CA) |
| **KG Edges Grounded** | HGNC:3236→HGNC:8975 (interacts_with) |
| **Rationale** | Studies EGFR downstream signaling (PI3K, MEK) and resistance mechanisms, but in HNSCC context. Provides pathway-level support for the EGFR signaling network described in the KG |
| **Source** | [Source: synapse_search("EGFR T790M resistance")] |

---

## Coverage Gaps

The following KG entities had **no direct Synapse grounding**:

| Entity | CURIE | Reason |
|--------|-------|--------|
| Osimertinib | CHEMBL:3353410 | No osimertinib-specific datasets found on Synapse |
| T790M resistance mechanism | variant:T790M | No T790M-specific resistance datasets; TCGA LUAD has mutation data but T790M-specific studies are primarily in published literature |
| Clinical trials (IPASS, FLAURA, BR.21, Tarceva IIIb) | NCT:* | Clinical trial data not hosted on Synapse; validated via ClinicalTrials.gov |
| STRING interactors (GRB2, SHC1, SOS1, PTPN11) | HGNC:* | No interaction-specific datasets found; validated via STRING database |

**Note:** Synapse has strong coverage for NSCLC genomic profiling (TCGA, GEO) and EGFR inhibitor response studies, but limited coverage for T790M-specific resistance mechanisms and third-generation TKI data. This is expected — T790M resistance data is primarily available through published clinical trials (FLAURA) and PubMed literature rather than Synapse-hosted datasets.

---

## Grounding Methodology

Three compound queries were executed via `synapse_search`:

1. `"EGFR non-small cell lung cancer"` — 10,000+ results; top 10 evaluated
2. `"EGFR T790M resistance"` — 1,254 results; top 10 evaluated (mostly non-EGFR resistance studies)
3. `"gefitinib erlotinib lung cancer"` — 10,000+ results; top 10 evaluated

Entity metadata retrieved via `synapse_get_entity` for the 4 most relevant datasets (syn232866, syn235691, syn274449, syn12035066).

Grounding strength classification follows the publication pipeline schema:
- **Strong**: Dataset directly tests the mechanistic claim
- **Moderate**: Dataset profiles relevant genes/pathways in the same disease
- **Analogous**: Same mechanism tested in a related disease or model system
- **Weak**: Contextual or pathway-level support only
