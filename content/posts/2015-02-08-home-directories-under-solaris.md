---
title: "Home directories under solaris"
date: 2015-02-08
draft: false
slug: "home-directories-under-solaris"
categories:
  - ""
---

Under Solaris, home directories are conventionally kept on one of two places,/homeor/export/home. The/homedirectory isunder control of the automounter and only the automounter can create directories there. The/export/homedirectory is where users home directories can be created by the system administrator.

To create an account for user 'oops' where the UID is 400 and users home directory is not automounted:

    useradd -u 400 -g user -c useroops -m -d /export/home/oops oops
    passwd oops(To set the password)
    chown oops /export/home/oops
    chgrp user /export/home/oops

To create an account for 'oops' where the home directory is automounted andthe mount point for/homeis/etc/auto_home:

    useradd -u 400 -g user -c useroops oops
    passwd oops(To set the password)
    mkdir /export/home/oops
    chown oops /export/home/oops
    chgrp user /export/home/oops
    
    vi /etc/auto_homeand add the line:

    oops remotehost:/home/&

This sets the user oops' home directory to/home/oopsexported from a remote host.

From : [Unix Workstation Administration Tasks](http://ibgwww.colorado.edu/~lessem/psyc5112/usail/tasks/toc.html)
