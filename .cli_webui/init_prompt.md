According to the following instructions, complete a full research task step by step.

# TianGong Research Plan

## 0. Workspace Bootstrap
- Study ID: `<STUDY_ID>`
- Workspace init command: `uv run python scripts/ops/init_study_workspace.py --study-id <STUDY_ID>` *(skip if already provisioned)*
- Blueprint sources: `.cache/tiangong/<STUDY_ID>/docs/runbook.md`, `.cache/tiangong/<STUDY_ID>/docs/study_brief.md`
- Auto execute after blueprint confirmation: `true` *(Codex proceeds immediately when true; otherwise pause for human sign-off)*

## 1. Environment & Readiness
- [ ] Run `uv run tiangong-research sources list` and confirm availability.
- [ ] Verify essential sources (`<SOURCE_IDS>`) via `uv run tiangong-research sources verify <id>`.
- [ ] If charts are required, run `uv run tiangong-research research visuals verify`.
- [ ] Missing tooling or credentials: <MISSING_ITEMS_AND_REMEDIATION>

## 2. Study Context
- Primary objective: {{primary_objective}}
- Scope / subtopics: {{subtopics}}
- Geography or sector focus: {{geography_focus}}
- Constraints or policies: {{policy_constraints}}
- Expected deliverables: {{expected_deliverables}}

## 3. Stage Plan (CLI-First)
### Stage 0 - Spec Alignment
- Purpose: confirm alignment with `AGENTS.md` and `tasks/blueprint.yaml`.
- CLI commands: `uv run tiangong-research sources list`, `uv run tiangong-research sources verify <id>`
- Auto-execution guard: confirm `<true|false>` setting from workspace blueprint and state whether Codex should continue without further confirmation.
- Outputs: readiness notes, blocked sources, escalation items.

### Stage 1 - Deterministic Acquisition
- Purpose: gather SDG matches, repositories, literature, carbon metrics, or KG data.
- CLI commands (select as needed):
  - `uv run tiangong-research research map-sdg <PATH_OR_TEXT>`
  - `uv run tiangong-research research find-code "<KEYWORDS>"`
  - `uv run tiangong-research research find-papers "<QUERY>"`
  - `uv run --group 3rd tiangong-research research get-carbon-intensity <GRID_ID>`
  - `uv run tiangong-research research query-kg --query <PATH_OR_QUERY>` *(when available)*
- Outputs: store JSON/Markdown under `.cache/tiangong/<STUDY_ID>/acquisition`.

### Stage 2 - Evidence Consolidation
- Purpose: normalise, deduplicate, and compute key metrics.
- Tooling: reuse cached CLI outputs; document any scripts invoked.
- Outputs: summary tables, merged datasets, cache paths.

### Stage 3 - Synthesis & Visualisation
- Purpose: translate deterministic evidence into insights or charts.
- CLI commands:
  - `uv run tiangong-research research synthesize "<QUESTION>" --prompt-template default --prompt-language en -P key=value`
  - `uv run tiangong-research research visuals verify`
  - `npx -y @antv/mcp-server-chart --transport streamable --spec <SPEC_PATH>` *(optional charts)*
- Outputs: synthesis report path, chart assets, traceability notes.

### Stage 4 - Wrap-up
- Purpose: record outcomes, blockers, follow-ups.
- Actions: archive outputs, update `tasks/backlog.yaml` when new work arises.
- Outputs: final summary, backlog entries, environment issues.

## 4. Deterministic Command Queue
1. `<CLI_COMMAND>` - purpose, key flags, expected output path.
2. `<CLI_COMMAND>` - purpose, key flags, expected output path.
3. *(Extend as required; keep order deterministic.)*

## 5. Reporting & Observability
- Output formats: <MARKDOWN_JSON_PDF>
- Observability tags: <TAG_LIST>
- Dry run mode: <true|false>
- Cache directory override (optional): <CACHE_PATH>
- Traceability: link each conclusion to the corresponding CLI output at `<OUTPUT_PATH>`.

## 6. Optional Extensions
- Deep Research trigger: enable only after deterministic evidence exists; specify scope and limits (<ITERATION_LIMIT>/<BUDGET>).
- AntV chart specification: <CHART_DESCRIPTION_AND_DESTINATION>
- Custom prompt packs or variables: <TEMPLATE_PATHS_AND_VALUES>

## 7. Fallbacks & Blockers
- Missing CLI capability -> deterministic Python fallback (`tiangong_ai_for_sustainability.<MODULE>.<FUNCTION>`) plus TODO to expose via CLI.
- External blockers (rate limits, missing credentials) -> <REMEDIATION_PLAN>.
- Escalation contact / next steps: <STAKEHOLDER_ACTIONS>.

## 8. Deliverable Checklist
- [ ] Structured summary covering objectives, methods, results, next steps.
- [ ] Stored CLI outputs (JSON/Markdown) at `<CACHE_PATH>`.
- [ ] Visualization assets referenced in the report.
- [ ] Backlog or TODO updates for identified gaps.
- [ ] Notes on rate limits, missing data, or dependency failures.