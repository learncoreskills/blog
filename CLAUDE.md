# CLAUDE.md

## This repo is the mandatory third step of every feature

Per the project constitution (`../specs/.specify/memory/constitution.md`, "Development
Workflow"), every feature follows `specs` → `app` → `blog` in order, and is **not considered
complete** until a write-up exists here — this repo is a running, chronological record of
everything shipped in the project, not optional polish.

## Format: one post per day, full-length article

As of 2026-09-14 (later revision) this blog is **one well-developed, longer-form article per
day**, not a compact digest and not one technical post per feature. It's written for a general
audience (parents/teachers reading it from inside the app) — still no implementation detail (no
file names, function/variable names, spec IDs) — but it should read like something that audience
actually wants to read, not a status update: give real, concrete detail on what changed and why it
matters to them. A short diagram or simple schema (plain text/markdown — an ASCII sketch, a small
table — nothing needing a rendering library beyond what the app already has) is welcome when it
makes a mechanism easier to picture. Light subheadings (bold lead-ins, or `##` headings) are fine
for a longer post; skip them for a short one.

The post's first paragraph doubles as its index/summary blurb (see
`app/apps/web/src/content/extractSummary.ts`, which takes the first paragraph after the title,
collapsed to ~200 characters) — write it as a real, standalone hook, not a throat-clearing
sentence.

When implementation work wraps up (in this session or handed off from one): if a post for
today's date (`YYYY-MM-DD-<slug>.md`) already exists, add a substantial paragraph or section to it
rather than creating a second file for the same day. Only start a new file when the calendar date
changes. Look at the current 2026-09-14 post for tone/length — the earlier 2026-09-12/13 posts
predate this longer format and are left as shorter historical entries, not a template to match.
