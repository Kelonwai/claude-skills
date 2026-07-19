---
name: choosing-agent-models
description: Use when dispatching subagents via the Agent tool, deciding whether to set a model or effort override on an agent, or authoring Workflow scripts with agent() calls
---

# Choosing Agent Models

## Overview

Route by **task nature**, not task importance. Three tiers:

- **Top tier** — strongest model available in your environment (e.g. fable, else opus): taste and judgment — design, critique, architecture decisions, adversarial verification, multi-agent synthesis
- **Mid tier** (e.g. sonnet): research, exploration, standard implementation, content drafting
- **Small tier** (e.g. haiku): mechanical bulk — inventory, format conversion, bulk extraction/classification

Adjacent tiers are typically several-fold apart in cost. Routing an inventory sweep down a tier is free money; routing a design critique down a tier is where cheap models fail worst.

## Quick Reference

| Task | Tier |
|---|---|
| Visual/frontend design, redesign, brand work | Top — via a pinned design agent (see Personalization) |
| Design critique, architecture decisions | Top |
| Adversarial verification, judging, final synthesis | Top — judges must outclass finders |
| Web/market research, competitor analysis | Mid — NOT top |
| Multi-file code exploration, standard implementation | Mid |
| Content drafting | Mid |
| File inventory, log scanning, TODO sweeps | Small |
| Format conversion, bulk extraction/classification | Small |
| Unsure, mixed-nature, or novel | **Omit `model`** — inherit the session model |

## When NOT to Override

Only pass `model` when the task clearly fits a row above — a wrong override is worse than none; omitting inherits the session model. If it's a single quick lookup, don't dispatch an agent at all.

## Workflow Scripts

For `agent(prompt, {model, effort})`:

- Mechanical stages (fetch, extract, classify): small or mid tier, `effort: 'low'`
- Judge/verify stages: top tier, higher effort — that's where hallucinations get caught
- Map→reduce→judge pattern: cheap finders, expensive judges

Instance: testing-with-synthetic-users runs its host top-tier, personas mid-tier.

## Personalization

Bind routing to agent types instead of re-deciding each time: define a custom agent in `~/.claude/agents/<name>.md` with `model:` pinned in its frontmatter (e.g. a `taste-designer` pinned top-tier for all design work). For recurring task types the table doesn't cover, fork this skill and add rows.

## Common Mistakes

| Mistake | Fix |
|---|---|
| "This research is complex, it needs the strongest model" | Complexity ≠ judgment. Research is mid tier; spend top tier verifying the findings instead |
| Routing design/taste work down-tier "to save cost" | Taste degrades worst on cheap models — always top tier |
| Paying top tier for grep sweeps or file inventories | Mechanical bulk is small tier |
| Guessing model id strings | Use your platform's exact ids (Claude Code: lowercase `"sonnet"`, `"opus"`, `"haiku"`, `"fable"`) |
| Overriding `model` on a custom agent that pins one | Its frontmatter already routes it — leave it alone |
