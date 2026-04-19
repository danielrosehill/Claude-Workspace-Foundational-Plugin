# Web Analytics Workspace

A structured workspace for ingesting, cleaning, analysing, and reporting on website analytics data (GA4, Plausible, Matomo, server logs).

## Layout

```
.
├── input/
│   ├── raw/           # Raw exports (CSV / JSON) — unprocessed
│   └── cleaned/       # Normalised data ready for analysis
├── output/
│   ├── reports/       # Markdown / PDF analysis output
│   └── visualizations/# Charts and graphs
├── archive/           # Historical raw data and older reports
├── context/           # Site context: URLs, goals, KPIs
└── scripts/           # Utility scripts for repeat analysis
```

## Workflow

1. Drop a raw export into `input/raw/`.
2. Run `/workspace-foundational:analytics-ingest` to normalise into `input/cleaned/`.
3. Ask targeted analysis questions against `input/cleaned/` — the agent produces markdown reports in `output/reports/` and charts in `output/visualizations/`.
4. Periodically archive stale data into `archive/`.

## Supported sources

- GA4 exports (CSV)
- Plausible exports
- Matomo exports
- Raw server access logs

## Normalised schema

Cleaned CSV columns: `date`, `page`, `sessions`, `users`, `pageviews`, `source`, `medium`, `country` (ISO code).
