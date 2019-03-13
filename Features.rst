.. _Features:

Features
========

.. Needs an update

A non-exhaustive, possibly outdated feature list

- Low memory footprint
- Support for the following hypervisors:
   + Qemu / KVM
   + Solo5/hvt
   + VMWare ESXi
-  **C++11/14 support**
   +  Full C++11/14/17 language support with `clang <http://clang.llvm.org>`__ v5 and later.
   +  Standard C++ library\*\* (STL) `libc++ <http://libcxx.llvm.org>`__ from `LLVM <http://llvm.org/>`__
   +  Exceptions and stack unwinding (currently using `libgcc <https://gcc.gnu.org/onlinedocs/gccint/Libgcc.html>`__)
-  **Standard C library** using `musl <https://www.musl-libc.org//>`__
-  **Virtio Network driver** with DMA. `Virtio <https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=virtio>`__ provides a highly efficient and widely supported I/O virtualization. Like most implementations IncludeOS currently uses "legacy mode", but we're working towards the new `Virtio 1.0 OASIS standard <http://docs.oasis-open.org/virtio/virtio/v1.0/virtio-v1.0.html>`__
-  **A highly modular TCP/IP-stack** written from scratch
   +  TCP with a few extensions (SAck, TSVal)
   +  UDP module
   +  DHCP and DNS clients that (as far as we know) work on the most common cloud platforms
   +  ICMP: Send/receive ping and some error handling code
   +  ARP cache
   +  An IP <-> Link layer/driver separation layer that will allow future link layers, such as WiFi
   +  Minimal beginnings on IPv6 support

-  **Completely silent while idling**. As we documented in our `IEEE CloudCom 2013 paper <http://ieeexplore.ieee.org/xpl/articleDetails.jsp?arnumber=6753801>`__, running a regular interval timer for concurrency inside a virtual machine will impose a significant CPU-load on hypervisors running many virtual machines. IncludeOS disables the timer interrupts completely when idle, making it use no CPU at all. This makes IncludeOS services well suited for resource saving through overbooking schemes.

-  **Node.js-style callback-based programming** - everything happens in one efficient thread with no I/O blocking or unnecessary guest-side context switching.

-  **No race conditions**. Delegated IRQ handling makes race conditions in "userspace" "impossible". ...unless you implement threads yourself (you have the access) or we do.

-  **All the guns and all the knives:**

   +  IncludeOS services run in ring 0, in a single address space without protection. That's a lot of power to play with. For example: Try ``asm("hlt")`` in a normal userspace program - then try it in IncludeOS. Explain to the duck exactly what's going on ... and it will tell you why Intel made VT-x (Yes IBM was way behind Alan Turing). That's a virtualization gold nugget, in reward of your mischief. If you believe in these kinds of lessons, there's always more :ref:`Fun with Guns and Knives`.

   +  *Hold your forces! I and James Gosling strongly object to guns and knives!*

      -  For good advice on how not to use these powers, look to the `Wisdom of the Jedi Council <https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md>`__.
      -  If you found the gold nugget above, you'll know that the physical CPU protects you from others - and others from you. And that's a pretty solid protection compared to, say, `openssl <https://xkcd.com/1354/>`__. If you need protection from yourself, that too can be gained by aquiring the 10 000 lines of `Wisdom from the Jedi Council <https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md>`__, or also from our friends at `Mirage <http://mirage.io>`__ ;-)
      -  *Are the extra guns and knives really features?* For explorers, yes. For a Joint Strike Fighter autopilot? Noooo. You need `even more wisdom <http://www.stroustrup.com/JSF-AV-rules.pdf>`__ for that.

.. Limitations:

If it's not listed under features, chances are that we don't have it yet. 