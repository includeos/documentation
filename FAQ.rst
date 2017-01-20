.. _FAQ:

FAQ
===

Why don't we have feature X by default?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

IncludeOS is a `unikernel <https://en.wikipedia.org/wiki/Unikernel>`__ and intended to be the "elastic" part of an elastic cloud service. That means we might want a whole lot of IncludeOS VM's going up and down, and adding any feature X to a vm, that's supposed to scale up to N instances, will add N times the resource cost of X to our service. The idea behind unikernels is that you don't want to pay for lots of such X's by default - but rather that you only pay for the ones you know your service needs.

Also - IncludeOS is new - feature X might very well become available sooner or later.

Why did IncludeOS change from gcc to clang?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Because `libc++ <http://libcxx.llvm.org/>`__ turned out to be much easier to port than `libstdc++ <https://gcc.gnu.org/libstdc++/>`__ - in our experience. And we wouldn't know where to start if we were to build ``libc++`` with gcc.
2. We like the fact that clang can be `used as a library <http://clang.llvm.org/docs/>`__. I suspect we'll be using this to build instrumentation and management tools for IncludeOS.

Why can't we have more than one process?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adding support for several processes requires virtual memory, which comes at an inherent cost: virtual to physical address translation for every memory lookup, plus the cost of context switches whenever a process wants to do system calls (it would require switching protection level and address space). Currently IncludeOS has no virtual memory, and system calls are just normal function calls.

Why no threads?
~~~~~~~~~~~~~~~

Classical threads add overhead to the virtual machine: they need context switching, scheduling and periodic timer interrupts. Particularly with virtual machines there are efficiency-problems stemming from having context switches and synchronization both on the host and inside the guest (see e.g. `[1] <http://ieeexplore.ieee.org/document/7396161/>`__,\ `[2] <https://www.computer.org/csdl/proceedings/cloudcom/2015/9560/00/9560a242.pdf>`__). Concurrency in turn requires everything to be reentrant and thread safe. We know how to do this, and we'll probably add full-blown threading support later, as an optional feature. However, we like how efficient `node.js <https://nodejs.org/en/>`__ / javascript is, with one thread and asynchronous / callback-based I/O, and we want to take this programming model to a mature level first. There are also a lot of interesting alternatives to classical threads that we're currently looking into. In particular we like the upcoming C++ coroutines, and `how you can build Go-like channels on top of them <https://github.com/CppCon/CppCon2016/blob/master/Presentations/Channels%20-%20An%20Alternative%20to%20Callbacks%20and%20Futures/Channels%20-%20An%20Alternative%20to%20Callbacks%20and%20Futures%20-%20John%20Bandela%20-%20CppCon%202016.pdf>`__.

Also note that we do have basic multi-core support. This is a work in progress, but an `example can be found here <https://github.com/hioa-cs/IncludeOS/tree/master/examples/smp>`__.

Will IncludeOS become POSIX-compliant?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We have parts of the `POSIX-interface <http://pubs.opengroup.org/onlinepubs/9699919799/>`__ for free with standard C and C++ interfaces, but especially with regards to I/O - (sockets / file systems) and threads, we'll be working under quite different assumptions. Also, we think C++ offers many advantages over just C, which we want to make use of. We might end up putting a POSIX-layer on top of IncludeOS (e.g. posix sockets on top of our ``TCP::Socket``-class), but that will probably be an optional module. The native API will remain C++, with classes etc. One of our main long term goals is to create a good, high-quality C++ OS interface, eventually employing all the `Wisdom of the C++ Core Guidelines <https://github.com/isocpp/CppCoreGuidelines>`__.

Can we port in high-level languages?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We can - and we might- but the way to do that isn't straight forward, especially since IncludeOS doesn't provide a standard POSIX interface. Also, we believe C++ has become much more flexible and easier to get started with, since C++11/C++14, so for now we want to focus on making good tools for new and experienced C++ developers. Also, we really like good performance.

Will IncludeOS boot on bare metal?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We don't know, but we assume it will on x86. You will however not get networking support unless you contribute a Nic-driver for your network device. Please do!

Will IncludeOS run on ARM?
~~~~~~~~~~~~~~~~~~~~~~~~~~

Not yet, but we're working towards that. Please let us know if you've got experience with ARM architecture and time to spare.
