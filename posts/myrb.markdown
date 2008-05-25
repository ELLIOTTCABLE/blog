MyRB
====

Not to be confused with [merb](http://merbivore.com "Merb - Rails done right"), MyRB ('My .RBs') is what I've christened one of my more fun sounding ideas so far. Let me expound a bit on what I see as the problem.

If you're a Ruby programmer who is anything like me, you have lots of favorite snippets of code that you use over and over, copying from project to project so they are available everywhere you code (for me, they are often core extensions that add new methods to, say, `String` or `Symbol` or `Kernel`). I, and I expect for a lot of the other Ruby programmers out there, each have something of our own library similar to [Ruby Facets](http://facets.rubyforge.org "Ruby Facets, the single largest collection of core extension methods and standard library additions available for the Ruby programming language"), with all of our own little favorite snippets and extensions.

What annoys me is having to either copy the entire group, which is usually pretty messy and unorganized as it is, to every project I work on (which adds bloat I dislike, especially if it's a minimalist kind of project) or having to remember all of the snippets in my head, and having to copy each one when there's a chance to use it, one at a time.

I propose a tool called MyRB, similar to Rake or Rspec, to have in our programmer toolboxes. MyRB would maintain (through a simple but powerful CLI executable) a central library on your computer of all your favorite snippets. When you wrote a re-usable piece of code, you could copy it into a `.myrb` file (or perhaps `Myrbfile`), and import it into MyRB. Perhaps there would be a way to tag snippets or something similar, to help organize - or even utilize [ParseTree](http://zenspider.com/ZSS/Products/ParseTree "Ruby ParseTree tools") to try to figure out exactly what is going on inside the snippet when it's added, and then automatically categorize it based on this.

Next time you start a project, you only have to `require 'myrb'`, and then start using your snippets. For basic snippets that define a new method on an existing class or define a new class or module, we could add some hooks into method\_missing and the equivalent for a missing constant, and then check if that method is defined in a myrb snippet - if so, myrb registers that snippet as used. For more advanced snippets that can't automatically be detected in this way, you can simply require an exact snippet with `require 'myrb/snippetname'` or whatever - or even require it's entire group with `require 'myrb/groupname'`.

Finally, when you go to deploy the project - whether this be to upload it to a server to be run, or to be shipped off to rubyforge, or pushed to GitHub - MyRB can 'compile' the snippets, brining all the snippets registered as used into the project as hard code, perhaps in a ./lib directory or ./myrb directory. It would be set up in such a way that MyRB itself wouldn't be a dependency for any user of the project using MyRB - once the snippets are imported, any `require 'myrb'` directives would point to the myrb.rb file (and `require 'myrb/something'` would point to the myrb/ folder) itself in the project, so the dependency for MyRB would disappear once the project was deployed.

If you're working in a group, MyRB would work for everybody - everybody's own snippets would be deployed when the project was pushed upstream or to somebody else, so all the other contributors' snippets would work in parallel with your own, and they wouldn't need to have all of your MyRB snippets in their MyRB (since they're stored in the project).

In this way, the coder would no longer have to do anything but catalog their snippets, and then they would become available to the coder in any project the coder works on.

On another level, MyRB would enable easy snippet sharing - you could publish your snippets using a simple myrb\_server of some sort, allowing the download (via HTTP) of all of your snippets, or perhaps excluding certain groups if you so desire. Then another MyRB user could browse your snippets on the web, and import one of your snippets the same way they would import a local Myrbfile.

I'm really excited about this - it needs a little ironing out on the technical level (how to store and deploy snippets, exactly), but if we could figure it out it could be very cool. If you think this is a cool idea, and want to contribute code/time/ideas, feel free to [contact me](http://elliottcable.name/contact.xhtml "contact elliottcable"), I've got all the instant messaging clients, as well as plain 'ol e-mail. Hell, feel free to contact me if you just wanna say you think this is cool (-:

update
------

Since originally posting this, I decided to just do it myself, because I liked the idea. So here y'all go, my first stab at MyRB, or as I have re-christened it, [my.rb](http://github.com/elliottcable/my.rb "elliottcable's my.rb on GitHub"). It's not really complete as of this writing - none of the smarts are in there, nor is the whole deploy tool or any hint of it. But it does work as a basic snippet storage utility as it is, so feel free to clone it and play with it.