# Day 3: a smarter daily practice, and a window into how it all works

Two things came together today: the app got noticeably better at deciding what a child should
practice each day, and parents and teachers got a proper way to see how the whole system works,
without leaving the app or digging through a code repository to find out.

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
