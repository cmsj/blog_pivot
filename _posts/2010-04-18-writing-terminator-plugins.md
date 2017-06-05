Title: Writing Terminator plugins
Date: 2010-04-18
Tags: FOSS Techie Ubuntu Python Terminator

Terminator Plugin HOWTO
-----------------------

One of the features of the new 0.9x series of Terminator releases that hasn't had a huge amount of announcement/discussion yet is the plugin system. I've posted previously about the decisions that went into the design of the plugin framework, but I figured now would be a good time to look at how to actually take advantage of it.
While the plugin system is really generic, so far there are only two points in the Terminator code that actually look for plugins - the Terminal context menu and the default URL opening code. If you find you'd like to write a plugin that interacts with a different part of Terminator, please let me know, I'd love to see some clever uses of plugins and I definitely want to expand the number of points that plugins can hook into.
The basics of a plugin
----------------------

A plugin is a class in a .py file in terminatorlib/plugins or ~/.config/terminator/plugins, but not all classes are automatically treated as plugins. Terminator will examine each of the .py files it finds for a list called 'available' and it will load each of the classes mentioned therein.
Additionally, it would be a good idea to import terminatorlib.plugin as that contains the base classes that other plugins should be derived from.
A quick example:
```
import terminatorlib.plugin as plugin
available = ['myfirstplugin']
class myfirstplugin(plugin.SomeBasePluginClass):
  etc.
```

So now let's move on to the simplest type of plugin currently available in Terminator, a URL handler.
URL Handlers
------------

This type of plugin adds new regular expressions to match text in the terminal that should be handled as URLs. We ship an example of this with Terminator, it's a handler that adds support for the commonly used format for Launchpad. Ignoring the comments and the basics above, this is ultimately all it is:
```
class LaunchpadBugURLHandler(plugin.URLHandler):
  capabilities = ['url_handler']
  handler_name = 'launchpad_bug'
  match = '\\b(lp|LP):?\s?#?[0-9]+(,\s*#?[0-9]+)*\\b'
```

```
  def callback(self, url):
    for item in re.findall(r'[0-9]+', url):
      return('https://bugs.launchpad.net/bugs/%s' % item)
```

That's it! Let's break it down a little to see the important things here:
-   inherit from plugin.URLHandler if you want to handle URLs.
-   include 'url\_handler' in your capabilities list
-   URL handlers must specify a unique handler\_name (no enforcement of uniqueness is performed by Terminator, so use some common sense with the namespace)
-   Terminator will call a method in your class called callback() and pass it the text that was matched. You must return a valid URL which will probably be based on this text.

and that's all there is to it really. Next time you start terminator you should find the pattern you added gets handled as a URL!
Context menu items
------------------

This type of plugin is a little more involved, but not a huge amount and as with URLHandler we ship an example in terminatorlib/plugins/custom\_commands.py which is a plugin that allows users to add custom commands to be sent to the terminal when selected. This also brings a second aspect of making more complex plugins - storing configuration. Terminator's shiny new configuration system (based on the excellent ConfigObj) exposes some API for plugins to use for loading and storing their configuration. The nuts and bolts here are:
```
import terminatorlib.plugin as plugin
from terminatorlib.config import Config
available = ['CustomCommandsMenu']
class CustomCommandsMenu(plugin.MenuItem):
  capabilities = ['terminal_menu']
  config = None
  def __init__(self):
    self.config = Config()
    myconfig = self.config.plugin_get_config(self.__class__.__name__)
    # Now extract valid data from sections{}
  def callback(self, menuitems, menu, terminal):
    menuitems.append(gtk.MenuItem('some jazz'))
```

This is a pretty simplified example, but it's sufficient to insert a menu item that says "some jazz". I'm not going to go into the detail of hooking up a handler to the 'activate' event of the MenuItem or other PyGTK mechanics, but this gives you the basic detail. The method that Terminator will call from your class is again "callback()" and you get passed a list you should add your menu structure to, along with references to the main menu object and the related Terminal. As the plugin system expands and matures I'd like to be more formal about the API that plugins should expect to be able to rely on, rather than having them poke around inside classes like Config and Terminal. Suggestions are welcome :)
Regarding the configuration storage API - the value returned by Config.plugin\_get\_config() is just a dict, it's whatever is currently configured for your plugin's name in the Terminator config file. There's no validation of this data, so you should pay attention to it containing valid data. You can then set whatever you want in this dict and pass it to Config().plugin\_set\_config() with the name of your class and then call Config().save() to flush this out to disk (I recommend that you be quite liberal about calling save()).
Wrap up
-------

Right now that's all there is to it. Please get in touch if you have any suggestions or questions - I'd love to ship more plugins with Terminator itself, and I can think of some great ideas. Probably the most useful thing would be something to help customise Terminator for heavy ssh users (see the earlier fork of Terminator called 'ssherminator')
