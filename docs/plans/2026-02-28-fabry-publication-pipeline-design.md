# Design: Fabry Disease Gb3 Accumulation — Multi-Agent Publication Pipeline

**Date**: 2026-02-28
**Status**: Approved
**Slug**: `fabry-gb3-accumulation`

## Context

The Fuzzy-to-Fact graph builder has completed Phases 1-5 for the Fabry disease Gb3 accumulation query. Three artifacts exist:

- `report.md` — Raw structured report (7 KB)
- `graph.json` — Machine-readable subgraph with 11 nodes, 12 edges (6 KB)
- `knowledge-graph.html` — Interactive Cytoscape.js visualization (26 KB)

The graph was also persisted to Graphiti Docker (`group_id: open-biosciences`).

## Goal

Transform the raw pipeline output into 5 publication-quality files using a multi-agent team, following the `biosciences-publication-pipeline` skill's 4-stage workflow.

## Pipeline Parameters

| Parameter | Value |
|-----------|-------|
| `slug` | `fabry-gb3-accumulation` |
| `output_dir` | `output/fabry-disease-gb3-accumulation/` |
| `cq_text` | "Identify the specific glycolipid substrate that accumulates in Fabry Disease, and name the cellular organelle where this pathological accumulation primarily takes place due to the absence of functional alpha-galactosidase A." |

## Agent Team

### Agent 1: Report Formatter (Stage 1a)

- **Skill**: `biosciences-reporting`
- **Template**: Template 6 (Mechanism Elucidation) — the query asks "by what mechanism does GLA absence cause Gb3 accumulation"
- **Input**: `graph.json` + `report.md` (raw)
- **Output**: `fabry-gb3-accumulation-report.md` + `fabry-gb3-accumulation-knowledge-graph.json`
- **Key tasks**:
  - Select Template 6 via decision tree
  - Apply L1-L4 evidence grading with modifiers
  - Add `[Source: tool(param)]` citations to every claim
  - Produce publication KG JSON with `synapse_grounding: []` placeholders

### Agent 2: Synapse Grounder (Stage 1b)

- **Skill**: `biosciences-publication-pipeline` Stage 1b
- **Input**: `graph.json`
- **Output**: `fabry-gb3-accumulation-synapse-grounding.md` + updated KG JSON
- **Key tasks**:
  - Search Synapse for Fabry disease, GLA, Gb3, lysosomal storage datasets
  - Classify grounding strength (Strong/Moderate/Weak)
  - Inject `synapse_grounding` arrays into KG JSON nodes/edges
  - Calculate edge grounding coverage (excluding MEMBER_OF edges)

### Agent 3: Quality Reviewer (Stage 2)

- **Skill**: `biosciences-reporting-quality-review`
- **Input**: All Stage 1 outputs (report, KG JSON, synapse grounding)
- **Output**: `fabry-gb3-accumulation-quality-review.md`
- **Key tasks**:
  - Apply 10-dimension evaluation framework
  - Classify failures as Protocol/Presentation/Documentation
  - Score protocol execution, presentation, KG structure, Synapse grounding (each 0-10)
  - Produce PASS/PARTIAL/FAIL verdict

### Agent 4: BioRxiv Drafter (Stage 3)

- **Skill**: `biosciences-publication-pipeline` Stage 3
- **Input**: All previous outputs
- **Output**: `fabry-gb3-accumulation-biorxiv-draft.md`
- **Key tasks**:
  - IMRaD format (Abstract, Introduction, Methods, Results, Discussion)
  - Convert `[Source: tool(param)]` to numbered references
  - Include supplementary tables (S1-S4)
  - Target 3500-5500 total words

### Agent 5: Verifier (Stage 4)

- **Type**: Automated checks (no skill needed)
- **Input**: All 5 output files
- **Output**: Console verification report
- **Checks**:
  - File existence (5 files)
  - JSON validity
  - Cross-file consistency (node counts, edge counts, disease CURIE)
  - No API keys in output
  - Quality review verdict is PASS or PARTIAL

## Execution Order

```
Phase 1 (parallel):  Agent 1 + Agent 2
Phase 2 (sequential): Agent 3 (after both Phase 1 agents complete)
Phase 3 (sequential): Agent 4 (after Agent 3 completes)
Phase 4 (sequential): Agent 5 (after Agent 4 completes)
```

## Output Files

| # | File | Agent |
|---|------|-------|
| 1 | `fabry-gb3-accumulation-report.md` | Report Formatter |
| 2 | `fabry-gb3-accumulation-knowledge-graph.json` | Report Formatter + Synapse Grounder |
| 3 | `fabry-gb3-accumulation-synapse-grounding.md` | Synapse Grounder |
| 4 | `fabry-gb3-accumulation-quality-review.md` | Quality Reviewer |
| 5 | `fabry-gb3-accumulation-biorxiv-draft.md` | BioRxiv Drafter |

## Decisions

- **Template choice**: Template 6 (Mechanism Elucidation) because the CQ asks about mechanism of accumulation
- **Synapse included**: Yes, full pipeline including Synapse grounding
- **Agent type**: `general-purpose` subagents with skill invocation in prompts
- **Parallel strategy**: Stages 1a/1b run concurrently; Stages 2-4 are sequential dependencies
