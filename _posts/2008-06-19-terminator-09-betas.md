Title: Terminator 0.9 Betas
Date: 2008-06-19
Tags: FOSS Techie Ubuntu Terminator

The [list of bugs](https://edge.launchpad.net/terminator/+milestone/0.9 "Terminator 0.9 Milestone bugs") left for Terminator 0.9 is getting shorter (although I do keep failing to resist the temptation to add more) and so it's time to try and get some wider testing.
Therefore, we've started uploading beta packages of 0.9 to our PPA. If you want to help test and you run Ubuntu, add this apt source:

```
deb http://ppa.launchpad.net/gnome-terminator/ubuntu hardy main
```

If you have Terminator installed already from a package, it should then be offered to you as an upgrade, otherwise run:
`sudo apt-get update && sudo apt-get install terminator`
Please report any bugs you find to [http://bugs.launchpad.net/terminator/](http://bugs.launchpad.net/terminator/%20 "Terminator Bugs @ Launchpad") and please feel free to drop by \#terminator on irc.freenode.net with any feedback!
