Title: Esoteric command line argument parsing in python
Date: 2008-02-12
Tags: FOSS Techie

[Terminator](http://www.tenshu.net/terminator/) will soon appear in GNOME's Preferred Applications preferences if you have it installed and as such I figure we need to support -x in the same way gnome-terminal does.
What that basically means is that \*anything\* which occurs after -x on the command line is the command to execute and its arguments, so:
`terminator -x screen -U`
should cause terminator to execute `screen -U`. By default, most options parsers will see this as the `-U` being passed to terminator and `screen` being the argument to `-x`.
After looking around the docs and asking on \#python, optparse seemed to be a better option than getopts, so I switched it over and implemented a callback to extend the default argument processing for -x. It wasn't quite working, so after another quick foray into \#python I ended up reading [this page](http://docs.python.org/dev/library/optparse.html#callback-example-6-variable-arguments) which provided everything I needed. More than I needed, in fact, since their while loop has conditionals which affect whether or not the next arguments are added. I just want to gobble them all up and stop them from being parsed :)
