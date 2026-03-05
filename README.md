# biosciences-workspace-template

Clone this repo and use it as your CoWork workspace for Open Biosciences research.

Part of the [Open Biosciences](https://github.com/open-biosciences) platform.

## What This Is

This repo is a **Claude CoWork workspace template** for life sciences research pipelines. You set it as your "work in a folder" directory when starting a new task in [Claude CoWork](https://claude.ai) (Anthropic's agentic desktop app). Each research session creates structured, versioned outputs under `output/{slug}/`.

It is not a bootstrap script collection or environment setup tool. Its sole purpose is to capture research pipeline outputs in a reproducible, git-tracked format.

## Three Core Research Personas

---

### Educational / Trainee

*"I'm learning pathway biology and need to understand how drugs connect to diseases"*

* **Benchmark:** FOP Drug Mechanism — 4-node linear path
* **Value:** Structured learning with verified relationships
* **Prompt/Action:** "Show me the mechanism step by step"

---

### Independent Investigator

*"I need to validate my hypothesis about synthetic lethal targets before writing a grant"*

* **Benchmark:** ARID1A Synthetic Lethality — multi-source validation
* **Value:** Multi-database triangulation with evidence grading
* **Prompt/Action:** "Cross-reference my targets against all data"

---

### Project Manager

*"I need to understand the clinical landscape to prioritize resources"*

* **Benchmark:** Health Emergency Landscape — 18,636+ trials
* **Value:** Rapid clinical landscape with structured output
* **Prompt/Action:** "Give me an evidence-based overview for leadership"

## Quick Start

### 1. Install the Open Biosciences plugin

In Claude Code or CoWork:

```
/plugin marketplace add open-biosciences/open-biosciences-plugins
/plugin install bio-research@open-biosciences-plugins
```

This gives you 11 domain skills (9 core + 2 beta competency-question skills), 6 research commands, and connections to 34+ life sciences API tools.

### 2. Clone this repo

```bash
git clone https://github.com/open-biosciences/biosciences-workspace-template.git
```

### 3. Set as your CoWork working folder

When starting a new task in Claude CoWork, choose **"Work in a folder"** and select the cloned `biosciences-workspace-template` directory.

### 4. Run research commands

Start a research session with any of the plugin commands:

| Command | Purpose |
|---------|---------|
| `/ob-research` | Run a full research pipeline on a topic |
| `/ob-report` | Generate a structured research report |
| `/ob-review` | Run a 10-dimension quality review |
| `/ob-publish` | Produce a publication-quality BioRxiv draft |
| `/ob-cq-discover` | Discover competency questions for a domain |
| `/ob-cq-run` | Execute a competency question pipeline |

## How It Works

The **[open-biosciences-plugins](https://github.com/open-biosciences/open-biosciences-plugins)** repo provides the research capabilities: domain skills for genomics, proteomics, pharmacology, clinical trials, CRISPR analysis, graph building, reporting, quality review, and publication pipelines. It also connects to 34+ life sciences API tools via the biosciences-mcp gateway, including edge tools for ORCS CRISPR essentiality and ChEMBL mechanism-of-action queries.

This **workspace template** is where the outputs land. The flow is:

1. You start a CoWork session with this repo as your working folder
2. You invoke a research command (e.g., `/ob-research` with a topic)
3. The plugin's skills orchestrate MCP tool calls across life sciences APIs
4. Pipeline outputs are written to `output/{slug}/` with standard artifact types
5. You commit the results -- each research session becomes a versioned record

## Example Workspaces

The `output/` directory contains completed research sessions:

| Workspace | Topic | Date | Key Outputs |
|-----------|-------|------|-------------|
| `fabry-disease-mar2` | Fabry disease (early run) | 2026-02-28 | Report, KG JSON, quality review |
| `fabry-disease-gb3-accumulation` | Fabry disease Gb3 mechanism | 2026-02-28 | Complete pipeline: report, KG, synapse grounding, quality review, BioRxiv draft |
| `cc-tyrosine-kinase` | EGFR/NSCLC first-gen TKI | 2026-03-01 | Report, KG, quality review, synapse grounding, BioRxiv draft |
| `cq14` | TP53 synthetic lethality | 2026-03-02 | Full validation run with `.ob-cq/` structured outputs, `WORKSPACE_MEMORY.md` |
| `cq14edgefix` | TP53 SL pipeline refinement | 2026-03-03 | Complete pipeline: report, KG, quality review, synapse grounding, BioRxiv draft, session learnings, pipeline chronology |

## What Each Workspace Contains

A completed research session typically produces these artifacts in `output/{slug}/`:

| Artifact | Filename Pattern | Description |
|----------|-----------------|-------------|
| Research report | `*-report.md` | Structured report with entities, edges, and evidence grading |
| Knowledge graph | `*-knowledge-graph.json` | Machine-readable knowledge subgraph |
| Quality review | `*-quality-review.md` | 10-dimension quality assessment |
| Synapse grounding | `*-synapse-grounding.md` | Synapse dataset grounding check |
| BioRxiv draft | `*-biorxiv-draft.md` | Publication-quality manuscript draft |

Some sessions also produce:
- `.ob-cq/` directories with structured competency question outputs
- `WORKSPACE_MEMORY.md` capturing session context and learnings

## Related Repos

| Repo | Relationship |
|------|-------------|
| [open-biosciences-plugins](https://github.com/open-biosciences/open-biosciences-plugins) | Claude plugin providing the research skills, commands, and MCP tool connections |
| [biosciences-mcp](https://github.com/open-biosciences/biosciences-mcp) | 12 FastMCP servers powering the 34+ life sciences API tools |
| [biosciences-program](https://github.com/open-biosciences/biosciences-program) | Platform coordination, ADRs, and specifications |

## License

MIT
