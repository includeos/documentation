.. _Install:

Install
=======

.. Not included in index.rst -> Getting started summarizes the most important information (now CMake)
.. Update CMake
.. Remember user perspective
.. The most of this could be removed

.. ---------------------------------- Ubuntu -----------------------------------------

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

.. ------------------------------------ Mac OS (Previously OS X) ----------------------------------------

Building on Mac OS
------------------

You can build IncludeOS (from bundle only) directly on a Mac by running `./etc/install\_osx.sh <https://github.com/hioa-cs/IncludeOS/blob/master/etc/install_osx.sh>`__. The following dependencies are utilized by the script:

- homebrew (Mac OS package manager - https://brew.sh)
- /usr/local directory with write access
- /usr/local/bin added to your PATH
- Xcode CTL (Command Line Tools)

If you wish to quickly test the installation perform the following steps:

::

        $ cd IncludeOS/examples/demo_service
        $ make
        $ ../../etc/vboxrun.sh Demo_Service.img

Developing on Mac OS
--------------------

Building services
~~~~~~~~~~~~~~~~~

To build services and run tests, LD\_INC need to be set:

::

    $ export LD_INC=$INCLUDEOS_INSTALL/bin/ld

Rebuilding IncludeOS
~~~~~~~~~~~~~~~~~~~~

To rebuild IncludeOS, AR\_INC and OBJCOPY\_INC need to be set (in addtion to LD\_INC):

::

    $ export AR_INC=$INCLUDEOS_INSTALL/bin/ar
    $ export OBJCOPY_INC=$INCLUDEOS_INSTALL/bin/objcopy

**NOTE:** ``$INCLUDEOS_INSTALL`` is used internally by the install scripts, and defaults to ``~/IncludeOS_install`` unless specified by you prior to the installation. We don't set the variable system-wide.

.. -------------------------------------- VirtualBox --------------------------------------

Using VirtualBox for development
--------------------------------

- VirtualBox does not support nested virtualization (a `ticket <https://www.virtualbox.org/ticket/4032>`__ has been open for 5 years). This means you can't use the kvm module to run IncludeOS from inside VirtualBox, but you can use Qemu directly, so developing for IncludeOS in a VirtualBox VM works. It will be slower, but a small VM still boots in no time. For this reason, this install script does not require kvm or nested virtualization.

- You might want to install VirtualBox vbox additions, if you want screen scaling. The above provides the prerequisites for this (compiler stuff).

Running IncludeOS images in VirtualBox
--------------------------------------

There are shell scripts provided for converting images into VirtualBox format and running them, but you can also use the VirtualBox GUI.

Use the "New" button in the main toolbar to create a new virtual machine as usual. But before starting your new virtual machine, you have to make some changes to the machine's settings:

Under **General** > **Basic**

Name: Your\_IncludeOS\_Service

Type: Linux

Version: Other Linux (32-bit)

Under **System** > **Motherboard**

Extended Features: *Enable* I/O APIC

Under **Network** > **Adapter 1** > **Advanced**

Adapter Type: Paravirtualized (virtio-net)

It is also very helpful to be able to see the serial output. To redirect it to a file and get useful info:

Under **Serial Ports** > **Port 1**

*Enable* Serial Port

Port Mode: Raw File

Path/Address:
C:\\Users\\\USERNAME\\Desktop\\Serial.txt

(This example assumes you are using Windows. Substitute own user name).

.. ------------------------------------- Vagrant ---------------------------------------

Building with Vagrant
---------------------

`Vagrant <https://www.vagrantup.com/>`__ is an awesome tool for creating and configuring virtual development environments.

You can use Vagrant to set up a virtual machine with the correct environment for building IncludeOS. The following commands will build and install IncludeOS into your home directory (``~/IncludeOS_install/``). The directory is mapped as a shared folder into the virtual machine vagrant creates.

::

        $ git clone https://github.com/hioa-cs/IncludeOS.git
        $ cd IncludeOS
        $ vagrant up
        $ vagrant ssh --command=/IncludeOS/etc/install_from_bundle.sh

You can now log in to the vagrant build environment and build and run a test service like so:

::

        $ vagrant ssh
        $ ./test.sh
