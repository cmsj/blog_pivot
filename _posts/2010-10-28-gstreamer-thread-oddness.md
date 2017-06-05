Title: GStreamer thread oddness
Date: 2010-10-28
Tags: FOSS Techie Ubuntu Python

I sometimes find myself in a place where there are a number of Icecast streams going out at once and I'm interested in finding better ways of monitoring these. It seems like a nice option would be a window showing a visualisation of each stream.
I quickly whipped up some python to do this, but it almost always locks up when I run it, but I'm not sure if I've done something fundementally wrong or if I've found a bug somewhere.
If you are a gstreamer expert, please take a look a [this code](http://bazaar.launchpad.net/~cmsj/%2Bjunk/icecastvis/annotate/head%3A/icecastvisualiser.py "Some python") and let me know what I should do next! If you know a gstreamer expert, please try and bribe them to read this post ;)
