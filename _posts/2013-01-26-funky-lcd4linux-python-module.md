Title: Funky lcd4linux python module
Date: 2013-01-26
Tags: FOSS LCD NAS Techie Ubuntu lcd4linux fileserver

I've got an LCD on the way, to put in my fileserver and show some status/health info.
Rather than wait for the thing to arrive I've gone ahead and started making the config I want with lcd4linux.
Since the LCD I'm getting is only 20 characters wide and 4 lines tall, there is not very much space, so I've had to get pretty creative with how I'm displaying information.
One thing I wanted was to show the percentage used of the various disks in the machine, but since I have at least 3 mount points, that would either mean scrolling text (ugly) or consuming ¾ of the display (inefficient).
It seemed like a much nicer idea to use a single line to represent the space used as a percentage and simple display each of the mounts in turn, but unfortunately lcd4linux's "Evaluator" syntax is not sufficiently complex to be able to implement this directly, so I faced the challenge of either writing a C plugin or passing the functionality off to a Python module.
I tend to think that this feature ought to be implemented as a C plugin because it makes it easier to use, but I am unlikely to bother with that because I prefer Python, so I went with a Python module :)
The code is [on github](https://github.com/cmsj/lcd4linux_rotator) and the included README.md covers how to use it in an lcd4linux configuration.
At some point soon I'll post my lcd4linux configuration - just as soon as I've figured out what to do with the precious 4th line. In the mean time, here is a video of the rotator plugin operating on the third line (the first line being disk activity and the second line being network activity):

Update: I figured out what to do with the fourth line:

That's another python module, this time a port of Chris Applegate's [Daily Mail headline generator](http://www.qwghlm.co.uk/toys/dailymail/) from JavaScript to Python. Code is on [github](https://github.com/cmsj/dailymail).
As promised, the complete lcd4linux config is available (also on github) [here](https://gist.github.com/4694242).

