# Fabry Disease: Gb3 Substrate Accumulation

**Query**: Identify the specific glycolipid substrate that accumulates in Fabry Disease, and name the cellular organelle where this pathological accumulation primarily takes place due to the absence of functional alpha-galactosidase A.

**Generated**: 2026-02-27 | **Protocol**: Fuzzy-to-Fact Graph Builder

---

## Answer

| Question | Answer | Evidence Source |
|----------|--------|-----------------|
| **Accumulating substrate** | **Globotriaosylceramide (Gb3)** | UniProt P06280, ChEBI:18313, PubChem CID 66616222 |
| **Cellular organelle** | **Lysosome** | UniProt subcellular location, GO:0005764 |

### Mechanism

Alpha-galactosidase A (EC 3.2.1.22), encoded by the *GLA* gene (HGNC:4296, Xq22.1), catalyzes the hydrolysis of terminal non-reducing alpha-D-galactose residues from globotriaosylceramide (Gb3). The specific reaction is:

```
Gb3Cer + H₂O → Lactosylceramide (LacCer) + D-galactose
```

When GLA is absent or non-functional, Gb3 cannot be degraded and accumulates progressively within **lysosomes** throughout multiple cell types, making Fabry disease a **lysosomal storage disorder** (sphingolipidosis).

---

## Anchor Node

| Property | Value |
|----------|-------|
| Gene Symbol | GLA |
| HGNC ID | HGNC:4296 |
| Full Name | Galactosidase alpha |
| Locus | Xq22.1 |
| Ensembl | ENSG00000102393 |
| Entrez | 2717 |
| OMIM | 300644 |
| EC Number | 3.2.1.22 |

## Protein (UniProt P06280)

| Property | Value |
|----------|-------|
| Protein Name | Alpha-galactosidase A |
| Length | 429 amino acids |
| Subcellular Location | **Lysosome** |
| Function | Catalyzes hydrolysis of glycosphingolipids in the lysosome |
| PDB Structures | 32 entries (best resolution: 1.90 A, PDB 3HG3) |
| BioGRID Interactions | 85 |
| ChEMBL Target | CHEMBL2524 |
| DrugBank | Migalastat (DB05018), Pegunigalsidase alfa (DB14992) |

## Substrate: Globotriaosylceramide (Gb3)

| Property | Value |
|----------|-------|
| Common Names | Gb3, Gb3Cer, GL-3, ceramide trihexoside (CTH) |
| ChEBI | CHEBI:18313 |
| PubChem CID | 66616222 |
| Molecular Weight | 827.9 Da |
| Molecular Formula | C₃₈H₆₉NO₁₈ |
| Classification | Globoside (glycosphingolipid) |
| Biosynthesis Enzyme | A4GALT (Gb3 synthase) — adds alpha-Gal to LacCer |

### Catalytic Reaction (Rhea:21112)

```
Gb3Cer (globoside) + H₂O  ──GLA (EC 3.2.1.22)──►  LacCer + D-galactose
    (CHEBI:18313)                                  (CHEBI:4139)  (CHEBI:17950)
```

## Disease: Fabry Disease (MONDO:0010526)

| Property | Value |
|----------|-------|
| MONDO ID | MONDO:0010526 |
| OMIM | 301500 |
| Inheritance | X-linked |
| Open Targets Score | 0.8935 |
| Total Disease Associations | 528 |
| Classification | Lysosomal storage disorder (sphingolipidosis) |

### Top Clinical Phenotypes (Open Targets)

| Phenotype | Association Score |
|-----------|------------------|
| Fabry disease | 0.8935 |
| Cardiomyopathy | 0.6000 |
| Angiokeratoma corporis diffusum | 0.5776 |
| Cardiovascular abnormality | 0.5637 |
| Hypertrophic cardiomyopathy | 0.5357 |
| Renal insufficiency | 0.4632 |
| Stroke | 0.3743 |
| Kidney failure | 0.3710 |

## Protein Interaction Network (STRING)

Top interactors for GLA (9606.ENSP00000218516):

| Partner | Score | Role |
|---------|-------|------|
| GLB1 (beta-galactosidase) | 0.984 | Lysosomal glycosidase (GM1 gangliosidosis) |
| GALC (galactosylceramidase) | 0.976 | Lysosomal enzyme (Krabbe disease) |
| ARSA (arylsulfatase A) | 0.974 | Lysosomal enzyme (MLD) |
| A4GALT (Gb3 synthase) | 0.949 | Synthesizes Gb3 from LacCer (opposing pathway) |
| GAA (acid alpha-glucosidase) | 0.943 | Lysosomal enzyme (Pompe disease) |
| HEXB (hexosaminidase B) | 0.941 | Lysosomal enzyme (Sandhoff disease) |
| HEXA (hexosaminidase A) | 0.939 | Lysosomal enzyme (Tay-Sachs disease) |

## Pathways

| ID | Pathway Name | Source |
|----|-------------|--------|
| WP4153 | Degradation pathway of sphingolipids, including diseases | WikiPathways |
| WP2788 | Sphingolipid metabolism | WikiPathways |
| WP5292 | Glycosphingolipid metabolism | WikiPathways |
| R-HSA-9840310 | Glycosphingolipid catabolism | Reactome |
| R-HSA-6798695 | Neutrophil degranulation | Reactome |

## Approved Therapies

### Enzyme Replacement Therapy (ERT)

| Drug | ChEMBL | Type | Approved | Trade Name | Route |
|------|--------|------|----------|------------|-------|
| Agalsidase beta | CHEMBL2108888 | Enzyme | 2001 | Fabrazyme | IV infusion |
| Agalsidase alfa | CHEMBL2108214 | Enzyme | 2001 | Replagal | IV infusion |

### Pharmacological Chaperone

| Drug | ChEMBL | Type | Approved | Trade Name | Route |
|------|--------|------|----------|------------|-------|
| Migalastat HCl | CHEMBL2107355 | Small molecule | 2016 | Galafold | Oral |

**Mechanism**: Migalastat is a pharmacological chaperone that binds to and stabilizes amenable GLA mutant variants in the endoplasmic reticulum, facilitating proper folding and trafficking to the lysosome where it dissociates, restoring enzymatic activity.

### Newer Therapy

| Drug | DrugBank | Type | Note |
|------|----------|------|------|
| Pegunigalsidase alfa | DB14992 | PEGylated enzyme | Extended half-life ERT |

## Knowledge Graph Summary

```
                      ENCODES
   [HGNC:4296 GLA] ──────────► [UniProtKB:P06280]
         │                            │
         │ ASSOCIATED_WITH            │ HYDROLYZES
         │                            │
         ▼                            ▼
   [MONDO:0010526]              [CHEBI:18313 Gb3]
   Fabry disease                      │
         ▲                            │ ACCUMULATES_IN
         │ TREATS                     │
         │                            ▼
   ┌─────┼─────────┐           [GO:0005764 Lysosome]
   │     │         │
   ▼     ▼         ▼
 [Agalsidase β] [Agalsidase α] [Migalastat]
 CHEMBL:2108888  CHEMBL:2108214  CHEMBL:2107355
 (ERT, 2001)     (ERT, 2001)    (Chaperone, 2016)
```

**Nodes**: 11 | **Edges**: 10 | **Data sources**: HGNC, UniProt, ChEBI, PubChem, Open Targets, STRING, ChEMBL, WikiPathways, Reactome, ClinicalTrials.gov

---

## Data Provenance

| Source | Endpoint | Identifier |
|--------|----------|------------|
| HGNC | rest.genenames.org/fetch/symbol/GLA | HGNC:4296 |
| UniProt | rest.uniprot.org/uniprotkb/P06280.json | P06280 |
| PubChem | pubchem.ncbi.nlm.nih.gov/rest/pug/compound/name/globotriaosylceramide | CID 66616222 |
| Open Targets | api.platform.opentargets.org/api/v4/graphql | ENSG00000102393 |
| STRING | string-db.org/api/json/interaction_partners | ENSP00000218516 |
| ChEMBL | ebi.ac.uk/chembl/api/data/mechanism | CHEMBL2524 |
| ChEMBL | ebi.ac.uk/chembl/api/data/drug_indication | CHEMBL2107355 |
| WikiPathways | webservice.wikipathways.org/findPathwaysByXref | Entrez:2717 |
| ClinicalTrials.gov | clinicaltrials.gov/api/v2/studies | Fabry disease |
| Graphiti | graphiti-docker add_memory | group: open-biosciences |
