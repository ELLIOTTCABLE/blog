Installing Io on Mac OS X Leopard
=================================

Well, I decided it was time to diversify my programming skills. I've tried, at various times in the past, to get into JavaScript (because I need it to really empower my web-related stuff), Objective C (for writing Cocoa apps for the OS X desktop), and Perl (since an old internet friend of mine is always bugging me to do so). However, in all cases, the syntax just seems cold and foreign to me, totally unintelligible. I guess I've been spoiled by Ruby's beautiful syntax, or something like that.

In any case, a few Rubyists I know have been bugging me to try [Io][io], for various reasons, so I finally got around to it. Really, I quite like the syntax - it's really interesting. Io is completely Object Oriented, just like Ruby (in that everything in the system, even the kernel, is an object) - but the really odd but intriguing part is that even the syntax is sort of 'object oriented' - all syntax is pretty much based around one syntactical block (like Lisp, or so I've heard):

    object message

Does that even look like code to you? Certainly not to me. There's no concept of classes in Io, as in traditional OO languages like Ruby - only Prototypes (sort of like JavaScript). Also, there's no distinction between Prototypes ('classes') and normal Objects - any Object can be used as a Prototype for another Object, by cloning it.

    myObject := Object clone

To break it down (though that little snippet is so simple it's hardly necessary), that's setting the 'myObject' slot on the Lobby (the default base context for all random code you enter, so to speak) to equal a new copy of the Object prototype (from which everything inherits at some point). Similarly, we can then clone _that_ prototype:

    anotherObject := myObject clone

Now you know about as much as I know about Io, and as far as I can tell, that's all there *is* to know. It just evolves from there, with different combinations of that syntax.

In any case, that's not the point of this post (see [the bottom](#related_links "Related links") for more info about getting started with the Io language itself). I'm here to help you install it on your OS X setup.

It's really not all that complicated, not much is needed to get started - Io itself, and maybe some libraries to power various addons.

First, you'll need the Developer Tools installed - you can find these on the Leopard disk, if you have one, or else on the system restore disks that came with your Apple computer. Insert the disc, and run the installer in the Developer Tools folder. The rest of the options are, well, optional, but *make sure* that `gcc` stays checked when you go through the install - that's the compiler that we'll build Io and the other addons' libraries with.

Now, to install said libraries, you're going to need [MacPorts][ports], so you should [download][ports-dl] the latest .DMG, and install it according to [their instructions][ports-help].

Once everything is prepared, all that's left to take care of is the installation itself. You could copy this block (minus the beginning delimiters), wholesale, into Terminal.app - but unless you trust your computer to never screw up, I wouldn't do so. You can also simply copy each line into the term when the previous one finishes.

Finally, you'll need Git to get the latest Io source - there's tarballs available as well, I'll leave it up to you to adapt these instructions to use those instead of mainline master if you feel like doing so.

Ware, though, that I use `$` and `#` at the start of lines to declare whether they should be run as a normal user or as root. This should be taken care of for you by the `sudo` command attached to the commands that must be run as a superuser (it will ask you for your user's password when you do so), so you probably need not worry about it.

    #> sudo {mkdir -p,cd} /usr/local/src
    #> sudo git-clone git://github.com/stevedekorte/io.git
    $> cd io

Not all of these following are necessary, but we'll run them so we can let you have as many modules available to you while you play with Io as possible. `sudo make port` should do all of this, but as of this writing it's broken. If one of these errors out or something else scary happens, don't worry - they aren't really necessary unless you happen to need the particular library they power in Io (which is unlikely if you're just wanting to play around with the library).

    #> sudo port install gmp
    #> sudo port install libffi
    #> sudo port install libpng
    #> sudo port install cairo
    #> sudo port install freetype
    #> sudo port install libpng
    #> sudo port install tiff
    #> sudo port install libsndfile
    #> sudo port install mysql5-devel
    #> sudo port install qdbm
    #> sudo port install portaudio
    #> sudo port install pcre
    #> sudo port install libsgml
    #> sudo port install libsamplerate
    #> sudo port install libevent
    #> sudo port install soundtouch
    #> sudo port install tokyocabinet
    #> sudo port install taglib
    #> sudo port install sqlite3

Most Apples are dual-core now, but replace `3` with your number of processing cores plus one (I use `-j9` on my desktop, for instance). If make fails for  some reason, try `make clean` and then try this again without the `-j` flag.

    #> sudo make -j3

This installs Io by creating symlinks from /usr/local/bin to this development directory - that makes updating as development continues easier. Feel free to use `make install` instead if you want to permanently copy the binaries instead.

    #> sudo make linkInstall

That's it! We're done! Go launch io, and start playing with it! The `io` binary, if you're familiar with Ruby, acts like a combination of IRb and Ruby - if you run it without any arguments, you get placed in an IRb-ish interactive Io interpreter. If you pass a filename, it will execute that file, like the `ruby` executable. So, let's launch the interactive Io interpreter!

    $> io

Feel free to fiddle with the syntax I described above, or to visit one of the following links for more help getting started with Io.

Related links
-------------

- The [Io Language][io] website is, of course, an essential resource while getting started with Io
- The [Io Programming Guide][guide] is an invaluable resource on the Io website itself, but it is rather dry and difficult to follow, even for me
- Another resource on the Io homepage worth marking is the [Io Reference][reference]
- [Olivier Ansaldi][oliv] has written a [small slow-paced intro to Io][oliv-intro], which is a great next step after following my instructions here
- [Jesse Ross][jesse] keeps an [updated copy of the Io API specifications][api] available, if you need a more in-depth reference to what's available

[io]: http://iolanguage.com/ "Io Programming Language"
[ports]: http://macports.org/ "MacPorts software distribution and install system"
[ports-dl]: http://svn.macports.org/repository/macports/downloads/MacPorts-1.6.0/ "Download MacPorts 1.6"
[ports-help]: http://trac.macports.org/wiki/InstallingMacPorts "Instructions for installing MacPorts"
[guide]: http://iolanguage.com/scm/git/checkout/Io/docs/IoGuide.html "Io Programming Guide"
[reference]: http://iolanguage.com/scm/git/checkout/Io/docs/IoReference.html "Official Io Reference"
[oliv]: http://ozone.wordpress.com "Olivier Ansaldi's Wordpress Blog"
[oliv-intro]: http://ozone.wordpress.com/2006/03/15/blame-it-on-io/ "Blame it on Io! A slow-paced introduction to the Io language by Olivier Ansaldi"
[jesse]: http://jesseross.com/ "Jesse Ross' Homepage"
[api]: http://io.jesseross.com/docs/current/ "Jesse Ross' Edge Io API"
*[DMG]: Disk Image