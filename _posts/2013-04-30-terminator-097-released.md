Title: Terminator 0.97 released!
Date: 2013-04-30
Tags: FOSS Techie Ubuntu Terminator

#### The present:

It's been a very long road since Terminator 0.96 back in September 2011, but I'm very happy to announce that Terminator 0.97 was released over breakfast this morning.
There's a reasonable amount of change, but almost all of it is bug fixes and translations.
Here is the changelog:
-   Allow font dimming in inactive terminals
-   Allow URL handler plugins to override label text for URL context menus
-   When copying a URL, run it through the URL handler first so the resulting URL is copied, rather than the original text
-   Allow users to configure a custom URL handler, since the default Gtk library option is failling a lot of users in non-GNOME environments.
-   Allow rotation of a group of terminals (Andre Hilsendeger)
-   Add a keyboard shortcut to insert a terminal's number (Stephen J Boddy)
-   Add a keyboard shortcut to edit the window title (Stephen J Boddy)
-   Add an easy way to balance terminals by double clicking on their separator (Stephen J Boddy)
-   Add a plugin to log the contents of terminals (Sinan Nalkaya)
-   Support configuration of TERM and COLORTERM (John Feuerstein)
-   Support reading configuration from alternate files (Pavel Khlebovich)
-   Allow creation of new tabs in existing Terminator windows, using our DBus API
-   Support the Solarized colour palettes (Juan Francisco Cantero Hutardo)
-   Translation support for the Preferences window
-   Lots of translation updates (from our fantastic translation community)
-   Lots of bug fixes

My sincere thanks to everyone who helped out with making this release happen.
#### The future:

So. Some of you might be wondering why this release isn't called 1.0, as it was tagged for a while in the development code. The main reason is that I just wanted to get a release out, without blocking on the very few remaining bugs/features targeted for the 1.0 release. I hope we'll get to the real 1.0 before very long (and certainly a lot quicker than the gap between 0.96 and 0.97!)
However, I do think that the Terminator project is running out of steam. Our release cadence has slowed dramatically and I think we should acknowledge that. It's entirely my fault, but it affects all of the userbase.
I am planning on driving Terminator to the 1.0 release, but the inevitable question is what should happen with the project after that.
The fact is that, like the original projects that inspired Terminator (gnome-multi-term, quadkonsole, etc.), technology is moving under our feet and we need to keep up or we will be obsolete and unable to run on modern open source desktops.
There is a very large amount of work required to port Terminator to using both Gtk3 and the GObject Introspection APIs that have replaced PyGtk. Neither of these porting efforts can be done in isolation and to make matters more complicated, this also necessitates porting to Python 3.
I am not sure that I can commit to that level of effort in a project that has, for my personal needs, been complete for about 5 years already.
With that in mind, if you think you are interested in the challenge, and up to the task of taking over the project, please talk to me (email cmsj@tenshu.net or talk to Ng in \#terminator on Freenode). My suggestion would be that a direct, feature-complete port to Python3/Gtk3/GObject would immediately bump the version number to 2.0 and then get back to thinking about features, bug fixes and improving what we already have.
