# Deep Research Prompt: Open Biosciences Plugin — Value Proposition and Future Roadmap

**Purpose**: Conduct a comprehensive analysis of the Open Biosciences plugin for Anthropic's Cowork platform. Evaluate its current capabilities, articulate its value proposition to researchers and organizations, and produce a prioritized roadmap of enhancements.

**Important**: Do NOT run this prompt immediately. It is designed for a dedicated deep research session.

---

## Context

The **Open Biosciences** plugin (version 1.0.0, Apache 2.0 license) is a Cowork plugin that transforms Claude into a life sciences research assistant. It provides:

- **5 MCP servers** connecting to 12+ biomedical databases via 34+ tools
- **9 domain skills** covering genomics, proteomics, pharmacology, clinical research, CRISPR validation, reporting, quality review, and publication
- **6 slash commands** (`/ob-research`, `/ob-report`, `/ob-review`, `/ob-publish`, `/ob-cq-discover`, `/ob-cq-run`)
- **15 competency questions** (CQs) stored as a HuggingFace dataset with gold standard paths and BioLink edge definitions
- **The Fuzzy-to-Fact protocol** — a bi-modal entity resolution system (LOCATE→RETRIEVE) that enforces database grounding and prevents hallucination

### Architecture Overview

| Component | Details |
|-----------|---------|
| **biosciences-mcp** (gateway) | 34 tools across HGNC, Ensembl, NCBI Entrez, UniProt, STRING, ChEMBL, PubChem, IUPHAR, WikiPathways, Open Targets, ClinicalTrials.gov, BioGRID |
| **biosciences-mcp-edge** | ChEMBL mechanism-of-action, BioGRID ORCS CRISPR essentiality |
| **Synapse MCP** | Dataset search, entity metadata, annotations, provenance, project traversal |
| **PubMed MCP** | Article search, metadata, full text, citation lookup, ID conversion |
| **bioRxiv MCP** | Preprint search, statistics, funder search, published article tracking |

### Protocol Fundamentals

- **LOCATE→RETRIEVE discipline**: Fuzzy search (slim=true, ~20 tokens/entity) followed by strict ID lookup (slim=false, ~115-300 tokens/entity). Prevents the agent from fabricating identifiers.
- **Token budgeting**: The `slim` parameter enables 5-10x more entities per context window turn. Critical for scaling knowledge graph construction.
- **8-phase pipeline**: ANCHOR → ENRICH → EXPAND → CRISPR_VALIDATION → TRAVERSE_DRUGS → TRAVERSE_TRIALS → VALIDATE → PERSIST
- **4-stage publication pipeline**: Report formatting (Template 1-3) → Synapse grounding → Quality review (10 dimensions) → BioRxiv manuscript draft (IMRaD)
- **L1-L4 evidence grading**: Claim-level numeric scoring (0.0-1.0) with modifiers for mechanism match, literature support, active trials, and mechanism mismatch

### Demonstrated Capabilities (CQ14 — TP53 Synthetic Lethality)

A complete pipeline run produced:
- 24-node, 23-edge knowledge graph (8 genes, 7 compounds, 4 pathways, 5 clinical trials)
- 11 evidence-graded claims (range: 0.35 L1 to 0.95 L4)
- 5 Synapse datasets mapped to graph entities (21% node coverage)
- Full BioRxiv-ready manuscript with supplementary tables
- 10-dimension quality review (7 PASS, 3 PARTIAL, 0 FAIL)
- Total output: ~89.5KB across 5 files

---

## Research Questions

### Section 1: Value Proposition Articulation (Questions 1-5)

**Q1. Competitive Landscape**: What existing tools, platforms, or LLM-based systems attempt to provide similar life sciences knowledge graph construction and evidence synthesis? Consider:
- Commercial platforms: BenchSci, Genestack, Linguamatics, SciBite
- Academic tools: KG-Hub, RTX-KG2, Biolink Model ecosystem
- LLM-based research tools: Elicit, Semantic Scholar, Consensus, Perplexity for scientific search
- Other Cowork/Claude Code plugins in the bio/life sciences space
- How does the Fuzzy-to-Fact protocol's hallucination prevention compare to these approaches?

**Q2. Unique Differentiators**: What capabilities does Open Biosciences provide that are NOT available in any competing tool? Evaluate:
- The LOCATE→RETRIEVE discipline as a hallucination prevention mechanism (no other LLM tool enforces database grounding at the entity resolution level)
- The ability to traverse gene→protein→pathway→drug→trial in a single conversational session
- The competency question framework as a reproducible research protocol
- Token budgeting as a scalability technique for knowledge graph construction
- The publication pipeline producing manuscript-ready output from raw API data
- Open source (Apache 2.0) vs. commercial alternatives

**Q3. Target Users and Use Cases**: Who benefits most from this plugin, and what are the highest-value use cases? Research:
- Academic researchers (drug discovery, target validation, literature review)
- Pharmaceutical R&D teams (target assessment, competitive intelligence, clinical landscape)
- Biotech startups (rapid due diligence on therapeutic hypotheses)
- Graduate students (learning to navigate biomedical databases systematically)
- Science communicators (creating evidence-grounded summaries)
- Which CQ categories map to which user segments?

**Q4. Evidence Grading as Differentiator**: How does the L1-L4 evidence grading system compare to existing frameworks? Research:
- GRADE (Grading of Recommendations Assessment, Development, and Evaluation) — widely used in clinical evidence
- NCI Evidence Levels for cancer drug development
- FDA approval evidence requirements
- How the plugin's numeric scoring with modifiers (+0.10 mechanism match, -0.20 mismatch) compares to these frameworks
- Could the grading system be formalized as a citable methodology?

**Q5. Reproducibility and Transparency**: How does the plugin's approach to provenance (source citations in every claim, tool call attribution, quality review) compare to best practices in computational biology reproducibility? Research:
- FAIR data principles (Findable, Accessible, Interoperable, Reusable)
- FORCE11 software citation principles
- The role of the KG JSON as a machine-readable provenance artifact
- How the CQ framework enables benchmark-style reproducibility testing

### Section 2: Current Limitations and Gap Analysis (Questions 6-9)

**Q6. Database Coverage Gaps**: Which important biomedical databases are NOT currently accessible through the plugin? Assess:
- **Genomics**: GEO/SRA (NCBI), DepMap (Broad), cBioPortal, COSMIC, ClinVar, dbSNP, gnomAD
- **Proteomics**: AlphaFold DB, PDB, InterPro, Pfam
- **Pharmacology**: DrugBank (currently placeholder only — requires commercial API key), FDA drug labels, DailyMed
- **Pathways**: Reactome, KEGG, GO (Gene Ontology)
- **Literature**: Semantic Scholar, Europe PMC, Google Scholar
- **Clinical**: FDA Adverse Event Reporting System (FAERS), WHO VigiBase
- For each gap, estimate: (a) how frequently CQs would benefit, (b) difficulty of integration, (c) licensing/cost barriers

**Q7. Protocol Gaps Identified in CQ14**: The pipeline execution revealed specific gaps. For each, assess severity and remediation:
- ATR and POLQ drug traversal was skipped — how often do CQs require complete target coverage?
- USP28 was identified but not fully enriched — how should the protocol handle discovery of unexpected entities?
- Compound nodes used custom CURIEs (COMPOUND:ADAVOSERTIB) not CHEMBL CURIEs — what's the impact on interoperability?
- Disease CURIEs were in a separate section, not graph nodes — should diseases be first-class entities?
- HGNC fuzzy search failed for short gene symbols (ATR, POLQ) — how widespread is this problem across all CQs?

**Q8. Synapse Integration Depth**: The Synapse integration currently provides metadata-level grounding only (21% node coverage, 16% edge coverage in CQ14). What would it take to achieve data-level access? Research:
- Synapse REST API capabilities for table queries and file downloads
- Python client (synapseclient) capabilities that could be exposed as MCP tools
- Privacy and authentication requirements for accessing controlled-access datasets
- The seven improvement opportunities identified in the Synapse integration analysis (see companion document)

**Q9. Scalability Constraints**: What are the current bottlenecks for scaling beyond single-CQ execution?
- Context window limits (CQ14 required two sessions due to ~200K token context exhaustion)
- Rate limiting across 5 MCP servers (different rate limits per API)
- Token budgeting effectiveness — how much headroom does `slim=true` actually buy?
- Can multi-CQ batch execution be supported? What architectural changes would be needed?
- What's the maximum knowledge graph size (nodes + edges) the pipeline can handle?

### Section 3: Enhancement Priorities (Questions 10-14)

**Q10. New MCP Server Integration**: Rank the following potential new MCP server integrations by impact/effort ratio:
- **Reactome** (pathway database, REST API available) — would complement WikiPathways
- **AlphaFold DB** (protein structure predictions) — would enable structure-based drug discovery CQs
- **GEO/SRA** (gene expression datasets) — would close the biggest gap in cancer genomics CQs
- **DepMap** (cancer dependency data) — would upgrade CRISPR validation from ORCS-only to ORCS+DepMap
- **COSMIC** (somatic mutations in cancer) — would ground mutation-specific claims
- **ClinVar** (clinical variant interpretation) — would enable pharmacogenomics CQs
- **Semantic Scholar** (ML-powered literature search) — would complement PubMed with citation graph analysis
- For each, provide: (a) estimated number of CQs impacted, (b) API maturity/documentation quality, (c) authentication requirements, (d) estimated development effort

**Q11. New Competency Questions**: The current 15 CQs cover 8 categories and 7 disease areas. What CQs should be added to demonstrate the plugin's range? Consider:
- Rare disease drug repurposing (Orphanet integration)
- Pharmacogenomics (CYP450 variants → drug metabolism)
- Immuno-oncology (checkpoint inhibitor biomarkers)
- Antimicrobial resistance (resistance gene networks)
- Neurodegenerative diseases (Alzheimer's target landscape)
- Multi-omics integration (genomics + proteomics + metabolomics)
- For each proposed CQ, define: anchor entity, expected phases used, gold standard path, and which existing/new MCP tools would be needed

**Q12. Skill Enhancements**: Evaluate potential improvements to the 9 existing skills:
- **biosciences-graph-builder**: Should Phase 3 EXPAND query Synapse for datasets that could contribute new entities? Should the protocol auto-detect when drug traversal should be performed for all targets (not just explicitly requested ones)?
- **biosciences-crispr**: Can the 5-phase ORCS workflow be extended to include DepMap CERES/Chronos scores for cell-line-specific essentiality?
- **biosciences-pharmacology**: The ChEMBL mechanism tool is available but underused. Should it be invoked automatically during drug traversal?
- **biosciences-publication-pipeline**: Should the BioRxiv drafter include figure generation (e.g., network diagrams, evidence heatmaps)?
- **biosciences-reporting**: Should new report templates be added (e.g., Template 4: Rare Disease, Template 5: Biomarker Discovery)?
- **biosciences-cq-runner**: How should gold standard validation evolve when CQs produce results that exceed the gold standard path (legitimate discovery vs. drift)?

**Q13. Visualization and Interactive Outputs**: The plugin currently produces markdown reports and JSON knowledge graphs. What visualization capabilities would increase adoption?
- Interactive network diagrams (D3.js, Cytoscape.js) showing the KG with evidence-colored edges
- Evidence heatmaps showing claim strength across targets
- Clinical trial landscape timelines
- Drug-target interaction matrices
- Excalidraw integration for pathway diagrams (MCP available)
- React/HTML artifacts for interactive exploration of the KG
- How would these integrate with Cowork's file presentation system?

**Q14. Quality and Governance**: How should quality assurance evolve?
- Should the quality review be mandatory (gate) or optional (advisory)?
- Can automated regression testing be built using the CQ gold standard paths?
- How should the plugin handle API changes (e.g., if a BioGRID endpoint changes response format)?
- Should there be a "confidence dashboard" that shows pipeline health across all CQs?
- How should version control work for KG JSON files over time?

### Section 4: Community and Ecosystem (Questions 15-17)

**Q15. Open Source Strategy**: The plugin is Apache 2.0 licensed. What's the optimal community engagement strategy?
- GitHub contribution model (issues, PRs, CQ contributions)
- HuggingFace dataset governance for CQ additions/modifications
- Documentation site (which framework? ReadTheDocs, Docusaurus, MkDocs?)
- Plugin marketplace visibility and discoverability
- Relationship to the broader MCP ecosystem and Anthropic plugin marketplace

**Q16. Academic Adoption**: How can the plugin gain traction in the research community?
- Preprint describing the Fuzzy-to-Fact methodology (using the CQ14 BioRxiv draft as proof-of-concept)
- Workshop at bioinformatics conferences (ISMB, RECOMB, BOSC)
- Integration with existing bioinformatics workflows (Nextflow, Snakemake, Galaxy)
- Comparison study: pipeline output vs. manual literature review for the same CQ
- Collaboration with existing knowledge graph projects (KG-Hub, Biolink Model, Translator)

**Q17. Institutional Deployment**: What would it take for a pharmaceutical company or research institution to adopt the plugin at scale?
- Data security requirements (can the plugin run air-gapped? Which MCP servers require external API access?)
- Customization needs (private databases, proprietary compound libraries, internal clinical data)
- Integration with existing ELN (Electronic Lab Notebook) and LIMS systems
- Compliance requirements (GxP, 21 CFR Part 11 for regulated environments)
- Training and onboarding requirements for bench scientists vs. computational biologists

### Section 5: Technical Architecture (Questions 18-19)

**Q18. MCP Server Architecture**: Evaluate the current 5-server architecture and propose improvements:
- Should the biosciences-mcp gateway (34 tools) be decomposed into smaller, domain-specific servers?
- What's the latency profile across the 5 servers? Are there bottlenecks?
- How does the connector/placeholder system (~~literature, ~~data repository) compare to direct server references?
- Should there be a caching layer for frequently-queried entities (e.g., TP53, BRCA1)?
- How should authentication evolve beyond API keys (OAuth, institutional SSO)?

**Q19. Knowledge Graph Persistence and Querying**: The current pipeline produces static JSON files. What's the path to a queryable knowledge graph?
- Graph database options: Neo4j, Amazon Neptune, Graphiti (mentioned in graph-builder skill)
- Should completed KGs be depositable to Synapse for sharing?
- Could multiple CQ KGs be merged into a unified knowledge base?
- What query language (Cypher, SPARQL, GraphQL) would best serve downstream consumers?
- How would a persistent KG change the plugin's value proposition (from single-query to cumulative knowledge)?

---

## Expected Output Format

### Executive Summary (1 page)
- Plugin's current position in the landscape
- Three strongest differentiators
- Top 3 high-impact, feasible enhancements
- Recommended 6-month focus

### Competitive Analysis Matrix
| Capability | Open Biosciences | BenchSci | Elicit | KG-Hub | Manual |
|------------|-----------------|----------|--------|--------|--------|
| Entity resolution | ... | ... | ... | ... | ... |
| Evidence grading | ... | ... | ... | ... | ... |
| ... | ... | ... | ... | ... | ... |

### Database Gap Heat Map
- Rows: 15 CQ categories
- Columns: Current databases + proposed additions
- Cells: Coverage level (Full / Partial / None)

### Enhancement Impact-Effort Matrix
- 4-quadrant plot: High Impact + Low Effort (do first), High Impact + High Effort (plan), Low Impact + Low Effort (opportunistic), Low Impact + High Effort (deprioritize)
- Each proposed enhancement plotted with justification

### Prioritized Roadmap

**Phase 1 (Months 1-3): Foundation**
- [Items from research findings]

**Phase 2 (Months 4-6): Expansion**
- [Items from research findings]

**Phase 3 (Months 7-12): Scale**
- [Items from research findings]

### Value Proposition One-Pager
- Suitable for sharing with potential users, institutional decision-makers, or conference submissions
- Should articulate WHY this plugin matters, not just WHAT it does

### Appendix
- Full database comparison table
- CQ coverage matrix (current vs. proposed)
- API documentation links for all proposed integrations
- Token budget analysis across representative CQs

---

## Research Sources

### Primary (Plugin Internal)
- Plugin README.md, plugin.json, CONNECTORS.md, .mcp.json
- All 9 skill SKILL.md files
- Reference documents: fuzzy-to-fact.md, token-budgeting.md, api-keys.md
- CQ14 pipeline outputs: KG JSON, report, Synapse grounding, quality review, BioRxiv draft
- Pipeline chronology and Synapse integration analysis documents

### Secondary (External)
- Synapse REST API documentation (https://rest-docs.synapse.org/)
- BioGRID ORCS documentation
- Open Targets Platform API documentation
- ClinicalTrials.gov API documentation
- ChEMBL API documentation
- PubChem PUG REST documentation
- STRING API documentation
- Ensembl REST API documentation
- MCP specification and ecosystem documentation

### Tertiary (Competitive/Landscape)
- BenchSci product documentation and published case studies
- Elicit methodology papers
- KG-Hub and Biolink Model publications
- FAIR data principles (Wilkinson et al., 2016)
- Translator project (NCATS) architecture documentation
- Recent publications on LLM-assisted biomedical research

---

*Prompt prepared 2026-03-03 based on CQ14 (TP53 synthetic lethality) pipeline execution and comprehensive plugin architecture review.*
