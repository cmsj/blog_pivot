Title: Rise of the Floating Fonters
Date: 2009-10-20
Tags: FOSS Techie Ubuntu

For about two years now I've been using a 127dpi laptop screen as my primary computer display. It's a comfortable thing to be looking at, and after much playing around I've settled on 6.5pt as my ideal application font size.
No problems with that, right? Fontconfig says font sizes are a double (a high precision floating point number), but not all libraries/applications follow this.
In my testing of Karmic I've found two such things that particularly stick out:

-   notify-osd
    -   Assumes font sizes are whole numbers, so ends up using a tiny font

-   Gwibber
    -   Assumes font sizes are integers and completely fails to run if they are not

Obviously this won't do, so I've checked that we have [filed](https://bugs.launchpad.net/ubuntu/+source/notify-osd/+bug/396736) [bugs](https://bugs.launchpad.net/gwibber/+bug/383759) (and in the case of Gwibber, a patch), but I seem to be meeting some resistance, or this just isn't considered to be a high priority.
Thus a new Launchpad team is born, [The Floating Fonters](https://launchpad.net/~floatingfonters), for exiles such as myself who won't kowtow to the integers. We even have [a PPA with fixed versions](https://launchpad.net/~floatingfonters/+archive/floatingfixes) of notify-osd and Gwibber, but no guarantees are included!
