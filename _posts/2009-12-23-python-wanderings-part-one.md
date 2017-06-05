Title: Python wanderings, part one
Date: 2009-12-23
Tags: FOSS Techie Python Terminator

As mentioned in my earlier post about refactoring Terminator, I want to talk about some of the things I've learned about Python and programming in the last few months. If I were you I wouldn't place any great significance in anything I'm about to say - after all I'm a rank amateur in the field of programming.
This is going to be a multi-part post so I at least get something out there, rather than leaving it to rot forever in my drafts folder.

1. Solving global warming^W variables
-------------------------------------

I have objects that represent terminal widgets, objects that represent widgets that contain terminals, objects that contain configuration, and one master object that functions as the brains of the operation.
Inevitably these objects need to know about each other, but how to achieve that? The brain object is simply called 'Terminator' and almost every other part of the system needs to know about it, same with the config object, and Terminator needs to know about all of the terminal objects, etc. The dependencies are all over the place and one aim of the re-factor was to separate all these parts out and decouple them, but ultimately I was never going to get away from different objects needing to know about each other.
So how to go about it? As far as I know the options are:
-   pass around object references (every time you create something, pass it your references to all the bits it needs)
    -   Pros: no hacks or tricks involved
    -   Cons: makes every \_\_init\_\_() more complicated, means passing references that an object doesn't need other than to pass to its children.

-   use global variables
    -   Pros: they're global
    -   Cons: everyone seems to hate global variables, perhaps because it's an implicit dependency not an explicit one, or because of potential namespace collisions, or maybe other reasons.

-   use singletons
    -   Pros: explicit dependency
    -   Cons: often seems to involve hackery to get the singleton object reference

In my searching around I came across a fourth option that somewhat relates to singletons... the Borg pattern.
This is a very simple idea - it's a class that always instantiates to the same thing. You don't need a factory or function or something that gives you a reference to the singleton, you just instantiate a class and it's the same as all of the others you've instantiated of the same class.
Best of all, the Borg pattern is incredibly simple in Python. Like, really simple. Don't believe me? Click [here](http://code.activestate.com/recipes/66531/). Yep, four lines of code. Technically it's probably a bit ugly, but the resulting code feels very clean.
So now I have the Borg pattern in use for the main class, a class that provides all the configuration, a class that discovers plugins and lets them be referenced, and a fairly new class I'm experimenting with that acts as a factory for all of my classes, as a way to break any possibility of circular module dependencies.
Reality has to bite though, the Borg isn't a panacea; One has to be very careful about how one creates Borg objects. I chose to create a base class called Borg which Terminator, Config, Factory and PluginRegistry all derive from, but this turns out to have been a very short sighted decision to abstract out the common 4 lines. It wasn't until I started building Config to have functions that allow it to be accessed as a dict that I realised all of my Terminator, Config, Factory and PluginRegistry instances were the same thing as opposed to each type being distinct. It's also terrifyingly important that the subclasses of Borg not use class attributes. Any attributes defined by these classes \*must\* be instantiated as None so they are instance variables, and \*after\* you've called Borg.\_\_init\_\_(self) in your own \_\_init\_\_() you can then set up your attributes however you want because they are then part of the shared state.
On the whole I am happy with the Borg pattern. I've written test code to ensure that all of the assumptions I explicitly made are guaranteed, and all of the implicit assumptions I've discovered I made are also safe. Nonetheless, it's not a completely clean solution and I find myself wishing it was somehow a primitive of the language.
