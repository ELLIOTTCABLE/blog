Arch Linux and EC2
==================

I just wanna put this out there: [Arch Linux](http://archlinux.org/ "Arch Linux homepage")
is absofuckinglutely awesome.

I recently found out the hard way (specifically, `cp: writing `access.log': No space left on
device`) that the only Arch Linux [AMI](http://developer.amazonwebservices.com/connect/kbcategory.jspa?categoryID=101 "Amazon Machine Image")
available [*ami-3132d758*], also being the one my server was created from, was
set up very broken - for some reason, the creator saw fit to give the root
drive only 1GB of storage space. So I had to create my own Arch AMI from
scratch. This meant I had to learn...

- How to use [VMWare](http://www.vmware.com/products/fusion/ "VMWare Fusion")
- How to install a Linux on my own, from scratch
- How to how to screw with partitions from a running Linux system without
    shutting it down
- How to package a running Linux system up as an AMI and ship it off to
    Amazon's [S3](http://aws.amazon.com/s3 "Amazon Simple Storage Service")
- How to make an Amazon AMI registered as public, so others could use it

After all of that, and numerous silly mistakes, I had a working instance
booted from my own AMI, running Arch Linux, with a (relatively) generous 10GB
root partition. Feels much better!

So, without further ado... pop an Arch, bitchez!

    ec2-run-instances ami-f7c92d9e

If you're interested in playing with Arch, pop and instance and play with it -
it's fully configured for EC2, so when you instantiate it, you can use your
EC2 keychain pubkey to login to the root account (make sure to pass a -k
argument to `ec2-run-instances` for this to work). Password logins are
disabled by default, and a few other things are not quite standard (basic Ruby
is installed for the EC2 tools, and whatnot)... but other than that it's a
fresh Arch install. I suggest you run the following updates after popping -
Arch Linux is on a rolling release system, meaning that when you run these
commands, you will then have a complete and upgraded Arch Linux install, as if
you had installed from the absolute latest available disks:

    pacman -Syu
    pacdiff
    pacman -Sc
    # And again, incase the first one included a pacman update
    pacman -Syu
    pacdiff
    pacman -Sc

The pacdiff commands will present you with updated versions of various system
configuration files, ensure that your settings are copied into the new
versions.

Anyway, I hope this AMI serves somebody well