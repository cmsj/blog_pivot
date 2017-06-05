Title: Terminator 0.96 released
Date: 2011-09-23
Tags: FOSS Ubuntu Python Terminator

I've just pushed up the release tarball and PPA uploads for Terminator 0.96. It's mainly a bug fix release, but it does include a few new features. Many thanks to the various community folks who have contributed fixes, patches, bugs, translations and branches to this release. The changelog is below:

terminator 0.96:

  * Unity support for opening new windows (Lucian Adrian Grijincu)
  * Fix searching with infinite scrollback (Julien Thewys \#755077)
  * Fix searching on Ubuntu 10.10 and 11.04, and implement searching by regular expression (Roberto Aguilar \#709018)
  * Optimise various low level components so they are dramatically faster (Stephen Boddy)
  * Fix various bugs (Stephen Boddy)
  * Fix cursor colours (\#700969) and a cursor blink issue (Tony Baker)
  * Improve and extend drag&drop support to include more sources of text, e.g. Gtk file chooser path buttons (\#643425)
  * Add a plugin to watch a terminal for inactvity (i.e. silence)
  * Fix loading layouts with more than two tabs (\#646826)
  * Fix order of tabs created from saved layouts (\#615930)
  * Add configuration to remove terminal dimensions from titlebars (patch from João Pinto \#691213)
  * Restore split positions more accurately (patch from Glenn Moss \#797953)
  * Fix activity notification in active terminals. (patch from Chris Newton \#748681)
  * Stop leaking child processes if terminals are closed using the context menu (\#308025)
  * Don't forget tab order and custom labels when closing terminals in them (\#711356)
  * Each terminal is assigned a unique identifier and this is exposed to the processes inside the terminal via the environment variable TERMINATOR\_UUID
  * Expand dbus support to start covering useful methods. Also add a commandline tool called 'remotinator' that can be used to control Terminator from a terminal running inside it.
  * Fix terminal font settings for users of older Linux distributions
