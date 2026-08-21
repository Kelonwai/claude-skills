---
name: writing-good-skills
description: Use when writing, editing, or reviewing a SKILL.md — before drafting the frontmatter description, or when an existing skill fails to auto-trigger at the right moment, gets ignored or rationalized away by agents mid-task, fires on trivial work, or reads like a feature announcement. Also use when extracting a skill from a pattern admired in someone else's repo.
---

# Writing Good Skills

## Overview

The craft layer of skill authoring: what a well-written SKILL.md looks like. For the *process* (baseline test → write → close loopholes), **REQUIRED SUB-SKILL:** superpowers:writing-skills — not repeated here.

Core principle: **prose cannot enforce prose.** A model that under-executes instructions also under-executes the instruction to try harder. Every check below moves weight off goodwill and onto structure.

Baseline-tested 2026-08: two competent agents writing skills naturally both failed checks 1, 3, and 5. Those are the defaults you are correcting.

## The seven checks

**1. Description = symptoms only. Zero workflow.**
List the situations and phrasings that should trigger the skill; never what it does. Why: agents treat a workflow summary as the skill and skip the body (verified in superpowers testing).

**2. Every hard rule carries its why, in one line.**
"Write gates to a file — intentions don't survive a long context; files do." Why: a bare imperative gets rationalized away under pressure; a rule with visible logic can't be dodged as "spirit vs letter."

**3. If an outcome is observable, name the artifact that proves it.**
A file the agent must write, a command whose output decides, a template to fill — never "make sure X." Why: a check replaces tokens of self-assessment with a free subprocess, and evidence can't drift the way a promise can.

**4. Absolute rules need an honest exit.**
A visible way to surrender a requirement, with a reason, instead of silently narrowing scope. Why: a trapped agent doesn't comply — it lies. Declared abandonment is recoverable; silent isn't.

**5. Bound the floor: say what the skill is NOT for.**
The trivial cases that get normal effort, no ceremony. Why: a skill without a floor fires on one-line fixes until the user disables it.

**6. Compress the core rule into one line that sticks.**
"You do not promise you are done. You prove it against a ledger." Why: at minute 90, an aphorism is still retrievable; bullet-soup is not. One per skill — more and none stick.

**7. Keep the main file lean; split weight out.**
≤ ~500 words of instruction; heavy reference → `references/`, formats → `templates/`. Name outcomes, not one harness's tool names as the only path. Why: the body taxes every conversation it loads into; hardcoded tools silently break elsewhere.

## Review pass (editing an existing skill)

Ask in order: Would this trigger at the right moment? (1) — Which rule would I rationalize away first under deadline? (2–4) — When would I resent this firing? (5) — What line will I remember tomorrow? (6). A check that genuinely can't apply (a pure reference skill has no rules): skip it and say so — don't force ceremony onto a lookup table.

## What this skill is not

Not the authoring process (superpowers:writing-skills), not CLAUDE.md conventions, not one-off prompts. A skill used once doesn't need craft — it needs to be an ordinary instruction.
