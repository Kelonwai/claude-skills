# Dogfood log: writing-good-skills

## RED — baseline (2026-08-22, 2 runs, sonnet, no skill)

Task: "write a SKILL.md for X" (X = verifying-deploys / weekly-traffic-report).
Both baselines were competent on substance — the failures were craft failures,
matching the unlazy v2 finding that visible failures are gone on frontier models
while subtle ones remain:

| Check | Baseline A (deploys) | Baseline B (traffic) |
|---|---|---|
| 1 description = symptoms only | ✗ workflow summary appended | ✗ workflow summary LEADS, triggers trail |
| 2 rules carry why | partial | partial |
| 3 artifact enforcement | ✗ all prose ("do not report until…") | ✗ all prose |
| 4 honest exit | ✓ (unprompted) | partial (honesty notes, no structure) |
| 5 not-for floor | partial (skip note) | ✗ absent |
| 6 memorable line | ✓ (one decent line) | ✗ |
| 7 lean / portable | ✗ ~1000w inline, harness tools hardcoded as only path | ✗ ~900w inline; also a zh/en typo ("internal数据 table") |

Most reproducible failures across both: **workflow-in-description (1), prose-only
enforcement (3), missing floor (5)**. These three are called out as
"baseline-tested defaults" in the skill body.

## GREEN — same task B with skill loaded (1 run, sonnet)

All seven checks pass or reasonably pass:

- Description: pure trigger phrases, zero workflow ✓
- Whys attached ("a snapshot wearing a comparison's name") ✓
- Output artifact mandated (numbers table, both-parts-always) ✓
- Honest exits ("flagged-but-unverified anomaly is still more useful than
  silence"; data gaps stated, never silently omitted) ✓
- Not-for section present ✓
- Explicit memorable line ✓
- ~450 words, tool named as one way ("e.g."), not the only path ✓

## REFACTOR notes

No compliance loophole found in GREEN. One cosmetic tic: the agent literally
labeled its aphorism "**One line that sticks:**" — mechanical but harmless; not
worth an edit cycle. Watch for it recurring; if labeling spreads, add a counter.

## REFACTOR applied

Self-audit caught the skill violating its own check 7 (672 words > its ≤~500
guideline). Trimmed to 551 total (~480 words of instruction body excluding
frontmatter). No content removed, only compression.

## Open items

- Only one GREEN run; a second task-family run (discipline-type skill) would
  raise confidence in checks 3–4 under that shape.
- Not yet pressure-tested (deadline framing) — checks 2/4 are the ones that
  would crack first.
