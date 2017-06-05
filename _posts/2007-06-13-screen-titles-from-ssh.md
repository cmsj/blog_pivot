Title: screen titles from ssh
Date: 2007-06-13
Tags: FOSS Techie Ubuntu

I usually have at least 4 terminals visible on my screen at once. Each one is running screen(1), and each screen has probably at least 3 or 4 different things going on in it (usually sshing to servers).
Once you are up to about a dozen or so shells spread across 4 terminals it can get quite interesting to remember where you left the one you are looking for.
Since screen can have a status line which lists its screens, and it is possible to change their names, I figured it ought to be possible to have ssh set the title of a screen when it connects to a remote machine. This would make things a lot easier to find, as well as being a cute hack.
It turns out that it is indeed possible.... to a degree.
ssh lets you specify a command to be run on the local machine after a connection is established, which is the ideal place to do this kind of thing. Sadly it doesn't help you out by setting any useful environment variables (such as the machine you just ssh'd to). You're probably thinking "but you know which one it is, you just ssh'd there!" and while that is true, it's not very easy to handle programatically. Mainly because it means parsing the arguments to ssh, which is no fun at all.
So, rather than do that, I am making the blanket assumption that the final word on ssh's command line is the host you are sshing to. If that is not true (e.g. you are doing "ssh someserver rm /etc/foo") you will get whatever the last word actually is, sucks to be you.
Also, if you use ProxyCommand, you really don't want the second ssh to do this, because it will confuse the first one and you'll never establish a connection, so detecting the type of output ssh is connected to is necessary.
Thanks to the many, many people I've consulted in the process of figuring this out. It doesn't seem like anyone has done this before (at least I can't find an example on google. There are some very similar things though), so after running out of ideas myself I started polling the community and got enough nuggets of inspiration back to produce a workable solution.
You will need to make sure screen is configured to show a status line (otherwise you won't see the screen names, except in a C-A-" or similar). Then drop this into ~/.ssh/config:
`  PermitLocalCommand yes  LocalCommand tty -s && cat /proc/$PPID/cmdline | xargs -0 | awk '{ printf ("\033k%s\033\\", $NF) }'`
(yes, that is hacky and disgusting. I am tempted to look at patching ssh to provide the hostname to the spawned LocalCommand shell, but right now the above config seems to be the best way of doing this).
