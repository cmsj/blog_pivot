Title: Monitoring an Apple Airport Express/Extreme with Munin
Date: 2011-01-29
Tags: FOSS Techie Ubuntu Python

So you have an Apple Airport (Express or Extreme), or a Time Capsule, and you want to monitor things like the signal levels of the connected clients? I thought so! That's why I wrote this post, because I'm thoughtful like that.
While it's not necessary, I'd like to mention that this was made possible by virtue of Apple having put out an [SNMP MIB file](http://support.apple.com/kb/DL1186 "Apple's SNMP MIB file for their Airport products"). Without that, finding the relevant OIDs would have been sufficiently boring that I wouldn't have bothered with this, so yay for that (even if the MIB is suspiciously ancient).
So if you don't need the MIB file, what do you need?

-   [Munin](http://munin-monitoring.org "Munin. Monitoring for lazy people!")
-   python
-   Net-SNMP's python bindings (in Debian/Ubuntu the package name is [libsnmp-python](apt:libsnmp-python "Click here to install libsnmp-python"))
-   my [airport munin plugin](http://bazaar.launchpad.net/~cmsj/+junk/munin-plugins/view/head:/snmp__airport "Airport SNMP plugin for Munin")

Having all of those things, how do you use it? Simple!
-   Place the munin plugin somewhere (doesn't really matter where, but the munin package probably put the other plugins in /usr/share/munin/plugins/)
-   Make sure you have a hostname or IP address for your Airport(s). If you have more than one you should either make sure they have static IPs configured, or that the one doing DHCP has static leases configured for all the other Airports.
-   Create a symlink for each of the types of graph for each of your Airports. Assuming that your Munin machine can resolve your Airport as 'myairport' you'd want to make the following symlinks:
    -   cd /etc/munin/plugins/
    -   ln -s /path/to/snmp\_\_airport snmp\_myairport\_airport\_clients
    -   ln -s /path/to/snmp\_\_airport snmp\_myairport\_airport\_signal
    -   ln -s /path/to/snmp\_\_airport snmp\_myairport\_airport\_noise
    -   ln -s /path/to/snmp\_\_airport snmp\_myairport\_airport\_rate

There is an explicit assumption that your SNMP community is the default of 'public'. If it's not then you'll need to hack the script. Otherwise, you're done! Now you win pretty graphs showing lots of juicy information about your Airport. Yay! You're welcome ;)
