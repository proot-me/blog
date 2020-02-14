.. include:: stylesheets/article-html.txt

========================================
How to migrate from Scratchbox2 to PRoot
========================================

:Author:         Cédric Vincent
:PRoot Version:  3.1

:Abstract:

    PRoot is designed with simplicity and consistency in mind, that's
    why most of the Scratchbox2's concepts can be simulated in PRoot
    with bindings and a wrapper only.  Obviously the comparison
    between Scratchbox2 and PRoot is biased as I'm the maintainer of
    PRoot.


Target
======

In Scratchbox2, "target" is a name for a configuration that specifies
the path to the guest rootfs and the path to the cross-compiler.

PRoot doesn't consider a cross-compiler like a special tool, thus the
user is free to execute it just like any other host programs, either
through the /host-rootfs binding (default when using QEMU):

    proot -q qemu-arm -r rootfs [...]
    $ /host-rootfs/opt/cross-tools/arm-linux-gcc [...]

or through a user-specified binding:

     proot -b /opt/cross-tools/arm-linux-gcc:/usr/bin/gcc -q qemu-arm -r rootfs [...]
     $ /usr/bin/gcc

Also, PRoot has no configuration support on purpose, this lets the
user free to forge the command-line with any programming languages
(shell, Perl, Python, ...).  To me, this is more powerful than any
configuration syntax.


Execution Policy
================

In Scratchbox2, the view of the virtual file-system is automatically
altered according to the executed program.  Here is a quote from
"Scratchbox2: Internals and Architecture":

    For example, /usr/lib/perl must be mapped to the target root when
    target's perl is running, but to the corresponding place in the
    tools directory [ed: where host tools are] when the other perl is
    running.

In PRoot, all processes see exactly the same virtual file-system,
there's *no exception* at all.  For example, to use the host Perl the
user can to adjust PERL5LIB explicitly or bind the host instance over
the guest one:

    proot -b /usr/bin/perl $bind_perl_modules [...]

where $bind_perl_modules is the output of the following command:

    perl -e 'print map { "-b $_ " } grep { m#^/# } @INC'


Modes
=====

Scratchbox2 comes with 3 predefined mapping rule sets ("emulate",
"simple" and "accel") that define how programs are executed (emulated
or natively) and what are the default bindings.

PRoot has 2 predefined binding sets: "none" ("-r" option) or
"recommended" ("-R" option).  Regarding how a program is executed
(emulated or natively), PRoot detects automatically if the binary is
made for the host architecture or not, wherever it lies on the virtual
file-system.


Sessions
========

In Scratchbox2, a "session" is a "target" instance where the rules
from a given "mode" (a.k.a mapping rule sets) are applied.  Also, the
/tmp directory is "private" to each session.

About the "target" and "mode" support in PRoot, please refer to the
sections above.  About "private" directories, the user can simulate
this feature by binding dedicated directories or files over "private"
ones::

    proot -b ~/tmp-session-01:/tmp [...]


Network Namespace
=================

Scratchbox2 can translate network addresses.  This feature doesn't
exist in PRoot but can be implemented easily, as an extension for
instance.


Permission Namespace
====================

Scratchbox2 (v2.4) has a permission engine that allows one to fake
privileged operations, this engine is even more powerful than
fakeroot's one.

PRoot has a permission engine ("-0" option) that I want to keep as
small as possible, its only purpose is to cheat package managers that
does some [useless] sanity checks.  Note that fakeroot can also be
used under PRoot.

