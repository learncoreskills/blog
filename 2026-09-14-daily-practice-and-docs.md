# Day 3: a smarter daily practice, and a window into how it all works

Three things came together today: the app got noticeably better at deciding what a child should
practice each day, parents and teachers got a proper way to see how the whole system works
without leaving the app, and parents got a place of their own inside it — one that also makes
sure a child's progress can survive losing a phone or switching to a new one.

**Building a session that actually adapts**

Until now, opening the app meant working through addition problems with no real plan behind
them — a flat set of exercises, the same shape no matter how a child was actually doing. That
changed today. Every time a child opens the app, it now puts together a full session for that
day — 50 questions, split into five ten-question rounds — built around one question: what does
this child need most, right now?

The logic behind that is closer to how a good tutor plans a lesson than to a random quiz
generator. Most of the session — three of the five rounds — stays at the level the child is
currently working on, so the bulk of the practice is neither too easy nor too far out of reach.
What happens with the other two rounds depends entirely on how that level has been going lately:

```
 Doing great there (90%+ correct)?   -> two rounds move UP,   a stretch to the next level
 Struggling, or brand new to it?     -> two rounds move DOWN, extra review of the last level
 Somewhere in between?               -> one round each way, a bit of both
```

At the very first level, there's nowhere lower to go, so a child who's still finding their feet
just gets more practice at that level instead of being pushed below it. At the top level, once
it's fully mastered, the "move up" case has nowhere to go either, so those rounds become a review
round instead — a small victory lap rather than a dead end.

None of this gets decided once and locked in for good. The plan is recalculated from scratch every
single day based on how the child has actually been performing, so one rough day or one lucky
streak doesn't lock in a plan that no longer fits by tomorrow. And for a parent or teacher who
already has a good read on where a child stands — say, at the very start, before the app has
collected any data of its own — there's now a way to set the starting level directly, rather than
waiting for the system to work it out from a blank slate.

**Opening the hood**

The second piece of today's work: the specifications and progress notes describing everything
above — not just today's feature, but the whole project — are now readable from inside the app
itself, in a section built specifically for parents and teachers rather than for the child using
the practice screens. It's deliberately calm and plain where the practice screens are playful and
game-like, because the two audiences reading them want very different things from what's in front
of them.

That distinction matters more than it might sound like at first. A parent handing a device to
their child, or a teacher weighing whether to bring the app into a classroom, shouldn't have to
take "it adapts to your child" on faith, or go spelunking through a GitHub repository to check for
themselves. As of today, they don't have to do either — the same documentation this project is
built from, and this very blog, are now one tap away from the practice app itself, with a clear
way back to it whenever they're done reading.

Small on its own, but it's the first time this project has had a proper front door for the people
deciding whether to trust it with their kids' practice time — not just for the kids using it.

**A place of their own, and a way to never lose it**

That new front door didn't stay empty for long. Alongside it, parents now get their own area
inside the app — separate from both the practice screens and the documentation — with a simple
report showing how their child is actually doing, competency by competency, in real numbers
rather than a vague sense of progress. It's deliberately plain for now: no charts or history yet,
just an honest current reading, with a clear "nothing to show yet" message for a child who hasn't
started practicing. The graphs and trend lines are a natural next step, but getting an accurate
number in front of parents mattered more than making it pretty on day one.

The more important piece living in that same area is quieter, but it's the one that actually
protects something: a way to export a child's entire practice history — their profile and every
recorded answer that feeds the mastery numbers — into a single file, and bring that same file back
in on another device. Losing a phone, switching to a new one, or just wanting a backup before
handing a tablet down to a younger sibling no longer means a child's progress vanishes with it.
Import even works before a single profile exists yet on the new device — restoring a backup is
exactly the situation where there's nothing there to begin with, so it had to work from a
completely blank slate, not just as a convenience for an already-set-up one.

The detail that took the most care wasn't the export itself — it was making sure a file exported
today still works after this project has changed several times over. Every exported file carries
a version marker, and the app is built to always understand its own older files rather than
quietly failing to read them a year from now. If a file ever comes from a version newer than what
a particular device is running — say, a backup made on a freshly updated phone, brought back to an
older one — the import says so plainly and stops, instead of guessing and getting it wrong. A
backup you can't trust isn't really a backup at all, so that guarantee mattered more than almost
anything else about how this was built.
