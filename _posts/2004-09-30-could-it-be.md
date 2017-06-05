Title: Could it be?
Date: 2004-09-30
Tags: FOSS Ubuntu

For a while now I've been thinking that it would be a good idea to have a distro that was Debian, but more pragmatic and much quicker to track upstream releases (presumably at the slight expense of package quality)
[Ubuntu](http://www.ubuntu.com/) Linux is a fork of Debian's unstable branch, sid. It includes newer GNOME packages than sid (although they are due very shortly I believe) and appears to have a much more pragmatic approach to distro making. Bad for servers, good for getting a distro that has no release as such, but is just tracking the current stable releases of as much software as possible.
And therein lies the key, making a lot of software available - something that Debian has an incredible history of, with many thousands of packages. Where Debian is limited by being out of date and stable or slightly behind the times and unstable, Ubuntu will hopefully offer more stability and predictability than sid, but still a current desktop (probably where the approach is best suited, since that is where development is happening quickest).
To get lots of software, the Ubuntu guys have been [talking](http://people.ubuntulinux.org/~mako/ubuntu-traffic/u20040924_05.html#4) about two things.

-   Allow easy importing of packages from sid that compile (which we can reasonably assume to be most of the interesting packages people are going to want)
-   Allow easy importing of user contributed packages

This second item is the real key I think. If it is incredibly easy to submit a source package, have it be autobuilt on all architectures and when that works, reviewed by a project member for inclusion, people will do it. We see on distros like Fedora that people are prepared to put in massive efforts to maintain current software (see, the [Dag](http://dag.wieers.com/) and other RPM repositories. Their efforts are significantly hampered by the fact that they are unable to integrate their packages into the main Fedora trees, leading to them having to replace Fedora packages with newer versions and other worrying things. A big problem is that the various trees are not necessarily entirely compatible and there is some overlap. On a personal note, I find that having all of the sites that support x86\_64 in yum's config makes it incredibly slow (I'd use apt, but it doesn't support Fedora's excellent biarch system).
If this goes well, I would propose formalising the system to a degree by using [GPG](http://www.gnupg.org/) to allow the maintainer of a package to upload new versions without requiring moderator time. This is obviously another tactic lifted largely from debian, although they require manual verification by a debian developer who are the only ones with keys capable of uploading a new package.
I will be watching Ubuntu closely, and if they do move in the direction I am hoping, it will have to be worth a shot :)
