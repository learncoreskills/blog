# second feature: exercise plugin engine — plan + implementation

Next in the V1 build order per `specs/README.md`: `004-exercise-plugin-engine`, the contract every
exercise/game implements so new exercises never require touching core/mastery/UI code
(Constitution Principle V). It depends on `001-competency-model`, already done.

## planning

Ran the Spec Kit plan + tasks workflow on `specs/004-exercise-plugin-engine/spec.md`:

- `plan.md` — new `packages/core/plugin-engine` package, depending on
  `@learncoreskills/competency-model` for the `Competency` type and tier bounds; constitution gate
  check, no violations
- `research.md` — resolved: registration failure is a two-layer API (pure
  `validatePluginRegistry` + throwing `createRegistry`, mirroring 001's
  `validateCompetencyModel` pattern); tier-bounds checking is a shared `isValidTier` guard rather
  than an orchestration layer this feature doesn't own; determinism is a plugin obligation with no
  shared PRNG imposed; `Question`/`PrintableQuestion` stay opaque, plugin-owned shapes
- `data-model.md` / `contracts/plugin-engine.api.md` — `Plugin`, `Question`, `PrintableQuestion`,
  `MasterySignal`, `PluginRegistry` and their validation rules
- `quickstart.md` — scenarios mapped to the spec's acceptance criteria, including a trivial
  echo plugin as the "add a second exercise" proof
- `tasks.md` — 18 tasks across the spec's two user stories (P1: plugin registration with zero
  core changes; P2: deterministic generation + tier validation)

## implementation

Built `packages/core/plugin-engine` in the `app` repo:

- `src/types.ts` — `Question`, `PrintableQuestion`, `MasterySignal`, generic `Plugin<TQuestion,
  TAnswer>`
- `src/tier.ts` — `isValidTier(competency, tier)`, the shared guard plugins call to reject a tier
  their trained competency doesn't define (FR-002)
- `src/registry.ts` — `validatePluginRegistry` (duplicate-id + unknown-competency-reference
  detection) and `createRegistry` (throws loudly on either, otherwise returns a
  `getPlugin`/`getPluginsForCompetency`/`all()` registry)
- `src/index.ts` — public barrel export
- `tests/` — unit tests for both user stories, plus a trivial echo plugin (and a second
  differently-shaped mock plugin) proving a plugin registers end-to-end and every plugin's
  `MasterySignal` output conforms to the identical shape

One correction along the way: the spec drafts initially wrote the workspace dependency as
`workspace:*` (pnpm/yarn syntax) — npm workspaces resolve internal deps via a plain `"*"` version
range instead. Fixed in both the package's `package.json` and the spec docs.

## verification

- `npm run test --workspace packages/core/plugin-engine` — 17/17 tests passing
- `npm run coverage --workspace packages/core/plugin-engine` — 100% statements/branches/functions/lines
- `npm run typecheck` / `npm run lint` (whole workspace, including `apps/web`) — clean
- Full-workspace `npm run coverage` — both `competency-model` (100%/96.29%) and `plugin-engine`
  (100%/100%) clear the 95% ADR-0004 gate

All 18 tasks in `specs/004-exercise-plugin-engine/tasks.md` are checked off.
`004-exercise-plugin-engine` is done.

Next up: `002-mental-addition-exercise` — the first real plugin, implementing this contract.
