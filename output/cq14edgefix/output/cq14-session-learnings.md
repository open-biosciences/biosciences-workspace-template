# CQ14 Session Learnings: TP53 Synthetic Lethality Pipeline

**Date**: 2026-03-03
**Scope**: Full Fuzzy-to-Fact pipeline execution (Phases 1-6a) + Publication Pipeline (Stages 1a-4) + post-pipeline analysis
**Sessions**: 3 (context exhaustion required continuation twice)

---

## 1. Protocol-Level Learnings

### 1a. Context Window Is the Binding Constraint

The single most impactful limitation was context window exhaustion. CQ14 required **three separate sessions** to complete the full pipeline (Phases 1-6a → Publication Stages 1-4 → Analysis & Prompts). This has direct implications for protocol design:

- **Observation**: Each phase accumulates tool call results, and by Phase 5 (TRAVERSE_DRUGS), the context was already under pressure. The publication pipeline stages then consumed the remaining budget.
- **Implication**: The protocol needs a **checkpoint/resume mechanism** — the ability to serialize intermediate state (resolved entities, partial KG) so a new session can pick up without re-executing earlier phases.
- **Workaround used**: The session continuation summary served as a manual checkpoint. It worked but lost fine-grained detail (exact tool call parameters, intermediate candidates that were rejected).

### 1b. Token Budgeting Works but Needs Discipline

The `slim=true` parameter during LOCATE phases genuinely saved context space (~20 tokens vs ~115-300 tokens per entity). However:

- **Pitfall observed**: It's tempting to use `slim=false` early "just to see" — this burns context budget unnecessarily.
- **Rule reinforced**: LOCATE always slim, RETRIEVE only for entities that survived filtering. No exceptions.
- **Enhancement idea**: A token budget tracker that estimates remaining capacity and warns before a phase that might exhaust it.

### 1c. Incomplete Drug Traversal Is a Real Risk

During Phase 5 (TRAVERSE_DRUGS), drug lookups were performed for WEE1 (adavosertib), CHEK1 (prexasertib, SRA737), and PLK1 (volasertib) — but **ATR and POLQ were skipped**. This wasn't a protocol failure; it was a prioritization decision under context pressure. But:

- **Impact**: The KG is incomplete for two of the five synthetic lethal partners. A reviewer would flag this.
- **Root cause**: No protocol mechanism to enforce "traverse ALL targets" vs. "traverse representative targets."
- **Recommendation**: The graph-builder skill should include an explicit completeness check before moving to TRAVERSE_TRIALS — "Have all anchor-adjacent targets been drug-traversed? If not, list skipped targets and justify."

---

## 2. Tool-Level Learnings

### 2a. HGNC Fuzzy Search Fails on Short Gene Symbols

HGNC search returned errors or irrelevant results for **ATR** (3 characters) and **POLQ** (4 characters). The minimum query length and fuzzy matching algorithm struggle with short, common letter sequences.

- **Workaround**: Use Ensembl or Entrez search instead — these handle short symbols better.
- **Protocol fix needed**: The graph-builder skill should include a fallback chain: HGNC → Ensembl → Entrez for gene resolution. Currently it starts with HGNC and may stall.

### 2b. ChEMBL Mechanism Tool Is Underutilized

The `get_mechanism` tool on biosciences-mcp-edge returns mechanism-of-action data (target, action type, mechanism description) that is highly relevant for evidence grading. In CQ14, it was used for adavosertib but not systematically for all compounds.

- **Missed opportunity**: Mechanism data would have strengthened evidence grading for prexasertib and volasertib.
- **Recommendation**: Make `get_mechanism` a mandatory step in TRAVERSE_DRUGS for every compound that has a ChEMBL ID.

### 2c. BioGRID ORCS Returns Noisy Results Without Filtering

The CRISPR essentiality query for TP53 returned screens across many cell lines and technologies. Without `hit_only=true`, the results are overwhelming.

- **Best practice confirmed**: Always use `hit_only=true` for initial queries. Only expand to `hit_only=false` if the hit set is too small or you need negative controls.
- **Enhancement idea**: A cell-line filter parameter would be valuable — e.g., "only show screens in pancreatic cancer cell lines" for disease-specific essentiality.

### 2d. Synapse Search Is Keyword-Based, Not Semantic

Synapse's `search_synapse` tool uses keyword matching, not semantic similarity. Searching for "TP53 synthetic lethality" misses datasets that discuss "p53 checkpoint deficiency" or "tumor suppressor loss."

- **Impact**: Node grounding coverage (21%) is likely an undercount of what's actually available on Synapse.
- **Workaround**: Run multiple synonym-based searches (gene symbol, protein name, pathway name) for each entity.
- **Enhancement**: A semantic search layer over Synapse, or at minimum, an automated synonym expansion step in the grounding phase.

---

## 3. Evidence Grading Learnings

### 3a. Mechanism Mismatch Detection Is the Highest-Value Feature

The evidence grading correctly flagged **MDM2 inhibitors (idasanutlin, milademetan) as contraindicated** for TP53-mutant cancers because they require wild-type TP53 to function. The `-0.20 mechanism mismatch` modifier dropped these from L3 to L1, preventing a dangerous false positive.

- **This is the single most important safety feature of the protocol.** A naive literature search would surface MDM2 inhibitors as "TP53-related drugs" without flagging the directional incompatibility.
- **Recommendation**: Highlight this capability prominently in any value proposition documentation. It's a concrete, clinically meaningful example of why database-grounded reasoning matters.

### 3b. Evidence Modifiers Stack Meaningfully

The modifier system (`+0.10` mechanism match, `+0.05` literature, `+0.10` active trials, `-0.20` mechanism mismatch) produced a plausible distribution across 11 claims:

- **Highest**: Adavosertib (WEE1 inhibitor) at 0.95 L4 — strong mechanism, CRISPR validation, active trials, literature support
- **Lowest**: Idasanutlin at 0.35 L1 — mechanism mismatch penalty dominated
- **Spread**: The 0.35-0.95 range with clear clustering around L2-L3 suggests the modifiers have appropriate granularity.
- **Open question**: Should there be a modifier for "compound in Phase 3 vs. Phase 1" trial stage? Currently all active trials get the same `+0.10`.

### 3c. L4 Requires Multiple Converging Evidence Types

The only claim to reach L4 (adavosertib + TP53-mutant cancers) had ALL of: genetic interaction data from BioGRID ORCS, known mechanism of action from ChEMBL, active clinical trials from ClinicalTrials.gov, and published literature support. This convergence pattern is a useful heuristic for triaging which claims deserve the most attention.

---

## 4. Publication Pipeline Learnings

### 4a. The Quality Review Caught Real Issues

The 10-dimension quality review (7 PASS, 3 PARTIAL) wasn't just a formality:

- **PARTIAL on Completeness**: Correctly flagged ATR/POLQ drug traversal gap.
- **PARTIAL on Synapse Grounding**: Correctly noted 21% coverage is below ideal.
- **PARTIAL on one other dimension**: Caught compound CURIE standardization issue.

This confirms the quality review adds genuine value as a pre-publication gate.

### 4b. BioRxiv Draft Generation Works but Is Formulaic

The IMRaD manuscript was structurally correct and contained no hallucinated claims (all traced to tool calls). However, the writing style is recognizably AI-generated:

- **Strength**: Every claim has a `[Source: tool(param)]` citation — this is more rigorous than many human-written preprints.
- **Weakness**: The prose is mechanical. A human co-author would improve readability significantly.
- **Recommendation**: Position the draft as a **structured first draft** that captures all evidence, not a submission-ready manuscript. The value is in the evidence assembly, not the writing.

### 4c. Verification Checklist Is Essential

The 8-point automated verification (file existence, JSON validity, cross-file consistency for nodes/edges/NCT IDs/disease CURIEs, secrets scanning) caught no errors in CQ14 — but the *existence of the checklist* is what matters. It provides a reproducible QA gate.

- **Enhancement**: Add a check for CURIE format consistency (all compounds should use CHEMBL:NNNNN, not COMPOUND:NAME).
- **Enhancement**: Add a check for orphaned nodes (nodes with no edges).

---

## 5. Workflow and Process Learnings

### 5a. Session Continuation Summaries Are Lossy

When context was exhausted and the session continued, the summary captured high-level state but lost:

- Exact tool call parameters and intermediate results
- Rejected entity candidates (useful for understanding why certain paths weren't taken)
- Token budget estimates at each phase boundary

**Recommendation**: The protocol should write a **machine-readable checkpoint file** at the end of each phase — not just for session recovery, but for audit trail purposes.

### 5b. The Scratchpad/Workspace Separation Is Useful

Keeping intermediate analysis (chronology, Synapse analysis, research prompts) in the scratchpad while final deliverables (report, KG, manuscript) lived in the workspace was a clean separation. The final `output` folder consolidation at the end was a good pattern.

### 5c. Deep Research Prompts Are a Valuable Meta-Output

Creating structured research prompts (Synapse-focused + plugin-wide) from pipeline experience was arguably as valuable as the pipeline outputs themselves. They capture tacit knowledge that would otherwise be lost between sessions.

---

## 6. Recommendations for Next CQ Execution

1. **Start with a token budget estimate**: Before beginning, estimate how many phases can fit in one context window. Plan session boundaries in advance.
2. **Use the fallback chain for gene resolution**: HGNC → Ensembl → Entrez, especially for short symbols.
3. **Enforce complete drug traversal**: Don't skip targets under context pressure — instead, plan a session break before TRAVERSE_DRUGS if needed.
4. **Run `get_mechanism` for every compound**: It's cheap and high-value for evidence grading.
5. **Write checkpoint files after each phase**: JSON files with resolved entities, edges, and token budget estimates.
6. **Run Synapse grounding with synonym expansion**: Multiple queries per entity, not just the canonical name.
7. **Position the BioRxiv draft as a structured first draft**: Set expectations that human editing is needed for submission quality.

---

## 7. Files Produced in This Session

| File | Size | Purpose |
|------|------|---------|
| `tp53-synthetic-lethality-report.md` | 17.1KB | Stage 1a formatted report |
| `tp53-synthetic-lethality-synapse-grounding.md` | 8.4KB | Stage 1b Synapse grounding |
| `tp53-synthetic-lethality-quality-review.md` | 14.9KB | Stage 2 quality review |
| `tp53-synthetic-lethality-biorxiv-draft.md` | 32.2KB | Stage 3 BioRxiv manuscript |
| `tp53-synthetic-lethality-kg.json` | 16.9KB | Knowledge graph (24 nodes, 23 edges) |
| `tp53-sl-pipeline-chronology.md` | 13.2KB | Full pipeline chronology |
| `synapse-integration-analysis.md` | 14.6KB | Synapse value/gap analysis |
| `synapse-deep-research-prompt.md` | 10.2KB | Synapse-focused research prompt |
| `ob-plugin-deep-research-prompt.md` | 18.0KB | Plugin-wide research prompt |
| `cq14-session-learnings.md` | **this file** | Lessons learned |

**Total output**: ~155KB across 10 files.

---

*Captured 2026-03-03 from three-session CQ14 pipeline execution.*
