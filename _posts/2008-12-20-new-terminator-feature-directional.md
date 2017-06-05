Title: New Terminator feature: Directional terminal navigation
Date: 2008-12-20
Tags: FOSS Techie Ubuntu Terminator

One of the nice side effects of being at an Ubuntu Developer Summit is the interesting conversations you have with people outside of the sessions themselves (which, as a sysadmin present to support the event itself, I was not particularly involved in).
One such conversation was over lunch in a busy Google canteen with Kees Cook, one of our rocking security engineers and a thoroughly decent chap. After a while we came to the subject of Terminator and some of the features we'd like to implement, the ones that have been requested and we're not particularly motivated to work on, and the odd ones.
I was trying to remember the things people have asked us to add, that for one reason or another we probably aren't going to and mentioned one about the ability to navigate between terminals in a directional way: That is, rather than doing Ctrl-Tab to cycle through the terminals in the order they were created, you could press something to move to the terminal to the left, or above, right or below the currently focussed one.
On the surface it sounds like a good idea and probably not too tricky, but only if you think from the point of view of a grid. That really would be easy, but our UI is produced by way of a tree structure, not a grid. This means you need to do some proper thinking to figure it out. Kees' face lit up and he said something like "that sounds like a graph theory problem!" and expressed an interest in working on it.
I'm very glad to say that in just under a week after we all went home from UDS, I've just merged his results into trunk.
Thanks very much to Kees for his code and helpful suggestions (such as resurrecting Marcus Korn's simultaneous-typing branch, which I did one evening at UDS - rewriting it and then merging it into trunk). Also thanks to the other Terminator users I met there :)
So that's two new features, although not really the big ticket ones that are holding back 1.0 ;)
