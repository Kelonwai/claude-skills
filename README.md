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

This skill runs the second one: synthetic users with real motivations ("newcomer who can't read English product names", "reseller hunting mispriced items") walk your live deployment round after round. Findings get triaged 🔴🟡🟢, fixed, deployed, and re-tested from fresh angles — until a full round comes back clean. Not a QA checklist: a loop with an exit condition. Battle-tested across production features where it caught everything from XSS and TOCTOU races to "the feature is perfect but scores 3/10 on discoverability".

→ [`skills/testing-with-synthetic-users/`](skills/testing-with-synthetic-users/)

### ⭐ `choosing-agent-models`

Dispatching ten subagents on your strongest model because the task "feels important" burns money; sending a design critique to your cheapest one burns quality. This skill routes by task *nature* instead: taste and judgment (design, critique, adversarial verification) go to the top tier, research and implementation to the mid tier, mechanical bulk to the small tier — and when in doubt, no override at all. Baseline testing found agents' most reliable failure is "this research is complex, it needs the strongest model"; a week of production dogfooding found the second: "mid tier to be safe" on bulk work. Both are countered explicitly.

→ [`skills/choosing-agent-models/`](skills/choosing-agent-models/)

| Skill | What it's for | Status |
|---|---|---|
| [`testing-with-synthetic-users`](skills/testing-with-synthetic-users/) | Multi-round persona-driven testing loop for shipped features — find real-user breakage and UX friction before users do | RED/GREEN-tested + 4 production runs → [log](feedback/testing-with-synthetic-users.md) |
| [`choosing-agent-models`](skills/choosing-agent-models/) | Route subagent dispatches to the right model tier (taste → top, research → mid, mechanical → small) | GREEN-verified ×4 scenarios + 1-week production dogfood (7 sessions, 20+ dispatches) → [log](feedback/choosing-agent-models.md) |

## Method

Every skill here follows a TDD loop before release:

1. **RED** — run the scenario on an agent *without* the skill; document exactly how it fails
2. **GREEN** — write the skill against those specific failures; verify the agent now complies
3. **REFACTOR** — dogfood on real work, close the rationalization loopholes agents find

If a skill never watched an agent fail, it's documentation, not a skill.

The receipts are in the repo: `specs/` holds the writing brief each skill was authored against; `feedback/` holds the dogfood logs — real runs, observed failure modes, and the REFACTOR commits they drove.

## License

MIT
