# Document Triage Workspace

This workspace is for parsing incoming documents (news articles, white papers, academic papers, corporate filings, legal briefs) and deciding whether they're worth a deep read.

The agent acts as a skeptical triage reader: quick-scans, flags bias, extracts load-bearing claims, and produces a verdict.

## Layout

- `input/` — drop incoming files here. Supported: `.md`, `.txt`, `.pdf`, `.docx`, `.html`.
- `input/markdown/` — auto-populated by the triage command when conversion is needed.
- `output/` — generated triage outputs, one file per source document.
- `output/triage-index.md` — a running index of every document triaged.

## Workflow

1. Drop a document into `input/`.
2. Run `/workspace-foundational:triage-document` (or pass a specific path).
3. Read the verdict in `output/<stem>-triage.md`.
4. If verdict is `yes`, escalate to a deeper analysis prompt.

## Triage passes performed

- Quick scan (3-sentence summary)
- Source quality (author, funder, conflicts)
- Original thinking vs. consensus rehash
- Bias check (political, commercial, ideological)
- Claims to verify (top 3–5 load-bearing factual claims)
- Worth-the-deep-read verdict: `yes / skim / skip`
