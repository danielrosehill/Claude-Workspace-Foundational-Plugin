# Inventory Analyst Workspace

This workspace analyses inventory data exports. The agent acts as an inventory analyst: imports exports, detects duplicates and anomalies, ranks high-value items, and produces actionable reports.

## Layout

- `imports/` — drop raw inventory exports here (CSV, JSON, XLSX).
- `analysis/` — intermediate working files, notebooks, pivot tables.
- `reports/` — finished analysis reports, one per scan.

## Workflow

1. Drop an export into `imports/`.
2. Run `/workspace-foundational:inventory-scan`.
3. Review the generated report in `reports/<stem>-scan-YYYY-MM-DD.md`.
4. Action the recommendations (merge duplicates, fill missing fields, archive discard candidates).

## Analysis passes

- Duplicate detection (name similarity, matching part numbers)
- Anomaly detection (missing fields, outlier values, inconsistent naming)
- High-value item ranking (for insurance / security / tracking)
- Discard / deprecation candidates
- Category distribution

## Guardrails

- Never modify the source export in `imports/`.
- Always date-stamp reports.
- Ask the user to confirm column mappings if they're ambiguous.
