Title: HP Microserver Remote Access helper
Date: 2012-01-06
Tags: Techie Python

I've only had the Remote Access card installed in my HP Microserver for a few hours and already I am bored of accessing it by first logging into the web UI, then navigating to the right bit of the UI, then clicking a button to download a .jnlp file and then running that with javaws(1).
Instead, I have written some Python that will login for you, fetch the file and execute javaws. Much better!
You can find the code: [here](http://bazaar.launchpad.net/~cmsj/+junk/microserver/view/head:/vkvm.py) and you'll want to have python-httplib2 installed.
