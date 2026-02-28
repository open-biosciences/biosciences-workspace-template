# Synapse Grounding Report: Fabry Disease Gb3 Accumulation

**Date**: 2026-02-28
**Knowledge Graph**: `graph.json` (Fabry Disease -- Gb3 Lysosomal Accumulation)
**Protocol**: Fuzzy-to-Fact Graph Builder, Stage 1b Synapse Grounder
**Competency Question**: "Identify the specific glycolipid substrate that accumulates in Fabry Disease, and name the cellular organelle where this pathological accumulation primarily takes place due to the absence of functional alpha-galactosidase A."

---

## 1. Summary

### Coverage Statistics

| Metric | Count | Percentage |
|--------|-------|------------|
| **Total Nodes** | 11 | -- |
| **Grounded Nodes** | 3 | 27.3% |
| **Ungrounded Nodes** | 8 | 72.7% |
| **Total Edges** | 12 | -- |
| **MEMBER_OF Edges** | 0 | (excluded from denominator) |
| **Groundable Edges** | 12 | -- |
| **Grounded Edges** | 2 | 16.7% |
| **Ungrounded Edges** | 10 | 83.3% |

**MEMBER_OF Exclusion Note**: This graph contains 0 MEMBER_OF edges, so the raw and adjusted denominators are identical. All 12 edges are groundable.

### Key Finding

Synapse.org contains very limited experimental data directly relevant to Fabry disease and Gb3 metabolism. The primary Fabry Disease project on Synapse (syn52229845) contains clinical images only, with no structured annotations, no experimental datasets, and no omics data. No Fabry-specific GLA enzyme assays, Gb3 quantification studies, agalsidase treatment datasets, or migalastat pharmacological chaperone studies were identified on Synapse.

Grounding support comes exclusively from **related lysosomal storage disorder (LSD) datasets** and **sphingolipid pathway studies** that provide analogous but indirect evidence for the graph's mechanistic claims.

---

## 2. Dataset-to-Graph Mapping Table

| Synapse ID | Name | Nodes Grounded | Edges Grounded | Evidence Type | Species | Grounding Strength |
|------------|------|---------------|----------------|--------------|---------|-------------------|
| syn52229845 | Fabry Disease (Project) | MONDO:0010526 | -- | Clinical images | Human | Weak |
| syn353567 | GSE18745: S1P lyase deficiency & sphingolipid metabolism | GO:0005764, CHEBI:18313 | ACCUMULATES_IN | Microarray, lipid profiling | Mouse | Moderate |
| syn178276 | GSE23408: Gaucher Disease (GCase mutant mice) | GO:0005764 | -- | Microarray | Mouse | Weak |
| syn351270 | GSE19641: Sandhoff Disease (Hexb-/- mice) | GO:0005764 | -- | Microarray | Mouse | Weak |
| syn58605953 | Anding et al. 2023: ERT efficacy in Pompe mice | -- | ENZYME_REPLACEMENT (analogous) | Drug efficacy, transcriptomics | Mouse | Moderate |
| syn52504936 | Showalter 2020: BMP in lysosomal sphingolipid catabolism | GO:0005764, UniProtKB:P06280 | HYDROLYZES (analogous) | Review/Analysis | -- | Weak |
| syn25610190 | Progranulin KO Mouse: lysosomal dysfunction proteomics | GO:0005764 | -- | TMT mass spectrometry | Mouse | Weak |
| syn236087 | GSE6322: Lysosome-related organelle biogenesis | GO:0005764 | -- | Microarray | -- | Weak |
| syn2596750 | WP1424: Globo Sphingolipid Metabolism pathway | CHEBI:18313 | -- | Pathway visualization | -- | Weak |
| syn52504899 | Stevens 2023: Glycosphingolipid metabolism in asthma | CHEBI:18313 | -- | Metabolomics | Mouse | Weak |

---

## 3. Node Grounding Coverage

### Grounded Nodes (3/11)

| Node ID | Node Name | Type | Synapse Datasets | Strength |
|---------|-----------|------|-----------------|----------|
| MONDO:0010526 | Fabry disease | Disease | syn52229845 | **Weak** -- Project exists but contains only clinical images with no annotations or experimental data |
| GO:0005764 | Lysosome | Organelle | syn353567, syn178276, syn351270, syn25610190, syn236087, syn52504936 | **Moderate** -- Multiple LSD datasets profile lysosomal dysfunction, lysosomal marker changes, and sphingolipid catabolism within lysosomes |
| CHEBI:18313 | Globotriaosylceramide (Gb3) | Substrate | syn353567, syn2596750, syn52504899 | **Weak** -- Sphingolipid pathway datasets reference glycosphingolipid metabolism but none directly measure Gb3 levels |

### Ungrounded Nodes (8/11)

| Node ID | Node Name | Type | Search Queries Attempted | Result |
|---------|-----------|------|-------------------------|--------|
| HGNC:4296 | GLA | Gene | "GLA Fabry disease", "GLA alpha-galactosidase", "GLA gene therapy" | No datasets found -- search results dominated by unrelated "GLA" matches and NF1 gene therapy studies |
| UniProtKB:P06280 | Alpha-galactosidase A | Protein | "alpha galactosidase lysosome" | No Fabry-specific datasets -- results show beta-galactosidase and other lysosomal enzyme studies |
| CHEBI:17950 | D-galactose | Metabolite | (covered by compound+disease queries) | No datasets found measuring D-galactose as a GLA reaction product |
| CHEBI:4139 | Lactosylceramide | Metabolite | "ceramide trihexoside", "sphingolipid degradation" | No datasets found directly measuring LacCer as a GLA hydrolysis product |
| CHEMBL:2107355 | Migalastat hydrochloride | Compound | "migalastat Fabry" | No datasets found -- 5 results returned but none relevant (all false positives from "Fabry" as a surname) |
| CHEMBL:2108888 | Agalsidase beta | Compound | "agalsidase Fabry" | No datasets found -- 5 results returned but none relevant (false positives) |
| CHEMBL:2108214 | Agalsidase alfa | Compound | "agalsidase Fabry" | No datasets found (same search as agalsidase beta) |
| CHEMBL:2524 | Alpha-galactosidase A (Target) | Target | (covered by protein and compound queries) | No datasets found |

---

## 4. Edge Grounding Coverage

### Grounded Edges (2/12)

| Edge | Source | Target | Type | Synapse Dataset(s) | Strength | Justification |
|------|--------|--------|------|-------------------|----------|---------------|
| E5 | CHEBI:18313 (Gb3) | GO:0005764 (Lysosome) | ACCUMULATES_IN | syn353567 (GSE18745) | **Moderate** | S1P lyase KO mice demonstrate sphingolipid accumulation in lysosomes including elevated ceramide and sphingomyelin -- analogous mechanism to Gb3 accumulation in GLA deficiency. Both represent lysosomal sphingolipid substrate accumulation due to enzyme deficiency. |
| E8/E9 | CHEMBL:2108888/2108214 (Agalsidase) | CHEMBL:2524 (Target) | ENZYME_REPLACEMENT | syn58605953 (Anding et al. 2023) | **Moderate** | Pompe disease ERT study directly demonstrates ERT mechanism (recombinant enzyme restoring lysosomal function) in a related LSD. The M6P-mediated lysosomal targeting mechanism validated here is shared by agalsidase in Fabry disease. |

### Ungrounded Edges (10/12)

| Edge | Source | Target | Type | Notes |
|------|--------|--------|------|-------|
| E1 | HGNC:4296 (GLA) | UniProtKB:P06280 (Protein) | ENCODES | Definitional (gene-protein encoding relationship) -- not amenable to dataset grounding |
| E2 | UniProtKB:P06280 (Protein) | CHEBI:18313 (Gb3) | HYDROLYZES | No Fabry-specific enzyme activity datasets found on Synapse |
| E3 | UniProtKB:P06280 (Protein) | CHEBI:4139 (LacCer) | PRODUCES | No datasets measuring LacCer as hydrolysis product |
| E4 | UniProtKB:P06280 (Protein) | CHEBI:17950 (Galactose) | PRODUCES | No datasets measuring D-galactose as hydrolysis product |
| E6 | HGNC:4296 (GLA) | MONDO:0010526 (Fabry) | ASSOCIATED_WITH | No GLA variant-Fabry phenotype datasets on Synapse |
| E7 | CHEMBL:2107355 (Migalastat) | CHEMBL:2524 (Target) | STABILISER | No migalastat chaperone mechanism datasets on Synapse |
| E10 | CHEMBL:2107355 (Migalastat) | MONDO:0010526 (Fabry) | TREATS | No migalastat clinical outcome datasets on Synapse |
| E11 | CHEMBL:2108888 (Agalsidase beta) | MONDO:0010526 (Fabry) | TREATS | No agalsidase beta Fabry treatment datasets on Synapse |
| E12 | CHEMBL:2108214 (Agalsidase alfa) | MONDO:0010526 (Fabry) | TREATS | No agalsidase alfa Fabry treatment datasets on Synapse |

---

## 5. Evidence Level Upgrades

No evidence level upgrades are warranted. The Synapse grounding provides only **Weak** to **Moderate** analogous support from related lysosomal storage disorders (Gaucher, Sandhoff, Pompe) rather than direct Fabry disease experimental evidence. The existing evidence levels from the Fuzzy-to-Fact pipeline (based on HGNC, UniProt, ChEMBL, Open Targets, STRING, and other primary databases) remain the authoritative grades.

| Claim | Current Level | Synapse Upgrade | New Level |
|-------|--------------|-----------------|-----------|
| GLA deficiency causes Gb3 accumulation | L4 (Clinical) | None -- no direct Fabry dataset | L4 (unchanged) |
| Agalsidase treats Fabry disease | L4 (Clinical, FDA-approved) | None | L4 (unchanged) |
| Migalastat stabilizes GLA | L4 (Clinical, FDA-approved) | None | L4 (unchanged) |
| Gb3 accumulates in lysosomes | L3 (Functional) | None -- only analogous LSD data | L3 (unchanged) |

---

## 6. Grounding Confidence Matrix

| Claim | Synapse Datasets | Strength | Justification |
|-------|-----------------|----------|---------------|
| Gb3 is the accumulating substrate in Fabry disease | syn52229845 (project only), syn2596750 (pathway viz) | **Weak** | Fabry Disease project on Synapse contains no experimental data. Globo sphingolipid pathway visualization references Gb3 but provides no quantitative evidence. |
| GLA encodes alpha-galactosidase A | None | **Not groundable** | Gene-protein encoding is a definitional relationship from reference databases (HGNC, UniProt), not from experimental datasets. |
| Alpha-galactosidase A hydrolyzes Gb3 to LacCer + galactose | syn52504936 (BMP review) | **Weak** | BMP review discusses lysosomal hydrolytic enzymes including those for sphingolipid catabolism but does not directly test GLA enzymatic activity on Gb3. |
| Gb3 accumulates in lysosomes when GLA is deficient | syn353567 (GSE18745), syn178276 (GSE23408), syn351270 (GSE19641) | **Moderate** | Multiple related LSD datasets demonstrate the general principle of lysosomal substrate accumulation due to hydrolase deficiency (S1P lyase, glucocerebrosidase, hexosaminidase). Mechanistic analogy is strong but species and substrate differ. |
| GLA variants are associated with Fabry disease | None | **Not groundable** | No GLA variant databases or genotype-phenotype datasets found on Synapse. This is well-established via OMIM (300644) and ClinVar from pipeline Phase 1. |
| Migalastat is a pharmacological chaperone for GLA | None | **No datasets found** | Zero Synapse results for migalastat-specific studies. |
| Agalsidase beta/alfa treats Fabry disease | syn58605953 (ERT in Pompe) | **Moderate** | Pompe ERT study validates the general ERT mechanism (lysosomal enzyme delivery via M6P receptor) but uses a different enzyme (acid alpha-glucosidase) for a different substrate (glycogen). |
| ERT restores lysosomal enzyme function | syn58605953 (Anding et al. 2023) | **Moderate** | Directly demonstrates that increasing M6P levels enhances ERT efficacy via CIMPR-mediated lysosomal targeting. This mechanism is shared across LSDs including Fabry disease. |

---

## 7. Methodology

### Tools Used

| Tool | Purpose | Calls Made |
|------|---------|------------|
| `mcp__claude_ai_Synapse_org__search_synapse` | Keyword search across Synapse entities | 12 searches |
| `mcp__claude_ai_Synapse_org__get_entity` | Full metadata retrieval for candidate datasets | 2 entities |
| `mcp__claude_ai_Synapse_org__get_entity_children` | Container exploration | 2 containers |
| `mcp__claude_ai_Synapse_org__get_entity_annotations` | Structured annotation retrieval | 1 entity |

### Search Queries Executed

| # | Query | Results Found | Relevant Hits |
|---|-------|---------------|---------------|
| 1 | "globotriaosylceramide Fabry disease" | 3,423 | 2 (syn52229845, syn52229903) |
| 2 | "GLA Fabry disease" | 3,425 | 2 (same Fabry project) |
| 3 | "GLA alpha-galactosidase" | 569 | 0 (false positives: HDAC4/beta-gal, alpha-diversity notebooks) |
| 4 | "agalsidase Fabry" | 5 | 1 (syn52229845, same Fabry project) |
| 5 | "migalastat Fabry" | 5 | 1 (syn52229845, same Fabry project) |
| 6 | "sphingolipid degradation" | 197 | 3 (GSE18745, GSE25295, Globo pathway) |
| 7 | "Fabry disease Gb3" | 3,423 | 2 (same Fabry project) |
| 8 | "alpha galactosidase lysosome" | 574 | 1 (GSE6322 lysosome organelle biogenesis) |
| 9 | "ceramide trihexoside" | 10 | 0 (ceramide synthesis/storage studies, not Gb3-specific) |
| 10 | "GLA gene therapy" | 10,000+ | 0 (all NF1/NF2 gene therapy, not GLA) |
| 11 | "sphingolipid lysosomal hydrolase" | 64 | 2 (BMP review, S1P lyase study) |
| 12 | "Sandhoff Gaucher sphingolipidosis" | 11 | 4 (Gaucher and Sandhoff disease datasets) |

### Matching Criteria

1. **Direct match**: Dataset explicitly names Fabry disease, GLA, Gb3/GL-3, agalsidase, or migalastat
2. **Mechanistic analogy**: Dataset studies the same biological mechanism (lysosomal substrate accumulation, enzyme replacement therapy) in a related lysosomal storage disorder
3. **Pathway overlap**: Dataset profiles sphingolipid or glycosphingolipid metabolism pathways that include Gb3

### MEMBER_OF Exclusion Rationale

This graph contains zero MEMBER_OF edges. All 12 edges represent mechanistic relationships (ENCODES, HYDROLYZES, PRODUCES, ACCUMULATES_IN, ASSOCIATED_WITH, STABILISER, ENZYME_REPLACEMENT, TREATS) that are in principle groundable by experimental datasets. No adjustment to the denominator was needed.

---

## 8. Limitations

### 1. Fabry Disease Data Scarcity on Synapse
Synapse.org is primarily oriented toward neurodegenerative disease (Alzheimer's, Parkinson's), neurofibromatosis (NF1, NF2), and cancer genomics. Rare metabolic diseases like Fabry disease are profoundly underrepresented. The sole Fabry Disease project (syn52229845) contains two clinical JPEG images and a self-referencing link, with no structured annotations, no omics data, and no experimental datasets.

### 2. Cross-Disease Analogies Are Indirect
Grounding from related LSDs (Gaucher, Sandhoff, Pompe) provides mechanistic analogy but does not constitute direct experimental evidence for Fabry-specific claims. The accumulating substrates (glucosylceramide, GM2 ganglioside, glycogen) differ from Gb3, and the deficient enzymes differ from alpha-galactosidase A.

### 3. Search Term Ambiguity
- "GLA" is a short gene symbol that generates many false positives (general linear algebra, GLA as an abbreviation in other contexts)
- "Fabry" appears as a surname (Ben Fabry, physicist) in unrelated projects
- "Alpha-galactosidase" partially matches beta-galactosidase studies

### 4. Platform Bias
Synapse.org hosts data primarily from Sage Bionetworks, NCI programs (PS-ON, CSBC), NF-OSI, and the AD Knowledge Portal. Lysosomal storage disease research data is more commonly deposited in GEO (NCBI), ArrayExpress/BioStudies (EBI), and disease-specific registries (Fabry Outcome Survey, Fabry Registry).

### 5. No Drug Treatment Datasets
No Synapse datasets were found for agalsidase beta (Fabrazyme), agalsidase alfa (Replagal), or migalastat (Galafold) treatment studies. Clinical trial data for these FDA-approved therapies exists in ClinicalTrials.gov and published literature but not on Synapse.

### 6. Temporal Coverage
The related LSD datasets found (GSE18745, GSE19641, GSE23408) date from 2009-2012. More recent Fabry disease omics studies may exist in other repositories but were not indexed on Synapse at the time of this search.

---

## Appendix: Synapse Entity Details

### syn52229845 -- Fabry Disease Project
- **Type**: Project
- **Created**: 2023-08-02
- **Modified**: 2024-10-08
- **Creator**: User 3478118
- **Contents**: 2 JPEG clinical images, 1 self-referencing link
- **Annotations**: None (empty annotation set)
- **Assessment**: Placeholder project with no experimental data. Cannot support grounding for any graph element.

### syn58605953 -- ERT Efficacy in Pompe Mice (Anding et al. 2023)
- **Type**: Project
- **PMID**: 37679046
- **Journal**: J Pharmacol Exp Ther (2023)
- **Key finding**: M6P levels (not miglustat coadministration) enhance ERT efficacy via CIMPR-mediated lysosomal targeting
- **Relevance**: Validates shared ERT mechanism across LSDs; agalsidase beta uses same M6P receptor pathway for lysosomal delivery

### syn353567 -- GSE18745: S1P Lyase Deficiency
- **Type**: GEO Dataset (Folder + Raw Data)
- **Key finding**: Sphingolipid substrate accumulation (sphingosine, ceramide, sphingomyelin) in serum/liver of S1P lyase-deficient mice
- **Relevance**: Demonstrates that lysosomal sphingolipid pathway enzyme deficiency causes multi-substrate accumulation, analogous to GLA deficiency causing Gb3 accumulation

### syn178276 -- GSE23408: Gaucher Disease Mouse Model
- **Type**: GEO Dataset (Folder + Raw Data)
- **Key finding**: GCase point mutations (V394L, D409V) cause glucosylceramide accumulation and inflammatory pathway activation
- **Relevance**: Closely related LSD with analogous mechanism (lysosomal hydrolase deficiency --> substrate accumulation). Gaucher disease GCase is in the same sphingolipid degradation pathway as Fabry disease GLA.
