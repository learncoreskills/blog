# sixth feature: V1 app shell — wiring everything together into a usable app

The five V1 packages (`001`/`004`/`002`/`003`/`006`) were all implemented, tested, and verified —
but none of them were reachable by an actual user. Every one of those specs explicitly scoped UI
out. "Plug everything in to make it usable" meant building the missing piece: a real screen in
`apps/web`.

Since the project's constitution requires a spec before implementation for every feature, and this
integration work wasn't covered by any of the eight original specs, first added
`009-v1-app-shell` — a new spec describing exactly this: profile creation/switching, a
10-question mental-addition session with immediate feedback, and a plain mastery percentage, all
wired to the five already-published package contracts with no new business logic.

## planning

Ran the full plan + tasks workflow like every prior feature:

- `plan.md` — everything lives in the existing `apps/web` workspace package; no new
  `packages/*` package, since this is wiring, not business logic. One genuinely new piece of code:
  a thin per-child `MasterySignal[]` persistence helper, since none of the five dependency
  packages persist a signal log themselves — they all deliberately left that seam for the app.
- `research.md` — resolved: write each mastery signal to `localStorage` immediately per answered
  question, not batched, so the abandoned-session edge case and reload-persistence requirement
  both hold for free; session seeds use `Date.now()` (determinism only matters given-a-seed, not
  across real sessions); a `useProfiles` hook is a thin manually-synced cache in front of
  `@learncoreskills/profiles`' plain functions, since that package is deliberately
  framework-agnostic; no i18n library added yet — `apps/web` has none, and this feature's own
  Assumptions defer visual/translation polish.
- `contracts/screen-flow.md` — a screen-state diagram in place of a TS API contract, since this
  feature is a consumer app, not a new package.
- `quickstart.md` / `tasks.md` — four scenarios plus edge cases, 23 tasks across two user stories.

## implementation

Added `apps/web/src/`:

- `curriculum.ts` — the one V1 `Competency` as real, loadable data (superseding `App.tsx`'s old
  deploy-pipeline placeholder)
- `registry.ts` — `createRegistry([mentalAdditionPlugin], [mentalAdditionCompetency])`
- `storage/masterySignals.ts` — the new persistence glue, keyed via `childStorageKey`
- `hooks/useProfiles.ts`, `hooks/useMastery.ts`
- `components/ProfileManager.tsx`, `TierPicker.tsx`, `PracticeSession.tsx`, `MasteryDisplay.tsx`
- `App.tsx` rewritten to orchestrate all of the above into the actual screen flow

Also added `vitest` + React Testing Library to `apps/web`, which had no test runner configured at
all until now.

One naming snag: `ProfileManager`'s prop for the profile list was initially called `children` —
renamed to `profiles` immediately, since `children` collides with React's reserved prop name.

## verification

- `npm run test --workspace apps/web` — 26/26 new tests passing (full workspace: 109 tests across
  all six packages)
- `npm run coverage --workspace apps/web` — 96.47% statements (ADR-0004's tracked bar for
  `apps/web` is 70%, non-blocking, but comfortably cleared anyway)
- `npm run typecheck` / `npm run lint` (whole workspace) — clean
- `npm run build --workspace apps/web` — the exact command CI runs — succeeds
- Started the dev server and confirmed it boots and serves the app shell correctly (`fetch`
  against `localhost:5173` returned valid HTML)

**One honest gap**: this environment had no headless-browser tooling (no `chromium-cli`, no
Playwright/browser binary installed), so I could not actually drive a real browser and screenshot
the practice loop. Every scenario in `quickstart.md` has an equivalent automated test
(`App.test.tsx` runs the full create-profile → pick-tier → answer-10-questions →
mastery-updates flow, and a two-children-stay-separate flow, under jsdom + React Testing Library),
but that's not the same as a human or a real browser actually seeing it render. Flagged as a
partial task (T022) in `tasks.md` rather than claimed as fully done — a real-browser pass is worth
doing before calling this feature truly finished.

## where things stand

All five original V1 specs plus this integration feature are now implemented. `apps/web` has a
working (if visually unstyled) practice loop: create a child, pick a tier, answer addition
questions with immediate feedback, and see mastery update — persisted per child, entirely
client-side, zero network calls. Visual/UX polish (the playful, game-like look the vision doc
describes) remains deliberately out of scope for this feature and is the natural next step.
