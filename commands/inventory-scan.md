---
description: Analyse an inventory data export (CSV / JSON) for duplicates, anomalies, high-value items, and discard candidates. Writes a structured report into reports/.
argument-hint: "<path-to-inventory-export> (or omit to scan the newest file in imports/)"
allowed-tools: Bash, Read, Write, Grep, Glob
---

# Inventory scan

Analyse an inventory export and produce actionable insights. Intended for workspaces provisioned with the `inventory-analysis` variant.

## Steps

### 1. Resolve the target

- If `$ARGUMENTS` is a path, analyse that file.
- Otherwise, pick the most recently modified file in `imports/`.

### 2. Load and normalise

Load the CSV / JSON / similar tabular export. Identify:

- Primary key column (id / sku / asset tag)
- Name / title column
- Value / price column
- Category, location, serial number columns if present

If columns are ambiguous, ask the user to confirm the mapping before proceeding.

### 3. Analysis passes

Write each as a section of `reports/<stem>-scan-YYYY-MM-DD.md`:

- **Duplicate detection** — near-identical names, matching model/part numbers, overlapping descriptions.
- **Anomalies** — missing fields, outlier values vs. category peers, inconsistent naming, contradictory metadata.
- **High-value items** — top N items by recorded or estimated value; flag those with thin documentation.
- **Discard / deprecation candidates** — items flagged stale, broken, or superseded.
- **Category distribution** — basic counts per category, with flags for under-populated categories.

### 4. Recommendations

Close the report with a prioritised action list: merges, field fill-ins, re-categorisations, archival candidates.

### 5. Non-destructive

Never modify the source export. All outputs go into `reports/` (and optionally `analysis/` for intermediate working files).
