# Workspace Memory

## HuggingFace Dataset Access — Parquet URL Resolution

**Date:** 2026-03-02
**Context:** Retrieving CQ data from `open-biosciences/biosciences-competency-questions-sample`

### Problem

DuckDB's `hf://` shorthand assumes Parquet files live on the `main` branch at a predictable path like:

```
hf://datasets/open-biosciences/biosciences-competency-questions-sample/data/train-00000-of-00001.parquet
```

This returned **404 Not Found** for this dataset. Several alternative dataset name guesses also failed.

### Root Cause

The dataset was originally uploaded in a non-Parquet format (e.g., JSON or CSV). HuggingFace auto-converts these to Parquet and stores them under a special branch (`refs/convert/parquet`), not on `main`. The `hf://` shorthand doesn't resolve this branch automatically.

### Working Solution

Use the **explicit HTTPS URL** pointing to the auto-converted Parquet branch:

```
https://huggingface.co/datasets/open-biosciences/biosciences-competency-questions-sample/resolve/refs%2Fconvert%2Fparquet/default/train/0000.parquet
```

### Example DuckDB Query

```python
import duckdb

con = duckdb.connect()
url = "https://huggingface.co/datasets/open-biosciences/biosciences-competency-questions-sample/resolve/refs%2Fconvert%2Fparquet/default/train/0000.parquet"

result = con.execute(f"""
    SELECT * FROM read_parquet('{url}')
    WHERE cq_id = 'cq14'
""").fetchall()
```

### General Pattern

For any HuggingFace dataset where `hf://` fails with 404, try:

```
https://huggingface.co/datasets/{org}/{dataset}/resolve/refs%2Fconvert%2Fparquet/default/train/0000.parquet
```

### Applicability

This applies to all CQ discovery queries against the `biosciences-competency-questions-sample` dataset and likely other HuggingFace datasets that were uploaded in non-Parquet formats.

---

## CQ14 Validation Run — `slim=true` Default Note

**Date:** 2026-03-02
**Context:** CQ14 execution against live MCP servers

### Observation

During the CQ14 run, `biogrid_get_interactions` for TP53 returned 355,618 characters (6,075 interactions), exceeding the tool output limit and requiring fallback to file-based parsing. The `slim=true` parameter was not used on the initial call.

### Rule

**Default to `slim=true` for all MCP search and lookup calls** to reduce token usage and avoid output truncation. Only use full mode (`slim=false`) when enriched data is specifically needed, such as:

- Gene node cross-references during the ENRICH phase (need ensembl, uniprot, location)
- Compound details when molecular weight, SMILES, or indications are required
- Trial details when eligibility criteria or outcome measures are needed

### Example

```python
# Preferred — slim by default
hgnc_search_genes(query="TP53", slim=True, page_size=1)
biogrid_get_interactions(gene_symbol="TP53", slim=True, max_results=100)

# Full mode only when cross-refs are needed
hgnc_get_gene(hgnc_id="HGNC:11998")  # always returns full (no slim option)
```

### CQ14 Execution Summary

- **Status:** PASS (4/4 entities, 3/3 edges, path VALIDATED)
- **APIs used:** HGNC, BioGRID, ChEMBL, ClinicalTrials.gov
- **Gold standard path:** TP53 → TYMS → Pemetrexed → NCT04695925 (all hops confirmed)
- **Persisted to:** `.ob-cq/cq14/` (results.json, validation.json, validation-report.md)

---

## OPEN ISSUE: HuggingFace Write Authentication in Cowork

**Date:** 2026-03-02
**Status:** Unresolved
**Context:** Attempted to push CQ14 validation results to `open-biosciences/biosciences-cq-validations` on HuggingFace

### Problem

The CQ Runner's Step 7 (PUBLISH) requires write access to HuggingFace via the Python `datasets` library (`ds.push_to_hub()`). This needs an `HF_TOKEN` environment variable with **write** permissions. In the current Cowork session:

1. **HF MCP connector** is authenticated as `dwb2023` (confirmed via `hf_whoami`), but this token is scoped to the MCP connector and is **not available** to the Python runtime.
2. **`HF_TOKEN` env var** is not set in the Cowork VM.
3. **`huggingface-cli login`** requires interactive token input, which the user would need to provide manually.
4. **No cached token** found via `huggingface_hub.get_token()`.

### Impact

CQ validation results can only be persisted locally (`.ob-cq/` directory), not published to the shared HuggingFace dataset. This blocks the collaborative validation workflow where multiple runs are aggregated for regression tracking.

### Potential Solutions

1. **Bridge MCP token to Python**: If the HF MCP connector token could be exposed as an env var or retrieved programmatically, the `datasets` library could reuse it. This would require a change to how the HF MCP connector shares credentials.
2. **Pre-set HF_TOKEN in Cowork session**: User could configure `HF_TOKEN` as a Cowork environment variable so it's available in all sessions. (How to do this in Cowork is unclear.)
3. **Use HF Hub API via MCP**: If the HF MCP connector exposed a `create_dataset` or `upload_file` tool, the publish step could bypass the Python `datasets` library entirely.
4. **Manual export**: Generate the publish-ready dataset file locally and provide instructions for the user to upload manually via HuggingFace web UI.

### Workaround (Current)

Results are persisted locally to `.ob-cq/cq14/`. To publish manually:
1. Go to https://huggingface.co/datasets/open-biosciences/biosciences-cq-validations
2. Upload `results.json` and `validation.json` from the `.ob-cq/cq14/` directory
3. Or use `huggingface-cli upload` from a terminal with a cached token

---

## CQ14 Quality Review — Root Cause Analysis for Gaps

**Date:** 2026-03-02
**Context:** Quality review scored CQ14 as PARTIAL PASS (8/10 protocol, 8/10 presentation). Five gaps identified in the report's Gaps and Limitations section. Root causes documented below for future CQ runs.

### Gap 1: BioGRID ORCS essentiality data not queried

**What happened:** The CQ definition's workflow step 2 references "BioGRID ORCS → 1,446 screens confirm TYMS essentiality." The runner used `biogrid_get_interactions` (standard BioGRID interactions API), which confirmed the TP53-TYMS genetic interaction but does not return ORCS CRISPR screen data.

**Root cause:** The biosciences-mcp gateway exposes `biogrid_search_genes` and `biogrid_get_interactions` but does **not** expose a dedicated BioGRID ORCS endpoint. ORCS data (CRISPR knockout essentiality scores) requires either a separate API call to the BioGRID ORCS REST API (`https://orcs.thebiogrid.org/`) or use of the `biosciences-crispr` skill which implements a 5-phase ORCS workflow via curl.

**Fix for re-run:** Invoke the `biosciences-crispr` skill or add a curl-based ORCS query step: `curl "https://orcs.thebiogrid.org/api/screen?searchTerm=TYMS&format=json"`.

### Gap 2: 5-fluorouracil clinical trials not searched

**What happened:** The clinical trial search used only `"TP53 pemetrexed"` as the query, matching the gold standard path. No search was performed for `"TP53 fluorouracil"` or `"TP53 5-FU"`.

**Root cause:** The CQ definition's workflow step 4 specifies only `clinicaltrials_search_trials('TP53 pemetrexed')`. The runner followed the prescribed workflow steps literally. 5-fluorouracil is listed as a key entity in the CQ definition but has no corresponding clinical trial search step.

**Fix for re-run:** Add an additional clinical search: `clinicaltrials_search_trials("TP53 fluorouracil")` and/or `clinicaltrials_search_trials("TP53 5-FU")`. This is an EXPAND step beyond the gold standard path and should be documented as a bonus finding, not a required validation step. Also consider updating the CQ definition to include this search.

### Gap 3: ChEMBL /mechanism endpoint not queried

**What happened:** Druggability was confirmed via `chembl_search_compounds` (finding the compound) and `chembl_get_compound` (getting phase and indications). The specific mechanism of action (TYMS inhibition) was inferred from the search match and compound name, not from a direct mechanism query.

**Root cause:** The ChEMBL MCP tools (`chembl_search_compounds`, `chembl_get_compound`) do not expose the `/mechanism` endpoint. Confirming mechanism of action requires a curl fallback: `curl "https://www.ebi.ac.uk/chembl/api/data/mechanism?molecule_chembl_id=CHEMBL225072&format=json"`. The CQ runner's tool dispatch table maps MCP tools but the mechanism endpoint is only available via curl.

**Fix for re-run:** Add a curl-based mechanism query after the ChEMBL compound lookup. This is documented in the CQ Runner skill's fallback patterns table but was not triggered because the MCP call succeeded (it just didn't return mechanism data).

### Gap 4: NCT03574402 (TRUMP study) not verified

**What happened:** `clinicaltrials_search_trials("TP53 pemetrexed")` returned two trials. NCT04695925 (gold standard) was verified via `clinicaltrials_get_trial`. NCT03574402 was listed in the report but marked as unverified.

**Root cause:** The runner prioritized verifying the gold standard trial (NCT04695925) and did not perform a second `clinicaltrials_get_trial` call for the additional search result. This is a workflow optimization decision — the gold standard path only requires one trial — but leaves a loose end in the report.

**Fix for re-run:** Add `clinicaltrials_get_trial(NCT:03574402)` as a verification step for all trials returned by the search, not just the gold standard target.

### Gap 5: No Open Targets association data for TP53

**What happened:** TP53 was resolved via HGNC and validated via BioGRID, but no `opentargets_get_associations` call was made to retrieve disease associations with scores.

**Root cause:** The CQ definition's workflow steps do not include an Open Targets step. The prescribed APIs are HGNC, BioGRID, ChEMBL, and ClinicalTrials.gov. Open Targets was not in the workflow and the runner followed the prescribed steps. This is also the root cause of the **Disease CURIE FAIL** in the quality review — without an OT associations query, no disease entity (MONDO/EFO CURIE) was resolved.

**Fix for re-run:** Add `opentargets_search_targets("TP53")` → `opentargets_get_associations(ENSG00000141510)` to resolve disease associations and obtain a disease CURIE. This addresses both the missing OT data and the Disease CURIE dimension failure. Consider updating the CQ definition to include this step.

### Summary: CQ Definition vs. Execution Gaps

| Gap | Root Cause Category | In CQ Definition? | Fix Location |
|-----|-------------------|-------------------|-------------|
| BioGRID ORCS | Missing MCP tool; needs curl or crispr skill | Yes (step 2 text) | Runner execution |
| 5-FU trials | Not in workflow steps | No (entity only) | CQ definition + runner |
| ChEMBL /mechanism | Not exposed via MCP; needs curl | Implicit | Runner fallback logic |
| NCT03574402 verify | Runner optimization (gold standard only) | No | Runner execution |
| Open Targets | Not in workflow steps | No | CQ definition + runner |

Three of five gaps trace to the CQ definition not including the relevant workflow step. The other two trace to MCP tool coverage (no ORCS endpoint, no /mechanism endpoint) requiring curl fallbacks that weren't triggered.

---

## OPEN ISSUE: BioGRID ORCS API Unreachable from Cowork

**Date:** 2026-03-02
**Status:** Unresolved
**Impact:** CQ14 protocol score capped at 9/10; TYMS CRISPR essentiality data unavailable

### Problem

BioGRID ORCS (`https://orcs.thebiogrid.org/`) provides CRISPR knockout screen data that would confirm TYMS essentiality across cell lines. The CQ14 definition references "1,446 screens confirm TYMS essentiality" from this source. During the gap-fill run, multiple endpoint patterns were attempted and all returned empty responses or HTML error pages:

- `https://orcs.thebiogrid.org/api/screen?searchTerm=TYMS&format=json` → empty
- `https://orcs.thebiogrid.org/api/1.0/get/screen?searchTerm=TYMS` → empty
- `https://orcs.thebiogrid.org/api/gene/7298` (Entrez ID for TYMS) → empty

### Root Cause (Likely)

The BioGRID ORCS REST API appears to require an API access key (`accessKey` parameter), similar to the main BioGRID REST API. No BioGRID API key is available in the Cowork environment. The `biosciences-crispr` skill documents a 5-phase ORCS workflow but relies on curl calls that also need the API key.

### Workarounds Considered

1. **MCP gateway**: The biosciences-mcp gateway exposes `biogrid_search_genes` and `biogrid_get_interactions` but has no ORCS-specific endpoint.
2. **Web scraping**: The ORCS web UI at `https://orcs.thebiogrid.org/Search?searchTerm=TYMS` renders data, but HTML scraping from Cowork is restricted.
3. **BioGRID API key**: Users can register for a free API key at `https://webservice.thebiogrid.org/`. If available, append `&accessKey=YOUR_KEY` to the REST URL.

### Impact on CQ14

Without ORCS data, the "validate synthetic lethal gene pairs" component relies on BioGRID standard genetic interaction data (which confirms TP53-TYMS interaction) and ChEMBL mechanism confirmation (which confirms TYMS as a drug target). The CRISPR essentiality evidence would add a third independent validation layer but is not strictly required for the gold standard path.

### Recommendation

For future CQ runs involving BioGRID ORCS:
1. Obtain a BioGRID API key and set as environment variable
2. Or: Request an ORCS-specific MCP tool in the biosciences-mcp gateway
3. Or: Pre-populate ORCS data in the CQ definition's `gold_standard` field so it can be validated without a live API call
