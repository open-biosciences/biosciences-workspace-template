# Plugin + Workspace Template Integration Design

**Date**: 2026-03-03
**Status**: Approved

## Problem

The `biosciences-workspace-template` README describes planned bootstrap scripts (Wave 4) that don't exist. The repo's actual purpose is as a Claude CoWork "work in a folder" target that captures versioned research pipeline outputs. The relationship between `open-biosciences-plugins` (the plugin) and this repo (the workspace) is undocumented.

## Design

### 1. README Rewrite (biosciences-workspace-template)

CoWork-only focus. No bootstrap script content.

**Sections:**
- What This Is — CoWork workspace template for capturing research sessions
- Quick Start — (1) Install plugin, (2) Clone repo, (3) Set as CoWork folder
- How It Works — Plugin provides skills/MCP; workspace captures outputs
- Example Workspaces — Table of 5 existing workspaces with topics/dates
- What Each Workspace Contains — Standard artifacts description
- Related Repos — Links to plugin and platform repos
- License — MIT

### 2. Graphiti Working Namespace Updates

Write to `open-biosciences-migration-2026`:
- Relationship: plugins provides skills/tools, workspace-template consumes them
- Usage pattern: install → clone → set folder → research → commit
- Role clarification: workspace-template = CoWork session capture repo
- 5 completed pipeline runs documented

### 3. Plugin Review

Validate plugin structure against official Claude plugin docs. No code changes.

## Implementation

Three parallel agents:
1. Plugin Reviewer — review plugin structure, produce summary
2. README Rewriter — rewrite README.md
3. Graphiti Updater — write facts to working namespace
