# workspace-foundational

Claude Code plugin for generic workspace patterns that don't fit a specific domain — setup, context/memory maintenance, report parsing, inventory analysis, web analytics, and template discovery.

Part of the [danielrosehill Claude Code marketplace](https://github.com/danielrosehill/Claude-Code-Plugins).

## What you get

### Primitives (always available once the plugin is installed)

**Workspace setup commands** (`/workspace-foundational:*`):
- `setup-workspaces` — interactive discovery + cloning of existing Claude Code workspace templates from a curated catalog
- `context-sweep` — audit and prune a workspace's context/ directory
- `triage-document` — quick-scan a document dropped into a workspace's `input/`
- `inventory-scan` — detect duplicates, anomalies, and high-value items in imported inventory data
- `analytics-ingest` — normalise raw web analytics exports into cleaned inputs

**Skills**:
- `new-workspace` — provisioning skill that scaffolds a workspace on disk from one of six variants
- `find-template` — advisory skill that recommends the best-fit template/variant for a described use case (absorbed from the retired `new-repo-from-template` plugin)

## Variants

| Variant | Purpose |
|---|---|
| `generic-workspace` | A general-purpose Claude Code workspace: `CLAUDE.md`, `context/`, `outputs/`, `prompts/` |
| `context-management` | Maintaining an external RAG / Pinecone-backed context store with operations log + staging |
| `report-parsing` | Dropping reports into `input/` for triage, bias-check, and deep analysis into `output/` |
| `inventory-analysis` | Ingesting inventory CSV/JSON exports, analysing for duplicates/anomalies/value |
| `web-analytics` | Structured workspace for ingesting and reporting on website analytics data |
| `template-discovery` | A lightweight sandbox for browsing and evaluating template scaffolds before committing |

## Pattern

Primitives live in the plugin → globally available from any cwd.
Workspace scaffolds are provisioned as **data** → no `.claude/` tree inside provisioned workspaces.
Plugin updates never touch your workspace data.

See [PLAN.md in Claude-Workspace-Reshaping-190426](https://github.com/danielrosehill/Claude-Workspace-Reshaping-190426) for the full pattern spec this plugin follows.

## Install

Via the danielrosehill marketplace:

```
/plugin marketplace add danielrosehill/Claude-Code-Plugins
/plugin install workspace-foundational
```

## Provenance

This plugin consolidates two previously-separate plugins and seven template repos as part of the Wave 3 refactor:

- Absorbed plugin: `workspace-setup` (the `setup-workspaces` command).
- Absorbed plugin: `new-repo-from-template` (the `recommend-template` skill, renamed to `find-template`). The 111 bundled template scaffolds are handled separately by the per-cluster plugins.
- Absorbed templates: Claude-Report-Parsing-Space-Template, Claude-Research-Space-Public-Template, Claude-Research-Workspace-General-Template, Claude-Web-Analytics-Space, Context-Maintenance-Template, Inventory-Analyst-Template, Report-Parsing-Space-Template.

## License

MIT.
