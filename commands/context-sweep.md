---
description: Audit and prune a workspace's context/ directory — flag stale entries, duplicates, and bloat; propose compaction. Works in any workspace with a context/ folder, and extends to external RAG stores if the workspace uses a Pinecone MCP connection.
argument-hint: "[optional: path to the workspace root; defaults to cwd]"
allowed-tools: Bash, Read, Grep, Glob
---

# Context sweep

Audit the current workspace's context store and recommend pruning actions. Intended for workspaces provisioned with the `context-management` or `generic-workspace` variants, but works on any repo with a `context/` directory.

## Steps

### 1. Locate the context store

Primary source: `context/` directory at the workspace root (or the path in `$ARGUMENTS` if provided).

Secondary source: if the workspace uses an external Pinecone index (check `CLAUDE.md` for a Pinecone index + namespace), include the remote store in the audit via the Pinecone MCP server. Otherwise skip.

### 2. Inventory local context

For every file under `context/`:

- **Size** — flag unusually large files (> 200 KB markdown).
- **Age** — flag files older than 90 days that haven't been modified.
- **Topical overlap** — grep for duplicated headings or overlapping titles across files.
- **Orphans** — files that are no longer referenced from `CLAUDE.md`, `MEMORY.md`, or `CONTEXT.md`.

### 3. Inventory remote context (if applicable)

Run `describe-index-stats` on the configured Pinecone index to get record counts per namespace. If a `context-management` workspace has an `operations-log/`, cross-reference to flag records that were ingested but never referenced.

### 4. Produce a sweep report

Write a markdown report to `operations-log/sweep-YYYY-MM-DD.md` (context-management variant) or print to stdout (generic variant). Include:

- Bloat candidates (large or old files)
- Likely duplicates
- Orphaned files
- Recommended pruning actions (delete, merge, re-chunk)

### 5. Do not delete without confirmation

Never delete or upsert anything automatically. Present recommendations and wait for user confirmation before running destructive operations.
