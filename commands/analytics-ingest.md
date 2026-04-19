---
description: Normalise a raw website analytics export (GA4, Plausible, Matomo, server logs) into cleaned input ready for analysis. Moves raw → input/cleaned and logs what was done.
argument-hint: "<path-to-raw-export> (or omit to process everything in input/raw)"
allowed-tools: Bash, Read, Write, Grep, Glob
---

# Analytics ingest

Clean and normalise raw web analytics exports. Intended for workspaces provisioned with the `web-analytics` variant.

## Steps

### 1. Resolve the target

- If `$ARGUMENTS` is a path, process that file.
- Otherwise, process every file in `input/raw/` that doesn't have a matching cleaned counterpart in `input/cleaned/`.

### 2. Detect the source

Sniff the first few rows / keys to identify the provider: GA4 export, Plausible CSV, Matomo export, raw server log, etc. If ambiguous, ask the user.

### 3. Normalise

For each source, produce a cleaned CSV with a consistent schema:

| Column | Notes |
|---|---|
| `date` | ISO 8601 |
| `page` | canonical URL path |
| `sessions` | integer |
| `users` | integer |
| `pageviews` | integer |
| `source` | traffic source / referrer |
| `medium` | organic / direct / referral / paid |
| `country` | ISO country code if present |

Drop columns that don't fit. Don't fabricate missing metrics — leave them empty.

### 4. Save cleaned output

Write to `input/cleaned/<stem>-cleaned.csv`. Log the ingest in `output/reports/ingest-log.md` with date, source, row counts in/out, and any columns dropped.

### 5. Archive the raw

Do not delete raw exports. If the workspace has `archive/old-data/`, move the raw file there once cleaning succeeds; otherwise leave it in `input/raw/`.
