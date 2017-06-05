Title: getting information out of a badly broken debian installer
Date: 2008-04-08
Tags: FOSS Techie Ubuntu

You have new hardware (most likely server).
You pop in a debian/ubuntu installer CD, tell it what kind of keyboard you have and expect it to scan the CDROM for packages, but....uh-oh, it can't find the CD!
What do you do?! Well, realistically there's not a lot you can do to make it work, but you can do a lot to help get it fixed.
You need to pull off /var/log/syslog, the output of lspci, lspci -v and lspci -vvnn.
You may very well find yourself having a problem with that though, because you're still pretty early in a typical linux boot process, so you probably don't have any disks mounted and you may find yourself missing any modules to make that happen.
You should have usb-storage.ko though. That and isofs.ko.
Can you see where this is going? :)
find the .udeb's on your install CD with a working computer, `ar -x` the core fs modules one and pull out ext3 (and jbd and mbcache), or vfat and its dependencies. put them in a directory, then do `mkisofs -o /dev/usbstick1 /path/to/modules`.
You now have a partition on your USB stick that is an ISO9660 filesystem (ie a CD). Obviously make sure you don't do this on a USB stick you care about the contents of.
Chuck the USB stick into the broken server, `modprobe usb-storage`, mount the newly appeared partition and copy the modules over to the right place in /lib/modules/. Unmount the USB stick, modprobe the drivers and now you can put in an ext3/vfat formatted USB stick and you have somewhere to write the debugging information to!
Easy! :) Now file a bug with the debugging information you collected.
