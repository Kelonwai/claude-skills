# Spec: writing-good-skills

## Problem

Skills written naturally by agents (and humans) tend to fail in predictable ways —
not in what they teach, but in how they're written. The knowledge is fine; the
*craft* is missing. Observed failure modes (from analyzing weak skills vs. strong
ones like [unlazy](https://github.com/Leonxlnx/unlazy)):

1. **Feature-list descriptions** — frontmatter describes what the skill does, not
   the symptoms that should trigger it. Skill never auto-loads at the right moment.
2. **Workflow-summarizing descriptions** — agent follows the description shortcut
   and skips the skill body (documented in superpowers:writing-skills testing).
3. **Rules without why** — bare imperatives get rationalized away under pressure.
4. **Prose enforcement** — "make sure X" instead of a runnable check or a file
   artifact. A model that under-executes instructions also under-executes the
   instruction to not under-execute.
5. **No escape hatch** — absolute rules with no honest exit push the agent to
   silently narrow scope or lie instead of visibly surrendering.
6. **No scope limits** — skill fires on trivial tasks, burning tokens and goodwill.
7. **Bullet-soup style** — long enumerations that don't stick in context; no
   memorable compression of the core rule.

## Position vs. existing skills

Complements superpowers:writing-skills (the *process*: TDD loop, baseline test,
close loopholes). This skill is the *taste layer*: what a well-crafted SKILL.md
looks like regardless of process. Cross-reference, don't duplicate.

## Success criteria

A subagent given "write a SKILL.md for X" with this skill loaded should produce:

- description that starts "Use when", lists symptoms/situations, no workflow summary
- every hard rule carries a one-line rationale
- enforcement pushed into artifacts (files, CHECK commands, templates) wherever the
  outcome is observable
- an honest escape hatch for rules that can become impossible
- a "not for" section bounding scope
- main file lean; heavy reference split out

Baseline (no skill) should measurably miss several of these — verified before
writing (RED), re-tested after (GREEN).

## Source material

Pattern extraction from unlazy v2 (Leonxlnx), superpowers:writing-skills,
anthropic skill authoring guidance, and this repo's own two skills.
