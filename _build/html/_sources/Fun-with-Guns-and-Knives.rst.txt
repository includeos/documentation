.. _Fun with Guns and Knives:

Fun with Guns and Knives
========================

.. Update

What's in an address
--------------------

-  Try writing to that damn well protected address 0! Just remember that you're overwriting a 16-bit heirloom. You have enough powers to dig it up and see that it still works - if you start early enough in the bootloader. (It's only 512 *bytes* - you can read it!)

Make sure nobody can call you, then go to sleep until somebody calls...
-----------------------------------------------------------------------

-  Try to ``asm("cli;hlt")`` in a userspace program - then try it in IncludeOS, directly on KVM/VirtualBox. Explain to the duck exactly what's going on - and he'll tell you why Intel made VT-x (Yes IBM was way behind Alan Turing). That's a virtualization gold nugget in reward of your mischief.

-  Now try it in IncludeOS inside a virtualBox VM. Explain *that* to the duck!

Throw frying pans
-----------------

-  Ever wondered what would happen if you ``throw FryingPan`` inside a (real) interrupt handler - or better yet - a std::Exception inside a CPU Exception handler? (Hey - maybe that way we could ``catch`` division by zero Exception?) You have the means to try!

A new segment
-------------

What if we protect the memory segment of the .rodata of the OS?

Interrupt please!
-----------------
