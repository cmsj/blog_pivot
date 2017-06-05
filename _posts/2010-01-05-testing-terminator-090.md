Title: Testing Terminator 0.90
Date: 2010-01-05
Tags: FOSS Techie Ubuntu Python Terminator

You might have seen my recent posts about the epic refactoring that has been going on in the Terminator codebase for the last few months.
I think it's finally time that we get some more eyeballs on it, mainly so I can check that I haven't massively screwed something up. I know there is lots of missing functionality right now, and probably a bunch of subtle bugs, but I could use your help quantifying exactly what these are!
If you're inclined to help, please branch *lp:~cmsj/terminator/epic-refactor*, cd into it and run *./terminator*, then use it like you always would and file bugs, preferably indicating clearly in the bug that you're using this branch and not trunk (maybe tag the bug '**epicrefactor**').
Things I know are broken right now:

-   -e and -x command line options
-   all forms of drag & drop
-   directional navigation
-   some keyboard shortcuts

Things I know are missing because they're not coming back:
-   Extreme tabs mode (sorry, it's just too insane to support)
-   GNOME Terminal profile reading (I'm trying to simplify our crazy config system and dropping GConf is a good way to achieve that)
-   Config file reading. At some point I'll write something that migrates old Terminator configs to the new format, but for now you'll have to live without your old config file. The new one isn't documented yet either, but it is a whole bunch better!

Now would also be a great time to start writing plugins for Terminator and telling me about them. I'm happy to ship good plugins, but more importantly I want feedback about the weaknesses/strengths of our plugin system. Right now you can only hook into URL mangling and the terminal context menu, but the latter of those gives you pretty serious flexibility I think. Obviously one massive weakness is a lack of documentation about the plugin API, but I'll get to that, I promise!
So there we have it, another step along the way to me being able to merge this branch into trunk and put out a real release of 0.90 and then eventually 1.0!
