# third feature: mental addition exercise — plan + implementation

Next in the V1 build order: `002-mental-addition-exercise`, the first real exercise plugin — the
first end-to-end proof of Exercise → Answer → Feedback → Mastery. Depends on
`004-exercise-plugin-engine` (the contract) and doesn't touch `apps/web`: the spec explicitly
scopes UI out, so this is a pure logic package.

## planning

Ran the Spec Kit plan + tasks workflow on `specs/002-mental-addition-exercise/spec.md`:

- `plan.md` — new `packages/exercises/mental-addition`, depending only on
  `@learncoreskills/plugin-engine`. Decided *not* to depend on `@learncoreskills/competency-model`
  for this plugin's tier count — the curriculum content (`docs/product/CURRICULUM.md`) is still a
  placeholder with no loadable `Competency[]` data anywhere yet, and FR-001 already fixes this
  plugin's 5 tiers directly in its own spec table.
- `research.md` — resolved: a local seeded PRNG (mulberry32, no dependency) makes generation
  deterministic; every tier's digit-count/carrying rule is satisfied by **construction** (pick
  operands from a range that guarantees the rule), never by generate-then-filter — this is what
  the spec's tier-boundary edge case explicitly requires; a `createMasterySignal` helper lives in
  this package since FR-008 assigns signal-shape-assembly to "the system" (this plugin)
- `data-model.md` / `contracts/mental-addition.api.md` — `AdditionQuestion`, `AdditionSession`,
  and the exact per-tier operand-range table
- `quickstart.md` — scenarios for the full session/answer/signal flow and each tier's rule
- `tasks.md` — 27 tasks across the two user stories (P1: session/answer/mastery-signal flow; P2:
  per-tier deterministic generation)

## implementation

Built `packages/exercises/mental-addition` in the `app` repo:

- `src/rng.ts` — `createRng`/`randInt`, the local mulberry32 PRNG
- `src/generate.ts` — `generateAdditionQuestion(tier, seed)`, one constructor function per tier
  (e.g. Tier 2 picks `operandA` first, then constrains `operandB`'s range so the sum is always
  ≥ 10 — never generates freely and checks afterward)
- `src/plugin.ts` — `mentalAdditionPlugin` (the actual `Plugin` implementation),
  `createSession(tier, seed)` (10 questions, sub-seeds drawn from one PRNG stream),
  `createMasterySignal`
- `tests/` — per-tier invariant tests (1000 seeds each), determinism tests, an abandoned-session
  test, and a direct `validatePluginRegistry` check proving this plugin conforms to
  `plugin-engine`'s contract with zero changes there (FR-010)

## verification

- `npm run test --workspace packages/exercises/mental-addition` — 26/26 tests passing
- `npm run coverage --workspace packages/exercises/mental-addition` — 100% across the board
- Full-workspace `npm test`/`typecheck`/`lint`/`coverage` — all three packages (59 tests total)
  green; `competency-model` and `plugin-engine` unaffected

All 27 tasks in `specs/002-mental-addition-exercise/tasks.md` are checked off.
`002-mental-addition-exercise` is done — three of five V1 specs now complete
(`001`, `004`, `002`).

Next up: `003-mastery-engine` — the v1 naive mastery formula that consumes these signals.
