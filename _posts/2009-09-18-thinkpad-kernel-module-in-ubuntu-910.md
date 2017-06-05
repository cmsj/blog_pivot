Title: Thinkpad kernel module in Ubuntu 9.10 (Karmic)
Date: 2009-09-18
Tags: FOSS Techie Ubuntu

The Ubuntu Kernel Team has decided to remove the tp\_smapi module from our kernel for 9.10 (Karmic Koala) because the author chooses to remain anonymous and it is therefore impossible to be sure that the code is not based on incorrectly obtained information.
Slightly annoying perhaps, but ultimately a decision that's hard to argue with and fortunately one that's pretty easy to work around. I wish the author would clear things up once and for all because the tp\_smapi module is desperately important for Thinkpad owners wishing to protect the life of their laptop battery. Given that my new X301 is tasked with a lifetime of 3 years, I am particularly keen to protect its battery!
The source for the module is still in the archive (and my understanding is that it will stay there, we just don't want to ship it by default) as tp-smapi-source, packaged by Evgeni Golov (a thoroughly decent chap who is the current owner of the Thinkpad X300 I've posted about previously). You can install it with the command:

    sudo apt-get install tp-smapi-source

Then run:
    sudo module-assistant

and select the tp-smapi module to build and install. You are now just a quick:
    sudo modprobe tp_smapi

away from having battery charge control options in /sys/devices/platform/smapi/BAT0/
Woo! If I get a chance I'll try and produce a version of the package which uses DKMS (Dell's Kernel Module management system which makes sure that additional modules like this are rebuilt automagically whenever you get a kernel update).
