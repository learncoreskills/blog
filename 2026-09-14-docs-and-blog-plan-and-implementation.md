# eighth feature: documentation & blog — plan + implementation

With the core practice loop (`001`–`006`, `009`) and daily practice (`005`) done, the next ask was
different in kind: a professional, parent-/teacher-facing documentation area showing this
project's own specs and blog inside the app itself — distinct in look from the playful
child-facing practice loop, per the constitution's two-audience split. New feature, `010`.

## specifying and planning

Three scope-defining questions came up in `/speckit-specify`'s clarification pass: where the
displayed content actually comes from, which content to show, and whether it's reachable before a
child profile exists. Resolved: bundle content at build time straight from the sibling `specs`
and `blog` repos (works offline, no new runtime dependency), show every spec — shipped and
planned — plus the narrative product/architecture docs, and make it reachable before the
profile gate so a parent can evaluate the product before creating a profile.

`/speckit-plan` picked `react-router-dom` (real, shareable URLs for content whose whole point is
to be read and referenced) and `react-markdown` + `remark-gfm` (this project's own specs lean
heavily on GFM tables and task-list checkboxes). The more interesting design decision:
"shipped vs. planned" status. An audit while planning found every spec's own `**Status**` line
still reads "Draft" — including six fully-implemented, deployed features. That field is simply
never updated after planning, so it's useless as a data source. Instead, status is derived
mechanically from each spec's `tasks.md`: no file → planned; fully checked → shipped; any `- [ ]`
remaining → in progress. Self-correcting, no bookkeeping.

## analyze caught two real risks before any code was written

Two HIGH findings from `/speckit-analyze`, both worth having caught early:

- **CSS leakage.** `App.css` sets a playful gradient background, a rounded font, and a purple ink
  color directly on the global `body` element — inherited properties that would bleed into any
  new content unless explicitly reset, not just overridden alongside. Tightened the plan to
  require `.docs-theme` to explicitly reset `background`, `font-family`, and `color`, not merely
  add new rules next to `body`'s.
- **CI path assumptions.** Moving the `app` checkout to `path: app` (needed so `specs`/`blog` can
  be sibling-checked-out) has three easy-to-miss consequences: the deploy step's `publish_dir`
  needs the same prefix, every `npm` step needs a `working-directory`, and — confirmed via the
  GitHub API — `specs` is private (needs an auth token to check out) while `blog` is public
  (doesn't). All three were spelled out in the task before writing the workflow, rather than
  discovered the way last session's custom-domain deploy bug was: by the site actually breaking.

## implementation surfaced a third bug analyze couldn't have caught

Writing the first content-loading test failed immediately with a denied-file error — not from bad
code, but because `apps/web` has a *separate* `vitest.config.ts` that doesn't inherit
`vite.config.ts`'s settings. The dev-server file-access allowance planned for `vite.config.ts`
needed duplicating into `vitest.config.ts` too, or `npm test` would fail the same way `npm run
dev` would have. Caught by actually running the test against real content before building
anything on top of it, rather than trusting the plan's path math — which, separately, was also
off by two directory levels in the original research/tasks docs (`../../specs` instead of the
correct `../../../specs` for `vite.config.ts`, `../../../specs/...` instead of
`../../../../../specs/...` for files under `src/content/`) until verified against the filesystem
directly with `readlink -f` rather than counted by hand.

Built inside the existing `apps/web` package — no new `packages/*` package, since this is content
wiring, not portable business logic:

- `src/content/` — `loadSpecs`, `loadNarrativeDocs`, `loadBlog` (each `import.meta.glob` reading
  raw markdown from the sibling repos), plus pure `deriveSpecStatus`/`parseBlogFilename`/
  `extractTitle` helpers, each independently unit-tested
- `src/components/docs/` — `DocsLayout`, `DocsIndex`, `DocPage`, `BlogIndex`, `BlogPostPage`,
  `MarkdownContent` (wrapped in a `MarkdownErrorBoundary` so one malformed document can't take
  down the page)
- `src/docs.css` — the calm/professional theme, explicitly resetting `body`'s playful cascade
- `App.tsx` — wrapped in `react-router-dom`, with `/`, `/docs`, `/docs/:id`, `/blog`, `/blog/:id`;
  the existing practice-app screen and its profile gate are otherwise untouched
- `.github/workflows/ci-cd.yml` — both jobs now check out `specs` (private, token) and `blog`
  (public) as siblings before any install/test/build step

## verification

- `npm test --workspace apps/web` — 53/53 passing (up from 26 before this feature)
- `npm run coverage --workspace apps/web` — 96.5% statements, 87.3% branches, 100% functions,
  98.1% lines — all comfortably above the 70% ADR-0004 bar
- `npx eslint` / `npx tsc --noEmit` — clean
- `npm run build --workspace apps/web` — succeeds; spot-checked the built bundle directly and
  confirmed real spec/blog content is present and zero references to any GitHub API/raw-content
  URL exist (genuinely static, no runtime fetch)
- Full workspace `npm test`/`lint`/`typecheck` — all packages plus `apps/web` still green

**One honest gap**: the CI workflow changes couldn't be run against real GitHub Actions in this
environment (no `act` CLI, no way to trigger a real workflow run) — verified by careful inspection
against the concrete risks `/speckit-analyze` raised, but not by an actual green CI run. Worth
confirming on the next real push before trusting it fully, the same caveat `009`'s write-up
flagged for its own untested-in-a-real-browser gap.

## where things stand

`010-docs-and-blog` is implemented, tested, and verified locally. All 35 tasks in
`specs/010-docs-and-blog/tasks.md` are checked off. Parents and teachers can now read every spec
— shipped and planned — plus the blog, from inside the app, in a visually distinct professional
theme, without creating a child profile first. Confirming the CI workflow change on a real push is
the natural next step, not a new feature.
