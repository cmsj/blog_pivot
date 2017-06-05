Title: Python wanderings, part two
Date: 2009-12-31
Tags: FOSS Techie Python Terminator

2. Plugging it all in
---------------------

Sometimes we get feature requests and merge proposals for features that are clearly useful for someone, but not appropriate for the general use cases. It's always unfortunate to have to say no to these folks, but we have a slim menu UI and I'm wary of cluttering it with niche features. Still, turning away legitimate users is something I don't like doing, so for a while we've been considering how to fix this.
The obvious answer is that we should support plugins, and I've been working on such a system for my epic refactoring. This is a quick wander through some thoughts I've had.
I started out by googling for python plugin systems; One if the top hits was [this page](http://lucumr.pocoo.org/2006/7/3/python-plugin-system) by Armin Ronacher . In it he demonstrates a plugin system in under 40 lines of Python. It's simple and flexible, but there are some issues, like it makes doctest very sad.
I asked about this in \#python and was politely informed that I was Doing It Wrong. I chatted for a while with the helpful residents and came away with a list of plugin frameworks to look at, namely twisted.plugin and zope.interface.
Pulling in external dependencies is a big deal for us - many of our users are on Ubuntu or similar desktops with lots of python packages already installed, but some are not using GNOME or a Linux desktop at all, so we have to be sure that we need a library before we depend on it.
After playing a little with both of the options I came to the conclusion that while they are both really well made and capable, they are far more formal than we need, and the added dependency issues continued to concern me.
I revisited Armin's plugin system and removed the use of .\_\_subclasses\_\_() that was breaking doctest and offending \#python instead having a list in each .py file which the plugin system extracts and treats any classes mentioned in that list as plugins. I also extended it to always instantiate the plugins and look for the plugin files in both the system directories and the user's home directory.
[This plugin system](http://bazaar.launchpad.net/%7Ecmsj/%2Bjunk/terminator-epic-refactor/annotate/head%3A/terminatorlib/plugin.py) is currently hooked into two places in the branch, URL mangling and the context menu. This allows plugins to add support for new URL types (e.g. we just added support for Launchpad code URLs like lp:~cmsj/+junk/terminator-epic-refactor), and insert new options into the context menu. I'm not sure if we need to go further, but if you would like to hook into other parts let me know - it's pretty easy to arrange now :)
