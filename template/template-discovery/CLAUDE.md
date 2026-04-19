# Template Discovery Sandbox

A lightweight workspace for browsing and evaluating Claude Code workspace templates before committing to one.

## Purpose

Use this sandbox when you're not sure which cluster plugin's workspace variant best fits a new project. Work through the decision here without cluttering a real workspace.

## Workflow

1. Describe the project you want to spin up (in `notes/project-idea.md` or interactively).
2. Invoke the `find-template` skill: `/workspace-foundational:find-template "<one-line description>"`.
3. The skill returns 2–4 ranked recommendations pointing at specific `<plugin>:<variant>` combinations.
4. Once you've picked one, run the matching plugin's `new-workspace` skill to provision the real workspace (in a different location).
5. Optionally delete this sandbox once the real workspace is live.

## Layout

- `notes/` — project-idea sketches and notes-to-self while deciding.

## Related

- `/workspace-foundational:find-template` — the advisory skill.
- `/workspace-foundational:new-workspace` — to provision one of this plugin's six variants.
- Other cluster plugins' `new-workspace` skills for domain-specific workspaces.
