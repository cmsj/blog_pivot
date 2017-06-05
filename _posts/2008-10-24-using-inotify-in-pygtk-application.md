Title: Using inotify in a pygtk application without pyinotify
Date: 2008-10-24
Tags: FOSS Techie Terminator

I am lazy. There's no denying it, it's simple fact.
That means, for example, when I am working with pygtk and I look at the API for pynotify, I am sad, because it's a polling API and I hate polling.
What I like is GTK's event model. I like telling it what to run when things happen and letting it take care of all the pain for me. Obviously it's possible to write some code which will do the polling and then trigger an event, but that compromises my freedom to be lazy.
Step in pygobject, which contains the bindings for GIO. Within the bowels of this beastie are the required components to monitor a file for changes in a very few lines of python:

```
#!/usr/bin/python
import gtk
import gio

def file_changed (monitor, file, unknown, event):
  if event == gio.FILE_MONITOR_EVENT_CHANGES_DONE_HINT:
    print "file finished changing"

file = gio.File('/path/to/some/file')
monitor = file.monitor_file ()
monitor.connect ("changed", file_changed)
gtk.main()
```

That's it. I don't know what the "unknown" argument for the callback is, probably the optional user\_data connect() argument and this is but a small part of what inotify/GIO can do, but if you just care about being told when a file is updated, it'll do (with caveats that you can never really know when a file has finished being changed, so be careful to validate it before you trust its contents).
