Multi-destination git-remotes
=============================

Just a short post on something I've been using lately. I'm not sure if it's a
feature or a bug, but you can supply multiple URIs for a single faux
git-remote entry. When you do this, you can no longer pull from this remote
(or, at least, I haven't dared to try that yet), but you can still push -
which simply consecutively pushes to each remote listed.

This is what it looks like - notice the `origin` remote's entry, and how it
combines the URIs for the `github` and `gitorious` remotes.

<script src="http://gist.github.com/2015.js"></script>

On a side note, [gist](http://gist.github.com/ "gist - versioned pasting") is
absolutely *full* of win. Go use it. Love it. Hold onto it with all your
heart.