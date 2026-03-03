# Synapse Grounding: TP53 Synthetic Lethality Knowledge Graph

**KG Source**: `tp53-synthetic-lethality-kg.json`
**Grounding Date**: 2026-03-03
**Tools Used**: `search_synapse`, `get_entity`, `get_entity_annotations`

---

## Summary

Synapse.org was searched for datasets that could ground the TP53 synthetic lethality knowledge graph entities and edges. TP53/cancer falls in the **Moderate-High** Synapse coverage domain for cancer genomics.

**Search queries executed** (compound queries first per protocol):
1. `"TP53 synthetic lethality cancer"` → 4,759 results [Source: search_synapse("TP53 synthetic lethality cancer")]
2. `"WEE1 inhibitor cancer CRISPR"` → 5,125 results [Source: search_synapse("WEE1 inhibitor cancer CRISPR")]
3. `"TP53 mutation cancer drug sensitivity"` → 6,569 results [Source: search_synapse("TP53 mutation cancer drug sensitivity")]
4. `"DepMap cancer cell line gene essentiality"` → 10,000+ results [Source: search_synapse("DepMap cancer cell line gene essentiality")]
5. `"checkpoint kinase CHEK1 PLK1 cancer"` → 5,015 results [Source: search_synapse("checkpoint kinase CHEK1 PLK1 cancer")]
6. `"adavosertib AZD1775 ovarian cancer"` → 4,676 results [Source: search_synapse("adavosertib AZD1775 ovarian cancer")]

**Coverage Summary**:

| Metric | Count |
|--------|-------|
| Total KG nodes | 24 |
| Nodes with Synapse grounding | 5 |
| Node coverage | 21% (5/24) |
| Total KG edges | 23 |
| MEMBER_OF edges (excluded) | 4 |
| Groundable edges | 19 |
| Edges with Synapse grounding | 3 |
| Edge coverage (adjusted) | 16% (3/19) |

---

## Dataset-to-Graph Mapping Table

| Synapse ID | Name | Nodes Grounded | Edges Grounded | Strength | Type | Source |
|-----------|------|---------------|---------------|----------|------|--------|
| syn2384331 | Broad-DREAM Gene Essentiality Prediction Challenge | WEE1, CHEK1, PLK1 | SL edges (essentiality) | **Moderate** | Project | [Source: get_entity(syn2384331)] |
| syn5889324 | Cancer Cell Line Data Repository (CCLE) | TP53, WEE1, CHEK1, PLK1 | Drug sensitivity edges | **Moderate** | Project | [Source: get_entity(syn5889324)] |
| syn21641955 | Investigating Synthetic Lethality Associated with NF1 Loss | — | SL methodology (NF1) | **Analogous** | Project | [Source: get_entity(syn21641955)] |
| syn274455 | TCGA Ovarian Serous Cystadenocarcinoma | TP53 | Disease association | **Moderate** | Folder | [Source: search_synapse("adavosertib AZD1775 ovarian cancer")] |
| syn53480749 | Cell Signaling and Targeted Drug Therapy of Cancer | — | Pathway context | **Weak** | Project | [Source: get_entity(syn53480749)] |

---

## Node Grounding Coverage

| Node | CURIE | Grounded? | Datasets | Strength |
|------|-------|-----------|----------|----------|
| TP53 | HGNC:11998 | Yes | syn5889324 (CCLE), syn274455 (TCGA-OV) | Moderate |
| WEE1 | HGNC:12761 | Yes | syn2384331 (Essentiality), syn5889324 (CCLE) | Moderate |
| CHEK1 | HGNC:1925 | Yes | syn2384331 (Essentiality), syn5889324 (CCLE) | Moderate |
| PLK1 | HGNC:9077 | Yes | syn2384331 (Essentiality), syn5889324 (CCLE) | Moderate |
| MDM2 | HGNC:6973 | Partial | syn5889324 (CCLE — expression only) | Weak |
| PPM1D | HGNC:9277 | No | — | — |
| POLQ | NCBIGene:10721 | No | — | — |
| ATR | NCBIGene:545 | No | — | — |
| Adavosertib | COMPOUND:ADAVOSERTIB | No | — | — |
| Prexasertib | COMPOUND:PREXASERTIB | No | — | — |
| Rabusertib | COMPOUND:RABUSERTIB | No | — | — |
| Volasertib | COMPOUND:VOLASERTIB | No | — | — |
| BI-2536 | COMPOUND:BI-2536 | No | — | — |
| Idasanutlin | COMPOUND:IDASANUTLIN | No | — | — |
| Navtemadlin | COMPOUND:NAVTEMADLIN | No | — | — |
| TP53 network | WP:WP1742 | No | — | — |
| Cell cycle checkpoints | WP:WP1775 | No | — | — |
| ATM signaling | WP:WP3878 | No | — | — |
| DNA Damage Senescence | WP:WP3565 | No | — | — |
| NCT01164995 | Trial | No | — | — |
| NCT02688907 | Trial | No | — | — |
| NCT02448329 | Trial | No | — | — |
| NCT02808650 | Trial | No | — | — |
| NCT02545283 | Trial | No | — | — |

---

## Edge Grounding Coverage

### Grounded Edges

| Edge | Type | Dataset | Strength | Justification |
|------|------|---------|----------|--------------|
| TP53 → WEE1 (SL) | SYNTHETIC_LETHAL | syn2384331 | Moderate | Gene essentiality prediction challenge profiles essential genes across cancer cell lines; WEE1 essentiality validated but not TP53-specific SL |
| TP53 → CHEK1 (SL) | SYNTHETIC_LETHAL | syn2384331 | Moderate | Same essentiality challenge; CHEK1 essentiality profiled |
| TP53 → PLK1 (SL) | SYNTHETIC_LETHAL | syn2384331 | Moderate | Same essentiality challenge; PLK1 essentiality profiled |

### Ungrounded Edges

| Edge | Type | Notes |
|------|------|-------|
| TP53 → POLQ (SL) | SYNTHETIC_LETHAL | No POLQ-specific datasets found on Synapse |
| TP53 → ATR (SL) | SYNTHETIC_LETHAL | No ATR SL datasets found on Synapse |
| MDM2 → TP53 (regulates) | REGULATES | MDM2-p53 regulation not directly tested in any Synapse dataset |
| PPM1D → TP53 (regulates) | REGULATES | No PPM1D datasets found |
| All INHIBITOR edges (7) | INHIBITOR | No drug-target binding datasets on Synapse for these compounds |
| All TESTED_IN edges (5) | TESTED_IN | Clinical trial data not hosted on Synapse |

### Ontological Edges (Not Groundable)

| Edge | Type | Notes |
|------|------|-------|
| TP53 → WP:WP1742 | MEMBER_OF | Pathway membership — definitional, not groundable |
| TP53 → WP:WP1775 | MEMBER_OF | Pathway membership — definitional, not groundable |
| TP53 → WP:WP3878 | MEMBER_OF | Pathway membership — definitional, not groundable |
| TP53 → WP:WP3565 | MEMBER_OF | Pathway membership — definitional, not groundable |

---

## Evidence Level Upgrades

No evidence level upgrades are warranted. Synapse datasets provide **Moderate** contextual support (essentiality profiling, cell line characterization) but do not directly test TP53-specific synthetic lethality claims. The grounding supports existing L2 evidence levels but does not meet the threshold for L2→L2+ upgrade, which would require a dataset directly testing the TP53-WEE1/CHEK1/PLK1 SL relationship.

---

## Grounding Confidence Matrix

| Claim | Datasets | Strength | Justification |
|-------|----------|----------|--------------|
| WEE1/CHEK1/PLK1 are broadly essential | syn2384331 | Moderate | Essentiality challenge profiles gene dependencies but uses different methodology (shRNA) than CRISPR |
| TP53 is frequently mutated in cancer | syn274455 | Moderate | TCGA-OV includes TP53 mutation profiling in ovarian cancer |
| Cancer cell lines show drug sensitivity | syn5889324 | Moderate | CCLE provides drug response and gene expression across cell lines |
| NF1 synthetic lethality methodology | syn21641955 | Analogous | Same CRISPR SL concept applied to NF1, not TP53 |

---

## Methodology

**Tools**: Synapse MCP tools (`search_synapse`, `get_entity`, `get_entity_annotations`)

**Matching criteria**: Compound queries (gene+disease, gene+mechanism) tried first per protocol. Single-term fallback queries used when compound queries returned no relevant results. Results manually assessed for relevance to KG entities and edges.

**MEMBER_OF exclusion**: 4 MEMBER_OF edges (WikiPathways membership) excluded from edge grounding denominator per protocol. These are ontological definitions, not experimental claims.

---

## Limitations

1. **No direct TP53 SL CRISPR screen data on Synapse**: The Feng et al. (2022) dataset is published in Science Advances and deposited in GEO/SRA, not Synapse. This is the primary data gap.

2. **Essentiality challenge uses shRNA, not CRISPR**: syn2384331 (Broad-DREAM) used RNAi-based essentiality screening, not CRISPR-Cas9. The methodological difference (knockdown vs knockout) limits grounding strength.

3. **No drug-target binding data**: Adavosertib, prexasertib, volasertib, and other compounds have no associated Synapse datasets. Drug mechanism data is better sourced from ChEMBL and Open Targets.

4. **No clinical trial data**: Synapse does not host clinical trial records. NCT IDs are grounded via ClinicalTrials.gov, not Synapse.

5. **Fallback sources** (not queried in this pipeline): GEO (NCBI), cBioPortal, and DepMap (Broad Institute) would provide stronger grounding for cancer genomics and CRISPR essentiality data.

---

*Synapse grounding generated by Fuzzy-to-Fact Publication Pipeline v1.0*
