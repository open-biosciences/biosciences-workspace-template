# CLAUDE.md — biosciences-workspace-template

## Purpose

Claude CoWork workspace template for capturing versioned life sciences research pipeline outputs. This repo is owned by the **Education & Workspace Engineer** agent (#9).

This is NOT a bootstrap script collection or environment setup tool. It is solely a CoWork "work in a folder" target where each research session produces structured, git-tracked outputs under `output/{slug}/`.

## How It Works

The **open-biosciences-plugins** (marketplace repo) provides research skills, commands, and MCP tool connections. This workspace template is where the outputs land.

**Usage pattern:** install plugin → clone repo → set as CoWork folder → run research commands → commit outputs

## Research Commands (from plugin)

| Command | Purpose |
|---------|---------|
| `/ob-research` | Run a full research pipeline on a topic |
| `/ob-report` | Generate a structured research report |
| `/ob-review` | Run a 10-dimension quality review |
| `/ob-publish` | Produce a publication-quality BioRxiv draft |
| `/ob-cq-discover` | Discover competency questions for a domain |
| `/ob-cq-run` | Execute a competency question pipeline |

## Output Artifacts

Each completed session in `output/{slug}/` typically produces:

| Artifact | Pattern | Description |
|----------|---------|-------------|
| Research report | `*-report.md` | Structured report with entities, edges, evidence grading |
| Knowledge graph | `*-knowledge-graph.json` | Machine-readable knowledge subgraph |
| Quality review | `*-quality-review.md` | 10-dimension quality assessment |
| Synapse grounding | `*-synapse-grounding.md` | Synapse dataset grounding check |
| BioRxiv draft | `*-biorxiv-draft.md` | Publication-quality manuscript draft |

## Dependencies

- **Upstream**: `open-biosciences-plugins` (provides skills, commands, MCP connections)
- **Upstream**: `biosciences-mcp` (34+ life sciences API tools via gateway)
- **Downstream**: None (this is a leaf repo — output capture only)

## MCP Connectivity

This repo does not have its own `.mcp.json`. MCP tool access comes from the **open-biosciences-plugins** plugin, which connects to the `biosciences-mcp` cloud gateway (34+ life sciences API tools) and optionally to local graph servers.

When used as a CoWork working folder, the plugin's MCP connections are available automatically.

### Aura Write-Freeze Note

`graphiti-aura` is in a write-freeze state (free-tier 1 GB capacity reached). All new graph writes must target `graphiti-docker`. Reads from Aura remain functional.
