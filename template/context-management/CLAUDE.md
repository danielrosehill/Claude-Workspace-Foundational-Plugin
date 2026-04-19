# Context Maintenance Workspace

This workspace manages an external RAG (Retrieval-Augmented Generation) vector store — typically via the Pinecone MCP server. The agent's role is to keep the knowledge base tidy: ingest new data, remove outdated entries, update records, and ensure data integrity.

## Configuration

Fill these in after provisioning:

- **Pinecone index**: `<set-after-provisioning>`
- **Namespace**: `<set-after-provisioning>`

Always confirm the target index and namespace before running destructive operations. If unsure, run `list-indexes` first.

## Layout

- `staging/` — raw source material queued for ingest. Chunk and upsert from here.
- `operations-log/` — a dated log of every ingest, update, or deletion operation.

## Core Operations

### Ingest

1. Read source material from `staging/` (files, URLs, or raw text).
2. Chunk into semantically coherent segments (aim for 200–500 tokens).
3. Generate metadata for each chunk (source, timestamp, topic, section).
4. Upsert records to the target index and namespace.
5. Append an entry to `operations-log/YYYY-MM-DD.md`.

### Prune

1. Run `/workspace-foundational:context-sweep` to flag stale or orphaned records.
2. Confirm removals with the user before deleting.
3. Log every deletion in `operations-log/`.

### Update

1. Identify records that need refreshed metadata or text.
2. Upsert replacements (same id, new values).
3. Log in `operations-log/`.

## Guardrails

- Never run destructive operations without user confirmation.
- Always log ingests/updates/deletions with timestamps and record counts.
- Confirm the index + namespace on every session's first op.
