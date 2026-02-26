# biosciences-workspace-template

Environment bootstrap and workspace configuration templates for the Open Biosciences platform.
This is the starting point for anyone setting up the full 11-repo workspace from scratch.

Part of the [Open Biosciences](https://github.com/open-biosciences) multi-repo platform.

## Status

**Wave 4 — Not Started.** This repo contains no scripts or templates yet. Content will be
created in Wave 4 after the core platform migration (Waves 1–3) is complete. Stable repo
structures and API key conventions must be established before templates can be finalized.

Wave 4 covers: biosciences-evaluation, biosciences-research, biosciences-education,
biosciences-workspace-template.

## What Will Be Here

### Bootstrap Scripts

Automated setup for the full Open Biosciences workspace (`scripts/`):

- **`bootstrap.sh`** — One-command workspace setup: clone all 11 repos under the
  `open-biosciences` org, configure the VS Code workspace file, run `uv sync` in each
  repo that has a `pyproject.toml`, copy `.env.example` files where `.env` is missing
- **`verify-env.sh`** — Check that all required environment variables are set before
  running any platform component
- **`init-repo.sh`** — Initialize a new `biosciences-*` repo from the standard template

### VS Code Workspace Configuration Templates

- Canonical `open-biosciences.code-workspace` file with all 11 repos as workspace folders
- Shared Python interpreter settings (Python >=3.11, `uv`-managed environments)
- Extension recommendations for the team (Python, Ruff, Pyright, GitLens, etc.)

### Environment Setup Templates

`.env.example` patterns for each repo that requires API keys:

| Repo | Required Variables |
|------|--------------------|
| biosciences-mcp | `BIOGRID_API_KEY`, `NCBI_API_KEY`, `OPENAI_API_KEY` |
| biosciences-deepagents | `NEO4J_URI`, `NEO4J_USERNAME`, `NEO4J_PASSWORD`, `LANGSMITH_API_KEY`, `ANTHROPIC_API_KEY`, `OPENAI_API_KEY` |
| biosciences-memory | `NEO4J_URI`, `NEO4J_USERNAME`, `NEO4J_PASSWORD`, `NEO4J_AURA_CLIENT_ID`, `NEO4J_AURA_CLIENT_SECRET` |
| biosciences-temporal | `OPENAI_API_KEY`, `TEMPORAL_HOST`, `NEO4J_URI`, `NEO4J_USERNAME`, `NEO4J_PASSWORD` |

### Workspace Onboarding Automation

Scripts that verify the workspace is correctly set up end-to-end:
check Python version, `uv` availability, `.env` files, MCP server connectivity, and
Docker services (Neo4j, Temporal).

## Target User

A developer or researcher joining the project for the first time, or setting up the
workspace on a new machine. Running `bootstrap.sh` should produce a fully functional
local environment in a single step.

## Relationship to biosciences-education

| Repo | Scope |
|------|-------|
| [biosciences-workspace-template](https://github.com/open-biosciences/biosciences-workspace-template) | Environment setup — cloning, dependencies, API keys, tooling |
| [biosciences-education](https://github.com/open-biosciences/biosciences-education) | Learning the platform — tutorials, onboarding guides, usage documentation |

Both repos are owned by the same agent (Education & Workspace Engineer, Agent 9) and are
designed to be used together: template gets the environment running; education teaches
what to do with it.

## Owner

**Education & Workspace Engineer** (Agent 9) — also owns
[biosciences-education](https://github.com/open-biosciences/biosciences-education).

## Dependencies

| Dependency | Relationship |
|-----------|-------------|
| [biosciences-architecture](https://github.com/open-biosciences/biosciences-architecture) | Convention compliance — templates encode ADR standards (Python >=3.11, uv, ruff, hatchling, pytest markers) |

## Related Repos

- [biosciences-education](https://github.com/open-biosciences/biosciences-education) — Training materials and onboarding (same owner)
- [biosciences-architecture](https://github.com/open-biosciences/biosciences-architecture) — ADRs and conventions that templates encode
- [biosciences-program](https://github.com/open-biosciences/biosciences-program) — Migration coordination and agent team definitions

## License

MIT
