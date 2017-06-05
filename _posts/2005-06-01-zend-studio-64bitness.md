Title: Zend Studio 64bitness
Date: 2005-06-01
Tags: FOSS Rant

I'm quite a big fan of the Zend Studio development environment for PHP - I use it quite extensively at work and generally speaking it's a very capable tool and makes developing PHP a lot easier/quicker.
However, it's closed source and quite expensive, which is a bit of a downside, but at the same time it should give me some leverage to get the features I want into future versions, right? Probably not.
I've been bugging the Zend support guys about AMD64 support for near enough 10 months now, with little success. Now, this might not seem too surprising, what with it being closed source, but the important difference here is that Zend's Studio is written in *Java*.
Given that Java is supposed to be a platform agnostic virtual machine, precisely why is it that Zend only ship binaries for a few platforms? The answer appears to be that the installer they use to install said binaries on customer machines is a complete nightmare.
Specifically they appear to be using InstallAnywhere, which is becoming quite common for installing java programs, especially on Linux. Sadly it has some pretty serious flaws. Firstly it's one of those godawful self-extracting/installing shell scripts, so modifying the installer is exceptionally hard. It also knows almost nothing about AMD64, despite the fact that it ought to be really quite compatible with 32bit code (especially for something as library-free as java) and triggers a lovely glibc bug (set "`LD_ASSUME_KERNEL=2.2.5`" on an AMD64 machine and then try to run anything ;)
So basically that all sucks and anyone using InstallAnywhere is cutting themselves off from potential customers for no particularly good reasons. Obviously I can't accept that, so knowing that Zend Studio is really just a Java program I went at it with a copy of vim and a lot of scribbling notes until I figured out how InstallAnywhere's crazy LAX configuration system worked. With that out of the way I was able to determine that all you need to do to make this thing run \*perfectly\* on AMD64 is make two tiny changes to two not-so-tiny text files. Simple!
Here's how:

1.  Install Zend Studio somewhere (a 32bit machine or a 32bit chroot), copy the folder to your 64bit install
2.  Look in the directory Studio is installed in (e.g. "`/usr/local/Zend/ZendStudioClient-4.0.2/`") and edit "`bin/ZDE.lax`", you need to have "`lax.nl.current.vm`" point to your 64bit Java VM binary (e.g. "`/usr/bin/java`").
3.  Now edit "`bin/ZDE`" and comment out the line "`export LD_ASSUME_KERNEL=2.2.5`"

That should be it, fire up `bin/ZDE` and you should be hacking PHP in 64bits of goodness (be aware you may need to reconfigure where Zend Studio finds external binaries like cvs - see the ZDE configuration window).
**Update**
I've spoken with Zend since writing this and although they are still not committing to supporting AMD64, they did provide me with a handy link to download the Zend Studio installer without the 32bit JVM in it, which (with some work) makes a native 64bit install possible. Hurrah!
So, what to do, well firstly you will need the tarball (I'm not going to link to it, ask Zend) and to extract it. This should leave you with a single file called `ZendStudio-4_0_2.bin` (in the case of 4.0.2, current release at the time of writing). Run the command:
`cat ZendStudio-4_0_2.bin | sed -e 's/=2.2.5/=a.a.a/g' >ZendStudio-4_0_2.bin.1`
Then run "`sh ./ZendStudio-4_0_2.bin.1`" and the installer should start. Once it has completed you still won't actually be able to start ZDE because the same LD\_ASSUME nonsense is going on there, so edit "bin/ZDE". Above I showed the quick and hacky way to make this work - comment out the "`LD_ASSUME_KERNEL=2.2.5`" - however there is a way that is probably better, so I will encourage you to do this instead... Edit line 1326 so instead of reading simply:
`` if [ `uname` = "Linux" ]; then ``
it now reads:
`` if [ `uname` = "Linux" -a `uname -m` != "x86_64" ]; then ``
and all will be well :o)
**Update 2**
One thing I hadn't noticed because it was working transparently is that not all of ZDE is Java, for example the code analyzer binary in Zend's bin/ folder appears to be a native 32bit binary. These should still work fine if you have some 32bit compatibility libraries installed (Fedora should install these by default on AMD64, Debian based systems may need to install the ia32-libs package).
