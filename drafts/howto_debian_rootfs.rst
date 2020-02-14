.. include:: stylesheets/article-html.txt

=======================================
How to Set Up a Debian RootFS for PRoot
=======================================

:Original Title:  PRoot sorcery
:URL:             http://ivoire.dinauz.org/blog/post/2012/04/16/PRoot-sorcery
:Part of:         http://ivoire.dinauz.org/blog/tag/PRoot
:Author:          Rémi Duraffort
:PRoot Version:   1.8.3

:Abstract:

    A good practice for software developer is to provide a test suite
    while developing a software. When developing for Linux, it's also
    a good practice to compile the software and run the test suite on
    many distributions like Debian, Ubuntu, Fedora, ArchLinux, Centos,
    Slackware and for both i386 and x86_64.

    Usually, softwares are compiled and tested on only one
    distribution because setting up the right environment is long and
    painful. Most of the time root privileges are also required to
    setup such environment.

    In this article and the following one, I will show that using
    PRoot_, such testing is quite handy and can be done by any users.

.. _PRoot: https://proot-me.github.io

Getting PRoot up and running
============================

In order to test PRoot, you can download the latest version on the
`official website`_ and compile it. You can also grab a package for
your distribution on the `Open Build Service`_.

.. _official website: https://proot-me.github.io
.. _Open Build Service: http://software.opensuse.org/download.html?project=home:cedric-vincent&amp;package=proot

If you choose to compile PRoot, that's just a matter of::

    #!/bin/sh
    git clone https://github.com/proot-me/proot.git
    cd proot/src
    make
    [...]
    ./proot


Grabbing a RootFS
=================

The second step to test VLC media player in different distributions is
to get a root file system for every distribution we want to try.

The first and easy way to have a working root file system is to
download it from `OpenVZ repository`_ or `OpenVZ contribs`_.

.. _OpenVZ repository: http://download.openvz.org/template/precreated/
.. _OpenVZ contribs: http://download.openvz.org/template/precreated/contrib/

It's also possible, under Debian and Ubuntu, to create a root file
system using debootstrap, but let's take the easy way for today::

    #!/bin/sh
    % mkdir debian-6.0-x86_64
    % cd debian-6.0-x86_64
    % wget http://download.openvz.org/template/precreated/debian-6.0-x86_64.tar.gz
    [...]
    % tar xf debian-6.0-x86_64.tar.gz
    % cd ..

As we will see later, you can safely ignore the warnings printed by
tar when extracting the file system. Let us note that everything is
run as a normal user.

Now you can "jump" into this new root file system using PRoot::

    #!/bin/sh
    % cat /etc/debian_version
    wheezy/sid
    % proot debian-6.0-x86_64
    ~ cat /etc/debian_version
    6.0.4

For now on, the root file system is the one you just downloaded. For
instance::

     #!/bin/sh
     ~ gcc --version
     gcc (Debian 4.4.5-8) 4.4.5
     ~ logout
     % gcc --version
     gcc (Debian 4.6.3-1) 4.6.3

You just tested the first feature provided by PRoot:

* Changing the root file system of a process

As you may have noticed, I used ``%`` for the host file system shell
prompt (a Debian Sid) and ``~`` for the PRooted one (a Debian
Squeeze).


Setting up the new RootFS
=========================

We now have a basic and working root file systems but some
configuration has to be done before any real usages:

* Adding the right users and groups to /etc/passwd and /etc/group
* Configuring DNS resolution
* Binding some special directories
* Updating the system

Normally, all this tasks can only be done by root as they will
modifies files owned by root. As we extracted the archive as a normal
user, the current user can modify any files in the root file system
though making root privileges pointless.

Adding some user and groups
---------------------------

In order to keep the same user inside the PRooted file system, you
just have to copy the right lines from ``/etc/passwd`` to the
corresponding file in the PRooted file system. You can do the same
thing for groups in ``/etc/group``.

Configuring DNS resolution
--------------------------

Just copy ``/etc/resolv.conf`` from the host root file system to the
PRooted one. This way the same mechanism will be used in the host
system and in the PRooted one.

Another solution is to ask PRoot to do the job for you: binding
/etc/resolv.conf to the same file in the PRooted file system. Adding
``-b /etc/resolv.conf`` to the PRoot command line will do the trick::

    #!/bin/sh
    % proot debian-6.0-x86_66
    ~ cat /etc/resolv.conf
    cat: /etc/resolv.conf: No such file or directory
    ~ logout
    % proot -b /etc/resolv.conf debian-6.0-x86_64
    ~ cat /etc/resolv.conf
    [...]same file as /etc/resolv.conf on the host[...]

You just tested the second feature provided by PRoot:

* Binding some files to another location in the file system.

In this case you bound /etc/resolv.conf to the same location inside
the PRooted environment. It's also possible to bind it somewhere else
with: ``-b /etc/resolv.conf:/Somewere_else_in_the_PRooted_FS``.

Binding some special directories
--------------------------------

For the moment, the ``/dev`` and ``/proc`` directories are
empty. However some programs need it in order to work correctly. For
example ssh-keygen will refuse to work without ``/dev/random``.

We should bind the real /dev and /proc in the new root file
system. Adding ``-b /dev -b /proc`` to the PRoot command line will
solve this issue.

Updating the system
-------------------

We already noticed that the current user can modify any file on the
PRooted file system because it was extracted by this user.

However most tools like dpkg required the current user id to be root
in order to work. For this reason, PRoot can be launched with the
``-0`` (zero) option which fake some syscalls and makes the programs
think the current user is root::

    #!/bin/sh
    % proot -b /etc/resolv.conf -0 debian-6.0-x86_64
    ~ id -a
    uid=0(root) gid=0(root) groupes=0(root)
    ~ cat /etc/apt/source.list
    deb http://ftp.debian.org/debian squeeze main contrib non-free
    deb http://security.debian.org squeeze/updates main contrib non-free
    ~ apt-get update
    Hit http://ftp.debian.org squeeze Release.gpg
    Ign http://ftp.debian.org/debian/ squeeze/contrib Translation-en
    [...]
    Reading package lists... Done
    ~ apt-get upgrade
    [...]

You can manage this root file system like a classical one. Pay
attention that some services that really required root privileges to
work (like apache or some daemons) could not run correctly under PRoot
as we only fake root privileges.

Future work
===========

This article is beginning to be really long so I will finish here for
today.

We saw a simple way to get a working Debian root file systems that we
can manage without real root privileges. This work will be useful for
the next article which will cover the compilation and testing of VLC
media player in this new root file system.
