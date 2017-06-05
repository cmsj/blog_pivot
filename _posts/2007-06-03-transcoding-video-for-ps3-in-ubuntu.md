Title: Transcoding video for the PS3 in Ubuntu
Date: 2007-06-03
Tags: FOSS Ubuntu Games

The title says it all really. I've banged on about games a bit and now it's time to do something involving curiosity!
So I know how to, should i want to for any reason, this is how you can make videos that are playable on a PS3 (only tested on firmware 1.80). Note that it doesn't transcode to H.264, but instead uses a lesser MPEG4 profile of some kind (mpeg4 appears to be entirely too complicated to figure out!).
This mostly comes from [here](http://po-ru.com/diary/bleeding-edge-ffmpeg-on-ubuntu-feisty/), but the gist is that you grab a specific version of ffmpeg from SVN and compile it against a variety of media libraries from multiverse. This makes me think ubuntu should have a bleeding edge version of ffmpeg in multiverse that links against the libraries there - if licences allow for it.
For what it's worth, this is the command line I've been using with it (note that it will strip surround audio down to 2 channel stereo. That's all i have, so I haven't bothered to figure out anything better).
`ffmpeg -y -i /path/to/source.avi -acodec aac -ab 192kb -vcodec mpeg4 -b 1500kb -mbd 2 -flags +4mv+trell -aic 2 -cmp 2 -subcmp 2 -title "Blah 2: The blahing" /path/to/output.mp4`
