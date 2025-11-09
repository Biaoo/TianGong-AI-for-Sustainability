# TianGong Research Plan - European Food Water Footprint Study

## 1. Environment & Readiness
- [ ] Run `uv run tiangong-research sources list` and confirm availability.
- [ ] Verify essential sources (OpenAlex, arXiv, Scopus for literature; GitHub for implementations) via `uv run tiangong-research sources verify openalex`.
- [ ] If charts are required, run `uv run tiangong-research research visuals verify`.
- [ ] Missing tooling or credentials:
  - Ensure Scopus API key is configured if using Scopus source
  - Verify AntV chart server is available for water footprint visualizations
  - Check access to water footprint databases (e.g., Water Footprint Network, AQUASTAT)

## 2. Study Context
- Primary objective: Analyze and compare water footprint metrics across major food categories in the European region (EU27), identifying high-impact products and sustainability opportunities.
- Scope / subtopics:
  - Blue water footprint (irrigation water consumption)
  - Green water footprint (rainwater consumption)
  - Grey water footprint (pollution load)
  - Food categories: cereals, dairy, meat, fruits, vegetables, beverages
  - Life cycle stages: agricultural production, processing, packaging, distribution
- Geography or sector focus: European Union (EU27) with potential breakdown by member states; focus on agricultural and food processing sectors
- Constraints or policies:
  - Align with EU Water Framework Directive
  - Consider Common Agricultural Policy (CAP) implications
  - Reference EU Farm to Fork Strategy targets
- Expected deliverables:
  - Comprehensive dataset of water footprint metrics by food category
  - Comparative analysis report with visualization charts
  - SDG alignment (SDG 6: Clean Water, SDG 12: Responsible Consumption, SDG 2: Zero Hunger)
  - Policy recommendations for water-efficient food systems

## 3. Stage Plan (CLI-First)
### Stage 0 - Spec Alignment
- Purpose: confirm alignment with `AGENTS.md` and `tasks/blueprint.yaml`; verify data source readiness
- CLI commands:
  - `uv run tiangong-research sources list`
  - `uv run tiangong-research sources verify openalex`
  - `uv run tiangong-research sources verify github`
- Outputs: readiness notes at `.cache/tiangong/eu_food_water_footprint/stage0_readiness.md`, blocked sources, escalation items.

### Stage 1 - Deterministic Acquisition
- Purpose: gather scientific literature on European food water footprint, implementation references, and SDG alignment data
- CLI commands:
  - `uv run tiangong-research research map-sdg "European food water footprint analysis covering agricultural water use, food processing, and sustainable water management in EU27 region" --json > .cache/tiangong/eu_food_water_footprint/acquisition/sdg_mapping.json`
  - `uv run tiangong-research research find-papers "water footprint food production Europe" --limit 50 --openalex --citation-graph --arxiv > .cache/tiangong/eu_food_water_footprint/acquisition/papers_water_footprint.json`
  - `uv run tiangong-research research find-papers "virtual water trade agricultural products EU" --limit 30 --openalex > .cache/tiangong/eu_food_water_footprint/acquisition/papers_virtual_water.json`
  - `uv run tiangong-research research find-papers "life cycle assessment water use food systems" --limit 40 --openalex --arxiv > .cache/tiangong/eu_food_water_footprint/acquisition/papers_lca_water.json`
  - `uv run tiangong-research research find-code "water footprint assessment" --limit 20 --json > .cache/tiangong/eu_food_water_footprint/acquisition/code_water_footprint.json`
  - `uv run tiangong-research research find-code "virtual water calculation agriculture" --limit 15 --json > .cache/tiangong/eu_food_water_footprint/acquisition/code_virtual_water.json`
- Outputs: store JSON/Markdown under `.cache/tiangong/eu_food_water_footprint/acquisition/`.

### Stage 2 - Evidence Consolidation
- Purpose: normalize water footprint data, deduplicate literature sources, and compute aggregate metrics by food category
- Tooling: reuse cached CLI outputs; document any consolidation scripts
- Key tasks:
  - Extract water footprint values (L/kg, m³/ton) from papers
  - Categorize findings by food type (cereals, dairy, meat, etc.)
  - Identify geographic variations within EU27
  - Cross-reference methodology standards (ISO 14046, Water Footprint Assessment Manual)
  - Compile implementation tools and code repositories
- Outputs:
  - Consolidated data table: `.cache/tiangong/eu_food_water_footprint/consolidated/food_water_footprint_data.json`
  - Literature summary: `.cache/tiangong/eu_food_water_footprint/consolidated/literature_summary.md`
  - Code registry: `.cache/tiangong/eu_food_water_footprint/consolidated/implementation_tools.json`

### Stage 3 - Synthesis & Visualisation
- Purpose: generate insights on high-impact food categories, regional variations, and policy implications; create comparative visualizations
- CLI commands:
  - `uv run tiangong-research research synthesize "What are the major food categories with highest water footprint in Europe and what are the key factors driving water consumption in food production?" --prompt-template default --prompt-language en -P study_region="European Union (EU27)" -P focus_sectors="agriculture,food_processing" -P metric_type="water_footprint" > .cache/tiangong/eu_food_water_footprint/synthesis/high_impact_analysis.md`
  - `uv run tiangong-research research synthesize "How can EU agricultural policies be optimized to reduce food water footprint while maintaining food security?" --prompt-template default --prompt-language en -P policy_context="CAP,Farm_to_Fork_Strategy" -P constraints="food_security,economic_viability" > .cache/tiangong/eu_food_water_footprint/synthesis/policy_recommendations.md`
  - `uv run tiangong-research research visuals verify`
- Visualization specs (using AntV):
  - Bar chart comparing water footprint by food category (blue/green/grey components stacked)
  - Geographical heatmap of water intensity across EU member states
  - Sankey diagram showing virtual water flows in EU food trade
  - Scatter plot: water footprint vs. nutritional value by food item
- Outputs:
  - Synthesis reports at `.cache/tiangong/eu_food_water_footprint/synthesis/`
  - Chart specifications at `.cache/tiangong/eu_food_water_footprint/charts/`
  - Traceability notes linking conclusions to source papers

### Stage 4 - Wrap-up
- Purpose: compile final report, archive outputs, document blockers and follow-ups
- Actions:
  - Generate executive summary with key findings
  - Archive all outputs with proper metadata
  - Update `tasks/backlog.yaml` with identified research gaps (e.g., specific crop varieties, seasonal variations)
  - Document data quality issues and source limitations
- Outputs:
  - Final report: `.cache/tiangong/eu_food_water_footprint/EU_Food_Water_Footprint_Report.md`
  - Archive manifest: `.cache/tiangong/eu_food_water_footprint/outputs_manifest.json`
  - Backlog entries for future research directions

## 4. Deterministic Command Queue
1. `uv run tiangong-research sources list` - verify all sources available, no flags needed, output to console for review.
2. `uv run tiangong-research sources verify openalex` - confirm OpenAlex API accessibility, output to console.
3. `uv run tiangong-research research map-sdg "European food water footprint analysis covering agricultural water use, food processing, and sustainable water management in EU27 region" --json` - map study to SDG framework, output to `.cache/tiangong/eu_food_water_footprint/acquisition/sdg_mapping.json`.
4. `uv run tiangong-research research find-papers "water footprint food production Europe" --limit 50 --openalex --citation-graph --arxiv` - gather primary literature, output to `.cache/tiangong/eu_food_water_footprint/acquisition/papers_water_footprint.json`.
5. `uv run tiangong-research research find-papers "virtual water trade agricultural products EU" --limit 30 --openalex` - gather virtual water literature, output to `.cache/tiangong/eu_food_water_footprint/acquisition/papers_virtual_water.json`.
6. `uv run tiangong-research research find-papers "life cycle assessment water use food systems" --limit 40 --openalex --arxiv` - gather LCA methodology papers, output to `.cache/tiangong/eu_food_water_footprint/acquisition/papers_lca_water.json`.
7. `uv run tiangong-research research find-code "water footprint assessment" --limit 20 --json` - discover implementation tools, output to `.cache/tiangong/eu_food_water_footprint/acquisition/code_water_footprint.json`.
8. `uv run tiangong-research research find-code "virtual water calculation agriculture" --limit 15 --json` - discover calculation implementations, output to `.cache/tiangong/eu_food_water_footprint/acquisition/code_virtual_water.json`.
9. `uv run tiangong-research research synthesize "What are the major food categories with highest water footprint in Europe and what are the key factors driving water consumption in food production?" --prompt-template default --prompt-language en -P study_region="European Union (EU27)" -P focus_sectors="agriculture,food_processing" -P metric_type="water_footprint"` - generate high-impact analysis, output to `.cache/tiangong/eu_food_water_footprint/synthesis/high_impact_analysis.md`.
10. `uv run tiangong-research research synthesize "How can EU agricultural policies be optimized to reduce food water footprint while maintaining food security?" --prompt-template default --prompt-language en -P policy_context="CAP,Farm_to_Fork_Strategy" -P constraints="food_security,economic_viability"` - generate policy recommendations, output to `.cache/tiangong/eu_food_water_footprint/synthesis/policy_recommendations.md`.
11. `uv run tiangong-research research visuals verify` - confirm chart generation capability, output to console.

## 5. Reporting & Observability
- Output formats: Markdown (primary reports), JSON (data tables and metadata), PNG/SVG (charts)
- Observability tags: `water_footprint`, `food_systems`, `europe`, `sustainability`, `sdg6`, `sdg12`, `lca`, `virtual_water`
- Dry run mode: false
- Cache directory override (optional): `.cache/tiangong/eu_food_water_footprint/`
- Traceability: each finding in synthesis reports must reference:
  - Source paper DOI/ID from acquisition outputs
  - Specific data points or methodology from literature
  - CLI command that produced the evidence
  - Timestamp and version of tools used

## 6. Optional Extensions
- Deep Research trigger: enable `--deep-research` flag in synthesis stage if initial findings reveal significant knowledge gaps or conflicting data; limit to 2 iterations, max 15-minute runtime per synthesis.
- AntV chart specification:
  - Comparative bar chart: water footprint by food category (specify JSON schema with data from consolidated outputs)
  - Geographical visualization: EU member states water intensity (choropleth map)
  - Save charts to `.cache/tiangong/eu_food_water_footprint/charts/`
- Custom prompt packs or variables:
  - Template: `default`
  - Variables: `study_region="European Union (EU27)"`, `focus_sectors="agriculture,food_processing"`, `metric_type="water_footprint"`, `policy_context="CAP,Farm_to_Fork_Strategy"`
  - Additional templates if needed: create `specs/prompts/water_footprint_specific.md` for domain-specific synthesis

## 7. Fallbacks & Blockers
- Missing CLI capability → deterministic Python fallback:
  - If water footprint database query not available via CLI: use `tiangong_ai_for_sustainability.data.water_footprint.query_database()` + create TODO to expose as `uv run tiangong-research data query-water-footprint`
  - If specialized LCA calculation needed: use `tiangong_ai_for_sustainability.analysis.lca.calculate_water_impact()` + create TODO for CLI wrapper
- External blockers (rate limits, missing credentials):
  - Scopus API rate limit: fallback to OpenAlex and arXiv only; document limitation
  - Missing water footprint database access: rely on literature-extracted values; note reduced primary data
  - AntV chart server unavailable: generate data tables only; defer visualization
- Escalation contact / next steps:
  - Data quality issues → consult domain experts for validation
  - Policy interpretation questions → engage EU policy advisors
  - Missing datasets → submit data request to Water Footprint Network or FAO AQUASTAT

## 8. Deliverable Checklist
- [ ] Structured summary covering objectives, methods, results, next steps.
- [ ] Stored CLI outputs (JSON/Markdown) at `.cache/tiangong/eu_food_water_footprint/`.
- [ ] Visualization assets (charts showing water footprint by food category, geographic variations) referenced in the report.
- [ ] Backlog or TODO updates for identified gaps (seasonal variations, specific crop varieties, emerging food technologies).
- [ ] Notes on rate limits, missing data, or dependency failures.
- [ ] Cross-references to relevant EU policies and SDG targets.
- [ ] Data quality assessment and source reliability notes.
- [ ] Reproducibility documentation: exact CLI commands, tool versions, timestamp.

## Additional Notes
- **Data Sources Priority**: Prioritize peer-reviewed literature from high-impact journals; cross-validate water footprint values from multiple sources
- **Methodology Standards**: Follow ISO 14046 (Environmental management - Water footprint) and Water Footprint Assessment Manual (Hoekstra et al., 2011)
- **Geographical Scope**: Focus on EU27; may include UK for historical comparisons; consider candidate countries if relevant
- **Time Frame**: Target recent data (2015-2024) for policy relevance; include historical trends where significant
- **Quality Thresholds**: Require at least 3 independent sources for key water footprint values; flag single-source data
- **Validation**: Cross-check findings against established databases (Water Footprint Network, FAOSTAT, Eurostat)
