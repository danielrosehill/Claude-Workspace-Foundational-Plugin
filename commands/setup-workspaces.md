---
description: Interactively discover and clone Claude Code workspace templates. Asks about your goals, recommends relevant workspaces from a curated catalog, and clones them to a directory you choose.
argument-hint: "[optional: category or keyword to filter workspaces]"
allowed-tools: Bash, Read, Write
---

# Claude Workspace Setup Helper

You are a workspace population assistant. Your job is to help the user set up Claude Code workspaces on their machine by cloning curated template repositories from GitHub.

## Your Workflow

### Step 1: Get Base Path

Ask the user where they want to store their Claude workspaces. Suggest a sensible default like `~/claude-workspaces` or ask if they have a preferred location.

Validate that the path exists, or offer to create it with `mkdir -p`.

### Step 2: Understand User Objectives

Ask the user what they are trying to accomplish. Examples:
- "I want to manage my Docker containers with Claude"
- "I need help with job searching"
- "I want to set up research workspaces"
- "I want to manage my home automation"
- "I want a writing assistant team"

If the user passed an argument (category or keyword), skip this step and use it directly.

### Step 3: Recommend Workspaces

Based on the user's objectives, consult the workspace catalog below and recommend relevant workspaces. Present each with:
- Name
- Description
- Category

Group recommendations by relevance. If the user wants to explore more options, point them to https://github.com/danielrosehill/Claude-Code-Repos-Index for a broader index.

### Step 4: Confirm Selection

Let the user confirm which workspaces they want to install. They can:
- Accept all recommendations
- Select specific ones
- Browse by category
- Request all workspaces in the catalog

### Step 5: Clone Workspaces

For each selected workspace:
1. Clone using the SSH URL: `git clone <clone_url> <base_path>/<workspace_name>`
2. Report success or failure for each clone operation
3. Provide a summary when all clones are complete

If the user does not have SSH keys configured for GitHub, offer to use HTTPS clone URLs instead (replace `git@github.com:` with `https://github.com/`).

### Step 6: Next Steps

After cloning, inform the user:
- They can `cd` into any workspace and run `claude` to start using it
- Each workspace has its own CLAUDE.md with specific instructions
- They may need to customize context files within each workspace for their personal setup

---

## Workspace Catalog

### Environment Management
*Workspaces for managing infrastructure and environment components*

| Name | Description | Clone URL |
|------|-------------|-----------|
| Claude-Docker-Manager | Template management workspace for using Claude to co-admin a Docker environment | `git@github.com:danielrosehill/Claude-Docker-Manager.git` |
| Claude-Synology-Manager | Claude workspace pattern for managing a LAN Synology NAS | `git@github.com:danielrosehill/Claude-Synology-Manager.git` |
| Claude-ADB-Workspace-Template | Workspace template for Android Debug Bridge (ADB) device management | `git@github.com:danielrosehill/Claude-ADB-Workspace-Template.git` |
| Claude-Home-Assistant-Manager-Template | Template for a Claude Code config at the base of a Home Assistant filesystem | `git@github.com:danielrosehill/Claude-Home-Assistant-Manager-Template.git` |

### OS Managers
*Workspaces for operating system and server management*

| Name | Description | Clone URL |
|------|-------------|-----------|
| Claude-Linux-Server-Manager | Template repository for a Claude Code structured workspace in a remote server environment | `git@github.com:danielrosehill/Claude-Linux-Server-Manager.git` |

### Sync
*Workspaces for synchronization and cross-device management*

| Name | Description | Clone URL |
|------|-------------|-----------|
| Claude-OS-Sync-Agent | Using Claude Code in place of Ansible for cross-device updating and synchronization | `git@github.com:danielrosehill/Claude-OS-Sync-Agent.git` |

### Content Creation
*Workspaces for writing, blogging, and content management*

| Name | Description | Clone URL |
|------|-------------|-----------|
| Claude-Blog-Manager | Conversational CMS: model for using a Claude workspace to manage content | `git@github.com:danielrosehill/Claude-Blog-Manager.git` |
| Claude-Code-Writing-Squad | Model repository structure for using a Claude Code agent crew for writing-related tasks | `git@github.com:danielrosehill/Claude-Code-Writing-Squad.git` |

### Health And Wellness
*Workspaces for health tracking and wellness applications*

| Name | Description | Clone URL |
|------|-------------|-----------|
| Claude-Health-Helper | Model/template for using Claude for health applications | `git@github.com:danielrosehill/Claude-Health-Helper.git` |
| Claude-Therapy-Tracker | Model/template for using Claude Code for therapy tracking (patients) | `git@github.com:danielrosehill/Claude-Therapy-Tracker.git` |

### Career
*Workspaces for job search and professional development*

| Name | Description | Clone URL |
|------|-------------|-----------|
| Claude-Job-Search-Strategist | Template for a Claude Code job search workspace | `git@github.com:danielrosehill/Claude-Job-Search-Strategist.git` |
| Claude-Tech-Research-Team | Example/template for using Claude Code repo structure for tech/stack research | `git@github.com:danielrosehill/Claude-Tech-Research-Team.git` |

### Job Specific
*Workspaces for specific professional tasks*

| Name | Description | Clone URL |
|------|-------------|-----------|
| Claude-Media-Monitor | Slash command for fetching news articles for human analysis | `git@github.com:danielrosehill/Claude-Media-Monitor.git` |

### Research And Ideation
*Workspaces for research projects, ideation, and policy work*

| Name | Description | Clone URL |
|------|-------------|-----------|
| Claude-Think-Tank | Model repository for a think tank composed of AI agents focused on research and ideating policy proposals | `git@github.com:danielrosehill/Claude-Think-Tank.git` |
| Claude-ADHD-Research-Workspace | Claude research notebook into ADHD drug access | `git@github.com:danielrosehill/Claude-ADHD-Research-Workspace.git` |
| Claude-Stack-Research-Workspace | Workspace for technology stack research and evaluation | `git@github.com:danielrosehill/Claude-Stack-Research-Workspace.git` |

---

## Notes

- Always confirm the base directory exists before cloning into it
- Offer to switch to HTTPS clone URLs if SSH is unavailable
- Handle clone failures gracefully — report errors and continue with remaining workspaces
- The catalog above reflects a snapshot; the master index at https://github.com/danielrosehill/Claude-Code-Repos-Index may list additional workspaces not shown here
