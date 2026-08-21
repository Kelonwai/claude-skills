---
name: choosing-agent-models
description: Use when about to dispatch subagents or parallel agents — the user asks to fan out, split work across multiple agents, or delegate research, implementation, or review to workers — when choosing a model or effort override for any agent, or when authoring workflow scripts with agent() calls
---

# Choosing Agent Models

## Overview

Route by **task nature**, not task importance. Three tiers:

- **Top tier** — strongest available (e.g. fable, else opus): taste and judgment — design, critique, architecture decisions, adversarial verification, multi-agent synthesis
- **Mid tier** (e.g. sonnet): research, exploration, standard implementation, content drafting
- **Small tier** (e.g. haiku): mechanical bulk — inventory, format conversion, bulk extraction/classification (heuristic: many similar items, no per-item judgment)

Adjacent tiers differ several-fold in cost. Routing an inventory sweep down a tier is free money; sending a design critique down is where cheap models fail worst.

## Quick Reference

| Task | Tier |
|---|---|
| Visual/frontend design, redesign, brand work | Top — via a pinned design agent (see Personalization) |
| Design critique, architecture decisions | Top |
| Adversarial verification, judging, final synthesis | Top — judges must outclass finders |
| Web/market research, competitor analysis | Mid — NOT top |
| Multi-file code exploration, standard implementation | Mid |
| Content drafting | Mid |
| Repo/log sweeps — "map X, report a structured list" | Small — a report output doesn't make a sweep judgment work |
| Format conversion, bulk extraction/classification | Small |
| Unsure, mixed-nature, or novel | **Omit `model`** — inherit the session model |

## When NOT to Override

Only pass `model` when the task clearly fits a row above — a wrong override is worse than none. A single quick lookup doesn't need an agent at all.

## Workflow Scripts

For `agent(prompt, {model, effort})`:

- Mechanical stages (fetch, extract, classify): small or mid tier, `effort: 'low'`
- Judge/verify stages: top tier, higher effort — that's where hallucinations get caught
- Map→reduce→judge pattern: cheap finders, expensive judges

Instance: testing-with-synthetic-users runs its host top-tier, personas mid-tier.

## Capped Top Tier

If your plan caps top-tier usage: that quota belongs to the main loop — orchestration, triage, final synthesis. Dispatch the table's "Top" work one tier down instead (judges, design, heavy implementation); the main loop reviewing their output IS the top-tier quality pass. **The top model thinks; agents do.** One tier down is typically half the price and still outclasses mid-tier finders — "judges must outclass finders" holds.

## Personalization

Bind routing to agent types: define a custom agent in `~/.claude/agents/<name>.md` with `model:` pinned in its frontmatter (e.g. a `taste-designer` pinned top-tier). For recurring task types the table doesn't cover, fork this skill and add rows.

## Common Mistakes

| Mistake | Fix |
|---|---|
| "This research is complex, it needs the strongest model" | Complexity ≠ judgment. Research is mid tier; spend top tier verifying the findings instead |
| Routing design/taste work down-tier "to save cost" | Taste degrades worst on cheap models — always top tier |
| Paying top or mid tier for sweeps, inventories, bulk extraction | Mechanical bulk is small tier — "mid tier to be safe" is the most common silent overspend |
| Guessing model id strings | Use your platform's exact ids (Claude Code: `"sonnet"`, `"opus"`, `"haiku"`, `"fable"`) |
| Overriding `model` on a custom agent that pins one | Its frontmatter already routes it — leave it alone |
