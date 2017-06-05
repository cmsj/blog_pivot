Title: Migrating Ubuntu to RAID1
Date: 2008-01-14
Tags: FOSS Techie Ubuntu

I have a fair stack of data that I quite like, some of which is vitally important, stored in multiple places and generally not at concern for loss without some very bad things happening already.
The rest I'd just like to keep. For example, I wouldn't cry if I lost all my uni work, but it would be a shame. Same for the working emulation of my trusty Amiga that I used for so long, but now barely remember how to use.
So, not wanting to trust it all to a single hard disk, I bought a second so I can could clone the data onto two disks with RAID1. Not a problem in Linux thanks to software RAID. I knock up a RAID1 with a single partition on the new disk (which the tools like doing as much as they make sandwiches, grr). rsync across the existing / to the raid volume, modify fstab, grub and maybe something else and reboot expecting tenshu to finally live up to its name...
Except that's not its name.
Not when it's booting.
It turns out that having created the RAID volume while fully booted, it had been tagged as being for a machine with a hostname of "tenshu". The initramfs does not know your hostname because your hostname is stored on your root partition, which is a RAID partition that hasn't been mounted yet. It is possible for the initramfs to have a hostname, and I expect it might even have a kernel commandline option to specify it, but either way it's not there by default.
As any string comparison function will tell you, "tenshu" is not an acceptable match for "(none)".
So to fix this, boot the system and it should sit for ages waiting for the RAID arrays to assem ble (this timeout is \*far\* too long. what the hell takes this long to be detected by the system?!). After a while it will get bored and give you an initramfs shell. Busybox to the rescue!
I suspect you can run `mdadm -A /dev/md0 --auto-update-homehost`, although I ran `mdadm -A /dev/md0 /dev/sdb3 --update=homehost` to be specific and because I found those options first.
Reboot and bam, the orange bar of progress skips up merrily.
Assured that my data is safe on the new partition, I can now proceed to trash the original disk and grow the RAID1 to include it.
