---
description: Triage a document dropped into a workspace's input/ directory — quick scan, bias check, claim extraction, and a worth-the-deep-read verdict. Writes structured output into output/.
argument-hint: "<path-to-document> (or omit to triage everything new in input/)"
allowed-tools: Bash, Read, Write, Grep
---

# Triage document

Quick-scan a document and decide whether it warrants a deep read. Intended for workspaces provisioned with the `report-parsing` variant, but works in any workspace that has `input/` and `output/` directories.

## Steps

### 1. Resolve the target

- If `$ARGUMENTS` is a path, triage that single file.
- Otherwise, scan `input/` for files that don't yet have a matching `output/<stem>-triage.md`.

### 2. Convert to markdown if needed

If the file is a PDF, docx, or HTML, convert to markdown first (via `pandoc` or `markitdown`) and drop the converted copy in `input/markdown/`. Skip if it's already `.md` or `.txt`.

### 3. Run the triage passes

For each file, run these passes and write them as sections into `output/<stem>-triage.md`:

- **Quick scan** — 3-sentence summary of what the document claims.
- **Source quality** — who wrote it, who funded it, any declared conflicts of interest.
- **Original thinking** — does this add new analysis, or is it rehashing consensus?
- **Bias check** — what biases or framing choices are visible? Political, commercial, ideological.
- **Claims to verify** — the 3–5 most load-bearing factual claims, each with a note on how they could be checked.
- **Worth the deep read?** — verdict: `yes / skim / skip`, with 1 sentence justifying it.

### 4. Summary index

Append a one-line entry to `output/triage-index.md` per triaged file: date, filename, verdict, top bias concern.

### 5. Do not rewrite the original

The file in `input/` is immutable from the agent's perspective. All derived artifacts go into `output/`.
