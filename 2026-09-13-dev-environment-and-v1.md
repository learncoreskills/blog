# Day 2: a real dev setup, and a first version kids can actually use

Day two started with a decision that had nothing to do with kids or math: getting a proper place
to actually build the thing. By the end of the day, that unglamorous groundwork had turned into
something real — a first version of the app a child could genuinely open and practice with.

The morning went into the development setup itself: a dedicated machine to build on, an AI coding
assistant to build with, and a proper code editor tying it together — deliberately kept separate
from a personal computer, so the project has its own clean, reproducible environment from the
start rather than being tangled up with anything else. Not exciting work, but the kind that pays
for itself the moment something needs debugging six months from now.

Once that was sorted, the rest of the day went into turning yesterday's plans into a real, working
first version. Everything built fits around one simple loop, repeated every time a child answers
a question:

```
Question  ->  Answer  ->  Feedback  ->  Progress updates  ->  Next question
```

Simple to say, but every step of it needed real thought behind it:

**A place to open, and room for the whole family.** The app now has an actual shell — screens a
parent or child can open, not just isolated pieces of logic. Profiles were built in from the very
start: a parent can set up more than one child on the same device, completely free, and each
child's practice and progress stay fully separate from any siblings sharing the device. Setting up
a profile asks for almost nothing personal — a nickname is enough, nothing more is required —
which was a deliberate choice, not an oversight: the less personal data the app asks for, the less
there is to ever worry about.

**The first real exercise.** A child can now work through a set of mental addition questions,
matched to a level chosen for them, with feedback the moment they answer — right or wrong, no
waiting. Get one wrong, and the correct answer is shown before moving on, rather than leaving a
child stuck or guessing again. There's deliberately no visible timer or countdown anywhere on
screen; the goal is focus, not the feeling of a clock ticking down on a seven-year-old.

**Progress that means something.** Maybe the most important piece: a child's progress on a skill
isn't just a raw count of correct answers. It's designed to reflect a real, sustained pattern
rather than one lucky run — the app waits for enough attempts before making any claim at all, and
only calls a level "learned" once a child has been consistently getting it right, not just once.
A single good session doesn't inflate the number, and a single bad one doesn't wreck it either.

**Built so the next exercise doesn't mean starting over.** Alongside addition itself, today's work
also nailed down the *shape* every future exercise has to follow — how it asks a question,
checks an answer, and reports what happened. That's what makes it possible to add a second
exercise later (subtraction, say, or multiplication) as its own self-contained piece, without
having to touch or risk breaking anything already working for addition.

Small pieces on their own, but put together they added up to something a child could actually
open and use: pick a profile, answer real questions at a level that fits them, get honest
feedback right away, and see progress that reflects how they're actually doing — not just how
lucky today happened to be.
