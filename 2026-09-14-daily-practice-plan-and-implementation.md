# seventh feature: daily practice — plan + implementation

With V1's core loop done (`001`/`004`/`002`/`003`/`006`-partial) and wired into a usable screen
(`009`), picked `005-daily-practice` as the next feature: it's next in the product roadmap's own
phase order, and it's the thing that turns "pick a tier and practice" into the actual adaptive
"Today's Practice" experience the product vision centers on — even with only one exercise plugin
(`math.addition.mental`) built so far.

## clarifying the spec

The original draft spec was vague on exactly the things that matter for a v1 algorithm: whether
weakness-targeting should really run today (with only one competency) or stay a stub, what score
thresholds decide the swing between tiers, how "~30 minutes" maps to a concrete generation target,
and whether a session persists across app loads. Five clarification rounds nailed these down:
real tier-level allocation now (50% baseline at the child's current tier, the rest swinging to the
next/previous tier based on that tier's own mastery), reusing `003-mastery-engine`'s existing
≥90% "mastered" line rather than inventing new thresholds, a fixed 50-question/5-block session
(not a raw minutes target), and regeneration on every app load rather than persisted state. A
sixth, post-plan clarification added a parent-forced starting-tier override (FR-006) — "the tier
starts at 1, moves up if too easy / down if too hard, but a user can force the initial level."

## planning, then catching a real bug in analyze

Ran plan → tasks → analyze. The plan put the allocation logic in a new, dependency-light package
(`packages/core/daily-practice`), deliberately excluding UI (out of scope per the spec) and
deliberately not touching the exercise-plugin packages (it schedules `(competencyId, tier,
questionCount)`, never concrete questions).

`/speckit-analyze` caught something worth catching before writing any code: the spec's own
Success Criterion (SC-002, "the session always includes at least 2 review blocks") was
mathematically false for the algorithm's most common case — a tier that's been attempted but
isn't mastered yet only gets 1 review block (the other swing block stretches forward instead), not
2. The original acceptance-scenario tests, if written as first drafted, would have quietly skipped
testing exactly the case that broke the claim. Fixed by loosening the Success Criterion to state
two honest guarantee levels instead of distorting the algorithm to hit an overstated one — the
mixed review-and-stretch behavior for that case was the actual point of the feature. Also caught
and fixed: two missing constitution-gate rows in the plan, and a plan/tasks file-layout mismatch.

## implementation

Writing the actual tests surfaced a second, deeper issue analyze hadn't caught: natural tier
derivation (lowest tier that isn't yet mastered) can *never* land on an already-mastered tier
except at the very ceiling — so the "mastered → swing toward next tier" branch of the algorithm is
only reachable in practice via the new `forcedTier` override, not through ordinary day-to-day use.
Not a defect (day-over-day advancement already happens through derivation itself once a tier is
mastered), but it meant one of the planned quickstart examples was testing an impossible fixture
and had to be corrected, and it's now documented as an explicit implementation note in
`research.md` rather than left implicit.

Built `packages/core/daily-practice` — one exported function, `generateDailySession(competencyId,
mastery, options?)` — mirroring `mastery-engine`'s package layout exactly:

- `src/daily-session.ts` — `deriveCurrentTier`, `selectSwingTiers`, and the public
  `generateDailySession`, all pure/synchronous
- `src/types.ts` — `DailySessionBlock`, `DailySession`, `GenerateDailySessionOptions`
- `tests/allocation.test.ts`, `review.test.ts`, `override.test.ts` — one file per user story

Also dropped a dependency the plan draft listed but the final contract never needed:
`competencyId` is a plain string and `tierCount` is read off `mastery.tiers.length`, so
`@learncoreskills/competency-model` isn't a dependency at all.

## verification

- `npm test --workspace packages/core/daily-practice` — 19/19 tests passing
- `npm run coverage --workspace packages/core/daily-practice` — 97.1% statements, 95.45% branches,
  100% functions, 97.0% lines — all above the 95% ADR-0004 gate
- `npx eslint` / `npx tsc --noEmit` on the package — clean
- Full-workspace `npm test`/`lint`/`typecheck` — all six packages plus `apps/web` still green,
  nothing else touched or broken

All 22 tasks in `specs/005-daily-practice/tasks.md` are checked off.

## where things stand

`005-daily-practice`'s algorithm is implemented, tested, and verified as a standalone package —
same pattern as `003`/`004`/`006` before `009` wired anything into the UI. Wiring
`generateDailySession` into an actual "Today's Practice" screen (and persisting a parent's tier
override across days, which this package deliberately leaves to its future caller) is the natural
next integration step, the same shape of work `009` was for the earlier packages.
