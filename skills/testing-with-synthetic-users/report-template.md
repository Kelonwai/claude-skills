# Synthetic User Test Report — [feature name]

> [date] | host: [model] + persona agents: [model] | target: [production/staging URL]
> Feature under test: [one line — what shipped]

## Test Design

[1-2 lines: round budget, personas per round, automation used (Playwright/Chrome MCP/API-level), findings taxonomy 🔴🟡🟢.]

> ⚠️ **Method & limitations** — declare any deviation from the ideal setup: host-driven instead of independent persona agents, sequential single browser, viewports you couldn't test, tool constraints. What this run could NOT verify is part of the result.

## Round-by-Round

| Round | Personas | Findings | Actions |
|---|---|---|---|
| R1 | [persona angles] | 🔴 [finding + evidence ref] / 🟡 [finding] | [fix shipped / deferred → backlog] |
| R2 | [rotated angles] | ... | ... |
| ... | | | |
| Rn | [full regression: desktop + mobile] | [must be zero 🔴🟡] | — sign-off |

## Final State

- [key metrics: coverage numbers, subjective scores after fixes, query timings, entry-point count — whatever proves the feature's health]

## Backlog (does not block ship)

| Item | Level | Notes |
|---|---|---|
| [deferred 🟡 finding] | 🟡 | [who decided to defer, when, why] |
| [idea] | 🟢 | [origin round, rough cost] |

## Related Commits

[chain: spec → plan → implementation → per-round fix commits, so the audit trail is walkable]
