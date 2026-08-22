# claude-skills

```bash
npx skills add Kelonwai/claude-skills
```

A Claude Code skill is a folder holding a `SKILL.md` that the agent loads by itself when your request matches it — standing instructions for a recurring task, not a prompt you paste each time. This repo gives you three: one that stress-tests shipped features with synthetic users, one that routes each subagent you dispatch to the right model tier, one that keeps the skills you write from being ignored mid-task. Every skill here was TDD-tested against baseline agent behavior before release — it exists because agents demonstrably get it wrong without it.

## Which skill do I want?

| Skill | Use it when | What it replaces |
|---|---|---|
| [`testing-with-synthetic-users`](skills/testing-with-synthetic-users/) | A user-facing feature just shipped (or is about to) and has never been walked end-to-end on production | Manual QA passes and "the tests are green, ship it" |
| [`choosing-agent-models`](skills/choosing-agent-models/) | You're about to fan out work across subagents and have to pick a model for each | Defaulting every agent to the strongest model, or guessing per task |
| [`writing-good-skills`](skills/writing-good-skills/) | You're drafting or fixing a `SKILL.md` — especially one that never auto-triggers | Copying someone else's skill and hoping the description works |

## Other ways to install

As a Claude Code plugin: `/plugin marketplace add Kelonwai/claude-skills` then `/plugin install claude-skills@kelonwai-skills`.

Or copy a single skill into your personal skills directory:

```bash
git clone https://github.com/Kelonwai/claude-skills
cp -r claude-skills/skills/<name> ~/.claude/skills/
```

## Skills

### ⭐ `testing-with-synthetic-users`

Your feature works. Your tests pass. And your users still bounce — because "works" and "usable" are different tests.

This skill runs the second one: synthetic users with real motivations ("newcomer who can't read English product names", "reseller hunting mispriced items") walk your live deployment round after round. Findings get triaged 🔴🟡🟢, fixed, deployed, and re-tested from fresh angles — until a full round comes back clean. Not a QA checklist: a loop with an exit condition. Battle-tested across production features where it caught everything from XSS and TOCTOU races to "the feature is perfect but scores 3/10 on discoverability".

→ [`skills/testing-with-synthetic-users/`](skills/testing-with-synthetic-users/) · RED/GREEN-tested + 6 production runs → [dogfood log](feedback/testing-with-synthetic-users.md)

### ⭐ `choosing-agent-models`

Dispatching ten subagents on your strongest model because the task "feels important" burns money; sending a design critique to your cheapest one burns quality. This skill routes by task *nature* instead: taste and judgment (design, critique, adversarial verification) go to the top tier, research and implementation to the mid tier, mechanical bulk to the small tier — and when in doubt, no override at all. Baseline testing found agents' most reliable failure is "this research is complex, it needs the strongest model"; production dogfooding found the second: "mid tier to be safe" on bulk work. Both are countered explicitly. A month-scale audit later found the biggest leak was *discovery*, not routing logic — sessions that never loaded the skill never routed — so the description now leads with the phrasings users actually say ("fan out", "split across agents") instead of mechanism language. There's also a "Capped Top Tier" section for plans that limit strongest-model usage: the top model thinks; agents do.

→ [`skills/choosing-agent-models/`](skills/choosing-agent-models/) · GREEN-verified + month-scale production dogfood, monthly audit loop → [dogfood log](feedback/choosing-agent-models.md)

### `writing-good-skills`

The craft layer of skill authoring: seven structural checks (symptom-only descriptions, rules that carry their why, observable artifacts over promises, honest exits, a floor, one line that sticks, lean bodies) distilled from baseline-testing agents who wrote skills naturally and failed three of them. Pairs with a TDD authoring process; this is what "well-written" means once you have one.

→ [`skills/writing-good-skills/`](skills/writing-good-skills/) · Baseline-tested ×2 agents + applied to both skills above → [dogfood log](feedback/writing-good-skills.md)

## Method

Every skill here follows a TDD loop before release:

1. **RED** — run the scenario on an agent *without* the skill; document exactly how it fails
2. **GREEN** — write the skill against those specific failures; verify the agent now complies
3. **REFACTOR** — dogfood on real work, close the rationalization loopholes agents find

If a skill never watched an agent fail, it's documentation, not a skill.

The receipts are in the repo: `specs/` holds the writing brief each skill was authored against; `feedback/` holds the dogfood logs — real runs, observed failure modes, and the REFACTOR commits they drove.

## License

MIT
