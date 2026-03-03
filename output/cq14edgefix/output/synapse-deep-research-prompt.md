# Deep Research Prompt: Prioritizing Synapse Integration Improvements for the Fuzzy-to-Fact Pipeline

> **Instructions**: Use this prompt with a deep research agent or extended thinking session. Do NOT execute it now — it is designed for a future focused research sprint.

---

## Context

The Fuzzy-to-Fact protocol is an automated knowledge graph construction pipeline for life sciences research. It federates queries across 8+ biomedical databases (HGNC, NCBI Entrez, Ensembl, UniProt, STRING, BioGRID ORCS, Open Targets, ClinicalTrials.gov) to build knowledge graphs that answer structured competency questions — for example, identifying druggable synthetic lethal partners for TP53-mutant cancers.

Synapse.org is currently integrated as a **post-hoc grounding layer** invoked during the publication stage (Stage 1b), after the knowledge graph is already complete. Four MCP tools are available: `search_synapse`, `get_entity`, `get_entity_annotations`, `get_entity_children`. A fifth tool, `get_entity_provenance`, is also available but unused.

In our most recent pipeline run (CQ14: TP53 synthetic lethality), Synapse grounding achieved **21% node coverage and 16% edge coverage** across 5 datasets, with all grounding classified as Moderate or weaker. No evidence level upgrades resulted. The integration functions but does not materially strengthen pipeline conclusions.

An internal analysis identified **seven candidate improvements** (summarized below). The goal of this research is to evaluate feasibility, estimate impact, identify dependencies, and produce a prioritized implementation roadmap.

---

## The Seven Candidate Improvements

| # | Improvement | Core Idea |
|---|------------|-----------|
| 1 | **Promote Synapse to Phase 3 EXPAND** | Query Synapse during graph construction, not just publication — let it contribute new entities and edges |
| 2 | **Add data-level query capabilities** | Query within Synapse Tables and download files for local analysis, not just read metadata |
| 3 | **Implement Synapse evidence modifiers** | Add +0.05/+0.10 grading modifiers for Moderate/Strong Synapse grounding so it actually affects evidence scores |
| 4 | **Use annotations for structured matching** | Replace keyword search with programmatic annotation-based filtering (species, assay type, disease) |
| 5 | **Bridge to GEO/DepMap** | Add GEO dataset search as a parallel grounding source for domains where Synapse coverage is thin |
| 6 | **Deep project traversal** | Use `get_entity_children` to walk project trees and find specific sub-files that ground specific edges |
| 7 | **Provenance-aware quality scoring** | Use `get_entity_provenance` to assess dataset methodology alignment and quality |

---

## Research Questions

For each of the seven improvements, investigate and answer:

### A. Feasibility Assessment

1. **Technical feasibility**: What would implementing this require? Is it a skill-file change (prompt engineering), an MCP server enhancement (new tools), a new MCP server entirely, or an architectural change to the pipeline phases?

2. **Synapse API capabilities**: Does the Synapse REST API or Python client (`synapseclient`) already support the required operations? Specifically:
   - Can Synapse Tables be queried with SQL-like syntax via API? What are the rate limits and row limits?
   - Can individual files within a Synapse project be downloaded programmatically without bulk download?
   - What structured annotation fields are consistently populated across cancer genomics projects? Is there a controlled vocabulary for `assayType`, `disease`, `species`?
   - How reliable and complete is the provenance graph across typical Synapse projects? What percentage of entities have registered provenance?

3. **MCP server gap analysis**: The current Synapse MCP server exposes 5 tools. For each improvement, what new MCP tools would need to be added? Could some improvements be achieved by better prompting of existing tools?

### B. Impact Estimation

4. **Coverage improvement**: For each improvement, estimate the expected change in node coverage and edge coverage, using CQ14 as the reference case (baseline: 21% nodes, 16% edges). Also estimate impact on a hypothetical neurodegenerative CQ (where Synapse coverage is High) and a cardiovascular CQ (where coverage is Low).

5. **Evidence score impact**: How many claims in CQ14 would have their evidence level change if the improvement were implemented? Would any L2 claims upgrade to L3? Would any L1 claims upgrade to L2?

6. **Grounding strength shift**: What percentage of "Moderate" groundings could be promoted to "Strong" if data-level access were available? Estimate this by examining the five datasets found in CQ14 and assessing whether their contents would directly confirm or refute specific KG claims.

### C. Dependencies and Ordering

7. **Prerequisites**: Which improvements depend on other improvements being completed first? Draw a dependency graph. For example: "Deep project traversal (#6) is useful on its own for metadata, but reaches full potential only with data-level access (#2)."

8. **Standalone value**: Which improvements deliver value independently, without any other improvement? Which are force multipliers that amplify the value of others?

9. **Risk assessment**: Which improvements carry risk of degrading pipeline performance (e.g., adding noise, increasing token consumption, introducing false-positive grounding)? What guardrails would be needed?

### D. Comparative Analysis

10. **Synapse vs. GEO vs. DepMap**: For cancer genomics competency questions specifically, compare the dataset density, metadata quality, and API accessibility of:
    - Synapse.org (current)
    - NCBI GEO (Gene Expression Omnibus) — via E-utilities API
    - DepMap (Broad Institute) — via DepMap portal API or data downloads
    - cBioPortal — via web API

    Which platform would provide the highest grounding coverage per engineering hour invested? Is the optimal strategy "deepen Synapse" or "add GEO as a second grounding source"?

11. **Synapse's comparative advantage**: Where does Synapse genuinely outperform GEO and DepMap? Is it the provenance tracking, the structured annotations, the collaborative project model, the FAIR data principles, or something else? This determines whether Synapse should be the *primary* grounding source or one of several.

12. **Domain-specific ROI**: For which disease domains (neurodegenerative, cancer, rare disease, cardiovascular) would each improvement deliver the most value? Create a matrix of improvement × domain with High/Medium/Low impact ratings.

### E. Implementation Roadmap

13. **Effort estimation**: For each improvement, estimate implementation effort in three categories:
    - **Skill-only** (prompt/template changes, no code): hours
    - **MCP tool addition** (new tools on existing server): days
    - **New MCP server or architectural change**: weeks

14. **Suggested phases**: Propose a 3-phase implementation roadmap:
    - **Phase A (Quick wins)**: Improvements achievable with skill-file changes and better use of existing tools. Target: 1–2 days.
    - **Phase B (MCP enhancements)**: Improvements requiring new MCP tools or server modifications. Target: 1–2 weeks.
    - **Phase C (Architectural)**: Improvements requiring changes to the Fuzzy-to-Fact pipeline phase structure. Target: 1 month.

15. **Validation plan**: For each phase, define how success would be measured. What CQ would serve as the benchmark? What coverage and evidence score thresholds would constitute a meaningful improvement over the CQ14 baseline?

### F. Open Questions

16. **Synapse community datasets**: Are there Synapse community projects (e.g., GENIE, Project HOPE, HTAN) that are particularly rich in structured, queryable data for cancer genomics? Could the pipeline maintain a curated registry of "gold standard" Synapse projects per disease domain, bypassing the noisy keyword search entirely?

17. **Synapse fileview and dataset entities**: Synapse supports "fileviews" (virtual tables that aggregate file metadata across a project) and "dataset" entities (curated collections). Could these be leveraged for more structured grounding than raw keyword search?

18. **Token budget impact**: If data-level access is added and the pipeline starts downloading and parsing Synapse files, how does this affect the token budget? Would a subagent architecture (delegating Synapse analysis to a separate agent with its own context window) be needed?

19. **Bidirectional value**: Could the pipeline *contribute back* to Synapse? For example, depositing the KG JSON, grounding report, and BioRxiv draft as a Synapse project — creating a feedback loop where pipeline outputs become grounding sources for future runs?

---

## Expected Output Format

Produce a structured document with:

1. **Executive summary** (1 page): Top 3 recommendations with rationale
2. **Feasibility matrix** (table): Each improvement rated on technical feasibility, effort, and risk
3. **Impact matrix** (table): Each improvement × disease domain, with estimated coverage and evidence score changes
4. **Dependency graph** (visual or textual): Which improvements unlock which others
5. **Prioritized roadmap** (3 phases with timelines and success criteria)
6. **Appendix**: Raw research findings for each of the 19 questions above

---

## Research Sources to Consult

- **Synapse REST API documentation**: https://rest-docs.synapse.org/rest/
- **Synapse Python client docs**: https://python-docs.synapse.org/
- **Synapse Table query syntax**: Documentation for SQL-like queries on Synapse Tables
- **NCBI GEO E-utilities**: https://www.ncbi.nlm.nih.gov/geo/info/geo_paccess.html
- **DepMap portal API**: https://depmap.org/portal/
- **cBioPortal web API**: https://www.cbioportal.org/api
- **Synapse community projects**: GENIE (syn7222066), HTAN (syn20979095), DREAM Challenges
- **Current pipeline skills**: `biosciences-publication-pipeline/SKILL.md`, `biosciences-graph-builder/SKILL.md`
- **CQ14 outputs**: `tp53-synthetic-lethality-kg.json`, `tp53-synthetic-lethality-synapse-grounding.md`
- **Internal analysis**: `synapse-integration-analysis.md`
