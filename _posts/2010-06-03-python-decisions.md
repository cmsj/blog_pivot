Title: Python decisions
Date: 2010-06-03
Tags: FOSS Techie Ubuntu Python

Every time I find myself hacking on some Python I find myself second guessing all sorts of tiny design decisions and so I figure the only way to get any kind of perspective on them is to talk about them. Either I'll achieve more clarity through constructing explanations of what I was thinking, or people will comment with useful insights. Hopefully the latter, but this is hardly the most popular blog in the world ;)
So. What shall we look at first. Well, I just hacked up a tiny script last night to answer a simple question:

*Is most of my music collection from the 90s?*

Obviously what I want to do here is examine the ID3 tags of the files in my music collection and see how they're distributed. A quick search with apt showed that Ubuntu 10.04 has two python libraries for dealing with ID3 tags and a quick play with each suggested that the one with the API most relevant to my interests was [eyeD3](apt:python-eyed3 "Install eyeD3"). After a few test iterations of the script I was getting bored of waiting for it to silently scan the roughly 4000 MP3s I have, so I did another quick search and found a [progress bar](apt:python-progressbar "Install progressbar")library.
So that's all of the motive and opportunity established, now let's examine the means to the end. If you want to follow along at home, the whole script is [here](/wp-content/uploads/2010/06/musicdecades.py_.txt "musicdecades.py.txt").

```python
try:
  import eyeD3
  import progressbar as pbar
except ImportError:
  print("You should make sure python-eyed3 and python-progressbar are installed")
 sys.exit(1)
```

First off this is the section where I'm importing the two non-default python libraries that I depend on. I want to provide a good experience when they're not installed, so I catch the exception and tell people the Debian/Ubuntu package names they need, and exit gracefully. I rename the progressbar module as I import it just because "progressbar" is annoyingly long as a name, and I don't like doing "from foo import \*".
Skipping further on, we find the code that extracts the ID3 year tag:

```python
year = tag.getYear() or 'Unknown'
```

This is something I'm really not sure about the "correctness" of; One of the reasons I went with the eyeD3 library was that the getYear() method returns None if it can't find any data, but I don't really want to capture the result, then test the result explicitly and if it's None set the value to "Unknown", so I went with the above code which only needs a single line and is (IMHO) highly readable.
This is ultimately the crux of the entire program - we've now collected the year, so we can work out which decade it's from:

```python
if year is not 'Unknown':
 year = "%s0s" % str(year)[:3]
```

If this isn't an unknown year we chop the final digit off the year and replace it with a zero. Job done!
Next up, another style question. Rather than store the year we just processed I want to know how many of each decade have been found, so the obvious choice is a dict where the keys are the decades and the values are the number of times each decade has been found. One option would be to pre-fill the dict with all the decades, each with a value of zero, but that seems redundant and ugly, so instead I start out with an empty dict. This presents a challenge - if we find a decade that isn't already a key in the dict (which will frequently be the case) we need to notice that and add it. We could do this by pre-emptively testing the dict with its has\_key() method, but that struck me as annoyingly wordy, so I went with:

```python
try:
  years[year] += 1
except KeyError:
  years[year] = 1
```

If we are incrementing a year that isn't already in the dict, python will raise a KeyError, at which point we know what's happened and know the correct value is 1, so we just set it explicitly. Seems like the simplest solution, but is it the sanest?
The only other thing I wanted to say is a complaint - having built up the dict I then want to print it nicely, so I have a quick list comprehension to produce a list of strings of the format "19xx: yy" (i.e. the decade and the final number of tracks found for that decade), which I then join together using:

```python
', '.join(OUTPUT)
```

which I hate! Why can't I do:

```python
OUTPUT.join(', ')
```

(where "OUTPUT" is the list of strings). If that were possible, what I'd actually do is tack the .join() onto the end of the list comprehension and a single line would turn the dict into a printable string.
So there we have it, my thoughts on the structure of my script. I'd also add that I've become mildly obsessive about getting good scores from pylint on my code, which is why it's rigorously formatted, docstring-ed and why the variable names in the \_\_main\_\_ section are in capitals.
What are your thoughts?
Oh, and the answer is no, most of my music is from the 2000s. The 1990s come in second :)
