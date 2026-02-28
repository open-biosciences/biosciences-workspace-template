---
name: pipeline-lessons-learned
description: "Captures lessons learned after completing a Fuzzy-to-Fact pipeline run (Phases 1-7). Extracts actionable refinements from quality review findings, Synapse grounding gaps, evidence grading issues, and agent execution patterns. Produces a structured lessons document and optionally proposes upstream skill updates via PR. Use when the user says 'lessons learned', 'what did we learn', 'capture refinements', 'review pipeline results', or after completing a publication pipeline run."
---

# Pipeline Lessons Learned

Capture, structure, and propagate refinements from completed Fuzzy-to-Fact pipeline runs.

## When to Use

Run this skill after any of these events:
- A publication pipeline (Phase 7) completes
- A quality review identifies issues (PARTIAL or FAIL dimensions)
- Synapse grounding reveals platform coverage gaps
- Evidence grading produces unexpected or disputed scores
- A new disease domain or query pattern is explored for the first time

## Grounding Rule

```
Lessons MUST be grounded in specific pipeline artifacts — quality review findings,
Synapse grounding reports, evidence grading scores, or agent execution logs.
Do NOT capture speculative improvements. Every lesson must cite the artifact
and line/section where the issue was observed.
```

## Workflow

```
Pipeline Complete (Phases 1-7)
        |
        v
  +-----------+
  | EXTRACT   |  Read quality review, synapse grounding, report
  | findings  |  Identify issues by category
  +-----------+
        |
        v
  +-----------+
  | CLASSIFY  |  Sort into: Skill Update / Architecture / Process / Domain Knowledge
  | lessons   |  Assess severity: Critical / Important / Minor
  +-----------+
        |
        v
  +-----------+
  | PERSIST   |  Write lessons doc to output/{slug}/
  | locally   |  Persist to Graphiti (open-biosciences namespace)
  +-----------+
        |
        v
  +-----------+
  | PROPOSE   |  (Optional) Draft skill edits for upstream PR
  | upstream  |  Target: biosciences-skills repo
  +-----------+
```

## Lesson Categories

### 1. Skill Updates
Changes to SKILL.md files in biosciences-skills that would prevent the issue in future runs.

**Examples from Fabry disease run**:
- Graph-builder should persist aggregate API counts in graph.json metadata
- L2 Multi-DB threshold needs clarification (same-API annotations are not independent sources)
- Publication pipeline needs platform suitability check for Synapse grounding

### 2. Architecture Decisions
Patterns or conventions that should be documented as ADRs or added to CLAUDE.md files.

**Examples**:
- Multi-agent parallel execution topology (Stages 1a/1b parallel, 2-4 sequential)
- Workspace template as sandbox → upstream PR as contribution path

### 3. Process Improvements
Workflow changes that improve pipeline reliability or quality.

**Examples**:
- Always persist trial NCT IDs as graph nodes, not just inline text
- Use compound queries (gene+disease) before single-term queries

### 4. Domain Knowledge
Disease-specific or database-specific knowledge that aids future pipeline runs.

**Examples**:
- Synapse has Very Low coverage for lysosomal storage diseases
- Short gene symbols (GLA, GAA) require compound query strategy

## Extraction Checklist

For each completed pipeline, review these sources and extract lessons:

### From Quality Review (`{slug}-quality-review.md`)
- [ ] Any dimension scored PARTIAL or FAIL?
- [ ] Any Documentation Errors identified?
- [ ] Any Protocol Failures identified?
- [ ] Hallucination risk items — were they preventable by better data persistence?
- [ ] Evidence grading disputes — were L-levels assigned correctly?

### From Synapse Grounding (`{slug}-synapse-grounding.md`)
- [ ] Node coverage below 40%? If so, is it a platform limitation or search strategy issue?
- [ ] Edge coverage below 30%? Same question.
- [ ] Any false positives from ambiguous search terms?
- [ ] Were cross-disease analogies used? Should "Analogous" tier be applied?
- [ ] Are there better fallback sources for this disease domain?

### From Report (`{slug}-report.md`)
- [ ] Any claims without `[Source: tool(param)]` citations?
- [ ] Any aggregate counts (totals, percentages) that lack provenance?
- [ ] Were all trial NCT IDs preserved from the interactive session?
- [ ] Template selection — was the right template used for the CQ type?

### From Agent Execution
- [ ] Did parallel agents (Stage 1a/1b) complete without conflicts?
- [ ] Were there context window issues in any agent?
- [ ] Did any agent need skills that weren't available?
- [ ] Total wall-clock time — any stages unexpectedly slow?

## Output Format

Write the lessons document to: `output/{slug}/{slug}-lessons-learned.md`

```markdown
# Lessons Learned: {CQ Title}

**Pipeline run**: {date}
**Quality verdict**: {PASS/PARTIAL/FAIL}
**Scores**: Protocol {N}/10, Presentation {N}/10, KG {N}/10, Synapse {N}/10

## Skill Updates Needed

### {skill-name}: {brief description}
- **Source**: {quality-review.md, line N} or {synapse-grounding.md, section N}
- **Issue**: {what went wrong}
- **Fix**: {specific edit to SKILL.md}
- **Severity**: {Critical / Important / Minor}

## Architecture Decisions
{Any new patterns or conventions worth documenting}

## Process Improvements
{Workflow changes for future runs}

## Domain Knowledge
{Disease-specific or database-specific insights}

## Upstream PR Candidates
{List of changes ready to propose to biosciences-skills}

| Change | Target File | Priority |
|--------|------------|----------|
| {description} | {skill}/SKILL.md | {High/Medium/Low} |
```

## Persistence

After writing the local lessons document:

1. **Graphiti**: Persist to `open-biosciences` namespace (working memory for research context)
2. **Graphiti**: Persist skill-specific lessons to `open-biosciences-migration-2026` namespace (platform evolution tracking)
3. **Local memory**: Update `memory/publication-pipeline-lessons.md` if lessons are reusable across CQs

## Upstream Contribution Path

When lessons identify skill updates:

```
1. Local workspace: Create/edit skill in .claude/skills/ (experiment)
2. Validate: Run another pipeline to confirm the fix works
3. Propose: Create PR to biosciences-skills with the refined SKILL.md
4. Review: Quality & Skills Engineer reviews and merges
5. Propagate: Other workspace clones pick up the update on next pull
```

This mirrors standard open-source contribution: fork → experiment → PR → review → merge.

**Important**: Do NOT edit biosciences-skills directly from a workspace template clone. Always propose changes via PR so they go through quality review.

## See Also

- **biosciences-graph-builder**: The pipeline that produces the artifacts this skill reviews
- **biosciences-reporting-quality-review**: 10-dimension quality framework (primary input)
- **biosciences-publication-pipeline**: Publication pipeline stages and grounding methodology
- **biosciences-reporting**: Evidence grading system and template standards
