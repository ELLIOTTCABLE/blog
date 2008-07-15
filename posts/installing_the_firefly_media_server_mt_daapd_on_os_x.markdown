Installing the Firefly Media Server (mt-daapd) on OS X
======================================================

*Update:* It [appears](http://forums.fireflymediaserver.org/viewtopic.php?t=7970 "Forum thread")
there is a pre-built binary for OS X available, it's just not prominently
displayed.

I just finished installing the [Firefly Media Server](http://fireflymediaserver.org "Firefly Media Server")
(previously known as mt-daapd, and still a commandline tool under that name).
It's really cool, much faster than iTunes, and doesn't require that iTunes be
running to serve an iTunes library's content (can you spell 'saved RAM'?).

It's not quite standard on OS X, so here I'll walk you through installing and
configuring it using MacPorts. I chose MacPorts to make things easy, most of
this will work if you choose to install from the tarball as well.

Installing MacPorts
-------------------

First, you'll need to install [MacPorts](http://macports.org "MacPorts OS X package manager").
It's not very difficult, just follow the instructions on [installing MacPorts](http://macports.org/install.php "Installing MacPorts").
Once that's complete, test that it was installed correctly by opening a
terminal window (`Terminal.app` is located in `/Applications/Utilities`) and
typing the following (without the `$` symbol at the start - that's standard lingo to
demonstrate that something should be typed at a Terminal prompt):
    
    $ port version
    
It should print something like "Version: 1.XXX" (i.e. it will show up on the
next line in the Terminal). If not, your `$PATH` is probably screwed up -
you'll have to figure out how to fix that yourself. Finally for MacPorts,
let's sync your new MacPorts install to the MacPorts rsync server:
    
    $ sudo port -v selfupdate
    
When a command is prefixed with `sudo`, that means it's been run as the
"`root`" user - that is, the system superuser/administrator user. Be very
careful with that command, it can have catastrophic consequences.

Installing Firefly
------------------

Now that MacPorts is configured, we need to install Firefly (aka mt-daapd) itself (from the
MacPorts port tree), as follows:
    
    $ sudo port install mt-daapd
    
This will print some status info - sooner or later, after installing some
other software upon which Firefly depends, it should print something like this:
    
    --->  Configuring mt-daapd
    --->  Building mt-daapd with target all
    --->  Staging mt-daapd into destroot
    --->  Installing mt-daapd 0.2.4.1_0

This indicates the install was successful. If it doesn't end with something
like this, or errors arise in the process, you can retry it with the following
commands:
  
    $ sudo port clean mt-daapd
    $ sudo port install mt-daapd
    
If it still doesn't work, I can't help you there.

Configuring Firefly for OS X
----------------------------

With Firefly installed, we need to create a configuration file to tell it how
to serve. Here's mine:
    
    web_root	/opt/local/share/mt-daapd/admin-root
    port		3689
    admin_pw	secret
    db_dir		/var/cache/mt-daapd
    mp3_dir		/Users/elliottcable/Music/iTunes/iTunes Music
    servername	Firefly
    runas	elliottcable
    playlist	/etc/mt-daapd.playlist
    # password	mp3
    extensions .mp3,.m4a,.m4p,.m4v,.mp4
    # logfile /var/log/mt-daapd.log
    rescan_interval 300
    scan_type  1
    # compress 0
    
You need to create the file `/etc/mt-daapd.conf`, owned by root, with that
content. To do so, we can use the command-line text editor `nano` as follows:
    
    $ sudo nano /etc/mt-daapd.conf
    
This will open the editor - once it is open in the terminal window, you can
paste the above configuration settings into the file. You'll want to change
the `admin_pw` to a password that suits your tastes, and you should change the
username to your own short 'system' username (the name of your Home folder under `/Users` -
if you're not sure what it is, you can just look at your prompt, which is the
text that shows up to the left of a line where you can type in the Terminal -
it should look something like `John-McCormicks-Mac-Mini:~ jmccormick$ `, in
which "jmccormick" is the short username) in two places - the path in the
`mp3_dir` setting, and the `username` setting itself.

Some directives are "commented", which means they will have no effect - this
is caused by the hash symbol (`# `) at the start of the line. They'll revert
to their default value if so "commented" or removed from the file. If you want
a password on your iTunes share, uncomment the `password` directive, and
change that password to whatever you want.

Finally, we need to create the cache directory, where Firefly will store
connections and similar temporary files. It's very simple:
    
    $ sudo mkdir -p /var/cache/mt-daapd
    
If you changed the `db_dir` directive above in the configuration file, you'll
need to use that path instead here.

Running Firefly
---------------

Starting Firefly is quite simple - just run the following at a Terminal prompt:
    
    $ sudo mt-daapd
    
Make sure you quit iTunes before running this (you can run iTunes after you
start it), just make sure Sharing isn't enabled if you start iTunes with
mt-daapd running, so disable it in iTunes before running the command for the
first time). It'll run until you shut down your computer, or kill it with the
Activity Monitor.

If you wish to watch it in action, you can start it in supervised mode with
this command:
    
    $ sudo mt-daapd -f
    
Thanks for reading, and I hope you love Firefly as much as I do now!