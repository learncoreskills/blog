# fourth feature: mastery engine — plan + implementation

Next in the V1 build order: `003-mastery-engine`, the v1 (explicitly provisional, per
Constitution Principle II) formula that turns the append-only `MasterySignal` log into the
mastery percentages parents/children see. Depends on `004-exercise-plugin-engine` (the
`MasterySignal` shape) and `001-competency-model` (a competency's `tierCount`, to know how many
tiers to average over).

## planning

Ran the Spec Kit plan + tasks workflow on `specs/003-mastery-engine/spec.md`:

- `plan.md` — new `packages/core/mastery-engine`, pure/stateless: takes a signal array and a
  `Competency` as input, recomputes from scratch every call, no stored aggregate anywhere (FR-004)
- `research.md` — resolved: `"not-started"` is a literal-type sentinel rather than a magic
  number, so it can't be silently averaged in as a real 0% by accident; recency ("last 10
  attempts") is determined by sorting on `timestamp`, not by trusting array order, since nothing
  in the `MasterySignal` contract commits callers to passing a chronologically-ordered array; this
  package *does* depend on the real `Competency` type from `001` (unlike `002`'s deliberate
  choice not to) because its entire job — averaging across however many tiers a competency
  defines — is generic over `tierCount` in a way `002`'s fixed 5-tier plugin never needed to be
- `data-model.md` / `contracts/mastery-engine.api.md` — `TierMastery`, `CompetencyMastery`, and
  the exact threshold/averaging rules
- `quickstart.md` — scenarios for each acceptance criterion plus the zero-attempts,
  more-than-10-attempts, and determinism edge cases
- `tasks.md` — 17 tasks across the two user stories (P1: trustworthy per-tier/per-competency
  percentages; P2: weakness ranking for the future daily-practice feature)

## implementation

Built `packages/core/mastery-engine` in the `app` repo:

- `src/tier-mastery.ts` — `computeTierMastery`: filters signals to one competency+tier, sorts by
  timestamp, takes the last `min(10, count)`, reports `"not-started"` below 5 attempts else an
  accuracy percentage with a `mastered` flag at ≥90%
- `src/competency-mastery.ts` — `computeCompetencyMastery`: averages every tier `1..tierCount`
  defines, `"not-started"` tiers counted as 0% (so reaching 100% requires mastering every tier,
  not just the easiest attempted one)
- `src/rank.ts` — `rankWeakestCompetencies`: sorts a `CompetencyMastery[]` ascending, non-mutating
- `tests/` — threshold/boundary tests, a recency-window test (older all-wrong + newer all-correct
  signals fed in shuffled order, proving timestamp — not array position — drives recency),
  averaging tests, and determinism tests

## verification

- `npm run test --workspace packages/core/mastery-engine` — 12/12 tests passing
- `npm run coverage --workspace packages/core/mastery-engine` — 100% across the board
- Full-workspace `npm test`/`typecheck`/`lint`/`coverage` — all four packages (71 tests total)
  green

All 17 tasks in `specs/003-mastery-engine/tasks.md` are checked off. `003-mastery-engine` is
done — four of five V1 specs now complete (`001`, `004`, `002`, `003`).

Next up: `006-accounts-privacy` (partial, per its own V1 Scope section) — multiple local child
profiles with fully separate mastery data, the last piece of V1.
