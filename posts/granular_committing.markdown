Granular Committing
===================

I’m going to attempt to quickly document my git usage strategy, as doing so seems to be popular
lately. My style is slightly different from the one that many others use, so I suppose somebody
might be able to learn something from this.

First off, some terminology that I’m going to use throughout the document (other than these,
standard git terminology applies as well, such as ‘remote’ and ‘upstream.’ In some of these cases,
the case of the words is important):

-  Official: the final remote repository that others will pull from (`github.com/Paws/Paws.c.git`)
-  Personal: the remote repository that you work in (`github.com/elliottcable/Paws.c.git`)

-  Master: the branch that completed work units are merged into
-  topic branch: a branch for the discrete safe-stages of a single work unit
-  granular branch: a branch for grains, that will later be squashed onto the topic branch

-  work unit: a single independent feature addition or bug-fix, existing separate from others
-  safe-stage: a single chronological subunit of a work unit that compiles and passes all tests,
   regardless of whether the work unit itself can be considered “complete” (these will
   eventually become commits against Master)
-  grain: individual working commits that will be squashed into a safe-stage

Don’t worry if a lot of those don’t make sense yet. We’ll get to that.

### Goals
Let me state some goals I have that differ from those of most git workflows:

-  I *care about the end-state of the commit list*. I care that when somebody comes to the project
   later, they can easily learn the history of the project by browsing the commit list; and I care
   that the commit list itself is a coherent, easy read.
   
   In addition, I care that individual commits in the comprehensive history individually compile and
   operate ‘correctly,’ as ‘correctly’ was understood in that point in history (obviously, this
   includes bugs unknown at the time of that commit, or the absence of features added later.) This
   allows later visitors to utilize any particular version of the project that they desire, simply
   by checking out at a given point and compiling.
   
   Because of this, I require that all of the following be true for “safe-stage” commits (I’ll
   explain the meaning of that phrase below, don’t worry):
   -  Must fully compile without any warnings, and pass all tests introduced in any earlier commits.
   -  Must have a well-written and comprehensive commit message (usually several paragraphs, but at
      the very least a few lines) describing all changes made at that stage, and the rationale
      behind those specific changes
   -  Must be [properly and thoughtfully labeled][.gitlabels]
   
-  I want a complete history of the changes made to the codebase, including intermediate steps. This
   means several things are off-limits:
   -  Traditional “squashes” or “fixups” (the squashing of a group of grains into a safe-stage is an
      important exception to this rule, that I will discuss later)
   -  Re-writing history in any way, for any reason
   -  Discarding old branches
   
-  Finally, I generally dislike ‘merge commits’ as they are supported by git. I tend much more
   towards manually rebasing, or when absolutely necessary, squashing. I prefer a single, linear
   history to a spaghetti of merges.

   [.gitlabels]: <https://github.com/elliottcable/.gitlabels#readme>

### Local strategy
First we’re going to discuss how I work locally, independent of the outside world (and any remotes.)
This methodology works equally well whether working alone or with a team.

As do many git users, I do all of my work in “topic branches.” These are individual git ‘branches’
that exist for the purpose of a single task; what I call a “work unit.” These branches are
*completely* dedicated to a single task; even if I come across code that needs tertiary changes or
recognize a bug while working on the relevant task, I cannot make those changes immediately; I must
stash my changes (or commit them as a grain, if I feel they are sufficiently ready) and switch to
another topic branch (or simply to the Master branch, if the change is of a sufficiently dissociated
nature to justify that) before making those tertiary changes.

However, I do not commit *directly* to the actual topic branch as I work; instead, I commit actively
and continuously to what I call a “granular branch.” I usually name the topic branch something like
“a-topic” and then create a granular branch off of *that* named “a-topic!” (notice the exclamation
point!) for intermediate grains.

As I make changes in the code, I will constantly commit those changes as “grains” (granular
commits), each of which will often contain anywhere from a few characters to a few hundred lines of
impact. Usually, I find that they tend towards only a few lines of change, separated across several
locations (when fixing bugs), or several dozen lines of change focused in one or two locations (when
adding portions of a new feature.) There are no implicit restrictions on the content of grains, or
on when you commit them; nor are there any implied restrictions on the state of the codebase at the
time you commit a grain. Hell, if you feel it appropriate, you could commit a half-finished sentence
or an unclosed parenthetical expression. Occasionally, I will even use `--allow-empty-message` to
make a quick grain-commit with no noise or interaction whatsoever:

      $  git config --global alias.rain 'commit --allow-empty-message -m ""'
      $  git rain
      [a-topic! deadbee]
       2 files changed, 12 insertions(+), 3 deletions(-)

The next step comes into play once I’ve reached a stopping point where the code compiles and the
tests that I’ve added thus far (if any) pass. At this point, assuming all my conditions (as listed
earlier) are met, I’ll make a new “safe-stage commit” to the topic branch itself (not the granular
branch I’ve been working in so far), by squashing all of the new grains. This usually consists of a
command like the following:

      $  git checkout 'a-topic'
      $  git merge --squash 'a-topic!'
      $  git commit --file=- <<-"EOF"
            (new api) Implemented a new foofer on the schnitzel.
            
            One can expect the schnitzel to properly …
            
            Squashes: dead123..bee4567
         EOF
      $  git checkout 'a-topic!'

(Obviously, the commit message is usually entered interactively, but I wanted to present it here.)

This process is cycled repeatedly, working in granular branches and constantly comitting grains,
until we reach points where we can archive groups of grains into a stable “safe-stage commit” on the
topic branch itself.

The final phase of this process is when the bugfix or feature is *fully completed*, with a complete
suite of tests (all passing, of course), and with all requirements handled. At that point, I will
fast-forward my Master to include all of the new safe-stage commits I’ve created in the process of
this topic branch.

As I mentioned above, I very much dislike git’s ‘merge commits,’ so I will generally not use
`git merge` to bring the topic branch’s safe-stage commits back into Master. If there has been
further advancement in Master since I begun on the topic branch (either by myself, with tertiary
topics; or by others, fetched from an upstream remote), I will generally at this point manually
rebase the topic *and granular* branches onto the current Master, with commands such as:

      $  git rebase Master 'a-topic!'
      $  git rebase Master 'a-topic'

Once the topic branch is fully prepared, and we’re ready to bring the changes into Master, we simply
fast-forward Master to match:

      $  git checkout Master
      $  git merge --ff-only 'a-topic'

That’s it!

Some final notes about the above process:
-  The granular branch for a given work unit *parallels* the topic branch itself, instead of being
   branched off of the topic branch at any point. Similarly, the granular branch and topic branch
   contain completely separate commit objects, with no intersection whatsoever (because the grain
   commits from the granular branch are always completely squashed into new “safe-stage” commits on
   the topic branch itself.)
-  The granular and topic branches are intended to be alternate views of history; both are intended
   to be ‘stored for posterity.’ The only way to infer linkage between a given set of
   granular-branch commits (which never make it into Master themselves), and a given “safe-stage”
   commit (on Master or in a topic branch), is via the “Squashes:” line at the end of the
   “safe-stage” commit message. Thus, adding this line is essential for automated traversal of
   the history at a later date.

### Remote strategy
Now, because code never exists in a vacuum (or rather, if it does, you’re doing something wrong),
we’ll discuss how I interact with remote repositories, and how I manage my own set of remotes.

I’m a big proponent of github. Each of my local repositories has a remote copy on github; and some
of my larger projects also have a separate “official” repository on another github ‘organization.’
As examples, we’ll be using my Paws.c project, for which I have two such remotes: my own, at
elliottcable/Paws.c, and the ‘official’ repo, at Paws/Paws.c. In addition to these, I often interact
with other remote repositories; in most cases, these are mirrors of the “official” repository on
other hosting services (for example, I keep a remote copy of the Paws.c repo on my personal server,
and copies of the official upstream on other services like http://repo.or.cz/ and [Gitorious][].

Every time I squash some grains into a “safe-stage” commit (or nearly every time; depending on how
often, chronologically speaking, that is happening), I will push that commit to the
identically-named topic branch on my personal remote.

         <as above, git merge --squash>
      $  git push Personal 'a-topic:a-topic' 'a-topic!:a-topic!'

Similarly, when I complete the task and rebase the topic to Master, followed by fast-forwarding
Master up to the topic branch, I’ll force-push the rebased topic branches, followed by
fast-forwarding the remote Master as well:

         <as above, git rebases and git merge --ff-only>
      $  git push Personal Master:Master 'a-topic:a-topic' 'a-topic!:a-topic!'

Now, this is only how I manage my “Personal” remote. The “Official” remote is handled differently:
when several work units (bugfixes and new features, aggregated) are completed and pushed to
Personal/Master, I write up a github ‘pull request’ detailing that set of work units, and increment
the version number of the project itself. (I don’t use [semver][]-style versioning; I instead opt
for a solitary, single-incrementing “release number.”) In this way, each “version” of the project
matches a single pull request on github, detailing the changes in that version (as aggregated from
several local work units.) I generally wait a few days after having created the pull request, giving
interested parties an opportunity to explore it and make commentary, before subsequently
fast-forwarding the Official/Master to match my Personal/Master, thus officially “releasing” a new
version of the project.

Additionally, I maintain a ‘Stale’ branch on my Personal remote, that tracks exactly the
Official/Master branch. This is purely for convenience.

         <pull request has been created and left for a while>
      $  git push Personal Master:Stale
      $  git push Official Master:Master

   [semver]: <http://semver.org/>

### tl;dr
And with that, we’re done. In summary, your changes are going to ‘bubble’ through the following
path, on approximately the listed timelines:

      grain commits on granular branch (every few minutes) ->
         safe-state commits on topic branch (hourly) -> 
            safe-state commits on local Master branch (daily) ->
               commits on remote Personal Master (daily) ->
                  pull request to Official Master (monthly) -> 
                     commits on remote Official Master (monthly)

I hope you’ve learned something from this post. Failing that, you will have, at least, realized that
I am completely and irreparably insane.

   [Gitorious]: <http://gitorious.org/>
