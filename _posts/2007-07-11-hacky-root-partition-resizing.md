Title: hacky root partition resizing
Date: 2007-07-11
Tags: FOSS Techie Ubuntu

How would you shrink the root file system of a remote machine? Of course the easy answer is to boot into a rescue environment and do it (because you can't shrink ext3 online).
If you have a good KVM or ILO setup, you already have a rescue environment of sorts - initramfs.
Chuck "break=mount" on your kernel commandline and the initramfs will drop out to a shell before it mounts the root filesystem. You can now mount the root fs manually and copy out the required tools/libs (e2fsck, resize2fs, fdisk and their libraries, in this case), then unmount the root fs.
Now, with appropriate $LD\_LIBRARY\_PATH mangling you can run the extracted binaries and operate on your root partition with impunity
