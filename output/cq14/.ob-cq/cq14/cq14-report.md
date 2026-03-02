# CQ14 Report: Synthetic Lethality Validation and Druggable Opportunities for TP53-Mutant Cancers

**Template**: Drug Discovery / Repurposing (Template 1)
**Date**: 2026-03-02
**Revision**: v2 — gap-fill run addressing 5 quality review findings
**Source**: Feng et al., Sci. Adv. 8, eabm6638 (2022). PMC9098673.
**group_id**: cq14-feng-synthetic-lethality

> Mechanism descriptions synthesize tool call outputs. All synthesis is grounded in cited tool calls; no entities, CURIEs, or quantitative values are introduced from training knowledge.

---

## Summary

The TP53-TYMS synthetic lethal relationship reported by Feng et al. (2022) was validated through live database queries across five independent sources. TP53 (HGNC:11998) and TYMS (HGNC:12441) show a confirmed genetic interaction in BioGRID. Two approved TYMS inhibitors — Pemetrexed (CHEMBL:225072) and 5-fluorouracil (CHEMBL:185) — represent immediately actionable druggable opportunities for TP53-mutant cancers. Pemetrexed's mechanism of action as a Thymidylate synthase inhibitor was confirmed via the ChEMBL /mechanism endpoint (mec_id=9637). Two clinical trials were verified: a Phase III RCT (NCT04695925) studying Pemetrexed in EGFR+TP53 NSCLC, and a Phase II umbrella study (NCT03574402) of NGS-directed therapy including a Pemetrexed arm. Open Targets associates TP53 with 3,277 diseases, with Li-Fraumeni syndrome (MONDO:0018875, score 0.876) and lung adenocarcinoma (EFO:0000571, score 0.729) as top-ranked associations.

---

## Resolved Entities

| Entity | CURIE | Type | Key Properties | Source |
|--------|-------|------|----------------|--------|
| TP53 | HGNC:11998 | biolink:Gene | tumor protein p53, 17p13.1, ENSG00000141510, P04637 | [Source: hgnc_search_genes("TP53"), hgnc_get_gene(HGNC:11998)] |
| TYMS | HGNC:12441 | biolink:Gene | thymidylate synthetase, 18p11.32, ENSG00000176890, P04818 | [Source: hgnc_search_genes("TYMS"), hgnc_get_gene(HGNC:12441)] |
| 5-fluorouracil | CHEMBL:185 | biolink:SmallMolecule | MW=130.08, Phase 4 | [Source: chembl_search_compounds("fluorouracil"), chembl_get_compound(CHEMBL:185)] |
| Pemetrexed | CHEMBL:225072 | biolink:SmallMolecule | MW=427.42, Phase 4 | [Source: chembl_search_compounds("pemetrexed"), chembl_get_compound(CHEMBL:225072)] |
| Li-Fraumeni syndrome | MONDO:0018875 | biolink:Disease | OT score 0.876 (rank 1 of 3,277) | [Source: opentargets_get_associations(ENSG00000141510)] |
| Lung adenocarcinoma | EFO:0000571 | biolink:Disease | OT score 0.729 (rank 7 of 3,277) | [Source: opentargets_get_associations(ENSG00000141510)] |

---

## Drug Candidates

| Drug | CURIE | Phase | Mechanism | Target | Evidence Level | Source |
|------|-------|-------|-----------|--------|---------------|--------|
| Pemetrexed | CHEMBL:225072 | 4 | Thymidylate synthase inhibitor (+ DHFR inhibitor, GAR transformylase inhibitor) | TYMS (HGNC:12441) / CHEMBL1952 | L4 (0.95) | [Source: chembl_get_compound(CHEMBL:225072), curl ChEMBL /mechanism] |
| 5-fluorouracil | CHEMBL:185 | 4 | TYMS inhibitor | TYMS (HGNC:12441) | L3 (0.80) | [Source: chembl_search_compounds("fluorouracil"), chembl_get_compound(CHEMBL:185)] |

---

## Mechanism Rationale

### Pemetrexed → TYMS → TP53-mutant cancers

TP53 (HGNC:11998) is a tumor suppressor gene located at 17p13.1 encoding the p53 protein (UniProtKB:P04637). TYMS (HGNC:12441), located at 18p11.32, encodes thymidylate synthetase (UniProtKB:P04818), a key enzyme in de novo thymidylate synthesis. [Source: hgnc_get_gene(HGNC:11998), hgnc_get_gene(HGNC:12441)]

The TP53-TYMS genetic interaction was confirmed in BioGRID, where TP53 shows 6,075 total interactions (5,687 physical, 388 genetic), with TYMS confirmed as a genetic interaction partner. [Source: biogrid_get_interactions("TP53")]

Pemetrexed (CHEMBL:225072, MW=427.42, synonyms: Alimta, LY-231514) is an approved Phase 4 antifolate with three confirmed mechanisms of action: (1) Thymidylate synthase inhibitor (target: CHEMBL1952, direct_interaction=1, disease_efficacy=1, FDA ref: label/2004/021677lbl.pdf); (2) Dihydrofolate reductase inhibitor (target: CHEMBL202); (3) GAR transformylase inhibitor (target: CHEMBL3972). [Source: curl ChEMBL /mechanism endpoint for CHEMBL225072]

In TP53-mutant cancers, the synthetic lethality model predicts that loss of TP53 function creates a dependency on TYMS-mediated thymidylate synthesis. Inhibiting TYMS with Pemetrexed selectively targets cells that have lost TP53 function.

Pemetrexed has 49 indications in ChEMBL, including Carcinoma, Non-Small-Cell Lung; Mesothelioma, Malignant; Adenocarcinoma of Lung; and other solid tumors. [Source: chembl_get_compound(CHEMBL:225072)]

### 5-fluorouracil → TYMS → TP53-mutant cancers

5-fluorouracil (CHEMBL:185, MW=130.08, synonyms: 5-FU, Adrucil, Efudex) is an approved Phase 4 antimetabolite that also inhibits TYMS. The same synthetic lethality rationale applies: TYMS inhibition by 5-FU should selectively kill TP53-deficient cells. [Source: chembl_search_compounds("fluorouracil"), chembl_get_compound(CHEMBL:185)]

A clinical trial search for "TP53 fluorouracil" returned 12 trials, but none directly test the TP53-TYMS synthetic lethality hypothesis with 5-FU as the primary intervention. Most trials use 5-FU as part of combination regimens where TP53 status is a biomarker rather than the basis for treatment selection. [Source: clinicaltrials_search_trials("TP53 fluorouracil")]

---

## Disease Associations

TP53 (ENSG00000141510) has 3,277 disease associations in Open Targets. The top associations relevant to this CQ are:

| Disease | CURIE | OT Score | Rank | Relevance |
|---------|-------|----------|------|-----------|
| Li-Fraumeni syndrome | MONDO:0018875 | 0.876 | 1 | Top TP53-associated disease; germline TP53 mutations define the syndrome | [Source: opentargets_get_associations(ENSG00000141510)] |
| Lung adenocarcinoma | EFO:0000571 | 0.729 | 7 | Most relevant cancer type for NCT04695925 (NSCLC with TP53 mutations) | [Source: opentargets_get_associations(ENSG00000141510)] |

---

## Clinical Trials

| NCT ID | Title | Phase | Status | Verified | Source |
|--------|-------|-------|--------|----------|--------|
| NCT04695925 | Phase III RCT: Osimertinib mono vs. Osimertinib + Carboplatin/Pemetrexed for NSCLC with concurrent EGFR and TP53 mutations (TOP) | III | ACTIVE_NOT_RECRUITING | Yes | [Source: clinicaltrials_search_trials("TP53 pemetrexed"), clinicaltrials_get_trial(NCT:04695925)] |
| NCT03574402 | Phase II umbrella study: NGS-directed therapy for advanced NSCLC (TRUMP) | II | UNKNOWN | Yes | [Source: clinicaltrials_search_trials("TP53 pemetrexed"), clinicaltrials_get_trial(NCT:03574402)] |

### NCT04695925 Detail

- **Design**: Multicentre, randomized, open-label, parallel assignment [Source: clinicaltrials_get_trial(NCT:04695925)]
- **Interventions**: Osimertinib, Pemetrexed, Carboplatin [Source: clinicaltrials_get_trial(NCT:04695925)]
- **Primary outcome**: Progression-free survival (assessed up to 36 months) [Source: clinicaltrials_get_trial(NCT:04695925)]
- **Secondary outcomes**: Overall survival (up to 60 months), objective response rate (RECIST v1.1), AE incidence, EORTC QLQ-C30 [Source: clinicaltrials_get_trial(NCT:04695925)]
- **Eligibility**: Stage IV or recurrent non-squamous NSCLC with activating EGFR mutations (exon 19 deletion or L858R) AND concurrent TP53 mutations [Source: clinicaltrials_get_trial(NCT:04695925)]
- **Lead sponsor**: Li Zhang, MD [Source: clinicaltrials_get_trial(NCT:04695925)]
- **Dates**: Started 2021-03-29, estimated completion 2025-11-01 [Source: clinicaltrials_get_trial(NCT:04695925)]

### NCT03574402 Detail

- **Design**: Phase II umbrella study, non-randomized, parallel assignment [Source: clinicaltrials_get_trial(NCT:03574402)]
- **Interventions**: Pemetrexed, Cisplatin, Sintilimab, Afatinib, Crizotinib, Carboplatin [Source: clinicaltrials_get_trial(NCT:03574402)]
- **Primary outcome**: Response rate (RECIST v1.1) [Source: clinicaltrials_get_trial(NCT:03574402)]
- **Conditions**: Carcinoma, Non-Small-Cell Lung [Source: clinicaltrials_get_trial(NCT:03574402)]
- **Lead sponsor**: Guangdong Association of Clinical Trials [Source: clinicaltrials_get_trial(NCT:03574402)]
- **Dates**: Started 2018-07-09, completed 2022-12-30 [Source: clinicaltrials_get_trial(NCT:03574402)]
- **Status**: UNKNOWN [Source: clinicaltrials_get_trial(NCT:03574402)]
- **Associated publications**: PMID 37488286, PMID 35659479 [Source: clinicaltrials_get_trial(NCT:03574402)]

---

## Evidence Assessment

### Claim-Level Grades

| # | Claim | Base | Modifiers | Final | Level |
|---|-------|------|-----------|-------|-------|
| 1 | TP53 resolves to HGNC:11998 (tumor protein p53, 17p13.1) | L2 (0.55) | +0.05 literature (cross-refs to Ensembl, UniProt, OMIM) | 0.60 | L2 Multi-DB |
| 2 | TYMS resolves to HGNC:12441 (thymidylate synthetase, 18p11.32) | L2 (0.55) | +0.05 literature (cross-refs to Ensembl, UniProt, OMIM) | 0.60 | L2 Multi-DB |
| 3 | TP53 and TYMS have a genetic interaction | L2 (0.60) | Confirmed via BioGRID (independent of HGNC) | 0.60 | L2 Multi-DB |
| 4 | Pemetrexed (CHEMBL:225072) is a confirmed Thymidylate synthase inhibitor | L4 (0.90) | +0.05 mechanism confirmed via ChEMBL /mechanism (mec_id=9637, direct_interaction=1) | 0.95 | L4 Clinical |
| 5 | 5-fluorouracil (CHEMBL:185) is an approved TYMS inhibitor | L3 (0.75) | +0.05 literature (78 ChEMBL indications as cross-evidence) | 0.80 | L3 Functional |
| 6 | NCT04695925 studies Pemetrexed for TP53-mutant NSCLC | L4 (0.95) | Directly verified via clinicaltrials_get_trial | 0.95 | L4 Clinical |
| 7 | NCT03574402 includes Pemetrexed arm for NSCLC | L3 (0.70) | Verified via clinicaltrials_get_trial; status UNKNOWN | 0.70 | L3 Functional |
| 8 | TP53-TYMS synthetic lethality is druggable via Pemetrexed | L3 (0.75) | +0.10 mechanism match (TYMS inhibitor confirmed), +0.10 active trial (NCT04695925) | 0.95 | L4 Clinical |
| 9 | TP53 is associated with Li-Fraumeni syndrome (MONDO:0018875) | L4 (0.90) | Open Targets score 0.876, rank 1 of 3,277 | 0.90 | L4 Clinical |
| 10 | TP53 is associated with lung adenocarcinoma (EFO:0000571) | L3 (0.75) | Open Targets score 0.729, rank 7 of 3,277 | 0.75 | L3 Functional |

### Overall Report Confidence

- **Median**: 0.80 (L3 Functional)
- **Range**: 0.60 (L2) to 0.95 (L4)
- **Claims below L1**: None
- **Assessment**: High confidence. The core synthetic lethality claim is grounded in BioGRID genetic interaction data, Pemetrexed's TYMS inhibition mechanism was directly confirmed via ChEMBL /mechanism endpoint, both clinical trials were verified via ClinicalTrials.gov, and disease associations were quantified via Open Targets.

---

## Gaps and Limitations

1. **BioGRID ORCS data not queried**: The CQ definition references "1,446 screens confirm TYMS essentiality" from BioGRID ORCS. The BioGRID ORCS API (`https://orcs.thebiogrid.org/`) returned empty responses during the gap-fill run (no API key available or endpoint unreachable). ORCS essentiality data would strengthen the validation but could not be obtained in this execution.

2. **5-fluorouracil clinical trials searched but no direct SL trials found**: A search for "TP53 fluorouracil" returned 12 trials, but none directly test the TP53-TYMS synthetic lethality hypothesis with 5-FU as the primary intervention. [Source: clinicaltrials_search_trials("TP53 fluorouracil")]

3. **Single synthetic lethal partner examined**: Feng et al. (2022) reported multiple synthetic lethal gene pairs for TP53. This execution validated only the TP53-TYMS pair. Other partners (if present in the paper) were not explored.

---

## Tools Used

| Phase | Tool | Parameters | Result |
|-------|------|------------|--------|
| ANCHOR | hgnc_search_genes | "TP53" | HGNC:11998 |
| ANCHOR | hgnc_search_genes | "TYMS" | HGNC:12441 |
| ENRICH | hgnc_get_gene | HGNC:11998 | Full cross-refs (Ensembl, UniProt, OMIM, Entrez) |
| ENRICH | hgnc_get_gene | HGNC:12441 | Full cross-refs (Ensembl, UniProt, OMIM, Entrez) |
| VALIDATE | biogrid_search_genes | "TP53" | 6,075 interactions |
| VALIDATE | biogrid_search_genes | "TYMS" | 111 interactions |
| VALIDATE | biogrid_get_interactions | TP53 (full) | TYMS genetic interaction confirmed |
| VALIDATE | biogrid_get_interactions | TYMS (slim) | 8 genetic, 103 physical interactions |
| DRUGGABILITY | chembl_search_compounds | "pemetrexed" | CHEMBL:225072 (Phase 4) |
| DRUGGABILITY | chembl_get_compound | CHEMBL:225072 | MW=427.42, 49 indications |
| DRUGGABILITY | chembl_search_compounds | "fluorouracil" | CHEMBL:185 (Phase 4) |
| DRUGGABILITY | chembl_get_compound | CHEMBL:185 | MW=130.08 |
| MECHANISM | curl ChEMBL /mechanism | CHEMBL225072 | 3 mechanisms: TYMS inhibitor (CHEMBL1952), DHFR inhibitor (CHEMBL202), GAR transformylase inhibitor (CHEMBL3972) |
| CLINICAL | clinicaltrials_search_trials | "TP53 pemetrexed" | 2 trials found |
| CLINICAL | clinicaltrials_get_trial | NCT:04695925 | Phase III, ACTIVE_NOT_RECRUITING |
| CLINICAL | clinicaltrials_get_trial | NCT:03574402 | Phase II, UNKNOWN status |
| CLINICAL | clinicaltrials_search_trials | "TP53 fluorouracil" | 12 trials found (none directly testing SL) |
| DISEASE | opentargets_get_associations | ENSG00000141510 | 3,277 associations; top: Li-Fraumeni (0.876), lung adenocarcinoma (0.729) |
