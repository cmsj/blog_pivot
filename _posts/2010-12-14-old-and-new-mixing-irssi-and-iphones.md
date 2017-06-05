Title: Old and new: Mixing irssi and iPhones for fun and no profit
Date: 2010-12-14
Tags: FOSS Techie Ubuntu

Introduction
============

I use irssi for IRC and an iPhone for pocket Internets; These two choices are both excellent, but they're not terribly compatible - typing in irssi on an iPhone via SSH is quite slow and annoying.
Obviously the thing to do is run an iPhone IRC client, but then I'm signing in and out all the time and I have multiple nicknames - what I want is a way to be connected to the same IRC session as normal, but from my phone using the excellent IRC client Colloquy. By taking advantage of several different pieces of Free Software, this is entirely doable! When we're not connected to IRC, messages which trigger irssi's highlight will be forwarded to the iPhone as a Push Notification.
Preparation
===========

These are the tools we are going to use to make this happen:
-   A patched irssi (don't worry though, the patch is \*tiny\*)
-   An irssi script (just some Perl really)
-   irssi-proxy
-   stunnel
-   Colloquy Mobile (from the iPhone App Store)

Throughout I will be assuming that you're running Ubuntu 10.04 (Lucid Lynx) as this is the currently most recent LTS release and thus most suited to servers. Also it's what I run, and this is my fun evening project :)
Although it will not be necessary to download it, I would like to note the original location of the patch and script that this method relies on. You can obtain both [here](http://static.ssji.net/colloquy_push.pl.txt "Colloquy Push script").
Instead, we are going to install a patched irssi from one of my PPAs, but if you do not care for this idea, the above URL will let you build your own patched irssi and contains the colloquy\_push.pl script.
Installation
============

These commands will install the patched irssi and the colloquy\_push.pl script:
```
sudo add-apt-repository ppa:cmsj/irssi-colloquy-push
sudo apt-get update
sudo apt-get install irssi
```

(If you don't have 'add-apt-repository' available, it's in the 'python-software-properties' package).
Configuration
=============

irssi-proxy
-----------

The first step is to load irssi-proxy. This is distributed as a plugin library in the irssi package, you can load it with:
```
/load proxy
/set irssiproxy_bind 127.0.0.1
/set irssiproxy_password PICKAGOODPASSWORD
/set irssiproxy_ports network1=31337 network2=31338 network3=31339
```

Obviously you'll need to replace "PICKAGOODPASSWORD" with a password, preferably a good one. Also you'll need to replace 'network1'/'network2'/'network3' with the names of the networks you've configured in irssi (which you can see with the command '/network list') and switch them to different ports if you want.
Finally you should run '/save' so irssi writes out its config file with all of these changes. Et voila, we have a running proxy, but as you noticed, we forced it to listed on 127.0.0.1, so we can't yet connect to it from the Internet. The reason we've done this is that irssi\_proxy is not able to directly offer encrypted connections. It would be a bad idea to allow all our proxy password and general IRC traffic to flow around unencrypted (even though many IRC server connections are unencrypted).
Stunnel
-------

Stunnel is a very simple tool that lets you  add SSL support to anything listening on a TCP socket. To get started, install the 'stunnel4' package and edit '/etc/default/stunnel4' and change ENABLED=0 to ENABLED=1.
Now we need to construct /etc/stunnel/stunnel.conf. The default contains various options we don't really care about, but one important one is the 'cert =' line - we need an SSL certificate for this to work. You can either buy one or generate your own (a so-called "snake-oil" certificate). There are many guides to generating a .crt file and this is left as an exercise for the reader. With that file in place somewhere, edit stunnel.conf to point at it.
The final step for stunnel is to add port configurations. Jump to the bottom of the file and add a section like this for each of the ports irssi\_proxy is listening on:
```
[myfirststunnel]
accept=123.123.123.123:31337
connect=127.0.0.1:31337
```

What we have done here is told stunnel to listen on our public IP on the same port that it will then connect to on 127.0.0.1. This might seem confusing, but I think it makes sense that the port numbers stay directly mapped between tunnels and proxy ports. Restart the stunnel4 service and you should see the appropriate ports being listened on.
colloquy\_push.pl
-----------------

This is the irssi script which glues all the magic together - it receives special commands from the iPhone version of Colloquy and uses those to pass on Push Notifications when necessary. To load it, type '/script load colloquy\_push.pl' and you probably want to symlink '/usr/share/irssi/scripts/colloquy\_push.pl' into ~/.irssi/scripts/autorun/.
Colloquy
--------

Now configure a new IRC Connection in Colloquy on your iPhone. Enter its hostname/IP and the port you have stunnel listening on (the port settings are in Advanced) and enable SSL. Finally, set Push Notifications to On and you're done.
Shortcomings
============

The script, while excellent, has one or two drawbacks - it's not yet able to detect when you're watching irssi, so it may well send lots of notifications to your phone unnecessarily (I'm looking into expanding it to detect if you're running in screen/tmux and are attached), also it doesn't have any concept of sleeping hours, so you may get woken up by notifications! Nonetheless, this is an excellent way to use your awesome iPhone and not sacrifice the magnificence of irssi!
