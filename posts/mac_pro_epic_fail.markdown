Mac Pro epic fail
=================

What follows is [a very, very long post](http://discussions.apple.com/thread.jspa?threadID=1532799 "Apple Support - Discussions - Kernel Panics during system boot, and no kind of network connectivity") I just made to the Apple Discussion forums, seeking help with my very very cursed Mac Pro. If you read the whole thing, you get a cookie. No, really. Marathon runne- uh, marathon readers, *GO*!

I've tried quite a lot of things here, so please read them all before you suggest something - and thanks for taking a look at my question (-:

I recently got my Mac Pro back from the [AlaskaMacStore](http://akmacstore.com "the Alaska not-really-a Mac Store") (not an [actual apple retail store](http://www.apple.com/retail/ "Apple Retail"), and they aren't very good at that), having brought it in for loss of FW800. They sent it to Apple, and I received it back, supposedly with a new motherboard - and my FW800 working. However, it proceeded to kernel panic (complete visual system freeze, not greyscreenofdeath - oddly enough, iTunes would never stop playing during them) at least once a day after that.

Not wanting to deal with them again, I just used it like that and put up with it - but soon enough, I lost any sort of network connectivity as well. With AirPort connected to my Time Capsule's network (which worked fine on my Macbook Air and iPhone and various other devices), I was being told "...Self-Assigned IP address, and thus may not be able to connect to the internet...". Even connected directly via Ethernet to the Time Capsule, or directly via Ethernet to the cable modem, or directly via Ethernet to another Mac, or connected to another Mac's ad-hoc network, I could get absolutely no network communication going on of any sort.

This was too much for me, as all of my work requires frequent and heavy internet access, so I gave in and took it back to the local mac store equivalent - they charged me 100$, and kept it two days, just to tell me they 'couldn't reproduce the problem'. I took the Mac Pro to a friend's house, and went through the same tests - connected to his modem, with his Ethernet cords, and to his wireless and other macs. Same deal, no connection or indication of a networking being connected beyond that blasted 'self-assigned IP' in Network prefs.

Having no desire to be defrauded by the local fools again, I decided I didn't really have anything important on my internal hard drive, and decided to wipe the internals and re-install Leopard. My horrible luck continuing, I found my only Leopard disc to be cracked, prompting me to drive out and purchase a new one. Inserting this into my Mac Pro's drive, I found myself faced with yet another horrible problem.

During the boot to the Leopard DVD, the grey loading screen (with the apple logo and the spinner) simply froze with the spinner pointing left. I left it for a while, expecting this to be part of what I'd previously experienced with the slow boot-from-DVD on a Macintosh... but after 30 minutes of waiting, I got quite worried, and booted back into my normal OS by holding down Option and selecting my RAID.

Now, note this for later - it booted absolutely fine, and though I still had no network connectivity, the system had no problems. I changed the startup-disk to my RAID and then back to the Leopard DVD, naïvely thinking this would solve the problem, and ran into the same problem after restarting and booting to the Leopard DVD again.

Dismissing this as a optical drive problem (haven't had occasion to use optical media in nearly 3 years, don't really trust it in general), I booted the Mac Pro into Firewire Target Disk mode, and connected it to my Macbook Pro, and then booted the Macbook Pro from the installation DVD. Again, note carefully here - there was nothing wrong with the DVD itself, as the Macbook Pro had no trouble booting to the DVD.

I used Disk Utility to preform some planned modifications to the internal drives of my Mac Pro (erasing my previous inefficient RAID setup in favor of a new, faster (but less secure) RAID0 striped setup for my main two 1TB drives), and then proceeded to install a fresh Leopard onto the newly minted RAID. I usually cancel the 'disc checksum' that the Leopard DVD runs on itself, not really worrying about it, but in light of recent possible problems with the DVD, I went ahead and let it run - no problems, it completed and went straight into the normal install.

The install itself completed in a timely manner, and my Macbook Pro re-booted automatically, to the newly Leopard-ed RAIDed internals on the Mac Pro. It booted to the new install without a hitch, played the Leopard intro video, and continued to the setup cube. I opted to shut down (by hitting command-q and selecting the shut down option), so I could set it up on the Mac Pro instead.

Surprised was I, when I experienced the same grey loading screen freeze on the Mac Pro, booting to the newly clean install on the new RAID, that I had seen trying to boot to the Leopard disc. Worrying that my Leopard disc was, infact, corrupted in some way, I tried placing the Macbook Pro into firewire Target Disc Mode, and booting that drive over FW800 on the Mac Pro. Same freezing problem occurred.

Well, now, the only installation that HADN'T kernel panicked on boot was the old install, and that had been erased for the new RAID configuration, so I was left without a way to boot to a working install and test things from the Term. This left me with Single User Mode, which I immediately booted into.

Interestingly enough, there was no problem booting into Single User Mode - so I poked around a bit before mounting, and I had seemingly fine access to the entire filesystem of the new Leopard install, and nothing looked screwed up to my (albiet untrained) eye. After mounting and continuing the boot, it froze - but since this was verbose SUM, and not simple normal boot, I was able to see exactly where it froze: something about 'mDNSResponder starting', and then no input or output was accepted, leaving me assuming it had kernel panicked again.

To test this, I again attempted to boot from the Leopard DVD on the Mac Pro, this time in verbose startup mode, and watched it freeze up at exactly the same point. After a little googling, I found some suggestions to disable the launchd plist for mDNSResponder - I tried SUM again, and did so, then continued with the boot. This time, there was no output relating to mDNSResponder, so the launchd change had worked; however, it still froze - this time, right after three lines, first something along the lines of 'Login Window Application Starting', and then two lines printing Ethernet IDs of some sort. After those Ethernet related lines, it again froze.

What with the DNS and Ethernet bits showing up in the freezing, I was getting worried this was still tied into my NIC in some way, so I unplugged my Ethernet cords and tried the exact same boot again, and again disabled mDNSResponder. This time the only sensible thing before the freeze was the 'Login Window Application Starting' line, which seemed perfectly normal. I doubt there is/was anything wrong with that, so I'm assuming there's still some networking-related process freezing/panicking the computer, and it just doesn't happen to log in a visible way during startup.

Does anybody have any suggestion as to what such a process might be, and how I can at least pause or kill it from SUM, so I can at least finish booting the computer?

If I can't figure out a solution to this, I'll have to buy a new Mac Pro tomorrow morning... I have very, very pressing matters that can't wait for a new Mac Pro to ship, nor can they wait for me to ship this one to Apple and back (I live in Alaska, we have at least a weeks wait if nothing is seriously wrong when sending an Apple product in for repairs on our own), and nor can I wait 2 days for the local Apple store to tell me there's nothing wrong with it and then have to wait another week to, again, send it to Apple myself. I don't want to have to shell out for a new, expensive computer when there's a (most likely) perfectly good one sitting here... so any help would be MUCH appreciated.

If you've read this far, phew, you deserve a cookie. That was one... *long*... post.

Unfortunately, the cookie is a lie.

Update
------

After getting my hands on another Mac Pro and some other equipment, I started trading out pieces and parts to see what was bad - turns out my wonderful brand new [NVIDIA GeForce 8800 GT](http://www.nvidia.com/page/geforce_8800.html "NVIDIA GeForce 8800 product page") is bad. Inserted in either of the Mac Pros, it caused kernel panics on boot. I'm going to try to see if Apple will replace it or something, but I dunno... we'll see.

Until then, struggling along with three of the normal lowest-end-available ([NVIDIA GeForce 7300](http://www.nvidia.com/page/geforce_7300.html "NVIDIA GeForce 7300 product page")) video cards for the Mac Pro - GOD EVE Online skips when running on one of those! It's just painful.