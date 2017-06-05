Title: Random puppetry
Date: 2010-07-14
Tags: FOSS Techie Ubuntu

I was talking to a colleague earlier about Puppet and its ability to install packages. I'd not really given it much thought beyond using it to install packages on classes of machines, but he mentioned one particular package which gets updated quite frequently, but is extremely low risk to update - tzdata. By setting this to "ensure =&gt; latest" rather than "ensure =&gt; present" I can forget about ever having to upgrade that package again \\o/
Simple really, but it hadn't occurred to me.
