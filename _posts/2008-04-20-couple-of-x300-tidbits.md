Title: A couple of X300 tidbits...
Date: 2008-04-20
Tags: Techie

... neither of them good.
First off, in a stunning piece of fail, Lenovo have used a drive from one of the few manufacturers that enforces region coding on DVDs in hardware - Matshita, aka Matsushita, aka Panasonic. This is very frustrating and made worse by the drive shipping in a state with no region code set, so no DVDs at all will play. I now either have the choice of changing it no more than 5 times, or risking the drive with some custom firmware which claims to remove the region code.
Secondly, and more worryingly, there is a new BIOS release for the X300, but one report from a Linux user thus far suggests that the machine has started hard locking. I can't confirm because this machine is far too important to me (ie I use it for work), so I am holding back on the update until I know what's going on.
*Update* It occurred to me that it might be useful to document how I changed the region code - install `regionset` and run it. It will show you your current region code (0xFF for me, ie region 0) and how many changes you have left, then ask if you want to change it.
*Second update* Lenovo actually withdrew the BIOS update, so clearly something was wrong with it. [This page](http://www-307.ibm.com/pc/support/site.wss/MIGR-69703.html) lists the models for which they have pushed out a fixed version. The X300 is currently still listed as "Coming Soon" :(
