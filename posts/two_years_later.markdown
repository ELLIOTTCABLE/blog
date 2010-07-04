Two years later
===============

Hah! My one‐off hacky code usually doesn’t have this kind of longevity. I was working on
[my résumé](http://elliottcable.name/resume.xhtml), and decided to re‐enable the navigation section on my
[landing page](http://elliottcable.name/). That navigation included a link to this blog, which I had entirely
forgotten about; going back to set it back up, I found the content was lost in my server move.

Well, luckily, I based my blog on git, right? All the *actual posts* were still distributed across multiple
hosts. All I had to do was re‐install the old blogging system I wrote, `git-blog`, and … oh shit. I had
absolutely no idea if `git-blog` would even run on Ruby 1.9, much less interface correctly with whatever API
changes may have happened to all the things it depended on in the last two years!

Long story short, two terminals, one SSH connection, and five commands later… the blog is re‐cloned onto the
server, with the content re‐generated. I’m about to push the first new post from my Macbook Pro, with one further
command.

Fuck yeah, `git-blog`.