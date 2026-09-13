# fifth feature: accounts & privacy (V1 slice) — plan + implementation, and V1 is done

Last piece of V1: `006-accounts-privacy`. Unlike the rest of that spec, its V1 Scope section pulls
only **User Story 1 and FR-001/FR-002** forward — multiple named local child profiles, switching,
and per-child data isolation — because mixing two children's mastery data is a correctness bug on
day one for any real family, not a Phase-3 polish item. Everything else in that spec (country/age
fields, export/import, deletion, a real parent-account concept) stays Phase 3 and got no tasks.

## planning

Ran the Spec Kit plan + tasks workflow, scoped explicitly to the V1 slice:

- `plan.md` — new `packages/core/profiles`, zero runtime dependencies (unlike the other four V1
  packages, this one doesn't touch competencies, plugins, or mastery signals at all — it's pure
  profile identity + storage namespacing)
- `research.md` — resolved: operate against an injected `KeyValueStore` interface
  (`getItem`/`setItem`) rather than a hardcoded `window.localStorage` import, so the package stays
  unit-testable under plain Vitest with an in-memory double, no `jsdom` needed; no separate
  `ParentProfile` entity for V1 (the device itself is the implicit parent profile, per the spec's
  own deferral); profile creation is intentionally non-deterministic (Constitution Principle VI
  scopes determinism to question generation / answer validation / mastery calculations
  specifically, not record CRUD); this package only hands out a namespacing key
  (`childStorageKey`) — it doesn't persist mastery signals itself, keeping `003-mastery-engine`'s
  already-decided "caller owns storage" boundary intact
- `data-model.md` / `contracts/profiles.api.md` — `ChildProfile`, `ProfilesState`,
  `KeyValueStore`
- `quickstart.md` — the two acceptance scenarios plus both edge cases
- `tasks.md` — 17 tasks, one user-story phase (there's no P2 phase here, since User Story 2 is
  out of scope for V1)

## implementation

Built `packages/core/profiles` in the `app` repo:

- `src/profiles.ts` — `createChildProfile` (required alias, no payment check anywhere),
  `listChildProfiles`, `getActiveChildId`/`switchActiveChild` (first child auto-activates)
- `src/namespace.ts` — `childStorageKey(childId, key)`, the isolation mechanism
- One snag: `crypto.randomUUID()` needs a type, and the workspace's shared `tsconfig.base.json`
  has no `"DOM"` lib (deliberately — core packages stay plain TypeScript). Rather than adding
  `"DOM"` workspace-wide for one call site, added a small local `src/global.d.ts` declaring just
  that one global's type.

## verification

- `npm run test --workspace packages/core/profiles` — 12/12 tests passing
- `npm run coverage --workspace packages/core/profiles` — 100% across the board
- Full-workspace `npm test`/`typecheck`/`lint`/`coverage` — all five packages (83 tests total)
  green

All 17 tasks in `specs/006-accounts-privacy/tasks.md` are checked off.

## V1 is complete

All five V1 specs (`001-competency-model`, `004-exercise-plugin-engine`,
`002-mental-addition-exercise`, `003-mastery-engine`, and this V1 slice of
`006-accounts-privacy`) are now specced, implemented, verified, and written up, per
`specs/README.md`'s V1 scope. The full core loop — Exercise → Answer → Feedback → Mastery → Next
Exercise, safe for more than one child on the same device — exists as five independent,
100%-covered TypeScript packages:

```
packages/core/competency-model    16 tests
packages/core/plugin-engine       17 tests
packages/exercises/mental-addition 26 tests
packages/core/mastery-engine       12 tests
packages/core/profiles             12 tests
                                    ─────────
                                    83 tests, all packages 95%+ coverage (ADR-0004)
```

What's not done yet: none of this is wired into `apps/web` as an actual playable screen — every
package so far is business logic only, UI-less by design (each spec explicitly scoped UI out).
Wiring these five packages together into a real practice-loop UI is the natural next step, but is
its own scope of work beyond what any of these five specs asked for.
