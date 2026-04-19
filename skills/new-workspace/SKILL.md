---
name: new-workspace
description: Provision a new workspace-foundational workspace on disk. Use when the user wants to start a generic workspace, context-management store, report-parsing space, inventory analysis workspace, web analytics workspace, or a template-discovery sandbox. Accepts a workspace name and optional variant. Scaffolds the workspace, personalises CLAUDE.md from the user's global memory, and (by default) creates a GitHub repo.
disable-model-invocation: true
allowed-tools: Bash(mkdir *), Bash(cp *), Bash(cat *), Bash(git init *), Bash(git add *), Bash(git commit *), Bash(gh repo create *), Bash(gh auth status), Bash(git push *), Read
---

# Provision workspace-foundational Workspace

Creates a new generic-purpose workspace. This plugin's commands (`/workspace-foundational:setup-workspaces`, `/workspace-foundational:context-sweep`, etc.) are globally available once installed — this skill only provisions the **data scaffold** (CLAUDE.md, context/, input/, output/, etc.) those commands read from and write to.

## Arguments

`$ARGUMENTS` is parsed as:

- **First positional**: workspace name (kebab-case, used as directory and GitHub repo name). Required.
- **Second positional** (optional): target parent path. Defaults to `~/repos/github/my-repos`.
- **`--variant=<name>`** (optional): which scaffold to copy. Default: `generic-workspace`. Valid values:
  - `generic-workspace`
  - `context-management`
  - `report-parsing`
  - `inventory-analysis`
  - `web-analytics`
  - `template-discovery`
- **`--local-only`** (optional): skip GitHub repo creation and push. Default: create a public GitHub repo and push.
- **`--private`** (optional): create the GitHub repo as private. Default: public.

### Examples

```
/workspace-foundational:new-workspace my-notes
/workspace-foundational:new-workspace reports-q2 --variant=report-parsing
/workspace-foundational:new-workspace garage-inventory --variant=inventory-analysis
/workspace-foundational:new-workspace site-analytics --variant=web-analytics --local-only
/workspace-foundational:new-workspace rag-store --variant=context-management
```

## Procedure

### 1. Parse arguments

Extract workspace name, target parent path, variant, and flags from `$ARGUMENTS`. If workspace name is missing, ask the user for it before proceeding.

### 2. Resolve the scaffold path

The bundled scaffold lives at `${CLAUDE_SKILL_DIR}/../../template/<variant>/`. Confirm it exists. If the variant isn't one of the six listed above, tell the user which variants are available.

### 3. Read ambient facts

Read `~/.claude/CLAUDE.md` if it exists. Extract OS, locale, timezone, and user identity facts. These will personalise the workspace's CLAUDE.md at step 5.

### 4. Create the workspace directory

```bash
mkdir -p <target-parent>/<workspace-name>
cp -r ${CLAUDE_SKILL_DIR}/../../template/<variant>/. <target-parent>/<workspace-name>/
```

Do **not** copy any `.claude/` tree. The plugin's primitives are global.

### 5. Personalise CLAUDE.md

Open the new workspace's `CLAUDE.md` and:

- Replace any placeholder identity/OS/locale with the facts from step 3.
- Add a short header noting the workspace name and variant.

### 6. Prompt for workspace-specific facts

Ask the user only for facts the variant actually needs:

- **context-management**: the Pinecone index + namespace this workspace will maintain.
- **report-parsing**: the kind of report being parsed (news, academic, legal, etc.) so the triage prompts can tune themselves.
- **inventory-analysis**: the source system the inventory export came from (Snipe-IT, Airtable, custom CSV, etc.).
- **web-analytics**: which analytics provider the exports are from (GA4, Plausible, Matomo, etc.).
- **generic-workspace** / **template-discovery**: no extra prompts by default.

### 7. Initialise git and (optionally) publish

```bash
cd <target-parent>/<workspace-name>
git init
git add .
git commit -m "Initial workspace from workspace-foundational plugin"
```

Unless `--local-only` is set:

```bash
gh repo create <workspace-name> --<public|private> --source=. --push
```

Use `--public` by default, `--private` if flag was passed.

### 8. Print next steps

Tell the user:

- Workspace path and variant.
- Which plugin primitives apply to the chosen variant (e.g. `/workspace-foundational:report-triage` for report-parsing, `/workspace-foundational:inventory-scan` for inventory-analysis).
- For `template-discovery`: suggest running the `find-template` skill when they describe a new project.
- Reminder that the workspace is **data** — they can delete/move it freely without losing the plugin's commands.

## Notes

- The scaffold path must be resolved via `${CLAUDE_SKILL_DIR}/../../template/` (not `${CLAUDE_PLUGIN_ROOT}` — that variable isn't exported in skill bash injection, only in hooks/MCP).
- Never copy `.claude/commands/`, `.claude/agents/`, or `.claude/skills/` into the new workspace.
- Don't hard-code any personal paths or identifiers here — everything comes from user memory or prompts.
