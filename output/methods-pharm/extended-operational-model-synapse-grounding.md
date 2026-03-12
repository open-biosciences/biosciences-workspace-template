# Synapse Dataset Grounding: Extended Operational Model

**Report**: extended-operational-model-report.md
**KG JSON**: extended-operational-model-kg.json
**Date**: 2026-03-12

---

## Status: Data Repository Not Connected

The Synapse.org data repository MCP tool was not available during this publication pipeline execution. Synapse grounding was therefore **skipped** for this report.

### Impact Assessment

This methodology CQ investigates pharmacometric frameworks (the Black–Leff operational model and extensions) rather than specific biological entities. Synapse.org's strongest coverage is in neurodegenerative and neurofibromatosis domains with experimental datasets. For a methodology-focused competency question, the expected Synapse grounding yield would be **low to negligible** because:

1. **No target genes or proteins**: The KG contains framework nodes (PMID-identified publications), metric nodes (IC₅₀, EC₅₀, AUC, log(τ/KA), S′, ΔS′), and phenomenon nodes (paradoxical BRAF activation) — none of which map directly to Synapse experimental datasets.

2. **Potential partial match**: The BRAF paradoxical activation phenomenon (PMID:27476449, PMID:24456413) could potentially match Synapse datasets containing BRAF inhibitor dose-response data in BRAF-WT vs. BRAF-V600E cell lines. However, this would be analogous grounding (dataset tests relevant phenomena) rather than direct grounding (dataset tests the specific methodological claim).

3. **Cancer HTS datasets**: If Synapse contains large-scale cancer drug screening datasets (e.g., GDSC, CCLE mirrors), these could serve as validation datasets for benchmarking S′ against traditional metrics. This would be **moderate** grounding strength — the datasets would not directly validate S′ (which doesn't exist yet) but would provide the substrate for future validation.

### KG JSON Synapse Fields

All `synapse_grounding` arrays in the KG JSON remain empty (`[]`). No Synapse dataset IDs, grounding strengths, or dataset descriptions have been populated.

### Recommendation

When Synapse MCP tools become available, prioritize the following grounding queries:

| Query | Target KG Node | Expected Grounding |
|-------|---------------|-------------------|
| "BRAF inhibitor dose-response" | PHENOMENON:PARADOXICAL_BRAF | Moderate — experimental BRAF data |
| "cancer drug screening dose-response curves" | METRIC:S_PRIME | Analogous — validation substrate |
| "vemurafenib dabrafenib cell line viability" | PHENOMENON:PARADOXICAL_BRAF | Strong (if present) — direct paradox data |
| "operational model pharmacology parameters" | FRAMEWORK:BLACK_LEFF_OM | Weak — methodological overlap only |

---

*Synapse grounding document generated 2026-03-12. Data repository not connected; all synapse_grounding arrays remain empty.*
