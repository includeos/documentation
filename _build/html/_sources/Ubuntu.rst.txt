.. _Ubuntu:

Ubuntu
======

Building in Ubuntu
------------------

There are two options to installing IncludeOS. The first is to download the binaries used from github, the second is to build everything from source directly. The first option is considerably faster than the second.

Prerequisites for building IncludeOS VM's in Ubuntu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- **Ubuntu 16.04 or 14.04 LTS, x86\_64**, either on a physical or virtual machine (A virtualbox VM works fine)

  + For the full source build, you'll need at least 1024 MB memory
  + In order to support VGA graphics inside a VM, we recommend a lightweight GUI, such as `lubuntu <https://help.ubuntu.com/community/Lubuntu/GetLubuntu>`__ which runs great inside a virtual machine.

    - *NOTE:* Graphics is by no means necessary, as all IncludeOS output by default will be routed to the serial port, and in Qemu

  + The install scripts may very well work on other flavours on Linux, but we haven't tried. Please let us know if you do.

- You'll need ``git`` to clone from GitHub.

Once you have a system with the prereqs (virtual or not), you can choose a full build from source, or a fast build from binaries.

A) Install libraries from binary bundle (fast)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

        $ sudo apt-get install git
        $ git clone https://github.com/hioa-cs/IncludeOS
        $ cd IncludeOS
        $ ./install.sh

**Time:** About a minute or two (On a 4-core Virtualbox Ubuntu VM, running on a 2015 MacBook Air)

.. _building everything from source:

B) Completely build everything from source (slow)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

        $ sudo apt-get install git
        $ git clone https://github.com/hioa-cs/IncludeOS
        $ cd IncludeOS
        $ ./install.sh --all-source

**The script will:**

- Install all the tools required for building IncludeOS, and all libraries it depends on:

  + ``build-essential make nasm texinfo clang-3.8 cmake ninja-build subversion zlib1g-dev libtinfo-dev``

- Build a GCC cross compiler along the lines of the `osdev howto <http://wiki.osdev.org/GCC_Cross-Compiler>`__ which we really only need to build ``libgcc`` and ``newlib``.

- Build `Redhat's newlib <https://sourceware.org/newlib/>`__ using the cross compiler, and install it according to ``./etc/build_newlib.sh``. The script will also install it to the mentioned location.

- Build a 32-bit version of `LLVM's libc++ <http://libcxx.llvm.org/>`__ tailored for IncludeOS.

- Build and install the IncludeOS library, which your service will be linked with.

- Build and install the ``vmbuild`` tool, which turns your service into a bootable disk image.

**Time:** On a VM with 2 cores and 4 GB RAM, running Ubuntu 14.04, running ./install.sh takes about 33 minutes depending on bandwidth.
