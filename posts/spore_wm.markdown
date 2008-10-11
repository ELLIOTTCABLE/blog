Spore WM
========

I've been thinking about window management systems recently. Let's consider
[Apple][]'s hodgepodge solution for [OS X][] — It started with the [Dock][]
and the [Application Switcher][] (technically a part of Dock.app, but that's
irrelevant). Not too complicated, and it worked well — but had problems when
you had more than a certain quantity of open windows (dependent on the
physical size of your screen).

So, in [Panther][], we saw the release of [Exposé][Expose] — heralded as the
fix for this problem, Exposé allowed a user to open many more windows than
they were previously capable of before the onset of chaos and confusion. Users
being what they are, though, this wasn't enough — with the increase of the
soft limit, came an increase in the need for a way to manage even more open
windows. So, in [Leopard][] we saw the introduction of yet another window
management system component — [Spaces][].

However, Spaces [saw][sc1] [quite][sc2] [a bit][sc3] of [controversy][sc4]
when Leopard was released, as people found it difficult to use — things simply
didn't map well to the way the human mind things. I myself hardly ever find
myself using Spaces, except as an alternative to minimizing a ton of windows — 
if I have a single quick task to get done, but I need to have multiple windows
open to get it done, I'll often switch to a second Space to complete it before
switching back to my primary Space. Other than that, I never use the tool —
it's simply too cumbersome.

[Apple]: <http://en.wikipedia.org/wiki/Apple_Inc.> (Apple on Wikipedia)
[OS X]: <http://en.wikipedia.org/wiki/Mac_OS_X> (OS X on Wikipedia)
[Dock]: <http://en.wikipedia.org/wiki/Dock_(Mac_OS_X)> (OS X's Dock on Wikipedia)
[Application Switcher]: <http://en.wikipedia.org/wiki/Application_switcher#Macintosh> (OS X's Application Switcher on Wikipedia)
[Panther]: <http://en.wikipedia.org/wiki/Mac_OS_X_v10.4> (OS X 10.4, "Panther", on Wikipedia)
[Expose]: <http://en.wikipedia.org/wiki/Exposé_(Mac_OS_X)> (OS X's Exposé on Wikipedia)
[Leopard]: <http://en.wikipedia.org/wiki/Mac_OS_X_v10.5> (OS X 10.5, "Leopard", on Wikipedia)
[Spaces]: <http://en.wikipedia.org/wiki/Spaces_(software)> (OS X's Spaces on Wikipedia)
[sc1]: <http://old.blog.elliottcable.name/articles/2007/11/spaces-solution> (My ages-old post, offering a solution to the "Spaces problem")
[sc2]: <http://blogs.sun.com/bblfish/entry/why_apple_spaces_is_broken> (A post entitled "Why Apple Spaces is broken" by Henry Story)
[sc3]: <http://daringfireball.net/linked/2007/november#mon-12-spaces> (Daring Fireball's link to Henry Story's post)
[sc4]: <http://www.dribin.org/dave/blog/archives/2007/11/13/spaces/> (Another post on the problems with Spaces, by Dave Dribin)

Spore
-----
Now, on a completely unrelated note, I've recently obtained Will Wright's
latest game, [Spore][] (by [EA][]). I have to say, it's pretty fun. It didn't
last long for me, but it's not the kind of game you play repeatedly for a long
time anyway.

Point being, Spore is split up into 5 'stages' of play, each of which is sort
of like a separate mini-game. Howe you choose to play each stage affects the
other stages, and your creature retains properties you give it from one stage
into the next… but other than that, the stages are rather disparate.

The first stage is the one relevant to our topic — the ['cell' stage][cell stage].
In this stage, the player's creature is a simple 'cell' organism in a
(relatively) huge pool of '[primordial soup][]'. In this stage, the playing area is
essentially two-dimensional, just like our desktops currently are (if you've
played Spore before, you may be beginning to see where I'm going with this).
However, at various stages of [microevolution][], one's cell would visually
float upwards a 'level', so that cells that had recently been much larger than
you were now about the same size as you, and cells that had been smaller would
now be floating 'below' you, inaccessible. Occasionally throughout this
mini-game, the player would see absolutely vast cells floating far 'above' them — 
cells so large that they were not yet intractable-with, and as such posed no
threat to the player's cell… yet. After another level or two of advancement,
the player's cell would again encounter this species, but this time it'd be
the same size or marginally larger than the player, and the player would be
able to interact with it. You can see something quite strikingly similar to
this concept in the [flash web-game flOw][flOw].

The pool in and of itself, as a playing area, was essentially infinite — you
could 'swim' in any direction for as long as you liked, and still encounter
activity and life. I think this metaphor could map quite well to a window
management system. An infinitely horizontally- and vertically-scrollable
workspace that extends in all directions, with an additional restrained
third-dimensional component. You could float 'up' and 'down' through the
workspace, with windows 'above' you becoming larger and more transparent as
you dug 'down'… and windows 'below' you becoming smaller and more transparent
as you floated 'up'.

This could be a very efficient method for organizing active projects — if you
had several similar sub projects, they could have groupings of window arranged
on or near the same 'level', and a window that's important to all the sub
projects could be 'above' them all. When you need to reference that window,
you float 'up' enough to see it, then back 'down' to your active projects. All
the while, you've got a completely unrelated project with completely different
applications' windows in a different location in the active workspace, far off
to the left.

[Spore]: <http://spore.com/> (The Spore home page)
[EA]: <http://ea.com/> (The Electronic Arts homepage)
[cell stage]: <http://www.youtube.com/watch?v=WoP5thatpr4> (A video of Spore's 'cell' stage on Youtube)
[primordial soup]: <http://en.wikipedia.org/wiki/Abiogenesis> (Primordial Soup on Wikipedia)
[microevolution]: <http://en.wikipedia.org/wiki/Microevolution> (Microevolution on Wikipedia)
[flOw]: <http://intihuatani.usc.edu/cloud/flowing/black.html> (A flash web-game similar to the initial stage of Spore)

Considerations
--------------
This system would obviously be very open and customizable — you use it however
you want to. You could simulate Apple's Spaces by having a three-by-three
array of groups of windows — float 'up' enough, and you've got a view
identical to that of Spaces, and then float back 'down' to the group you want
to work with. The system could provide a tool similar to Exposé that
simultaneously moves all windows out from all other windows (temporarily), and
floats you 'up' enough to see all the windows you can currently see, but in
their new positions — and then float back 'down' to your original view (re-
positioning the windows as they originally were) after selecting one to be
'moved to the front' (i.e. floated up to your [original] current level).

The ability to 'bookmark' a certain view would be essential, so that you could
quickly jump between tasks without too much time spent tediously floating
around (with the manual spatial maneuvering obviously still available for when
you need to re-arrange things or relate things). Global hotkeys for bookmarks
would expedite things even more. Jumping to a bookmark could include a breif
and requisite animation wherein the user's view was floated 'up' just enough
to see both the original location and the destination, and then back 'down'
to the destination — giving the user the illusion of an actual physical jump
between the two virtual locations.

The interface widgets and images involved in such a windowing system would
have to be fully scalable, so that they worked just as well (visually as well
as functionally) at half or double size as they do at their base size. This
way the layer directly above and all the layers below the user's current layer
could continue to be fully interactive and usable, giving them more freedom to
navigate as they choose. Apple has already introduced [similar technology][ri1] in
their [patent filings][ri2] for [resolution independent interfaces][ri3], but
there's already been plenty of buzzing about that topic already.

[ri1]: <http://arstechnica.com/reviews/os/mac-os-x-10-5.ars/10> (Details on RI in Ars Technica's review of Leopard)
[ri2]: <http://www.cabel.name/2007/01/apples-next-generation-themes.html> (Cabel Sasser's take on Apple's plans regarding RI)
[ri3]: <http://developer.apple.com/releasenotes/GraphicsImaging/RN-ResolutionIndependentUI/> (Apple's documentation for partially RI UI elements in Leopard)

Conclusion
----------
There's been many attempts made at various sorts of 3d interfaces, and none of
them have really worked out too well. I think this is simply because we're
trying to map two-dimensional concepts to a three-dimensional interface on
two-dimensional hardware — that can't possibly turn out well. What I saw in
Spore that inspired me, was how you were given a *restricted* three-
dimensional working area — you were actually working in a two-dimensional
space, but that space mapped to a single layer in a super-workspace in three
dimensions.

Although I don't really have the programming-fu to do anything about it (the
extent of my desktop-UI programming ability is some mucking about with Ncurses
in Ruby), I think this idea has potential.