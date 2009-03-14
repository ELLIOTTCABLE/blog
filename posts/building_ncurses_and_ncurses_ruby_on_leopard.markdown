Building Ncurses and ncurses-ruby on Leopard
============================================

I just finished spending a week banging my head against a problem in [rat][],
where my [XMPP][] connection would stop receiving any data (status updates,
messages, whatever) whenever [`Ncurses`][Ncurses]`::`[`getch`][getch] was
looping. This occurred across threads, across Ruby instances, across
everything running in the system as far as I could tell (though a lot of this
network I/O and terminal control stuff is far over my high-level programming
head). It was driving me crazy, until I [sought][xmpp4r-post] (sidenote: I
hate that word. It should totally be sook, or possibly seeked - but sook looks
so much cooler) out help on the [xmpp4r][] [mailing list][xmpp4r-ml].

After some emails were exchanged and the problem was clarified, I reduced the
problem to [a rather minimal test case][test-case] that pretty much just connected to XMPP
and then started looping `Ncurses::getch` (not even bothering to do anything
with the input). It failed as all related code had been on my computers (not
receiving any XMPP stream data). However, when I sent this test case to the
mailing list, I got mixed results - it worked fine for several people, and
failed as it had for me for some others.

After a little more research, it appeared to always fail on all of my systems
(all running Mac OS X Leopard), as well on several others (I believe I
remember somebody mentioning a Debian system, as well as several people on
Ubuntu systems).

Recently, I had to wipe my system (well, not exactly *had* to...), and I
decided to try building Ncurses and the ncurses-ruby code from scratch
(instead of using the prebuilt Ncurses code that came with OS X, or the
Ncurses package in the MacPorts system as I had previously).

As I had begun to expect it would, my test snippet worked absolutely perfectly
on the custom-built Ncurses foundation.

I still don't know *what* is broken on the default installs of Leopard and
those other systems, nor do I know what could be done to fix it. But at least
I know now how to work around it!

So, I offer here, without further ado (and hell was that a lot of ado!),
instructions for custom-building Ncurses and ncurses-ruby on Leopard. These
instructions should work fine for any other system, but they haven't been
tested there.

Instructions, already!
----------------------

You need to start by creating a 'working directory' where we'll download and
build all the relevant code. I use `/usr/local/src`, and let it belong to root
(to ensure I'm running all the build commands as root). If you have another
place you prefer, feel free to use that. Here's the commands to setup and goto
that location:
    
    $ sudo mkdir -p /usr/local/src
    $ cd !$
    
Next, we need to download and unpack both the Ncurses and ncurses-ruby source.
Again, it's easy - we'll use the `curl` command-line tool to download the
files from their respective repositories (feel free to use `wget` if you, like me, have
it installed), and then use `tar` to unpackage the source from the tarball (be
careful, the versions you install may already have/need to have different
versions, make sure you pass the current version number where I have one
below):
    
    $ sudo curl -O ftp://ftp.gnu.org/gnu/ncurses/ncurses-5.6.tar.gz
    $ sudo tar -xzvf ncurses.tar.gz
    $ sudo wget http://download.berlios.de/ncurses-ruby/ncurses-ruby-1.1.tar.bz2
    $ sudo tar -xjvf ncurses-ruby-1.1.tar.bz2
    
Now we're going to actually build the software - this can take a long time,
depending on the speed of your computer. Where you see `-j9` below,
replace the number **`9`** a number one larger than the total number of
processor cores in your computer[^optimize]. Also, make sure to watch that
the ruby command uses the correct Ruby installation - that is, if you've
installed a custom Ruby from source or MacPorts, you should make sure that the
`which ruby` command returns the path to the Ruby which you intend to use
with Ncurses.
    
    $ cd ./ncurses-5.6
    $ sudo ./configure
    $ sudo make -j9 all install
    $ cd ../ncurses-ruby-1.1
    $ sudo ruby extconf.rb
    $ sudo make -j9 all install
    
To test that the library installed in the correct location, you can run this
command (it should print "**true**"):
    
    $ ruby -e "p(require 'ncurses')"
    
A few things to note - this is *NOT* a RubyGem installation! You cannot later
uninstall this build of ncurses-ruby with RubyGems, and this installation won't
count for the purposes of RubyGem dependencies. Also, for all I know,
installing the normal RubyGem version of ncurses-ruby will override this
installation, so be careful of doing so.

[rat]: http://github.com/elliottcable/rat "elliottcable's rat"
[XMPP]: http://xmpp.org/ "XMPP Standards Foundation"
*[XMPP]: Extensible Messaging and Presence Protocol
[Ncurses]: http://ncurses-ruby.berlios.de/ "ncurses-ruby"
[getch]: http://www.mkssoftware.com/docs/man3/curs_getch.3.asp "Ncurses' getch() method"
[xmpp4r-post]: https://mail.gna.org/public/xmpp4r-devel/2008-07/msg00022.html "[XMPP4R] Weird Ncurses and threading problems"
[xmpp4r]: http://home.gna.org/xmpp4r/ "XMPP4R: XMPP/Jabber Library for Ruby"
[xmpp4r-ml]: https://mail.gna.org/listinfo/xmpp4r-devel/ "xmpp4r-devel info page"
[test-case]: http://gist.github.com/2950 "gist: 2950 by elliottcable – GitHub"
[^optimize]: You can run this to figure out what flag you should use:
  `echo -j$(sysctl -a | grep 'hw.physicalcpu:' | awk '{gsub("hw.physicalcpu: ", ""); print}')`.
  For instance, I used **`9`** because my Mac Pro has 2 processors with 4
  cores each (thus, 8 total cores, plus 1 is 9) - this will optimize the
  process for your computer, and make it run as fast as possible.