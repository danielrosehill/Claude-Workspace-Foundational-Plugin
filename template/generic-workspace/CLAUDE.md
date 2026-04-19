# Generic Workspace

A general-purpose Claude Code workspace. Use it as a starting point when no more specific workspace shape fits.

## Layout

- `context/` — background material the agent should read before acting. Drop any relevant docs, notes, or reference files here.
- `outputs/` — generated artifacts: analyses, summaries, drafts.
- `prompts/` — saved prompts you re-use in this workspace.

## Relevant plugin primitives

Provisioned by `workspace-foundational`. Useful commands in this workspace:

- `/workspace-foundational:context-sweep` — audit and prune the `context/` directory.
- `/workspace-foundational:find-template` (skill) — ask if a more specific workspace type suits a new project.

## How to use

1. Drop context files into `context/`.
2. Write prompts interactively or save re-usable ones in `prompts/`.
3. Save notable outputs to `outputs/`.
