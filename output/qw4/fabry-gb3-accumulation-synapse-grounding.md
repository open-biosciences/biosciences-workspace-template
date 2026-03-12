# Synapse Dataset Grounding: Fabry Disease Gb3 Accumulation

**CQ:** Identify the substrate that accumulates in Fabry disease and explain why
**Slug:** fabry-gb3-accumulation
**Date:** 2026-03-06

---

## 1. Summary

**Domain Coverage Assessment:** Lysosomal storage diseases have **Very Low** coverage on Synapse.org. The platform suitability matrix identifies this domain as underrepresented, with recommended fallback sources including GEO (NCBI), disease-specific registries (Fabry Outcome Survey), and ArrayExpress/BioStudies (EBI).

**Direct Fabry disease data:** Only 1 project found on Synapse (syn52229845, "Fabry Disease"), containing 2 image files and 1 link — no omics or experimental datasets.

**Cross-disease grounding strategy:** Searches were broadened to related lysosomal storage disorders sharing the M6P receptor-mediated ERT delivery mechanism or the sphingolipid degradation pathway.

### Coverage Statistics

| Metric | Count | Notes |
|--------|-------|-------|
| Synapse datasets identified | 5 | All cross-disease (Analogous or Weak) |
| KG nodes grounded | 3 / 17 (18%) | HEXB, Agalsidase beta, WP4153 pathway |
| KG edges grounded (total) | 2 / 10 (20%) | HYDROLYZES (drug→substrate), CO_PATHWAY (HEXB→GLA) |
| KG edges grounded (adjusted*) | 2 / 10 (20%) | No MEMBER_OF edges in this graph — no adjustment needed |
| Grounding strength | 0 Strong, 0 Moderate, 4 Analogous, 1 Weak | No disease-matched datasets available |

*Adjusted denominator excludes MEMBER_OF edges (ontological pathway membership). This KG has 0 MEMBER_OF edges, so raw and adjusted are identical.

---

## 2. Dataset-to-Graph Mapping Table

| Synapse ID | Title | Nodes Grounded | Edges Grounded | Strength | Species | Data Type |
|-----------|-------|----------------|----------------|----------|---------|-----------|
| syn58605953 | Anding et al. 2023 — Pompe ERT | Agalsidase beta (CHEMBL:2108888), Agalsidase alfa (CHEMBL:2108214) | HYDROLYZES (drug→Gb3) | **Analogous** | Human | RNA-seq (FASTQ) |
| syn360896 | GSE38680 — Pompe muscle transcriptomics | Agalsidase beta (CHEMBL:2108888) | — | **Analogous** | Human | Microarray |
| syn351270 | GSE19641 — Sandhoff HEXB-/- thymus | HEXB | CO_PATHWAY (HEXB→GLA) | **Analogous** | Mouse | Microarray |
| syn353567 | GSE18745 — S1P lyase sphingolipid metabolism | WP4153 pathway | — | **Weak** | Mouse | Microarray |
| syn256935 | GSE27280 — Pompe ERT skeletal muscle | — | — | **Analogous** | Human | Microarray |

---

## 3. Node Grounding Coverage

### Grounded Nodes (3/17)

| Node | Synapse Datasets | Strength | Justification |
|------|-----------------|----------|---------------|
| HEXB | syn351270 | Analogous | Sandhoff disease is caused by HEXB deficiency. This dataset profiles HEXB-/- tissue in a co-pathway lysosomal storage disorder, providing experimental data on the consequences of HEXB loss of function. HEXB is a STRING interaction partner of GLA (score 0.941) in the sphingolipid degradation pathway (WP4153). |
| Agalsidase beta (CHEMBL:2108888) | syn58605953, syn360896 | Analogous | Pompe disease ERT (acid alpha-glucosidase) shares the same delivery mechanism as Fabry ERT (alpha-galactosidase A): recombinant enzyme uptake via mannose-6-phosphate receptor into lysosomes. The Pompe ERT datasets provide transcriptomic data on tissue response to lysosomal enzyme replacement. |
| WP4153 (Sphingolipid degradation) | syn353567 | Weak | S1P lyase operates in the broader sphingolipid metabolism network. This dataset provides contextual pathway-level support but does not directly test Gb3 catabolism. |

### Ungrounded Nodes (14/17)

| Node | Reason for No Grounding |
|------|------------------------|
| GLA (HGNC:4296) | No Fabry-specific omics datasets on Synapse. GLA is a short gene symbol generating high false-positive rates in Synapse search. |
| Alpha-galactosidase A (P06280) | Same as GLA — no protein-level experimental data found. |
| Gb3 (PubChem:CID66616222) | Metabolite-level datasets not found on Synapse. Fabry lipid profiling data may exist in GEO or Metabolomics Workbench. |
| Fabry disease (MONDO:0010526) | syn52229845 project exists but contains no experimental data (images and links only). |
| A4GALT | Biosynthetic enzyme — no datasets profiling Gb3 synthesis specifically. |
| HEXA | No Tay-Sachs datasets found on Synapse. |
| ARSA | No metachromatic leukodystrophy datasets found on Synapse. |
| Agalsidase alfa (CHEMBL:2108214) | Grounded indirectly via Pompe ERT analogy (syn58605953) — same mechanism class as agalsidase beta. |
| Migalastat (CHEMBL:110458) | No chaperone therapy datasets on Synapse. |
| Pegunigalsidase alfa (CHEMBL:4297801) | No PEGylated ERT datasets on Synapse. |
| Lucerastat (CHEMBL:1086997) | No substrate reduction therapy datasets on Synapse. |
| Voxeralgagene autotemcel (CHEMBL:5095471) | No Fabry gene therapy datasets on Synapse. |
| Duvalgagene otiparvovec (CHEMBL:6068111) | No Fabry gene therapy datasets on Synapse. |
| WP5292 (Glycosphingolipid metabolism) | No datasets mapping to this specific biosynthetic pathway. |
| WP2788 (Sphingolipid metabolism) | Broad pathway — syn353567 provides partial overlap but insufficient specificity. |

---

## 4. Edge Grounding Coverage

### Grounded Edges (2/10)

| Edge | Type | Synapse Dataset | Strength | Evidence |
|------|------|----------------|----------|---------|
| CHEMBL:2108888 → PubChem:CID66616222 | HYDROLYZES | syn58605953 | Analogous | Pompe ERT study demonstrates recombinant lysosomal enzyme → substrate clearance paradigm. Same M6P receptor-mediated uptake mechanism. |
| STRING:HEXB → HGNC:4296 | CO_PATHWAY | syn351270 | Analogous | Sandhoff HEXB-/- dataset experimentally validates HEXB function in sphingolipid degradation, supporting the co-pathway relationship with GLA. |

### Ungrounded Edges (8/10)

| Edge | Type | Notes |
|------|------|-------|
| HGNC:4296 → PubChem:CID66616222 | HYDROLYZES | Core enzymatic reaction — no Fabry enzyme assay datasets on Synapse |
| STRING:A4GALT → PubChem:CID66616222 | SYNTHESIZES | Gb3 biosynthesis — no Gb3 synthase datasets on Synapse |
| HGNC:4296 → MONDO:0010526 | CAUSES | Gene-disease link — strongly validated by Open Targets (0.893) but no Synapse dataset |
| MONDO:0010526 → PubChem:CID66616222 | CHARACTERIZED_BY | Disease-substrate accumulation — no Fabry lipid profiling on Synapse |
| CHEMBL:110458 → HGNC:4296 | STABILIZES | Chaperone mechanism — no migalastat datasets on Synapse |
| CHEMBL:1086997 → UGCG | INHIBITS | SRT mechanism — no lucerastat datasets on Synapse |
| STRING:ARSA → HGNC:4296 | CO_PATHWAY | No MLD datasets on Synapse for ARSA grounding |
| STRING:HEXA → HGNC:4296 | CO_PATHWAY | No Tay-Sachs datasets on Synapse for HEXA grounding |

---

## 5. Evidence Level Upgrades

No evidence level upgrades warranted. All Synapse grounding is **Analogous** (cross-disease) or **Weak** (pathway-level context), which provides supporting evidence but does not meet the threshold for upgrading L1→L1+ or L2→L2+. An upgrade would require at minimum **Moderate** strength grounding (same disease, relevant assay).

---

## 6. Grounding Confidence Matrix

| Claim | Datasets | Strength | Confidence | Justification |
|-------|----------|----------|------------|---------------|
| ERT hydrolyzes accumulated Gb3 | syn58605953, syn360896, syn256935 | Analogous | Medium | Pompe ERT validates the shared delivery mechanism (M6P receptor → lysosomal targeting → substrate clearance). Different enzyme/substrate but identical therapeutic paradigm. |
| HEXB is a co-pathway enzyme | syn351270 | Analogous | Medium | Direct experimental profiling of HEXB-/- tissue confirms functional role in co-pathway sphingolipid degradation. Different disease (Sandhoff) but shared pathway (WP4153). |
| Sphingolipid degradation pathway context | syn353567 | Weak | Low | S1P lyase dataset provides general sphingolipid pathway context but does not address Gb3 catabolism specifically. |
| GLA mutations cause Gb3 accumulation | — | Ungrounded | N/A | Core mechanistic claim validated by multi-database evidence (OT, OMIM, UniProt, NCBI Gene) but no experimental dataset on Synapse directly tests this. |

---

## 7. Methodology

### Tools Used

| Tool | Queries Executed | Results |
|------|-----------------|---------|
| `search_synapse` | "Fabry disease alpha-galactosidase enzyme replacement therapy" | Mostly Pompe/NF1 results; syn52229845 (empty Fabry project) |
| `search_synapse` | "GLA alpha-galactosidase globotriaosylceramide" | Unrelated results |
| `search_synapse` | "lysosomal storage disease enzyme replacement therapy" | syn58605953, syn360896, syn256935 |
| `search_synapse` | "sphingolipid lysosomal hydrolase" | syn351270, syn353567 |
| `get_entity` | syn52229845, syn58605953, syn360896, syn351270, syn353567, syn256935 | Entity metadata retrieved |
| `get_entity_children` | syn58605953 | FASTQ folder (syn58606024), Design table (syn58831436) |
| `get_entity_annotations` | syn52229845 | Empty annotations |

### Matching Criteria

- **Compound queries first:** "gene+disease" and "drug+disease" combinations tried before single-term fallbacks
- **Token budget discipline:** All `search_synapse` calls used `limit=5`
- **MEMBER_OF exclusion:** Not applicable — this KG contains 0 MEMBER_OF edges
- **Cross-disease grounding:** Classified as "Analogous" when the biological mechanism is shared (M6P-mediated ERT, sphingolipid degradation) but the specific gene/substrate/disease differs

---

## 8. Limitations

1. **No Fabry-specific experimental data on Synapse:** The only Fabry-tagged project (syn52229845) contains no omics data. All grounding relies on cross-disease analogy.

2. **All grounding is Analogous or Weak:** No Strong or Moderate grounding was achievable. This is expected for lysosomal storage diseases (Very Low Synapse coverage).

3. **Cross-species considerations:** syn351270 (Sandhoff) and syn353567 (S1P lyase) are mouse studies. Cross-species extrapolation adds uncertainty to grounding confidence.

4. **Platform age:** Several datasets (GSE19641: 2009, GSE18745: 2009, GSE27280: 2011, GSE38680: 2012) use older microarray technology. Modern RNA-seq datasets would provide higher-resolution grounding.

5. **Recommended fallback sources for stronger grounding:**
   - **GEO (NCBI):** Search "Fabry disease" for tissue-specific transcriptomics
   - **Fabry Outcome Survey:** Clinical registry data for phenotype-genotype correlations
   - **ArrayExpress/BioStudies (EBI):** European Fabry disease datasets
   - **Metabolomics Workbench:** Gb3 and lyso-Gb3 lipid profiling datasets
   - **ClinVar:** GLA variant-level pathogenicity data

---

*Synapse grounding generated by biosciences-publication-pipeline Stage 1b · 5 datasets from Synapse.org · Very Low domain coverage documented · 2026-03-06*
