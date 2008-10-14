Installing the v8 JavaScript Interpreter Mac OS X Leopard
=========================================================

I've been getting a little interested in JavaScript, and a recent conversation
with a self-titled "JavaScript Superstar"
([Michelle Steigerwalt][msteigerwalt]) has inspired me to get off my ass and
prepare my computer to develop JavaScript.

Of course, being me, I have to have the best of the best… so I set out to
figure out how to install the v8 JavaScript interpreter (of Chrome fame) as a
standalone shell on my Mac. Turned out not to be too hard.

First we need [Python][], because Google
[has a permanent hardon][google-python] for Python (and will almost certainly
continue to do so as long as [Guido van Rossum][Guido] is a Google employee).
Then we'll need to install the [SCons][] tool that Google uses internally as a
replacement for [gnumake][]. The first is made easy by [MacPorts][] (if you
don't already have that installed, it's a [cinch][installing-macports]), and
the second is a no-brainer as well.
    
    cd ~/Code/src # or wherever you want to keep the v8 source
    sudo /opt/local/bin/port install python25 # You'll need to enter your account password
    curl -O http://prdownloads.sourceforge.net/scons/scons-1.1.0.tar.gz ./scons-1.1.0.tar.gz
    tar -xzvf ./scons-1.1.0.tar.gz ./scons-1.1.0
    cd ./scons-1.1.0
    /opt/local/bin/python setup.py install
    
All that's left is getting and building v8 itself — for this, you'll need
[Subversion][] (Google's still a bit behind on the [git][] boat,
unfortunately), so we'll install that with MacPorts as well:
    
    sudo /opt/local/bin/port install subversion # You'll need to enter your account password
    /opt/local/bin/svn checkout http://v8.googlecode.com/svn/trunk/ ./v8
    cd ./v8
    scons sample=shell
    
If you're anything like me, you'll want to have that shell available
from anywhere - run something like this:
    
    sudo ln -s ./shell /usr/bin/v8
    
Now you've got a working v8-powered JavaScript shell! To launch it and try it
out, try the following:
    
    -- v8
    V8 version 0.3.4.1
    > print('Hello, world!');
    Hello, world!
    > ^C
    
You can also pass it a file to be run:

    -- echo 'print("Hello, world!");' > test.js
    -- v8 test.js
    Hello, world!
    
Be careful, however — this is not a browser, so a lot of the basic JavaScript
idioms you may be familiar with won't be available/make sense.

Have fun playing with 'purist' javascript!

[msteigerwalt]: <http://msteigerwalt.com/> "Michelle Steigerwalt's home page"
[Python]: <http://python.org/> "Python Programming Language"
[google-python]: <http://www.sauria.com/~twl/conferences/pycon2005/20050325/Python%20at%20Google.notes> "A writeup of Python's integration at Google by Greg Stein"
[Guido]: <http://www.python.org/~guido/> "Guido van Rossum's home page"
[SCons]: <http://scons.org/> "SCons home page"
[gnumake]: <http://gnu.org/software/make/> "GNU Make home page"
[MacPorts]: <http://macports.org/> "MacPorts home page"
[installing-macports]: <http://www.macports.org/install.php> "Installing MacPorts"
[Subversion]: <http://subversion.tigris.org/> "Subversion home page"
[git]: <http://git-scm.com/> "git-scm - Fast Version Control System"