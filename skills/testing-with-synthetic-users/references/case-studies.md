# Case Studies

Real production runs of this skill, abstracted. Each illustrates a rule that exists because a run without it failed.

## Collectibles database — species-profile feature (6 rounds)

R1 personas caught wrong-type labels from dirty source data and a price summary blown up by a single outlier record. R2's SEO-self-audit persona found 1,025 pages missing from the sitemap. R4's discovery-path persona scored the feature **3/10 on discoverability** — technically perfect, but the only entry point was a small chip on a detail page; the fix added header nav and a homepage CTA, and the returning persona re-scored it 8/10. R5 found two entries showing the wrong script variant (source data had zh-Hant/zh-Hans swapped) — a fresh-eyes mobile persona caught what four earlier rounds missed. R6: full regression clean, ship.

*Rules illustrated: rotation finds what earlier rounds can't; a round that finds problems can never be the last (budget was 5, ran 6); a device-matrix test plan would have found none of R4.*

## P2P trading feature (5 rounds, adversarial-heavy)

Personas probing as bad actors surfaced XSS in listings, a TOCTOU race in report handling, and admin-queue concurrency needing optimistic locking — across R1-R4, each round's fixes verified by the next. R5 clean. A side catch before R1: the signup trigger itself was broken, likely the root cause of near-zero registrations — found only because personas started from "create an account", not from the feature.

*Rules illustrated: adversarial archetype in later rounds; personas walk whole journeys, so they catch breakage outside the feature under test.*

## Brick-mosaic web app (7 rounds) — the test-pollution case

R1 ran three personas **in parallel on one shared Chrome profile**. Result: four suspect 🔴 findings (tab crash, wizard snap-back, locale flips, phantom drags) — a fresh-context verification agent proved **all four were personas contaminating each other's localStorage and tabs**, plus an unrelated session driving tabs mid-test. A full round burned on artifacts. Rounds 2+ ran personas sequentially in dedicated tabs; later, a "PDF takes 2 minutes" report turned out to be a backgrounded-tab artifact (measured 95–208ms) — but investigating it exposed a real hidden-tab hang, which got fixed.

*Rules illustrated: parallel personas need isolated profiles or sequential runs; reproduce-fresh-before-triage; excluded false positives belong in the report with evidence — and sometimes chasing an artifact still finds a real bug.*

## Local-services site (2 rounds)

Four motivated personas (anxious beginner, skeptical parent, career-switcher, exam re-taker) found a broken-markdown FAQ, a "DRAFT" label scaring users off the terms page, and a cost calculator missing its largest line item. Three suspicious findings were excluded with byte-level evidence (an emoji encoding "bug" was the messenger app's rendering, not the site's). R2 rotated to regression + new surfaces, came back clean — sign-off at round 2 of a 3-round budget, legitimately, because rotation had happened.

*Rules illustrated: the budget is a plan, not a floor — small surface, clean rotated round, done; false positives excluded with evidence, not silently dropped.*
