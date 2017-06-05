Title: openssh 5.1 chrootdirectory permissions issue
Date: 2008-10-09
Tags: FOSS Techie Ubuntu

If you're playing with the excellent new *ChrootDirectory* and *internal-sftp* options in recent OpenSSH releases (such as 5.1 which is in Ubuntu 8.10 Intrepid), you may have hit an error like:

```
fatal: bad ownership or modes for chroot directory
```

You may also have searched on Google for what to do about it and come away with very little useful information.
Well no more! I did the same thing and got bored of reading mailing list posts, so read the source code instead. The relevant section is in *session.c*:
        if (stat(component, &st) != 0)
          fatal("%s: stat(\"%s\"): %s", __func__,
              component, strerror(errno));
        if (st.st_uid != 0 || (st.st_mode & 022) != 0)
          fatal("bad ownership or modes for chroot "
              "directory %s\"%s\"",
              cp == NULL ? "" : "component ", component);

This is quite simple really, it's *stat()*ing the directory specified for "*ChrootDirectory*" and **all its parents up to /** and checking that they are:
-   owned by *root*
-   not *group* or *other* writable
-   (it also checks they are actually directories, but I'm going to assume you're not stupid enough to try and chroot into a file ;)

Note my emphesis that these checks apply to the chroot directory itself and its parents and */*, so if you are chrooting users into /srv/chroot/ then you need to ensure that */*, */srv* and */srv/chroot* are owned by root and not writable by the group (even if it's root, bizarrely) or other users.
Sorted.
