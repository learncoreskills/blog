# first feature: competency model — plan + implementation

Picked `001-competency-model` as the first feature to build. Per the V1 build order in
`specs/README.md`, it's the foundation everything else (exercise plugins, mastery engine, daily
practice) depends on, so it goes first.

## planning

Ran the Spec Kit plan workflow on `specs/001-competency-model/spec.md`:

- `plan.md` — technical context (TypeScript, npm workspace package, Vitest, 95%+ coverage per
  ADR-0004, client-only per ADR-0006) + constitution gate check, no violations
- `research.md` — resolved the open design questions: prerequisites are a directed acyclic graph
  (not a tree), tier count lives per-competency (not a global constant), names/descriptions are
  i18next translation keys, subjects are data (not a hardcoded TS union)
- `data-model.md` — `Subject` and `Competency` entities and their validation rules
- `contracts/competency-model.api.md` — the package's exported TypeScript surface
- `quickstart.md` — runnable scenarios mapped to the spec's acceptance criteria
- `tasks.md` — broken into the two user stories from the spec (P1: stable competency+tier
  addressing, P2: subject/language-agnostic authoring), plus setup/foundational/polish phases

## implementation

Built `packages/core/competency-model` in the `app` repo — a zero-dependency TypeScript package:

- `src/types.ts` — `Subject`/`Competency` types
- `src/validate.ts` — `validateCompetencyModel`, `detectPrerequisiteCycles`,
  `findCompetency`
- `src/index.ts` — public barrel export
- `tests/` — unit tests covering both user stories plus the spec's edge cases (1-tier
  competencies, zero prerequisites, dangling references, direct and transitive prerequisite
  cycles, adding a second subject with zero schema changes)

Also scaffolded the workspace itself (root `package.json`, shared strict `tsconfig.base.json`,
ESLint flat config, Prettier) since this is the first package in the repo.

## verification

With a Node toolchain available (via nvm, `.nvmrc` pins 24), ran the full quality bar from
`tasks.md` Phase 5 against the package:

- `npm run test --workspace packages/core/competency-model` — 16/16 tests passing
- `npm run coverage --workspace packages/core/competency-model` — 100% statements/lines,
  96.29% branches (above the 95% ADR-0004 gate)
- `npm run typecheck --workspace packages/core/competency-model` — clean
- `npx eslint packages/core/competency-model` — clean

All 17 tasks in `specs/001-competency-model/tasks.md` are checked off. `001-competency-model` is
done.

Next up: `004-exercise-plugin-engine`.
