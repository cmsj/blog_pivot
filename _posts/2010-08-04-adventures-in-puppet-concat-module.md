Title: Adventures in Puppet: concat module
Date: 2010-08-04
Tags: FOSS Puppet Techie Ubuntu

R.I. Pienaar has a Puppet module on github called "concat". Its premise is very simple, it just concatenates fragments of text together into a particular file.
I'm sure that a more seasoned Puppet veteran would have had this running in no time, but since it introduced some new concepts for me, I thought I'd throw up some notes of how I'm using it. I was particularly interested in an example usage I saw which lists the puppet modules a system is using in its /etc/motd, but because of the way Ubuntu handles constructing the motd, I needed to slightly rework the example. In Ubuntu, the /etc/motd file is constructed dynamically when you log in - this is done by pam\_motd which executes the scripts in /etc/update-motd.d/. One of those scripts (99-footer) will simply append the contents of /etc/motd.tail to /etc/motd after everything else - my example will take advantage of this. If you are already using motd.tail, you could just have this puppet system write to a different file and then drop another script into /etc/update-motd.d/ to append the contents of that different file.
This is what I did:

-   git clone http://github.com/ripienaar/puppet-concat.git
-   Move the resulting git branch to /etc/puppet/modules/concat and add it to my top-level site manifest that includes modules
-   Create a class to manage /etc/motd.tail. In my setup this ends up being /etc/puppet/manifests/classes/motd.pp, which is included by my default node, but your setup is probably different. This is what my class looks like:

    class motd {
           include concat::setup
           $motdfile = "/etc/motd.tail"

           concat{$motdfile:
                   owner => root,
                   group => root,
                   mode => 644
           }

           concat::fragment{"motd_header":
                   target => $motdfile,
                   content => "\nPuppet modules: ",
                   order => 10,
           }

           concat::fragment{"motd_footer":
                   target => $motdfile,
                   content => "\n\n",
                   order => 90,
           }
    }

    # used by other modules to register themselves in the motd
    define motd::register($content="", $order=20) {
       if $content == "" {
          $body = $name
       } else {
          $body = $content
       }

       concat::fragment{"motd_fragment_$name":
          target  => "/etc/motd.tail",
          content => "$body ",
          order => $order
       }
    }

So that's quite a mouthful. Let's break it down:
-   We have to include concat::setup so the concat module can...set... up :)
-   We then set a variable pointing at the location of the file we want to manage
-   We then instantiate the concat module for the file we want to manage and set properties like the ownership/mode
-   We then call the concat::fragment function for two specific fragments we want in the output - a header and a footer (although I do this on a single line, so it's the phrase "Puppet modules" and "\\n\\n" respectively). They're forced to be header/footer by the "order" parameter - by making sure we use a low number for the header and a high number for the footer, we get the layout we expect.
-   Outsite this class we define a function motd::register which other modules will call and the content they supply will be handed to concat::fragment with a default order parameter of 20 (which is higher than the value we used for the header and lower than the footer one).

Finally, in each of my modules I include the line:
    motd::register{"someawesomemodule":}

and now when I ssh to a node, I see a line like:
    Puppet modules: web ssh 

It's a fairly simple little thing, but quite pleasing and from here out it's almost zero effort - just adding the motd::register calls to each module.
