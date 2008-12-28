Git-Versioning Interface Builder's XIB files
============================================

So, I've been getting into [Cocoa][] recently. Finally. Learning Cocoa has been
a goal of mine for, what, a year or more now? I've just always had trouble
with, mostly because of the [Objective-C][] syntax. It's always seemed really
obtuse to me. The idioms and, well, “feel” of the Objective-C community's code
hasn't helped either. However, that's for another post.

Originally, I had hoped to follow my purist's heart and start with getting
plain [C][] completely down pat, then move on to Objective-C, and finally
onwards to the Cocoa framework. After all, the decision to learn [Ruby][]
before [Rails][] is what made me the programmer I am today.

Unfortunately, I had a lot of trouble finding excuses to learn plain [C89][] — any
project I could do in C, I could much easier do in Ruby. After all, who really
cares about execution speed nowadays? Certainly not me. Given that I can't
learn about something except by ‘doing’, I was kind of screwed for about a
year. I kept coming back to [K.N. King][King]'s [*C Programming — A Modern
Approach (Second Edition)*][c-programming], and subsequently getting bogged
down and not actually learning much of anything useful.

I finally gave up, and skipping over the Objective-C specific book that I had
bought, [Stephen G. Kochan][Kochan]'s [*Programming in Objective-C*][objective-c-programming]
, I proceeded directly to [Aaron Hillegass][Hillegass]'
[*Cocoa® Programming for Mac® OS X*][cocoa-programming]. This rubbed me
against the grain, but so far it has resulted in me actually learning
something new about Cocoa, after nearly a year of completely failing to do so.

Now, to dispense with the dry history that nobody cares about save me, let's
get on with the point of this article. [git][]. That ever-so-sexy source
code management / version control system? The one that's [all the rage][gitisbetter] nowadays?
This isn't an article about how awesome git is and how you should use git, so
I'll not say too much about it, but if you're not using it, you should at
least check it out. Especially if you're still using [Subversion][] (*eww!*).

If you've used git on a Cocoa project (or, for that matter, any project in
which you utilize [Xcode][]), you know that it's a bit finicky. You have to
put in place some [special][gitignore] [files][gitattributes] to even begin to
work with Xcode projects in a git repository, and then you're still left
babysitting every commit you make.

After my first few weeks working in Cocoa, I've ended up paying very special
attention to the changes that [Interface Builder][] makes every time you save
a [`.xib`][xib] file. What I've discovered is that Interface Builder makes lots
of superfluous changes to these files each time you save, and these changes
really clutter up your git repository — they can cause all sorts of problems,
from breaking merges that would otherwise be fine, to increasing the entire
byte-size of your repo, to making it *that much more difficult* for an
outsider to come read your changelog and learn from what's going on (if your
repository is a public one).

So, without further ado, let me go through what I've learned.

  [Cocoa]: <http://cocoadevcentral.com/d/learn_cocoa/> (Cocoa Dev Central: Learn Cocoa)
  [Objective-C]: <http://gnustep.org/resources/ObjCFun.html> (GNUstep: Fun with Objective-C)
  [C]: <http://en.wikipedia.org/wiki/C_programming_language> (C programming language)
  [Ruby]: <http://ruby-lang.org/en/> (Ruby programming language)
  [Rails]: <http://google.com/search?q="rails+isn't"> (What Rails... isn't)
  [C89]: <http://en.wikipedia.org/wiki/ANSI_C> (The ANSI C standard, a.k.a. C89)
  [King]: <http://knking.com/> (K.N. King's homepage)
  [c-programming]: <http://amazon.com/dp/0393969452/elliottcable-20> (“C Programming: A Modern Approach” on Amazon.com)
  [Kochan]: <http://kochan-wood.com/> (Stephen G. Kochan's homepage)
  [objective-c-programming]: <http://amazon.com/dp/0672325861/elliottcable-20> (“Programming in Objective-C” on Amazon.com)
  [Hillegass]: <http://bignerdranch.com/instructors/hillegass.shtml> (Hillegass' homepage)
  [cocoa-programming]: <http://amazon.com/dp/0321503619/elliottcable-20> (“Cocoa® Programming for Mac® OS X” on Amazon.com)
  [git]: <http://git-scm.com/> (git, the fast version control system)
  [gitisbetter]: <http://whygitisbetterthanx.com/> (why git is better than <x>)
  [Subversion]: <http://svnbook.red-bean.com/> (Version Control with Subversion)
  [Xcode]: <http://developer.apple.com/Tools/xcode/> (Apple Developer Tool — Xcode)
  [gitignore]: <http://github.com/elliottcable/cocoa_play/tree/62aa35e1095c139be9168b2c5186693dcbb7d223/.gitignore> (My current Xcode-specific .gitignore)
  [gitattributes]: <http://christopherroach.com/archives/35> (Christopher Roach's “Git for the Cocoa Developer: A Typical Workflow”)
  [Interface Builder]: <http://developer.apple.com/tools/interfacebuilder.html>
  [xib]: <http://developer.apple.com/documentation/developertools/conceptual/IB_UserGuide/BuildingaNibFile/chapter_3_section_2.html> (Interface Builder User Guide: About Nib and Xib Files)

Types of hunks and changes
--------------------------
### Moving windows in Interface Builder
<script src="http://gist.github.com/40195.js">
  // 
</script>

I see these hunks all the time. I always think, apparently mistakenly, that *I*
have control over the locations of the windows when I'm working. But god
forbid you move a window around, because Interface Builder will want to save
that change to the repository. For the main application window, I can kind of
understand this — you're just setting the default starting location for the
window. But for the menubar‽ That makes no sense whatsoever.

The keys to recognizing changes relating the location of windows that you've
moved around, as far as I can tell, is that it's a sizing change (see below)
prefixed by an identifier containing `com.apple.InterfaceBuilder.CocoaPlugin`.

From what I can tell, this is also manually editable; if you want to change
the default starting positions of windows (and, for whatever reason, the
menubar) in your application, feel free to mux with the numbers.

Unless I specifically want to change the starting location of a window with a
commit, I never stage these hunks.

### Sizing changes
<script src="http://gist.github.com/40196.js">
  // 
</script>

These also appear a lot in my diffs. Any time you change the size of an
element in the Interface Builder, these show up. Often matching to the classes
of `NSFrame`, `NSWidth`, `NSSize`, and similar, these hunks are usually fine
and should be staged if you've actually changed something sizing-related in
this commit. However, occasionally, I see Interface Builder spit out some of
these changes even when they have nothing to do with my commit (e.g. I didn't
intentionally re-size anything) — in those cases, I do *not* stage these hunks.

Again, these are manually editable if you so please.

### Identifier changes
<script src="http://gist.github.com/40200.js">
  // 
</script>

These get complicated. These IDs reference frozen objects in the NIB system,
as I understand it. When the XIB is compiled into a NIB, objects are created
and serialized into the NIB. When these objects are created, any two places
where an object is referenced with the same ID in the XIB, they reference the
same object in the objectspace that is subsequently created. Again, this is
what I'm assuming, not what I know for sure — as far as I know, all of the
specifics regarding this are jealously guarded by Apple.

When there are changes involving `id=""` or `ref=""` attributes, it comes down
to three different situations. Either the ID is changed, the whole ID
attribute is removed with it's old value, or the whole attribute is added with
a new value.

If it's a new ID number that has been added, chances are that you should stage
it. This often refers to a new element that you've added (see below), or a
change that has caused an existing element to need a reference ID.

If it's an ID number that has been removed, again, you should probably stage
it — it possibly references an element that you have removed in this commit,
or an element that itself no longer needs an ID is referenced.

If an ID number has changed from one number to another, it's about half and
half in my experience. Sometimes it's relevant, sometimes it's not.

In all situations, to be sure, you should really grep the relevant file for
the ID number, and be smart about it. If an ID number changed in a hunk, and
the you added an element, check the ID number of the element you added. If
that's the new number, you should probably stage the hunk involving the
relevant ID number. If an ID number was removed in a hunk, and the element
that that ID number referenced was also removed (or it's ID number was also
removed), then you should stage that hunk, so you don't end up with a
reference to an non-existent ID. Just use common sense, and utilize the `git
stash` method described below.

For instance, in [this commit][commit], I chose to stage several hunks (lines
[1138][], [1149][], and [1158][]) that only added ID references to existing
objects. This is because the references added referred to objects in the new
block that was [added below][added].

  [commit]: <http://github.com/elliottcable/cocoa_play/commit/a2961c24d51a919f083022a3236c05403f2719ee> (Commit a2961c2 to elliottcable's cocoa_play on GitHub)
  [1138]: <http://github.com/elliottcable/cocoa_play/commit/a2961c24d51a919f083022a3236c05403f2719ee#L0L1138>
  [1149]: <http://github.com/elliottcable/cocoa_play/commit/a2961c24d51a919f083022a3236c05403f2719ee#L0L1149>
  [1158]: <http://github.com/elliottcable/cocoa_play/commit/a2961c24d51a919f083022a3236c05403f2719ee#L0L1158>
  [added]: <http://github.com/elliottcable/cocoa_play/commit/a2961c24d51a919f083022a3236c05403f2719ee#L0R2812>

### Flag changes
<script src="http://gist.github.com/40398.js">
  // 
</script>

These are an easy one. These, in my experience, are nearly always intentional.
They seem to result exclusively from options that you specifically change in an
element's inspector in Interface Builder, so you nearly always know that you
want to stage them for sure.

Identification is easy — pretty much any `key=""` that contains `NSvFlags` or
some variation thereupon is an insta-stage in my book. However, unless you're
quite a lot more lucky than I, and happen to know what every possible integer
value maps to in terms of object options, I wouldn't suggest manually changing
these.

### Sequences
<script src="http://gist.github.com/40400.js">
  // 
</script>

Another easy one, I find these to be pretty easy — they seem to appear in
conjunction with a large, purposeful addition or removal of objects, and are
almost always necessary. Again, don't edit these by hand, I have no idea what
they do.

They're easily identifiable by the fact that they add lines that pretty much
mirror the other pre-existing lines surrounding them.

### Large changes
I don't want to directly include a [gist][] of this type of change in here,
because they are, by nature, long. I will, however, [link one](http://github.com/elliottcable/cocoa_play/commit/a2961c24d51a919f083022a3236c05403f2719ee#L0R1165)!

These are an obvious add in every situation where I've come across them. Huge
hunks are nearly always demonstrative of additions or removals of actual
elements in the Interface Builder, and those are (of course) made on purpose.

Whether or not they should be edited by hand, well... that's up to you, it
depends on what part of it you're planning to modify.

  [gist]: <http://gist.github.com/> (Gist, the best git-pastie service EVAR)

### `maxID`
<script src="http://gist.github.com/40402.js">
  //
</script>

Anytime the `key="maxID"` element is changed, stage its hunk. It is vitally
important, and not doing so will cause Interface Builder to crash.

Dealing with all of this
------------------------
If you're like me, this is all rather overwhelming. It's a lot of stuff to
watch out for, and it means checking the XIB file manually after every...
single... change. It's extremely time consuming.

At some point, I may write a script or tool to mostly take care of this kind
of thing for you, but until then, here's some existing tools to make dealing
with it a bit easier.

### GitX
[GitX][] is a wonderful tool, developed mostly by [Pieter de Bie][Pieter] and
[Ciarán Walsh][Ciaran]. It allows you to view and interact with diffs against
the working directory at any given time, and stage hunks interactively in a
usable GUI.

I suggest spending your time with the GitX window open on another screen (if
you have one) next to your Interface Builder window, and sitting in the Commit
pane of GitX. Then, whenever you make a change in Interface Builder, save the
file, and hit ⌘R in GitX. You'll be rewarded with a differential view of what
Interface Builder just changed. At this point, I use those sexy little blue
'stage' buttons extensively, to add those parts that I think I want.

See the next two sections for further information of the rest of the workflow,
since GitX only provides staging functionality (as of this posting, though
hopefully Pieter and I can work out a GUI to do more of this - see [some][ticket1]
of my [tickets][ticket2]).

  [GitX]: <http://github.com/pieter/gitx/wikis/home> (GitX's wiki on GitHub)
  [Pieter]: <http://frim.nl/> (Ciarán Walsh's homepage)
  [Ciaran]: <http://ciaranwal.sh/> (Pieter de Bie's homepage)
  [ticket1]: <http://gitx.lighthouseapp.com/projects/17830/tickets/48-staging-of-single-lines>
  [ticket2]: <http://gitx.lighthouseapp.com/projects/17830/tickets/52-commit-view-selects-first-unstaged-item-every-time-you-stage-an-item>

### `git add --interactive`
This is the bog-standard way of interactively working with the stage. Using
the `5. (p)atch` option will allow you to iterate through the hunks of the XIB,
watching for different types as they appear above. If multiple types appear in
one hunk, you can try the `(s)plit` directive to tell git to try to use less
padding in calculating the contents of that particular hunk.

When you've staged the bits that you believe are relevant, you can either `git
checkout <filename>.xib` or try the `git stash` method described below. In
either case, you'll then want to switch back to Interface Builder to ensure
the changes you ignored still leave the file valid. Interface Builder will
alert you that the files have changed, and you should chose the 'Revert'
option. If Interface Builder crashes, you'll know that you left unstaged at
least one hunk that was necessary to Interface Builder's understanding of the
file.

### `git stash save --keep-index` and `git stash apply`
If you prefer not to destructively revert the unstaged hunks (which is what
the `checkout` command above will do) after staging a set of changes that you
believe to be complete (for instance, if you're not entirely sure whether or
not that `ref=""` attribute change you ignored was actually important), you
can instead use git's stash store to store the hunks you believe to be
irrelevant. Once you've staged the hunks you believe to be important using
GitX or `git add --interactive`, you can use `git stash save --keep-index` to
toss the rest of the (unstaged) hunks onto the stash pile.

At this point, go back into Interface Builder as you normally would, and make
sure it doesn't crash from the hunks you ignored (and that you didn't
accidentally revert an entire intended functionality!). If it *does*, you can
recover the hunks you deemed unworthy and continue staging bits by running `git stash apply`.

If you've been using this method for a while, you'll have a lot of stash saved
up. You should thus occasionally clear it with `git stash clear`.

Conclusion
----------
I really hope all the time I've spent investigating Interface Builder's XIB
files will benefit somebody other than me. I know how much of an arduous task
managing all of these files by hand is, and I wish there was a better way.

Hopefully, Apple will eventually make their XIB file format even more clear
and semantic, allowing us to better understand what's going on in there, and
improving mergability. If not, well, at least we're not totally out in the
cold!

On a last note, given how much trouble manually policing each commit like this
is, you can expect some sort of tool from me in the future to diff/merge/keep
clean these XIB files, though I don't know exactly when I'll consider myself
knowledgeable enough / have the time to work on it.

Go forth then, and be gitty! All hail the Apple!