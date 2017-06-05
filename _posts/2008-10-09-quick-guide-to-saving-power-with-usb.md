Title: Quick guide to saving power with USB devices
Date: 2008-10-09
Tags: FOSS Techie Ubuntu

I have a laptop with some USB stuff built in. Some devices (such as Bluetooth) can be made to entirely disappear from the USB bus, however, the fingerprint reader and webcam can't, but they sit on the USB bus and draw power.
Fortunately the USB specs allow devices to be put to sleep if they're not being used and support that feature. Unfortunately many devices advertise they support it when they really don't, so Linux is unable to automatically put every USB device to sleep.
Fortunately you can control the setting by hand, and this is how. Firstly, start off with a Terminal and run the command "lsusb":

`cmsj@kodachi:~$ lsusbBus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hubBus 007 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hubBus 006 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hubBus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hubBus 002 Device 002: ID 0483:2016 SGS Thomson Microelectronics Fingerprint ReaderBus 002 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hubBus 004 Device 002: ID 17ef:4807 LenovoBus 004 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hubBus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hubcmsj@kodachi:~$ `
You can ignore the "*root hub*" entries, the interesting two are "*SGS Thomson Microelectronics Fingerprint Reader*" (guess which device that is ;), and "*Lenovo*" (this is the webcam).
So now we need to poke at those devices to enable their autosleeping. If we look at the entry for the webcam:

Bus 004 Device 002: ID 17ef:**4807** Lenovo

I've highlighted "*4807*". This is the Product value for this USB device (if you're curious, the "*17ef*" part is the Vendor value and uniquely identifies the maker of this device).
Now we need to find out where abouts the webcam lives in the */sys/* filesystem (which is something the kernel provides to give applications lots of information about the hardware in your system).
The following command will put us on the right path:
```
grep 4807 /sys/devices/*/*/usb*/*/idProduct
```

Which, on my laptop, returns:
```
/sys/devices/pci0000:00/0000:00:1d.7/usb4/4-5/idProduct:4807
```

Take that information you get, and chop the "*idProduct:4807*" bit off the end, just leaving "*/sys/devices/pci0000:00/0000:00:1d.7/usb4/4-5/*" (yours will look a little different to this) and add "*power/level*" to the end.
You should now have something that looks pretty much like "*/sys/devices/pci0000:00/0000:00:1d.7/usb4/4-5/power/level*" and if you get the current setting:
```
cmsj@kodachi:~/Desktop$ cat /sys/devices/pci0000:00/0000:00:1d.7/usb4/4-5/power/level
on
cmsj@kodachi:~/Desktop$
```

you can see it is "on", which means it will not be automatically put to sleep. To change that, run:
```
echo "auto" | sudo tee /sys/devices/pci0000:00/0000:00:1d.7/usb4/4-5/power/level
```

and test if your device still works (so if it's a webcam, fire up "*cheese*", or if it's a fingerprint scanner that you use, test if it still accepts your finger). If everything is good then you can put something in /etc/rc.local so the power saving will be set up every time you reboot your computer:
```
echo "auto" > /sys/devices/pci0000:00/0000:00:1d.7/usb4/4-5/power/level
```

and that's it! Repeat this for all the USB devices in your laptop and enjoy the power saving (run "*powertop*" about 10 minutes before you start doing this so it has time to get a good average of your power usage, then see how much difference this makes. It could be up to 0.5Watts per device). Note that this won't work particularly well for external USB devices you plug in, becuase the /sys/ path won't exist until you plug the device in, so you'd need to do the above steps every time you connect it.
Hopefully HAL will start whitelisting devices which can be suspended, but I don't know of any work in this direction at the moment.
