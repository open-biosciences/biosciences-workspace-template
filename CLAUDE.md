# CLAUDE.md — biosciences-workspace-template

## Purpose

Canonical workspace configuration, bootstrap scripts, and shared tooling for the Open Biosciences platform. This repo is co-owned by the **Education & Workspace Engineer** agent.

## Responsibilities

- Maintain the canonical `.code-workspace` template
- Provide bootstrap scripts for new contributor setup
- Distribute shared configuration (ruff, pyright, pre-commit)
- Repo initialization templates for new `biosciences-*` repos

## Directory Structure

```
biosciences-workspace-template/
├── scripts/
│   ├── bootstrap.sh          # One-command workspace setup
│   ├── verify-env.sh         # Check all required env vars
│   └── init-repo.sh          # Initialize a new biosciences-* repo
├── templates/
│   ├── pyproject.toml        # Standard pyproject.toml template
│   ├── ruff.toml             # Shared ruff configuration
│   ├── pyrightconfig.json    # Shared pyright configuration
│   └── CLAUDE.md.template    # Template for repo-level CLAUDE.md
└── configs/
    └── open-biosciences.code-workspace  # Canonical workspace file
```

## Bootstrap Process

New contributors run:
```bash
cd open-biosciences
./biosciences-workspace-template/scripts/bootstrap.sh
```

This script:
1. Checks Python >=3.11 is available
2. Installs `uv` if missing
3. Runs `uv sync` in each repo that has a `pyproject.toml`
4. Copies `.env.example` files where `.env` is missing
5. Verifies required environment variables

## Shared Conventions

All new repos should be initialized with:
- `pyproject.toml` using hatchling build backend
- ruff configuration matching the shared template
- pyright in basic mode
- pytest markers: `unit`, `integration`, `e2e`

## Dependencies

- **Upstream**: `biosciences-architecture` (convention compliance)
- **Downstream**: All repos (consume templates and configs)

## Canonical MCP Configuration

### .mcp.json Template

All workspace repos that need MCP access should use this canonical MCP server configuration.
The `biosciences-mcp` entry connects to the FastMCP Cloud gateway with 40+ life sciences API tools.
The graph/memory entries connect to local MCP servers that must be running on localhost.

```json
{
  "mcpServers": {
    "biosciences-mcp": {
      "type": "http",
      "url": "https://biosciences-mcp.fastmcp.app/mcp",
      "headers": {
        "Authorization": "Bearer ${BIOSCIENCES_API_KEY}"
      }
    },
    "graphiti-aura": {
      "type": "stdio",
      "command": "/home/donbr/graphiti-fastmcp/scripts/run_mcp_server.sh",
      "args": []
    },
    "neo4j-aura-management": {
      "type": "http",
      "url": "http://localhost:8004/mcp/"
    },
    "neo4j-aura-cypher": {
      "type": "http",
      "url": "http://localhost:8003/mcp/"
    },
    "graphiti-docker": {
      "type": "http",
      "url": "http://localhost:8002/mcp/"
    },
    "neo4j-docker-cypher": {
      "type": "http",
      "url": "http://localhost:8005/mcp/"
    }
  }
}
```

### Server Reference

| Server | Transport | Endpoint | Purpose |
|--------|-----------|----------|---------|
| `biosciences-mcp` | HTTP (cloud) | `https://biosciences-mcp.fastmcp.app/mcp` | 40+ life sciences API tools (HGNC, UniProt, ChEMBL, Open Targets, STRING, etc.) |
| `graphiti-aura` | stdio | `/home/donbr/graphiti-fastmcp/scripts/run_mcp_server.sh` | Graphiti FastMCP for Neo4j Aura (read-only — write-freeze in effect) |
| `neo4j-aura-management` | HTTP (local) | `:8004` | Neo4j Aura instance management |
| `neo4j-aura-cypher` | HTTP (local) | `:8003` | Direct Cypher queries on Aura |
| `graphiti-docker` | HTTP (local) | `:8002` | Graphiti FastMCP for local Docker Neo4j (primary write target) |
| `neo4j-docker-cypher` | HTTP (local) | `:8005` | Direct Cypher queries on local Docker |

### Current State: Duplication Audit

As of 2026-02-26, **3 repos** have `.mcp.json` files with identical content:

| File | Contains biosciences-mcp? |
|------|--------------------------|
| `/home/donbr/open-biosciences/.mcp.json` | No |
| `/home/donbr/open-biosciences/biosciences-memory/.mcp.json` | No |
| `/home/donbr/open-biosciences/biosciences-program/.mcp.json` | No |

All three files are byte-for-byte identical (same 5 graph/memory servers, no `biosciences-mcp` entry).
The remaining 8 repos (`biosciences-mcp`, `biosciences-research`, `biosciences-deepagents`,
`biosciences-temporal`, `biosciences-architecture`, `biosciences-skills`, `biosciences-evaluation`,
`biosciences-workspace-template`, `biosciences-education`) have no `.mcp.json` at all.

**Key gaps identified:**
1. `biosciences-mcp` is not present in any `.mcp.json` — agents cannot reach the FastMCP Cloud gateway
2. Identical file content across 3 repos creates a maintenance burden — any change requires 3 manual edits
3. 8 repos lack MCP access entirely, including `biosciences-deepagents` and `biosciences-temporal` which depend on MCP tools

### Bootstrap Script Pattern

A `scripts/bootstrap.sh` should handle canonical `.mcp.json` distribution. Planned behavior:

1. Copy the canonical `.mcp.json` from this template to each repo that needs MCP access
2. Run `uv sync --extra dev` in each repo that has a `pyproject.toml`
3. Verify key environment variables are set

Repos that need the full `.mcp.json` (all servers):

```
biosciences-memory
biosciences-research
biosciences-deepagents
biosciences-temporal
biosciences-program
```

Repos that need only `biosciences-mcp` (cloud gateway only, no local graph servers):

```
biosciences-evaluation
biosciences-skills
biosciences-architecture
```

Environment variables to verify in `scripts/verify-env.sh`:

```bash
# LLM
OPENAI_API_KEY

# Life sciences APIs
BIOGRID_API_KEY
NCBI_API_KEY

# Neo4j Aura
NEO4J_URI
NEO4J_USERNAME
NEO4J_PASSWORD
NEO4J_AURA_CLIENT_ID
NEO4J_AURA_CLIENT_SECRET
```

### Claude Code Multi-Agent (CoWork) Pattern

When running Claude Code in multi-agent workspace mode:

- Each agent instance reads `.mcp.json` from its **working directory** (the repo it is opened in)
- The `biosciences-mcp` HTTP transport is **stateless** — safe for concurrent access by multiple agents
- The local graph servers (`graphiti-docker`, `neo4j-docker-cypher`, etc.) are also stateless HTTP — safe for concurrent reads; write ordering is handled by the Graphiti queue service
- Authentication API keys must be set as **environment variables**, never embedded in `.mcp.json`
- The workspace-root `.mcp.json` (`/home/donbr/open-biosciences/.mcp.json`) applies when Claude Code is opened at the workspace level rather than in a specific repo

### Aura Write-Freeze Note

`graphiti-aura` is in a write-freeze state (free-tier 1 GB capacity reached). The `graphiti-aura`
entry in `.mcp.json` is retained for **reads only**. All new graph writes must target `graphiti-docker`.
Do not remove the `graphiti-aura` entry — reads (search, query, analytics) remain functional.
