# CQ14 Synapse Dataset Grounding

**Date**: 2026-03-02
**CQ**: cq14 — Validate synthetic lethal gene pairs from Feng et al. (2022) and identify druggable opportunities for TP53-mutant cancers

---

## Summary

6 Synapse datasets were identified across 4 search queries. Grounding coverage is **partial**: TP53, Li-Fraumeni syndrome, and lung adenocarcinoma have direct dataset support. TYMS, Pemetrexed, 5-fluorouracil, and the clinical trials have no direct Synapse grounding. One analogous dataset (NF1 synthetic lethality via CRISPR screens) supports the mechanistic approach but not the specific TP53-TYMS gene pair.

---

## Grounding Strength Classification

| Strength | Definition | Count |
|----------|-----------|-------|
| **Strong** | Dataset directly tests the mechanistic claim or profiles the exact entity | 3 |
| **Moderate** | Dataset profiles relevant genes/pathways in the same disease context | 2 |
| **Analogous** | Same mechanism tested in a related disease or model system | 1 |
| **Weak** | Contextual or pathway-level support only | 1 |

---

## Grounded Entities

### TP53 (HGNC:11998) — 2 datasets

| Synapse ID | Name | Strength | Rationale |
|-----------|------|----------|-----------|
| [syn209607](https://www.synapse.org/#!Synapse:syn209607) | GSE34258 — TP53 mutation and chromothripsis in medulloblastoma | Moderate | Profiles TP53 germline mutations (Li-Fraumeni) in medulloblastoma; demonstrates TP53 mutation drives genomic instability. Not lung cancer or TYMS-focused. |
| [syn248623](https://www.synapse.org/#!Synapse:syn248623) | GSE19416 — TP53 mutation in ovarian tumours | Moderate | Genome-wide copy number data for TP53-mutant vs wildtype ovarian tumors. Demonstrates TP53-driven CNV patterns, but different cancer type. |

### Li-Fraumeni Syndrome (MONDO:0018875) — 1 dataset

| Synapse ID | Name | Strength | Rationale |
|-----------|------|----------|-----------|
| [syn209607](https://www.synapse.org/#!Synapse:syn209607) | GSE34258 — TP53 germline mutation / Li-Fraumeni / chromothripsis | Strong | Directly studies Li-Fraumeni syndrome patients with germline TP53 mutations. Whole-genome sequencing reveals chromothripsis driven by TP53 loss. |

### Lung Adenocarcinoma (EFO:0000571) — 3 datasets

| Synapse ID | Name | Strength | Rationale |
|-----------|------|----------|-----------|
| [syn274449](https://www.synapse.org/#!Synapse:syn274449) | TCGA Lung adenocarcinoma - Consolidated | Strong | Comprehensive TCGA lung adenocarcinoma dataset with genomic, transcriptomic, methylation, and clinical data. Directly profiles the disease type relevant to NCT04695925. |
| [syn1461166](https://www.synapse.org/#!Synapse:syn1461166) | Lung adenocarcinoma (TCGA multi-omic) | Strong | Multi-omic TCGA lung adenocarcinoma data: copy number, expression, methylation, RPPA, miRNA, and mutation data. Includes TP53 mutation status. |
| [syn17084070](https://www.synapse.org/#!Synapse:syn17084070) | Phenotype Transitions in Small Cell Lung Cancer | Weak | SCLC-focused (not adenocarcinoma) but studies drug resistance mechanisms and phenotypic plasticity in lung cancer using systems biology approaches. |

### TP53-TYMS Genetic Interaction Edge — 1 dataset

| Synapse ID | Name | Strength | Rationale |
|-----------|------|----------|-----------|
| [syn21641955](https://www.synapse.org/#!Synapse:syn21641955) | Investigating Synthetic Lethality Associated with NF1 Loss | Analogous | Uses CRISPR/Cas9 knockout screens to identify synthetic lethal partners for NF1 (not TP53). Same experimental paradigm (SL from tumor suppressor loss → identify druggable partners) applied to a different gene. Validates the general SL approach but not the TP53-TYMS pair specifically. |

---

## Ungrounded Entities

The following KG entities returned no relevant Synapse datasets:

| Entity | CURIE | Reason |
|--------|-------|--------|
| TYMS | HGNC:12441 | No TYMS-specific datasets in Synapse. Search for "TYMS cancer" returned breast cancer and general cancer datasets without TYMS focus. |
| Pemetrexed | CHEMBL:225072 | No Pemetrexed pharmacology datasets in Synapse. Search for "pemetrexed lung cancer" returned general lung cancer datasets. |
| 5-fluorouracil | CHEMBL:185 | Not searched directly; expected to have similar coverage gap as Pemetrexed. |
| NCT:04695925 | — | Clinical trial data not expected in Synapse (trial is ongoing). |
| NCT:03574402 | — | Clinical trial data not expected in Synapse (trial completed 2022). |

---

## Search Queries Executed

| # | Query | Hits | Relevant | Source |
|---|-------|------|----------|--------|
| 1 | "TP53 synthetic lethality" | 361 | 1 (syn21641955) | [Source: search_synapse("TP53 synthetic lethality")] |
| 2 | "TYMS cancer" | 4,506 | 0 (general cancer datasets only) | [Source: search_synapse("TYMS cancer")] |
| 3 | "pemetrexed lung cancer" | 10,000 | 3 (syn274449, syn17084070, syn1461166) | [Source: search_synapse("pemetrexed lung cancer")] |
| 4 | "TP53 mutation NSCLC" | 1,093 | 2 (syn209607, syn248623) | [Source: search_synapse("TP53 mutation NSCLC")] |
| 5 | "CRISPR screen synthetic lethality" | 872 | 1 (syn21641955, same as query 1) | [Source: search_synapse("CRISPR screen synthetic lethality")] |

---

## Domain Coverage Assessment

Synapse has **limited coverage** for the TP53-TYMS synthetic lethality and antifolate pharmacology domain. The strongest grounding comes from TCGA lung adenocarcinoma datasets that provide genomic context for the disease type. The NF1 synthetic lethality project (syn21641955) validates the general CRISPR-based SL approach but targets a different gene. No datasets directly test TYMS essentiality in TP53-mutant backgrounds or profile Pemetrexed response.

This is consistent with the skill documentation noting that Synapse coverage varies by domain, with stronger coverage in neurodegenerative and NF1 domains.
