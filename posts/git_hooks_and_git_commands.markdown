git hooks and git-* commands
============================

Well, this post is half to test my fix, and to share the results with the world. If I'm right, this post should require no fiddling (past a simple `git-push`) to publish. If you ever have reason to run a git command (i.e. git-something) inside a git hook (i.e. post-receive)... make sure to manually set `$GIT_DIR` to the path to the relevant `.git` directory before doing anything else. Here's an example, from my git-blog post-receive hook file:

    #!/bin/bash
    GIT_DIR=$(pwd) # Note that the hooks are run from inside the .git dir
    cd ../
    git-checkout -f master

As it says, notice that the hook itself is run with `pwd` being the .git dir of the repository that was updated. Thus you have to `cd ..` before you can actually work with the repository.

Hope this helps somebody who has been as stumped as I have for the past few hours!