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
