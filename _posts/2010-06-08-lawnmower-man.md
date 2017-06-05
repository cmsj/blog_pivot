Title: The Lawnmower Man
Date: 2010-06-08
Tags: FOSS Techie Ubuntu

Introduction
============

This website shares a server with various other network services that form the foundation of my online life (i.e. IRC and Email) and I've been running into capacity issues in the last few months, so I'm running an experiment whereby I upgrade to brand new hardware (Quad Core i7, 8GB of RAM) and partition the available resources across virtual machines so the various network services are isolated into logical security zones.

Whining
=======

I have plenty of experience using Xen for this sort of thing, but that's becoming more and more irrelevant in newer kernels/distributions. As much as I think that's a shame and a stupid upstream decision, I can't change it, so I need to move on to KVM and libvirt.

Resolution
==========

So, with the beefy new server booted up in a -server kernel and a big, empty LVM Volume Group I got to work creating some virtual machines. This post is mainly a reminder to myself of the things I need to do for each VM :)

Action
======

These are the steps I used to make a VM with 1GB of RAM, 10GB / and 1GB of swap:

### Create an LVM Logical Volume

```
lvcreate -L11G -n somehostname VolumeGroup
```

### Create a VM image and libvirt XML definition

```
ubuntu-vm-builder kvm lucid --arch amd64 --mem=1024 --cpus=1 \
--raw=/dev/VolumeGroup/somehostname --rootsize=10240 --swapsize=1024 \
--kernel-flavour=server --hostname=somehostname \
--mirror=http://archive.ubuntu.com/ubuntu/ --components=main,universe \
--name 'Chris Jones' --user cmsj --pass 'ubuntu' --bridge virbr0 \
--libvirt qemu:///system --addpkg vim --addpkg ssh --addpkg ubuntu-minimal
```

Catchy command, huh? ;)

### Wait

(building the VM will take a few minutes)

### Modify the libvirt XML definition for performance

The best driver for disk/networking is the paravirtualised "virtio" driver. I found that ubuntu-vm-builder had already configured the networking to use this, but not the disk, so I modified the disk section to look like this:

```
<disk type='block' device='disk'>
  <source dev='/dev/VolumeGroup/somehostname'/>
  <target dev='vda' bus='virtio'/>
</disk>
```

### Modify the libvirt XML definition for emulated serial console

I don't really want to use VNC to talk to the console of my VMs, so I add the following to the &lt;devices&gt; section of the XML definition to make a virtualised serial port and consider it a console:

```
    <serial type='pty'>
      <target port='0'/>
    </serial>
    <console type='pty'>
      <target port='0'/>
    </console>
```

### Modify the libvirt XML definition for a better CPU

I'm running this on an Intel Core i7 (Nehalem), but libvirt's newest defined CPU type is a Core2Duo, so we'll go with that in the root of the &lt;domain&gt; section:

```
<cpu match='minimum'>
  <model>core2duo</model>
</cpu>
```

### Import the XML definition into the running libvirt daemon

```
virsh define /etc/libvirt/qemu/somehostname.xml
```

### Mount the VM's root filesystem

The Logical Volume we created should be considered as a whole disk, not a mountable partition, but dmsetup can present the partitions within it, and these should still be present after running ubuntu-vm-builder:

```
mkdir /mnt/tmpvmroot
mount /dev/mapper/VolumeGroup-somehostnamep1 /mnt/tmpvmroot
```

### Fix fstab in the VM

Edit /mnt/tmpvmroot/etc/fstab and s/hda/vda/

### Configure serial console in the VM

Edit ﻿/etc/init/ttyS0.conf and place the following in it:

```
    # ttyS0 - getty
    #
    # This service maintains a getty on ttyS0 from the point the system is
    # started until it is shut down again.
    start on stopped rc RUNLEVEL=[2345]
    stop on runlevel [!2345]
    respawn
    exec /sbin/getty -L 115200 ttyS0 xterm
```

Edit /boot/grub/menu.lst and look for the commented "defoptions" line. Change it to:

```
    # defoptions=console=ttyS0 console=tty0
```

(the default "quiet splash" is not useful for servers IMHO)

### Unmount the VM's root filesystem

```
umount /mnt/tmpvmroot
rmdir /mnt/tmpvmroot
```

### Start the VM

```
virsh start somehostname
```

### SSH into the VM

I didn't specify any networking details to ubuntu-vm-builder, so the machine will boot and try to get an address from DHCP. By default you'll have a bridge device for libvirt called virbr0 and dnsmasq will be running, so watch syslog for the VM getting its address.

```
ssh cmsj@192.168.122.xyz
```

you should now be in your VM! Now all you need to do is configure it to do things and then fix its networking. My plan is to switch the VMs to static IPs and then use NAT to forward connections from public IPs to the VMs, but you could bridge them onto the host's main ethernet device and assign public IPs directly to the VMs.
