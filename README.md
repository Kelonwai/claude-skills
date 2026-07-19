# claude-skills

Agent skills distilled from real production workflows. TDD-tested against baseline agent behavior before release — each skill exists because agents demonstrably get it wrong without one.

## Install

```bash
npx skills add Kelonwai/claude-skills
```

Or copy a single skill into your personal skills directory:

```bash
git clone https://github.com/Kelonwai/claude-skills
cp -r claude-skills/skills/<name> ~/.claude/skills/
```

## Skills

### ⭐ `testing-with-synthetic-users`

Your feature works. Your tests pass. And your users still bounce — because "works" and "usable" are different tests.

This skill runs the second one: synthetic users with real motivations ("newcomer who can't read English product names", "reseller hunting mispriced items") walk your live deployment round after round. Findings get triaged 🔴🟡🟢, fixed, deployed, and re-tested from fresh angles — until a full round comes back clean. Not a QA checklist: a loop with an exit condition.

Battle-tested on production e-commerce features, where it caught everything from XSS and TOCTOU races to "the feature is perfect but scores 3/10 on discoverability".

→ [`skills/testing-with-synthetic-users/`](skills/testing-with-synthetic-users/)

### `choosing-agent-models`

Route subagent dispatches by task **nature**, not task importance: taste and judgment go to the top model tier, research and implementation to mid, mechanical bulk to small. Kills the classic over-spend ("this research is complex, better use the biggest model") and the classic under-spend (design critique on a small model). Includes workflow-script routing (cheap finders, expensive judges) and a personalization pattern using pinned custom agents.

→ [`skills/choosing-agent-models/`](skills/choosing-agent-models/)

## Method

Every skill here follows a TDD loop before release:

1. **RED** — run the scenario on an agent *without* the skill; document exactly how it fails
2. **GREEN** — write the skill against those specific failures; verify the agent now complies
3. **REFACTOR** — dogfood on real work, close the rationalization loopholes agents find

If a skill never watched an agent fail, it's documentation, not a skill.

## License

MIT
