Title: Photo import workflow
Date: 2012-07-01
Tags: apple Techie workflow Docs aperture

**Introduction**
----------------

Since I'm writing about workflows today, I thought I'd also quickly chuck in a guide to how I get the photos and movies that I've taken with my iPhone, onto my laptop and specifically, imported into Aperture.
The Mechanics
This requires a few moving parts to produce a final workflow. The high-level process is:
1.  Plug iPhone into a USB port
2.  Copy photos from the iPhone into a temporary directory, deleting them as they are successfully retrieved
3.  Import the photos into Aperture, ensuring they are copied into its library and deleted from the temporary directory

Simple, right? Well yes and no.
**Retrieval from iPhone**
-------------------------

This really ought to be easier than it is, but at least it is possible.
Aperture can import photos from devices, but it doesn't seem to offer the ability to delete them from the device after import. That alone makes it not even worth bothering with if you don't want to build up a ton of old photos on your phone.
OS X does ship with a tool that can import photos from camera devices and delete the photos afterwards, a tool called AutoImporter.app, but you won't find it without looking hard. It lives at:
/System/Library/Image Capture/Support/Application/AutoImporter.app

If you run that tool, you will see no window, just a dock icon and some menus. Go into its Preferences and you will be able to choose a directory to import to, and choose whether or not to delete the files:
![prefs](http://4.bp.blogspot.com/--OEaNLL9SEE/T_DS6VM8rzI/AAAAAAAAAKc/INjg4f4Cj6k/s400/autoimporter.png)

Easy!
**Importing into Aperture**
---------------------------

This involves using Automator to build a Folder Action workflow for the directory that AutoImporter is pulling the photos into. All it does is check to see if AutoImporter is still running and if so wait, then launch Aperture and tell it to import everything from that directory into a particular Project, and then delete the source files:
[<img src="http://4.bp.blogspot.com/-5B0PgNdoJ_4/T_DT_eMKLqI/AAAAAAAAAKk/9mYLM7P5IR4/s640/aperture-autoimport-workflow.png" width="459" height="640" />](http://4.bp.blogspot.com/-5B0PgNdoJ_4/T_DT_eMKLqI/AAAAAAAAAKk/9mYLM7P5IR4/s1600/aperture-autoimport-workflow.png)

**That's it!**
--------------

Really, that's all there is. Now whenever you plug in your iPhone, all of the pictures and movies you've taken recently, will get imported into Aperture for you to process, archive, touch-up, export or whatever else it is that you do with your photos and movies.
