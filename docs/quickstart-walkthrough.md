# Quickstart Walkthrough: From Research Question to Real Data

This guide walks you through setting up the Open Biosciences plugin in Claude CoWork and running a complete research workflow — from asking a biology question to tracing a disease mechanism to finding real experimental datasets on Synapse.

**Time**: ~15 minutes | **API keys needed**: None (all tools are pre-connected via the plugin)

---

## Step 1: Install the Plugin

Open Claude CoWork (or Claude Code) and run:

```
/plugin marketplace add open-biosciences/open-biosciences-plugins
/plugin install bio-research@open-biosciences-plugins
```

This installs 11 domain skills, 6 research commands, and connections to 34+ life sciences API tools across 5 MCP servers.

Verify the plugin is active:

```
/plugin list
```

You should see:

```
bio-research @ open-biosciences-plugins
  Version: 1.0.0
  Investigate biological targets, map protein interactions, and evaluate
  drug candidates using 12 curated life sciences APIs. Generate evidence-
  graded reports and BioRxiv-ready manuscripts from a single research question.
```

## Step 2: Set Up Your Workspace

Clone the workspace template and set it as your CoWork working folder:

```bash
git clone https://github.com/open-biosciences/biosciences-workspace-template.git
```

In Claude CoWork, choose **"Work in a folder"** and select the cloned `biosciences-workspace-template` directory.

## Step 3: Ask a Research Question

We'll trace how a drug treats a rare disease — a 3-hop mechanism chain that's easy to follow but teaches the full Fuzzy-to-Fact protocol.

Type this:

```
/ob-research Identify the substrate that accumulates in Fabry disease and explain why
```

The plugin's graph-builder skill activates and runs through 6 phases automatically:

| Phase | What Happens | What You See |
|-------|-------------|-------------|
| 1. ANCHOR | Resolves "Fabry disease" and "GLA" to canonical IDs | `HGNC:4296`, `MONDO:0010526` |
| 2. ENRICH | Gets full metadata — protein function, enzyme activity | UniProt P06280, alpha-galactosidase A |
| 3. EXPAND | Finds interaction partners via STRING | A4GALT (Gb3 synthase, score 0.949) |
| 4a. TRAVERSE_DRUGS | Discovers drugs targeting GLA | Agalsidase beta, agalsidase alfa, migalastat |
| 4b. TRAVERSE_TRIALS | Finds clinical trials | Active trials for ERT and chaperone therapy |
| 5. VALIDATE | Verifies every entity, edge, and trial ID | All CURIEs confirmed |

### What You Get

A structured mechanism report showing the causal chain:

```
Gene(GLA/HGNC:4296)
  → encodes → Protein(alpha-galactosidase A/UniProt:P06280)
    → fails to hydrolyze → Substrate(Gb3/CHEBI:18313)
      → accumulates in → Organelle(lysosome/GO:0005764)
        → causes → Disease(Fabry disease/MONDO:0010526)
```

Every node has a canonical ID. Every edge traces to a tool call. Nothing comes from training knowledge.

See a completed example: [`output/fabry-disease-gb3-accumulation/`](../output/fabry-disease-gb3-accumulation/)

## Step 4: Find Real Datasets on Synapse

Now comes the data literacy step. The pipeline built a knowledge graph from curated databases — but where is the *raw experimental data* behind these claims?

Synapse.org is a collaborative research data platform where scientists share datasets, code, and results. The plugin connects to Synapse via MCP, so you can search it directly.

### Why Synapse Comes After Entity Resolution

Notice that we search Synapse *after* the knowledge graph is built, not before. This is intentional. The pipeline's earlier phases resolve vague terms into precise identifiers — "Fabry disease" becomes `MONDO:0010526`, the gene becomes `GLA`, the enzyme becomes "alpha-galactosidase A." These resolved terms make Synapse queries far more efficient than searching with raw natural language.

This follows the same `slim=true` philosophy used throughout the pipeline: minimize token usage in early discovery phases so you have budget for rich retrieval later.

### Search for Fabry Disease Datasets

Using the resolved terms from your knowledge graph, ask Claude:

```
Search Synapse for Fabry disease alpha-galactosidase datasets, limit
results to 5. I want to find experimental data related to enzyme
replacement therapy.
```

Claude will call `search_synapse` with `limit=5` and find results like:

| Synapse ID | Name | Type |
|-----------|------|------|
| syn52229845 | Fabry Disease | Project |

### Explore a Dataset

Ask Claude to dig deeper:

```
Get the details and children of the Fabry Disease project syn52229845.
What files does it contain?
```

Claude calls `get_entity` and `get_entity_children` to show you:
- **Project metadata**: who created it, when, what license
- **Contents**: files, folders, and links within the project
- **Annotations**: structured metadata (species, assay type, disease)

### Connect Data to Your Knowledge Graph

This is the key learning moment. Ask:

```
How does the Fabry Disease data on Synapse relate to the knowledge graph
we just built? Which nodes could this dataset ground?
```

Claude will map Synapse datasets to your KG nodes:

| KG Node | CURIE | Synapse Grounding | Strength |
|---------|-------|-------------------|----------|
| GLA | HGNC:4296 | syn52229845 (Fabry Disease project) | Moderate |
| Alpha-galactosidase A | UniProt:P06280 | Same project — enzyme data | Moderate |
| Agalsidase beta | CHEMBL:2108888 | Not on Synapse — drug data lives in ChEMBL | N/A |

This teaches a crucial data literacy lesson: **different databases serve different purposes**. Synapse holds raw experimental data. ChEMBL holds compound pharmacology. ClinicalTrials.gov holds trial records. The knowledge graph connects them all.

### Try a Broader Search

Fabry disease is a lysosomal storage disorder. Search for related datasets:

```
Search Synapse for lysosomal storage disease enzyme replacement therapy
```

You'll find richer datasets like:

| Synapse ID | Name | Relevance |
|-----------|------|-----------|
| syn58605953 | ERT efficacy in Pompe mice (Anding et al., 2023) | Same disease family, ERT mechanism |
| syn256935 | GSE27280 — Pompe disease gene expression | Lysosomal enzyme deficiency expression data |

These are analogous datasets — same disease mechanism (lysosomal enzyme deficiency → substrate accumulation), different specific enzyme. Understanding this analogy is a core skill in translational research.

## Step 5: Generate a Report (Optional)

If you want to see the full publication pipeline:

```
/ob-report
```

This formats your pipeline output into a professional report with evidence grading, source citations, and a structured entity table.

For the full pipeline (report + Synapse grounding + quality review + BioRxiv draft):

```
/ob-publish
```

## What You Learned

By completing this walkthrough, you've practiced:

1. **Entity resolution** — turning natural language ("Fabry disease") into canonical identifiers (MONDO:0010526)
2. **Multi-database traversal** — following a question across HGNC, UniProt, ChEMBL, STRING, and Open Targets
3. **The Fuzzy-to-Fact protocol** — always LOCATE first (fuzzy search), then RETRIEVE (strict lookup by ID)
4. **Data grounding** — connecting knowledge graph claims to real experimental datasets on Synapse
5. **Evidence assessment** — understanding grounding strength (Strong, Moderate, Weak, Analogous)
6. **Database roles** — knowing which database to query for which type of information

## Next Steps

| Want to... | Do this |
|-----------|---------|
| Try a harder question | `/ob-research What drugs targeting BCL2 could treat CLL?` |
| Validate synthetic lethality | `/ob-research How can we validate synthetic lethal partners for TP53-mutant cancers?` |
| Survey a clinical landscape | `/ob-research Which EGFR inhibitors are validated in NSCLC and what limits their efficacy?` |
| Browse competency questions | `/ob-cq-discover` |
| Run a structured benchmark | `/ob-cq-run cq1` |

## Troubleshooting

**Plugin not found**: Make sure you ran both the `marketplace add` and `plugin install` commands.

**MCP tools not responding**: The plugin connects to cloud-hosted MCP servers. Check your internet connection. You can verify connectivity by asking Claude to search for a gene: "Search HGNC for TP53."

**Synapse returns too many results**: Synapse responses can be very large (50KB+) and consume context budget quickly. Three rules:
1. **Always set `limit=5`** (or even 3) on search queries — the default of 20 is too many
2. **Use resolved terms** from earlier pipeline phases, not raw natural language — "alpha-galactosidase A lysosomal" beats "enzyme disease"
3. **Filter by entity type** when possible — `entity_type="project"` skips individual files and links

**Context window exhaustion**: For complex questions, the pipeline may need multiple sessions. The workspace template captures intermediate outputs in `output/{slug}/` so you can resume.
