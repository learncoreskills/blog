# Day 5: making child setup faster and clearer

Before today, adding a new child to the app meant filling in their name, then scrolling down to find a separate grade selector somewhere below. Today that grade selector moved right into the form itself—no scrolling, no hunting, just a dropdown next to the name field where you'd naturally expect to find it.

**Why this small change matters more than it sounds**

When a parent sits down to set up their child's profile, they're thinking in one direction: name, grade, done. Before, that thought got interrupted by a card appearing below the form asking "by the way, what grade are they?" It was optional (you could skip it and set the grade later), but it was also separated, creating a moment of "wait, do I need this?" and a small step back from the task. Now it's just another field in the same form: name, grade dropdown, add button. One shape, one flow, done.

The dropdown itself is simpler to use than clicking individual grade buttons too—it's familiar to anyone who's filled out a form before. Pick from the list (P for Pre-K, then Grades 1 through 5), and if you're not sure yet, the default "Select grade..." is right there, letting you come back to it later without any friction.

**It's still optional**

The grade you pick when adding a child still isn't locked in stone—parents can change it later in the Parent Area if they got it wrong or if a child gets promoted mid-school-year. But now the app doesn't *hide* that option away; it puts it where you're already looking, in the moment when you're setting up that child's profile for the first time. Less thinking, fewer steps, just the setup parents actually need.

**A clearer front door**

The other change today is about what greets you when you open the app. Until now, the very first screen tried to do everything at once—add or switch a child, pick an activity, and answer questions, all stacked on top of each other. It worked, but it meant the "getting ready" part of the experience and the "actually practicing" part never really separated from one another.

Now, that first screen has one job: help you get the right child selected, and show you what's available to practice. Once a child is active, a short list of subjects appears—for now, that's just Mathematics, since it's the only subject the app teaches today, but the app is built so more can join that list without changing how any of this works. Picking a subject takes you somewhere new: a page that belongs to that subject alone, where the actual activities and practice sessions live. Finish a session there, and you land right back on that same subject's activity list, ready to pick the next one—no detour back through the child-picker in between. There's always a way back to the front door, too, whenever you want to switch children or head off to try something else.

It's a small reorganization, but it sets up something bigger: a home base that scales. Today it's one subject, presented plainly. Down the road, as more subjects come online, this is the layout that lets them show up side by side without the front page getting crowded or confusing.

**Seeing the whole year at a glance**

The last piece of today's work answers a question every parent eventually asks: what is my child actually supposed to know by the end of this grade, and how much of it have they got down already? Up to now, the honest answer meant piecing it together from a scattered set of tags and reports. Today, Mathematics gets a proper syllabus view: open the subject, flip to "Syllabus," and see every skill expected at your child's grade listed out in one place, each one clearly marked Passed or Not yet.

It's a real reflection of practice history, not a guess. A skill only earns "Passed" once there's enough recent practice behind it to trust the result, and even a skill that was passed before can slip back to "Not yet" if recent answers haven't kept up—the label always describes where things stand right now, never a badge earned once and forgotten. Curious what next year looks like, or want to check on something from an earlier grade? Little arrows let you step to the grade before or after without losing your place or touching your child's actual grade setting—it's just a window you can look through, not a lever you can pull. And if a "Not yet" catches your eye, clicking it drops your child straight into practice on exactly that skill, at exactly that grade, no hunting through menus required.

Under the hood, how a skill earns its "Passed" label also got an overhaul—the bar is a little more forgiving on the number of tries it takes to prove yourself, but firmer on requiring genuinely sustained accuracy rather than a lucky short streak. That's expected to keep evolving as we see how kids actually use it; it's a starting point tuned by judgment, not a final answer carved in stone.

**Tidying up that new syllabus view**

Once the syllabus view was actually in front of real eyes, a few rough edges showed up fast. A row of topic tags that used to sit above the Practice/Syllabus toggle was quietly showing nothing useful most of the time, so it's gone now—one less thing cluttering the top of the page before you've even started. The Practice and Syllabus buttons themselves, along with the little "Previous grade" / "Next grade" arrows, got a proper visual pass too: they now look and feel like the rest of the app instead of like plain, unstyled buttons dropped onto the page. And the syllabus list itself—the actual "here's what your child should know" content—got the same treatment, with each skill's Passed/Not-yet status now shown as a clear little badge rather than plain text sitting awkwardly next to the name.

The biggest fix, though, was one of clarity rather than polish: the syllabus page used to just say "Syllabus" at the top, with no indication of which grade you were actually looking at. Now it names it plainly—"Pre-school baseline," "Grade 1," and so on—so a glance at the page tells you exactly what stage of the curriculum is on screen, with a small "X of Y passed" count sitting right underneath for good measure.

One more rough edge got smoothed out after that: the syllabus opens straight to your own child's grade now, rather than making anyone hunt for it, and each row got rethought to behave less like a button and more like a menu. What a child is expected to know—the skill's name and whether it's Passed or Not yet—now just sits there plainly, the same way it does in the "what your child will learn" overview parents already know from the Parent Area; there's nothing to accidentally click on by tapping the skill's name. Underneath it, the actual way to practice that skill shows up as its own small tappable chip—sometimes more than one, if a skill can be practiced a couple of different ways—plus a little dice-shaped "Random" option when there's a choice to make and no strong preference either way. For the handful of skills that don't have a practice activity built yet, the row simply says "Coming soon" instead of pretending there's something to click. It's a small distinction, but it matters: now the row tells you *what* you're expected to know, and the chips underneath tell you *how* to go work on it—two different questions that no longer get answered by the same click.
