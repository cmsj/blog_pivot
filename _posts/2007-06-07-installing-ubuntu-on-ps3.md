Title: Installing Ubuntu on the PS3
Date: 2007-06-07
Tags: FOSS Ubuntu Games

I've yet to complete this, because I stopped my attempts last night when I reached an unusual situation.
Specifically, I was doing the partitioning manually, but the two visible disks had no partition tables. Not wanting to trash the PS3 disk I didn't let it create the tables, so had to abort.
After consulting with the very helpful Colin Watson, it turns out that the disk Ubuntu sees as sda is not the whole PS3 disk, it's the Other OS partition virtualised to look like a whole disk. It is therefore fine to create a partition table and proceed with the install, which I will do tonight.
(I'm not sure yet what sdb is, it smells like the PS3s internal flash and again I'm not sure if it's wise to mess with it)
*UPDATE:* The disks that Linux sees are virtualised by the PS3 (they're actually just partitions made to look like whole disks), so it is fine to make the partition tables (or indeed let the installer do automatic partitioning). [The bug](https://bugs.launchpad.net/ubuntu/+source/ubiquity/+bug/106683) where the installer hangs at 15% is due to the low RAM in the PS3. Stop some services (cupsys and hplip are good candidates) and remove some things from your session (update-manager and gnome-cups-icon, for example). Removing applets from the panel is not a bad idea either, and don't run anything else while you are installing. Of course you could plug in a disk of some kind and set up swap, but [this bug](https://bugs.launchpad.net/ubuntu/+source/linux-source-2.6.20/+bug/102044) makes that quite hard at the moment.
