# Synapse Integration in the Fuzzy-to-Fact Pipeline: Value Assessment and Improvement Opportunities

**Date**: 2026-03-03
**Context**: Analysis based on CQ14 (TP53 synthetic lethality) pipeline execution

---

## 1. What Synapse Does Today

Synapse.org serves as a **post-hoc grounding layer** in the Fuzzy-to-Fact publication pipeline. It is invoked during Stage 1b of the 4-stage publication workflow — after the knowledge graph has already been constructed (Phases 1–6a) and the report formatted (Stage 1a). Its role is to search for existing datasets on Synapse that could independently corroborate the claims, entities, and edges already present in the KG.

### Current Tool Surface

Four MCP tools are available:

| Tool | Purpose | Used in CQ14? |
|------|---------|---------------|
| `search_synapse` | Keyword search across all entity types | Yes (6 queries) |
| `get_entity` | Full metadata for a specific syn ID | Yes (4 entities) |
| `get_entity_annotations` | Structured annotations (species, platform) | Mentioned but lightly used |
| `get_entity_children` | Explore folders/projects for sub-datasets | Not used |

### What Happened in CQ14

Six compound queries were constructed from KG entity labels. Five Synapse datasets were mapped to the KG:

- **syn2384331** (Broad-DREAM Gene Essentiality) → Moderate grounding for WEE1/CHEK1/PLK1 essentiality claims
- **syn5889324** (CCLE) → Moderate grounding for TP53 and checkpoint gene expression/drug sensitivity
- **syn21641955** (NF1 SL Investigation) → Analogous methodology grounding only
- **syn274455** (TCGA Ovarian) → Moderate grounding for TP53 disease association
- **syn53480749** (Cell Signaling) → Weak pathway context

**Final coverage**: 21% of nodes (5/24), 16% of groundable edges (3/19). No evidence level upgrades were warranted.

---

## 2. The Value Synapse Provides

### 2.1 Independent Corroboration (Primary Value)

Synapse's main contribution is providing a check against an independent data repository. The entire Fuzzy-to-Fact pipeline draws from curated databases (HGNC, STRING, Open Targets, BioGRID ORCS, ClinicalTrials.gov). These databases are authoritative but they represent processed, annotated knowledge. Synapse holds raw experimental datasets — the kind of data that *generated* the annotations in those databases. When a Synapse dataset independently confirms a KG claim, it connects the pipeline's derived knowledge back to primary experimental evidence.

In the CQ14 case, the DREAM Essentiality Challenge (syn2384331) provides an independent experimental validation path for the WEE1/CHEK1/PLK1 essentiality claims. The pipeline already confirmed these via BioGRID ORCS (which aggregates CRISPR screen results), but the DREAM dataset used RNAi-based screening — a methodologically distinct approach. This kind of orthogonal confirmation is genuinely valuable.

### 2.2 Reproducibility Signal

Synapse's structured metadata (annotations, provenance tracking, versioned files) means that grounded datasets have a clear path to reproducibility. A reader of the BioRxiv draft can follow syn2384331 to the actual data files, download them, and verify the essentiality claims independently. This is not possible with most database API responses, which are snapshots of processed data.

### 2.3 Domain Coverage Awareness

The pipeline's platform suitability matrix (neurodegenerative = High, cancer = Moderate-High, cardiovascular = Low) is itself a useful feature. It sets expectations upfront about what Synapse can and cannot provide for a given CQ domain, preventing false conclusions from low coverage.

---

## 3. Where the Integration Falls Short

### 3.1 It's a Passive Validation Layer, Not an Active Discovery Tool

Today, Synapse is only consulted *after* the KG is built. It never contributes new entities, edges, or hypotheses. The pipeline constructs 24 nodes and 23 edges from 8 other databases, then asks Synapse: "do you have anything relevant?" This makes Synapse a confirmatory afterthought rather than a generative data source.

In the CQ14 run, Synapse contributed zero new information to the knowledge graph. It confirmed things the pipeline already knew — and confirmed them at "Moderate" strength at best. The 21% node coverage and 16% edge coverage numbers reflect this: the integration is too shallow to meaningfully strengthen the pipeline's conclusions.

### 3.2 Search Is Blunt

`search_synapse` does keyword matching across entity metadata. It returns thousands of results per query (4,676 to 10,000+) with no semantic ranking, relevance scoring, or entity-type filtering beyond basic parameters. Two queries returned HTTP 500 errors and had to be worked around. The tool lacks:

- Filtering by data type (genomics, transcriptomics, CRISPR screen, drug response)
- Filtering by species, disease ontology, or assay type at the query level
- Relevance ranking beyond keyword match
- Faceted search (e.g., "CRISPR screens in cancer cell lines containing TP53 mutations")

This means the agent must construct broad keyword queries and then manually assess relevance from result metadata — a process that produces high false-positive rates and misses datasets that don't mention the exact search terms in their names or descriptions.

### 3.3 No Data-Level Access

The current tools retrieve metadata *about* datasets but never access the data *within* them. The agent can learn that syn2384331 is a gene essentiality challenge, but cannot query whether WEE1 is actually scored as essential in that dataset's results. This limits grounding to metadata-level pattern matching ("this dataset is about essentiality, and our KG includes essentiality claims") rather than data-level confirmation ("WEE1 has a CERES score of -0.95 in this dataset, confirming essentiality").

### 3.4 Compound and Trial Nodes Are Structurally Ungroundable

7 compound nodes and 5 trial nodes (12/24 = 50% of the KG) are essentially impossible to ground via Synapse because:

- Drug-target binding assay data is better sourced from ChEMBL and PubChem
- Clinical trial records are the domain of ClinicalTrials.gov, not Synapse
- Synapse rarely hosts pharmacological compound data with drug names in searchable metadata

This means the theoretical maximum node coverage is closer to ~50% even with perfect search, not 100%. The 21% achieved is actually ~42% of the groundable ceiling.

### 3.5 No Feedback Loop to Evidence Grading

The pipeline's evidence grading system (L1–L4) includes modifiers for literature (+0.05), mechanism match (+0.10), and active trials (+0.10) — but there is no Synapse-specific modifier. A Strong Synapse grounding (direct experimental confirmation) adds exactly zero to a claim's evidence score. The grounding document itself noted: "No evidence level upgrades are warranted." This isn't because Synapse data was weak; it's because the grading system has no mechanism to reward Synapse confirmation.

---

## 4. Opportunities for Improvement

### 4.1 Promote Synapse from Post-Hoc Grounding to Phase 3 EXPAND

**Current**: Synapse is only queried during the publication pipeline (Stage 1b), after the KG is complete.

**Proposed**: Add a Synapse query step within Phase 3 EXPAND of the graph-building pipeline itself. After STRING PPI expansion, query Synapse for datasets matching the anchor gene + disease context. If a high-quality dataset is found (e.g., a CRISPR screen in TP53-mutant cell lines), it could contribute new SL partner candidates that the reference paper or STRING network didn't surface.

**Impact**: Transforms Synapse from confirmatory to generative. A CRISPR screen dataset on Synapse could identify synthetic lethal partners that Feng et al. missed or that were below their significance threshold.

### 4.2 Add Data-Level Query Capabilities

The most transformative improvement would be the ability to query *within* Synapse datasets — particularly tabular data (gene expression matrices, drug sensitivity tables, essentiality scores).

**What this would look like**:
- `get_entity_table_columns(syn_id)` → returns column names/types for a Synapse Table
- `query_synapse_table(syn_id, sql="SELECT * WHERE gene='WEE1' AND score < -0.5")` → returns filtered rows
- `download_entity_file(syn_id, file_name)` → retrieves a specific file from a project for local analysis

**Impact on CQ14**: Instead of "syn2384331 is about gene essentiality (Moderate grounding)," the pipeline could report: "In the DREAM essentiality dataset (syn2384331), WEE1 has a predicted essentiality probability of 0.87 across 216 cancer cell lines, independently confirming BioGRID ORCS data (Strong grounding, L2 → L3 upgrade)."

This is the single highest-impact improvement. It would convert Moderate grounding to Strong grounding for multiple claims and create a legitimate pathway for evidence level upgrades.

### 4.3 Implement a Synapse Evidence Modifier in the Grading System

Add explicit Synapse grounding modifiers to the L1–L4 evidence grading system:

| Modifier | Value | Criteria |
|----------|-------|----------|
| Synapse Strong | +0.10 | Dataset directly tests the claim with matching gene/drug/disease |
| Synapse Moderate | +0.05 | Dataset profiles relevant genes in same disease context |
| Synapse Analogous | +0.00 | Related mechanism in different disease (informative but not confirmatory) |
| Synapse Weak | +0.00 | Contextual only |

This would allow Synapse grounding to actually influence evidence scores. In CQ14, the three SYNTHETIC_LETHAL edges grounded at Moderate strength would each gain +0.05, moving the ORCS essentiality claims from 0.60 to 0.65 — a small but principled increase reflecting independent corroboration.

### 4.4 Use Synapse Annotations for Structured Matching

`get_entity_annotations` returns structured key-value pairs (species, assay type, disease, platform). These could be used for programmatic matching instead of keyword search:

**Current approach**: `search_synapse("WEE1 inhibitor cancer CRISPR")` → 5,125 results, manual relevance assessment

**Proposed approach**:
1. `search_synapse("gene essentiality CRISPR")` → candidate datasets
2. For each candidate, `get_entity_annotations(syn_id)` → check `species=Homo sapiens`, `assayType=CRISPR`, `disease=cancer`
3. Programmatic filtering to find datasets matching KG entity properties
4. Auto-score grounding strength based on annotation overlap

This would reduce false positives and enable semi-automated grounding instead of the current manual assessment.

### 4.5 Bridge Synapse to DepMap / GEO for Higher Coverage

The biggest coverage gap in CQ14 was that Feng et al.'s CRISPR screen data lives on GEO/SRA, not Synapse. More broadly, the DepMap project (Broad Institute) — the single most important source for cancer cell line essentiality data — has its primary distribution on the DepMap portal and Figshare, not Synapse. The DREAM challenge (syn2384331) is an older related effort.

Two approaches:

**A. Expand Synapse search to include DepMap mirrors**: The Cancer Dependency Map has some Synapse presence via the Cancer Cell Line Encyclopedia (syn5889324). A dedicated search strategy for DepMap-related Synapse projects could improve coverage.

**B. Add GEO/SRA as a parallel grounding source**: The publication pipeline's platform suitability matrix already identifies GEO as a fallback for cancer genomics. Adding GEO search tools (NCBI E-utilities for GEO datasets) alongside Synapse would dramatically increase coverage for domains where Synapse is thin. The grounding document would then have a "Synapse + GEO" combined coverage metric.

### 4.6 Leverage `get_entity_children` for Deep Project Exploration

`get_entity_children` was available but unused in CQ14. For large Synapse projects like CCLE (syn5889324), the top-level entity is a container with dozens of sub-datasets (gene expression, copy number, drug sensitivity, mutation calls). Walking the project tree could identify specific sub-files that directly ground specific KG edges.

For example: CCLE contains drug sensitivity data for hundreds of compounds. If the agent traversed `get_entity_children(syn5889324)` and found a file named `drug_sensitivity_AUC.csv`, it could (with data-level access per §4.2) check whether adavosertib sensitivity correlates with TP53 mutation status — providing Strong grounding for the TP53→WEE1 synthetic lethal claim.

### 4.7 Synapse Provenance for Edge Attribution

Synapse has a provenance system (`get_entity_provenance`) that tracks how datasets were generated — what code was run, what inputs were used. This metadata could be used to assess dataset quality and methodology alignment:

- A dataset with clear provenance (registered code, versioned inputs) is higher-quality grounding than one with no provenance
- If the provenance shows the dataset was derived from CRISPR screens, that's a stronger match for CRISPR-validated claims than if it came from expression profiling

Adding provenance quality as a factor in grounding strength classification would make the assessment more rigorous.

---

## 5. Summary: Synapse's Current vs Potential Role

| Dimension | Current State | Potential State |
|-----------|--------------|----------------|
| **When invoked** | Post-hoc (Stage 1b, after KG complete) | Active (Phase 3 EXPAND, during KG construction) |
| **What it queries** | Entity metadata (names, descriptions) | Entity metadata + tabular data within datasets |
| **What it contributes** | Confirmation at Moderate strength | New entities, Strong confirmations, evidence upgrades |
| **Coverage (CQ14)** | 21% nodes, 16% edges | Could reach 40-50% nodes, 30-40% edges with data access |
| **Evidence impact** | Zero score changes | +0.05 to +0.10 per Synapse-grounded claim |
| **Search precision** | Keyword matching, high false-positive rate | Annotation-based structured matching |
| **Exploration depth** | Top-level project metadata | Project → folder → file → data table traversal |
| **Complementary sources** | None (Synapse only) | Synapse + GEO + DepMap for combined coverage |

The integration has a solid architectural foundation — the 4-stage pipeline, grounding strength classification, MEMBER_OF exclusion logic, and platform suitability matrix are well-designed. The main limitation is depth: Synapse is currently treated as a catalog to browse, not a data source to query. Moving from metadata-level to data-level interaction is the single change that would most transform its value to the pipeline.

---

*Analysis based on CQ14 pipeline execution and publication pipeline skill documentation.*
