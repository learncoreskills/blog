# Day 6: A phone-like navigation for mobile, clearer menus everywhere

Today's work tackled a list of small but nagging UI gaps that felt especially sharp on smaller screens. Replace "Menu" with an icon. Make the flag visible everywhere, not just on desktop. Turn switch-child and switch-language into buttons that open full-screen panels instead of hiding them in dropdown menus. Let users toggle those panels closed again with a second click, just like a phone app would. The result is an interface that finally feels built for mobile first, even if you're reading it on your laptop.

**From text to icon, in one character**

The navigation bar at the top of every page used to say "Menu" as plain text next to a clickable area. On a phone, that's wasted real estate—space you could use for actual content, or just breathing room. Today that became a simple hamburger icon: ☰. It's a convention everyone recognizes, takes up almost no room, and leaves the bar feeling less cluttered. Click it, and the same dropdown menu appears—Home, Docs, Blog, Parent Area—unchanged underneath. But now it gets out of the way when you're not looking for it.

**The flag that got lost on phones**

The language preference (that little 🇬🇧 or 🇫🇷 flag showing which language you've chosen) was appearing fine on a full-width screen, but on narrower phones it had a habit of vanishing or getting squashed. The problem was partly layout—everything was competing for space in the top bar—and partly CSS rules that were hiding the flag when real estate got tight. Today that got fixed: the flag is always visible now, at every width, because it's important enough to stay. If the bar runs out of room, everything else squeezes smaller first; the flag stays readable.

**Switch-kid and switch-language become proper buttons**

Before, if you wanted to change which child was active or pick a different language, you'd click "Switch kid" or "Switch language" links hidden inside that Menu dropdown. They were there, buried in the same list as navigation, acting more like menu items than the significant controls they actually are. A parent switching between their daughter and their son mid-session shouldn't have to dig through a menu—it should be quick and obvious.

Now, right there on the navigation bar at the top, there are two new buttons: one with a child emoji (👧 when showing an existing child, 👋 if none have been set up yet), and one with a globe (🌐). Click either button, and a panel slides down from the top of the screen, full-width, and stays there until you close it. That panel shows you the list of children to switch to, or the language choices, depending which button you clicked. And here's the phone-app behavior: click the same button again, and the panel closes without selecting anything—a toggle. Frustrated with the open panel? Click the X button in its header, and it disappears. Want to switch from one child to another? Click the child's name, watch the panel close, and the new child is active. The whole experience feels immediate, not buried.

**Panels at the top, not scattered on the page**

Before, the "Switch kid" and "Switch language" options lived as regular cards somewhere down on the home page, mixed in with the subjects list and everything else. They got the job done, but they meant a parent looking to switch children had to know to scroll down the home page and find that card. It's not obvious, and it's not fast.

Now those controls are at the top of every page, instantly accessible, and when they open, they're full-screen panels that take your attention—not just another card on a list. The panels are positioned above the main content, so clicking inside them is responsive and clear: this is the control, that's the work you're about to do. If you're on the Mathematics page and you want to switch to your other child before starting a new session, the button is right there at the top, always. No scrolling up and back to home to find the control you need.

**The experience under the hood stayed the same**

None of this changed how the app actually stores or uses your data. A child is still a child, a language preference is still a preference, and all your practice history is still right where it was. The app still remembers who you were last using and what language you last picked. All that changed is where the controls live and how they behave—making them faster to reach and more phone-like in feel.

For parents who prefer bigger buttons and clearer headers over compact dropdowns, this was a big win. For the app itself, it's a foundation: as more controls move to the top navigation—report cards, settings, notifications—they'll have a consistent home instead of scattering across different pages or hiding deep in menus.
