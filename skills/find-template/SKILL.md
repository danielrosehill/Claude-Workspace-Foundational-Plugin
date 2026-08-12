---
name: find-template
description: Recommend the best-fit workspace template or plugin variant for a described use case, project idea, or research question. Use when the user asks "what template should I use for X", "is there a scaffold for Y", "which workspace fits Z best", "recommend a template for…", or describes a project without naming a template. Returns a short ranked shortlist with rationale — does not create the repo. If the user wants to proceed, hand off to the matching cluster plugin's `new-workspace` skill.
---

# Find Template

Propose the best-fit workspace scaffold for a described use case. This is an **advisory** skill — it does not create a repo. When the user then wants to spin one up, delegate to the appropriate cluster plugin's `new-workspace` skill (e.g. `/research-space:new-workspace`, `/workspace-foundational:new-workspace`, etc.) with the chosen variant.

## Background

This skill absorbs the recommender from the retired `new-repo-from-template` plugin. In the new model, each of the ~27 cluster plugins in the danielrosehill marketplace bundles its own variants; this skill's job is to route the user to the right plugin + variant, not to enumerate 111 standalone template repos.

## Inputs

The user describes what they want to do — e.g.:

- "what template should I use to research distribution platforms for Suno-created music?"
- "got a template for tracking expenses across multiple family members?"
- "which scaffold fits best for a single-company deep dive on a potential acquisition target?"
- "I want to map the AI coding assistant ecosystem — anything for that?"
- "I need to parse a stack of PDF reports and pull out the key claims."

Extract the *intent* (what the workspace is for), not just surface keywords.

## Steps

### 1. Classify the intent

Bucket the request into one of:

- **Research / investigation** → likely `research-space` plugin
- **Report parsing / document triage** → `workspace-foundational` (report-parsing variant)
- **Context / RAG maintenance** → `workspace-foundational` (context-management variant)
- **Inventory / asset tracking** → `workspace-foundational` (inventory-analysis variant)
- **Web analytics** → `workspace-foundational` (web-analytics variant)
- **Career / job search / salary** → `career` plugin
- **Purchasing / procurement — a decision between known options** → `purchasing` plugin (general-purchasing variant)
- **Procurement research — "what's on the market that meets this spec?"** → `purchasing` plugin (market-landscape variant)
- **Region-specific shopping** → `shopping` plugin
- **Knowledge base / wiki / SOP** → `knowledge-documentation` plugin
- **Content writing / blog / thinkpiece** → `content-writing` plugin
- **Household / personal finance** → `personal-finance` plugin
- **Filesystem organisation** → `filesystem-organiser` plugin
- **Brainstorming / idea generation on a subject** → `ideation-planning` plugin (ideation-session variant)
- **Deep evaluation of one idea** → `ideation-planning` plugin (single-idea-eval variant), or the `business-idea-eval` plugin when the idea is commercial
- **Ranking or shortlisting many candidates** → `ideation-planning` plugin (multi-idea-ranking variant)
- **Low-friction idea inbox** → `ideation-planning` plugin (idea-capture variant)
- **Choosing between defined options** → `decision-evaluation-framework` plugin
- **Generic ongoing workspace** → `workspace-foundational` (generic-workspace variant)

If the request genuinely doesn't fit, say so and suggest the generic-workspace variant as a starting point.

### 1b. Disambiguate decision vs survey

Several clusters split along the same seam, and it's the commonest way this skill routes wrong. Ask which side the user is on whenever it isn't explicit:

- **"Which of these should I get?"** — the options are known, the output is a pick. → `purchasing:general-purchasing`
- **"What's even out there that does this?"** — the spec is known, the options aren't, the output is a map of the field and its gaps. → `purchasing:market-landscape`

Signals for the survey side: the user states a requirement rather than a product; asks what the "lay of the land" is; doesn't know whether the thing exists; wants to know what's available *currently*; or says they're not buying yet. A survey can legitimately conclude "this doesn't exist" — if that outcome would be useful to them, they want the landscape variant.

The same seam separates `research-space:ecosystem` (mapping a technology or vendor ecosystem for understanding) from `purchasing:market-landscape` (mapping a market against a buying spec, with conformance and price bands). If there's a spec with must-haves behind the question, it's the purchasing one.

### 2. Return a shortlist

Output **2–4 ranked recommendations**. Each entry:

```
**<plugin-name>:<variant>** — <one-line purpose>
Why: <rationale tying the variant's purpose to the user's stated intent>
```

- **Top pick** with a 1–2 sentence rationale.
- **Runner-ups** with one line each on why they might also fit and why they're ranked below.
- **If nothing fits well**: suggest `workspace-foundational:generic-workspace` and note the user could always adopt a more specific cluster later.

### 3. Offer the next step

End with a single line offering to proceed: "Want me to spin up `<plugin-name>:<variant>` as a new repo? (I'll hand off to `/<plugin-name>:new-workspace`.)"

Do not auto-invoke any `new-workspace` skill — wait for the user to confirm.

## Guardrails

- Never invent plugins or variants that don't exist in the danielrosehill marketplace.
- If the user's description is ambiguous, ask one clarifying question before recommending (e.g. "Is this a one-off report or an ongoing workspace?").
- Don't dump the full cluster map — shortlist only.
- Prefer narrower variants (e.g. `inventory-analysis`) over generic ones (`generic-workspace`) when the intent clearly fits.
