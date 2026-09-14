# Day 1: laying the foundation before writing a line of code

The project's actual first day didn't produce anything a child could touch — no app, no
exercises, not even a line of code. What it did produce was a plan clear enough that everything
built over the days that followed had an obvious place to go, and a reason for existing, instead
of getting figured out on the fly.

The day started with the basics most projects skip past too quickly: a proper home for the work,
in the form of two repositories set up before anything else.

```
specs/  -> the written rules a feature has to satisfy, agreed on *before* any code gets written
blog/   -> this running story of what actually gets built, one entry per day
```

The reasoning behind splitting things that way was simple, if a little unusual for a small
project: write down what a feature is supposed to do and why, in enough detail to check the
finished thing against, before building it — rather than deciding as you go and hoping it holds
together later. The third piece, an actual app a family can open, would only start once there was
something concrete in `specs/` to build it against — that came the following day.

Most of the rest of day one went into questions that are easy to skip and expensive to get wrong
later: what is this product actually for, and for whom? The answer settled on was deliberately
narrow — a practice app to help kids build core skills, starting with one thing done well (mental
math) rather than a sprawling curriculum from day one. The thinking was that a single skill,
built and proven end-to-end, is worth far more early on than five half-built ones: it's the
difference between a real, working example to point to and a pile of promises.

With that settled, the rest of the day went into organizing how the specs repository itself
should be structured — one folder per feature, so each piece of the product has its own
self-contained description of what it does and why, plus a small set of higher-level notes
covering the product's overall vision and the bigger architectural decisions that individual
features would otherwise have to keep re-explaining. On top of that structure, a first rough pass
at requirements went down on paper (well — into a repository), covering the handful of pieces the
app would eventually need: a way to represent what a "skill" even is, a consistent shape for
exercises to follow, and a trustworthy way to tell whether a child had actually learned something
rather than just gotten lucky on a given day.

None of those pieces existed yet by the end of day one. What existed was a place for each of them
to go, and a reason, written down, for why each one mattered — which turned out to matter more
than it might sound, once actual building started the very next day.
