---
layout: post
title: "If a skill never watched an agent fail, it's documentation"
kicker: Method
standfirst: "Agents ignore your instructions the way users ignore your docs. So we stopped writing skills and started testing them: run the failure first, write against it, then keep the logs."
---

I wrote a lot of skill files before I admitted that. A `SKILL.md` is a folder of standing instructions the agent is supposed to load by itself when your request matches. Mine read well. They were organised. They were also, most of the time, not doing anything — the agent either never loaded them, or loaded them and talked itself out of the rule mid-task with a reason that sounded sensible.

So we started testing skills like code.

## The loop

Three steps, and the first one is the one everybody skips.

**RED.** Run the real scenario on an agent with no skill loaded. Watch it fail. Write down exactly how — the verbatim reasoning, not a summary. For `writing-good-skills` that meant asking two fresh agents to write a `SKILL.md` from scratch. Both were competent on substance. Both failed the same three craft checks: they stuffed a workflow summary into the description, they enforced everything in prose ("do not report until…") instead of demanding an artifact, and neither drew a floor saying when *not* to fire. Same three, both runs. That reproducibility is the whole point — you're not guessing at what agents get wrong, you're reading it.

**GREEN.** Write the skill against those specific failures, then rerun the same task and check compliance line by line. Not "does it read better" — does the agent now do the thing it refused to do before.

**REFACTOR.** Ship it, use it on real work, and keep a dogfood log. Not vibes: verbatim rationalizations, which dispatch went where, which checks got skipped. Every edit we've made to these skills came from a line in one of those logs.

If a skill never watched an agent fail, it's documentation, not a skill.

## The month-scale audit

Then REFACTOR found something the first two steps couldn't.

I audited a month of session logs for `choosing-agent-models` — the skill that decides which model each subagent gets. 27 launches, 19 sessions, 9 projects, several of them repos the skill had never been tested in. On routing quality it held up. The picks were right.

The problem was upstream. In one month, 61 sessions dispatched subagents. Only 19 of them had loaded the skill. A 69% miss rate. Every careful rule in that file was irrelevant four times out of five, because the file wasn't in the room.

The diagnosis took ten minutes once I looked at the description. It was written in mechanism language — *"when choosing a model for agents dispatched via the Agent tool"*. Nobody types that. What I actually type is "fan out", "split this across a few agents", "派 agent". The skill described its own machinery. Discovery matches on the user's words, not yours.

The fix was rewriting the trigger surface into the phrasings people actually say, in both languages I work in, then GREEN-testing it 3/3: a Cantonese positive, an English positive, and a negative control that must *not* fire.

We'd already seen the same failure and not generalised it. Two sessions in July did synthetic-user testing on an iOS app and the transcripts show zero invocations of the skill that exists for exactly that. It had English-only triggers; the sessions were in Cantonese. Bilingual triggers shipped on 21 July. From 26 July: 7 sessions, 8 invocations, across 5 projects.

Discovery beats content. A perfect rule that never loads scores zero.

## The charge that got thrown out

The same audit accused the routing skill of under-using the small model tier — an earlier week-long audit had found literally zero haiku dispatches, which did drive a real fix. So this time I sampled 30 dispatches out of 501 and handed them to an adversarial judge on the strongest model.

It threw the charge out. Of the sample: 20 sonnet calls were correct for the work, 3 should have been haiku, 5 were genuinely borderline. The task mix was just sonnet-shaped; the realistic ceiling for the small tier was 10–20%, not the "you should be using haiku everywhere" story I'd arrived with.

It found a smaller, truer thing instead. Sweep tasks were skipping the small tier whenever the request mentioned a "structured report" — the word *report* made the work sound like judgment. One new row went in: a report output doesn't make a sweep judgment work.

That's the part worth copying, more than any specific rule. Run your own audit adversarially, and let it tell you you were wrong. The logs live in [`feedback/`](https://github.com/Kelonwai/claude-skills/tree/main/feedback) in the open repo — including the entries where the skill didn't fire and the ones where I deviated from my own rules on purpose.

## The three skills

- **`testing-with-synthetic-users`** — synthetic users with real motivations walk your live deployment in rounds until a full round comes back clean.
- **`choosing-agent-models`** — routes each dispatched subagent by the nature of the task, not by how important it feels.
- **`writing-good-skills`** — seven structural checks for a `SKILL.md`, drawn from the ones baseline agents reliably miss.

```bash
npx skills add Kelonwai/claude-skills
```

Read the logs before the skills. The logs are the argument.
