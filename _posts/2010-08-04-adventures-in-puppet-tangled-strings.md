Title: Adventures in Puppet: Tangled Strings
Date: 2010-08-04
Tags: FOSS Puppet Techie Ubuntu

I am trying to do as much management on my new VM servers as possible with Puppet, but these are machines I still frequently log on to, and not everything is managed by Puppet, so it's entirely possible that in a fit of forgetfulness I will start editing a file that Puppet is managing and then be annoyed when my changes are lost next time Puppet runs.
Since prior preparation and planning prevents pitifully poor performance, I decided to do something about this.
Thus, I present a VIM plugin called TangledStrings, which I'm distributing as a Vimball (.vba) you can download from its [project page](http://launchpad.net/tangledstrings "TangledStrings") on Launchpad. For more information on Vimball formatted plugins, see [this page](http://vimdoc.sourceforge.net/htmldoc/pi_vimball.html "Vimball Documentation"). To install the plugin, simply:

-   vim tangledstrings.vba
-   Follow the instructions from Vimball to type: :so %

By default, TangledStrings will show a (configurable) warning message when you load a Puppet-owned file:
[<img src="http://www.tenshu.net/wp-content/uploads/2010/08/puppetstrings_alert.png" title="tangledstrings_alert" class="aligncenter size-full wp-image-11573" width="403" height="127" />](http://www.tenshu.net/wp-content/uploads/2010/08/puppetstrings_alert.png)
This message can be disabled, and you can choose to enable a persistent message in the VIM status line instead:
[<img src="http://www.tenshu.net/wp-content/uploads/2010/08/tangledstrings_statusline.png" title="tangledstrings_statusline" class="aligncenter size-full wp-image-11574" width="403" height="127" />](http://www.tenshu.net/wp-content/uploads/2010/08/tangledstrings_statusline.png)
(or you could choose to enable both of these methods).
For more information, see the documentation included in the Vimball which you can display with the VIM command:
```
:help TangledStrings
```

Suggestions, improvements, patches, etc. are most welcome! Email me or use Launchpad to file bugs and propose merges.
