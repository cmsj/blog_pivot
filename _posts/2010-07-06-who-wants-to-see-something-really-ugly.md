Title: Who wants to see something really ugly?
Date: 2010-07-06
Tags: FOSS Techie Ubuntu Python Terminator

I think it should be abundantly clear from my postings here that I'm not a very good programmer, and this means I give myself a lot of free rope to do some very stupid things.
I'm in constant need of debugging information and in Terminator particularly where we have lots of objects all interacting and reparenting all the time. We've had a simple dbg() method for a long time, but I was getting very bored of typing out dbg('Class::method:: Some message about %d' % foo), so I decided to see what could be done about inferring the Class and method parts of the message.
It turns out that python is very good at introspecting its own runtime, so back in January, armed with my own stupidity and some help from various folks on the Internet, I came up with the following:

    # set this to true to enable debugging output
    DEBUG = False
    # set this to true to additionally list filenames in debugging
    DEBUGFILES = False
    # list of classes to show debugging for. empty list means show all classes
    DEBUGCLASSES = []
    # list of methods to show debugging for. empty list means show all methods
    DEBUGMETHODS = []

    def dbg(log = ""):
        """Print a message if debugging is enabled"""
        if DEBUG:
            stackitem = inspect.stack()[1]
            parent_frame = stackitem[0]
            method = parent_frame.f_code.co_name
            names, varargs, keywords, local_vars = inspect.getargvalues(parent_frame)
            try:
                self_name = names[0]
                classname = local_vars[self_name].__class__.__name__
            except IndexError:
                classname = "noclass"
            if DEBUGFILES:
                line = stackitem[2]
                filename = parent_frame.f_code.co_filename
                extra = " (%s:%s)" % (filename, line)
            else:
                extra = ""
            if DEBUGCLASSES != [] and classname not in DEBUGCLASSES:
                return
            if DEBUGMETHODS != [] and method not in DEBUGMETHODS:
                return
            try:
                print >> sys.stderr, "%s::%s: %s%s" % (classname, method, log, extra)
            except IOError:
                pass

How's about that for shockingly bad? ;)
It also adds a really impressive amount of overhead to the execution time.
I added the DEBUGCLASSES and DEBUGMETHODS lists so I could cut down on the huge amount of output - these are hooked up to command line options, so you can do something like "terminator -d --debug-classes=Terminal" and only receive debugging messages from the Terminal module.
I'm not exactly sure what I hope to gain from this post, other than ridicule on the Internet, but maybe, just maybe, someone will pop up and point out how stupid I am in a way that turns this into a 2 line, low-overhead function :D
