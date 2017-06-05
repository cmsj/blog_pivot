Title: gtk icon cache search tool
Date: 2010-05-13
Tags: FOSS Techie Ubuntu Python

Earlier on this evening I was asking the very excellent Ted Gould about a weird problem with my Gtk+ icon theme - an app I'd previously installed by hand in /usr/local/, but subsequently removed, had broken icons because Gtk+ was looking in /usr/local/share/icons/ instead of /usr/share/icons/.
We did a little digging and realised I had an icon theme cache file in /usr/local/ that was overriding the one in /usr/. A bit of deleting later and it's back, but in the process we whipped up a little bit of python to print out the filename of an icon given an icon name.

```python
#!/usr/bin/python
# gtk-find-icon by Chris Jones <cmsj@tenshu.net>
# Copyright 2010. GPL v2.

import sys
import gtk

THEME = gtk.IconTheme()
ICON = THEME.lookup_icon(sys.argv[1],
 gtk.ICON_SIZE_MENU,
 gtk.ICON_LOOKUP_USE_BUILTIN)

if not ICON:
 print "None found"
else:
 print(ICON.get_filename())
```
