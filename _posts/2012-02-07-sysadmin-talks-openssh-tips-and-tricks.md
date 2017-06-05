Title: A sysadmin talks OpenSSH tips and tricks
Date: 2012-02-07
Tags: FOSS Techie Ubuntu Canonical

<span class="Apple-style-span" style="font-size: large;">**My take on more advanced SSH usage**</span>
I've seen a few articles recently on sites like HackerNews which claimed to cover some advanced SSH techniques/tricks. They were good articles, but for me (as a systems administrator) didn't get into the really powerful guts of OpenSSH.
So, I figured that I ought to pony up and write about some of the more advanced tricks that I have either used or seen others use. These will most likely be relevant to people who manage tens/hundreds of servers via SSH. Some of them are about actual configuration options for OpenSSH, others are recommendations for ways of working with OpenSSH.
<span class="Apple-style-span" style="font-size: large;">**Generate your <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">~/.ssh/config</span>**</span>
This isn't strictly an OpenSSH trick, but it's worth noting. If you have other sources of knowledge about your systems, automation can do a lot of the legwork for you in creating an SSH config. A perfect example here would be if you have some kind of database which knows about all your servers - you can use that to produce a fragment of an SSH config, then download it to your workstation and concatenate it with various other fragments into a final config. If you mix this with distributed version control, your entire team can share a broadly identical SSH config, with allowance for each person to have a personal fragment for their own preferences and personal hosts. I can't recommend this sort of collaborative working enough.
**
**
**<span class="Apple-style-span" style="font-size: large;">Generate your <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">~/.ssh/known\_hosts</span></span>**
<span class="Apple-style-span" style="font-weight: normal;">This follows on from the previous item. If you have some kind of database of servers, teach it the SSH host key of each (usually something like <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">/etc/ssh/ssh\_host\_rsa\_key.pub</span>) then you can export a file with the keys and hostnames in the correct format to use as a <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">known\_hosts</span> file, e.g.:</span>

> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace; font-weight: normal;">server1.company.com 10.0.0.101 ssh-rsa BLAHBLAHCRYPTOMUMBO</span>

<span class="Apple-style-span" style="font-weight: normal;">You can then associate this with all the relevant hosts by including something like this in your <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">~/.ssh/config</span>:</span>
> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">Host \*.mycompany.com
>   UserKnownHostsFile ~/.ssh/generated\_known\_hosts
>   StrictHostKeyChecking yes</span>

This brings some serious advantages:
-   **Safer** - because you have pre-loaded all of the host keys and specified strict host key checking, SSH will prompt you if you connect to a machine and something has changed.
-   **Discoverable** - if you have tab completion, your shell will let you explore your infrastructure just by prodding the Tab key.

**<span class="Apple-style-span" style="font-size: large;">Keep your private keys, private, private</span>**
<span class="Apple-style-span">This seems like it ought to be more obvious than it perhaps is... the private halves of your SSH keys are very privileged things. You should treat them with a great deal of respect. Don't put them on multiple machines (SSH keys are cheap to generate and revoke) and don't back them up.</span>
<span class="Apple-style-span">
</span>
**<span class="Apple-style-span" style="font-size: large;">Know your limits</span>**
If you're going to write a config snippet that applies to a lot of hosts you can't match with a wildcard, you may end up with a very long <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">Host</span> line in your ssh config. It's worth remembering that there is a limit to the length of lines: 1024 characters. If you're going to need to exceed that, you will have to just have multiple <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">Host</span> sections with the same options.

<span class="Apple-style-span" style="font-size: large;">**Set sane global defaults**</span>

> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">HashKnownHosts no
> Host \*
>   GSSAPIAuthentication no
>   ForwardAgent no</span>

These are very sane global defaults:

-   Known hosts hashing is good for keeping your hostnames secret from people who obtain your <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">known\_hosts</span> file, but is also really very inconvenient as you are also unable to get any useful information out of the file yourself (such as tab completion). If you're still feeling paranoid you might consider tightening the permissions on your <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">known\_hosts</span> file as it may be readable by other users on your workstation.
-   GSSAPI is very unlikely to be something you need, it's just slowing things down if it's enabled.
-   Agent forwarding can be tremendously dangerous and should, I think, be actively and passionately discouraged. It ought to be a nice feature, but it requires that you trust remote hosts unequivocally as if they had your private keys, because functionally speaking, they do. They don't actually have the private key material, but any sufficiently privileged process on the remote server can connect back to the SSH agent running on your workstation and request it respond to challenges from an SSH server. If you keep your keys unlocked in an SSH agent, this gives any privileged attacker on a server you are logged into, trivial access to any other machine your keys can SSH into<span class="Apple-style-span" style="font-family: Times, 'Times New Roman', serif;">. </span>If you somehow depend on using agent forwarding with Internet facing servers, please re-consider your security model (unless you are able to robustly and accurately argue why your usage is safe, but if that is the case then you don't need to be reading a post like this!)

<span class="Apple-style-span" style="font-size: large;">**Notify useful metadata**</span>

If you're using a Linux or OSX desktop, you either have something like <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">notify-send(1)</span> or Growl for desktop notifications. You can hook this into your SSH config to display useful metadata to yourself. The easiest way to do this is via the <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">LocalCommand</span> option:

> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">Host \*
>   PermitLocalCommand yes
>   LocalCommand /home/user/bin/ssh-notify.sh %h</span>

This will call the <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">ssh-notify.sh</span> script every time you SSH to a host, passing the hostname you gave, as an argument.  In the script you probably want to ensure you're actually in an interactive terminal and not some kind of backgrounded batch session - this can be done trivially by ensuring that <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">tty -s</span> returns zero. Now the script just needs to go and fetch some metadata about the server you're connecting to (e.g. its physical location, the services that run on it, its hardware specs, etc.) and format them into a command that will display a notification.

**<span class="Apple-style-span" style="font-size: large;">Sidestep overzealous key agents</span>**

If you have a lot of SSH keys in your ssh-agent (e.g. more than about 5) you may have noticed that SSHing to machines which want a password, or those which you wish to use a specific key that isn't in your agent, can be quite tricky. The reason for this is that OpenSSH currently seems to talk to the agent in preference to obeying command line options (i.e. <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">-i</span>) or config file directives (i.e. <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">IdentityFile or PreferredAuthentications</span>). You can force the behaviour you are asking for with the <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">IdentitiesOnly</span> option, e.g.:

> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">Host server1.company.com
>   IdentityFile /some/rarely/used/ssh.key
>   IdentitiesOnly yes</span>

(on a command line you would add this with <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">-o IdentitiesOnly=yes</span>)
**
**

**<span class="Apple-style-span" style="font-size: large;">Match hosts with wildcards</span>**

Sometimes you need to talk to a lot of almost identically-named servers. Obviously SSH has a way to make this easier or I wouldn't be mentioning this. For example, if you needed to ssh to a cluster of remote management devices:

> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">Host \*.company.com management-rack-??.company.com
>   User root
>   PreferredAuthentications password</span>

This will match anything ending in <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">.company.com</span> and also anything that starts with <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">management-rack-</span> and then has two characters, followed by <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">.company.com</span>.

**<span class="Apple-style-span" style="font-size: large;">Per-host SSH keys</span>**

You may have some machines where you have a different key for each machine. By naming them after the fully qualified domain names of the hosts they relate to, you can skip over a more tedious SSH config with something like the following:
> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">Host server-??.company.com
>   IdentityFile /some/path/id\_rsa-%h</span>

(the <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">%h</span> will be substituted with the FQDN you're SSHing to. The <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">ssh\_config</span> man page lists a few other available substitutions.)

**<span class="Apple-style-span" style="font-size: large;">Use fake, per-network port forwarding hosts</span>**

If you have network management devices which require web access that you normally forward ports for with the <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">-L</span> option, consider constructing a fake host in your SSH config which establishes all of the port forwards you need for that network/datacentre/etc:

> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">Host port-forwards-site1.company.com
>   Hostname server1.company.com
>   LocalForward 1234 10.0.0.101:1234</span>

This also means that your forwards will be on the same port each time, which makes saving certificates in your browser a reasonable undertaking. All you need to do is <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">ssh port-forwards-site1.company.com</span> (using nifty Tab completion of course!) and you're done. If you don't want it tying up a terminal you can add the options <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">-f</span> and <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">-N</span> to your command line, which will establish the ssh connection in the background.

If you're using programs which support SOCKS (e.g. Firefox and many other desktop Linux apps) you can use the <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">DynamicForward</span> option to send traffic over the SSH connection without having to add <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">LocalForward</span> entries for each port you care about. Used with a browser extension such as FoxyProxy (which lets you configure multiple proxies based on wildcard/regexp URL matches) makes for a very flexible setup.

<span class="Apple-style-span" style="font-size: large;">**Use an SSH jump host**</span>
Rather than have tens/dozens/hundreds/etc of servers holding their SSH port open to the Internet and being battered with brute force password cracking attempts, you might consider having a single host listening (or a single host per network perhaps), which you can proxy your SSH connections through.
If you do consider something like this, you must resist the temptation to place private keys on the jump host - to do so would utterly defeat the point.
Instead, you can use an old, but very nifty trick that completely hides the jump host from your day-to-day usage:
> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">Host jumphost.company.com
>   ProxyCommand none
> Host \*.company.com
>   ProxyCommand ssh jumphost.company.com nc -q0 %h %p</span>

You might wonder what on earth that is doing, but it's really quite simple. The first <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">Host</span> stanza just means we won't use any special commands to connect to the jump host itself. The second <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">Host</span> stanza says that in order to connect to anything ending in <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">.company.com</span> (but excluding <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">jumphost.company.com</span> because it just matched the previous stanza) we will first SSH to the jump host and then use <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">nc(1)</span> (i.e. netcat) to connect to the relevant port (<span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">%p</span>) on the host we originally asked for (<span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">%h</span>). Your local SSH client now has a session open to the jump host which is acting like it's a socket to the SSH port on the host you wanted to talk to, so it just uses that connection to establish an SSH session with the machine you wanted. Simple!
For those of you lucky enough to be connecting to servers that have OpenSSH 5.4 or newer, you can replace the jump host <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">ProxyCommand</span> with:
> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">ProxyCommand ssh -W %h:%p jumphost.company.com</span>

<span class="Apple-style-span" style="font-size: large;">**Re-use existing SSH connections**</span>
Some people swear by this trick, but because I'm very close to my servers and have a decent CPU, the setup time for connections doesn't bother me. Folks who are many milliseconds from their servers, or who don't have unquenchable techno-lust for new workstations, may appreciate saving some time when establishing SSH connections.
The idea is that OpenSSH can place connections into the background automatically, and re-use those existing secure channels when you ask for a new <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">ssh(1)</span>, <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">scp(1)</span> or <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">sftp(1)</span> connections to hosts you have already spoken to. The configuration I would recommend for this, would be:
> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">Host \*
>   ControlMaster auto
>   ControlPath ~/.ssh/control/%h-%l-%p
>   ControlPersist 600</span>

This will do several things:

-   <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">ControlMaster auto</span> will cause OpenSSH to establish the "master" connection sockets as needed, falling back to normal connections if something is wrong.
-   The <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">ControlPath</span> option specifies where the connection sockets will live. Here we are placing them in a directory and giving them filenames that consist of the hostname, login username and port, which ought to be sufficient to uniquely identify each connection. If you need to get more specific, you can place this section near the end of your config and have explicit <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">ControlPath</span> entries in earlier <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">Host</span> stanzas.
-   <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">ControlPersist 600</span> causes the master connections to die if they are idle for 10 minutes. The default is that they live on as long as your network is connected - if you have hundreds of servers this will add up to an awful lot of <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">ssh(1)</span> processes running on your workstation! Depending on your needs, 10 minutes may not be long enough.

***Note:*** You should make the <span class="Apple-style-span" style="font-family: 'Courier New', Courier, monospace;">~/.ssh/control</span> directory ahead of time and ensure that only your user can access it.

**<span class="Apple-style-span" style="font-size: large;">Cope with old/buggy SSH devices</span>**

Perhaps you have a bunch of management devices in your infrastructure and some of them are a few years old already. Should you find yourself trying to SSH to them, you might find that your connections don't work very well. Perhaps your SSH client is too new and is offering algorithms their creaky old SSH servers can't abide. You can strip down the long default list of algorithms to this to ones that a particular device supports, e.g.:
> <span class="Apple-style-span" style="background-color: #ead1dc; font-family: 'Courier New', Courier, monospace;">Host power-device-1.company.com
>   HostkeyAlgorithms ssh-rsa,ssh-dss</span>

<span class="Apple-style-span" style="font-size: large;">**That's all folks**</span>
Those are the most useful tips and tricks I have for now. Hopefully someone will read this and think "hah! I can do ***much*** more advanced stuff than that!" and one-up me :)
Do feel free to comment if you do have something sneaky to add, I'll gladly steal your ideas!

