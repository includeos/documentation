.. _Getting started:

Getting started
===============

.. Punktet etter Install i oversikten (install.rst)
.. Fjerne Bochs? Må oppdateres hvis ikke

Install libraries
~~~~~~~~~~~~~~~~~

**NOTE:** The script will install packages and create a network bridge, and thus will ask for sudo access.

::

	$ git clone https://github.com/hioa-cs/IncludeOS
	$ cd IncludeOS
	$ ./install.sh

**The script will:**

- Install the required dependencies: `curl make clang-3.8 nasm bridge-utils qemu`.
- Download the latest binary release bundle from github.
- Unzip the bundle to `$INCLUDEOS_INSTALL_LOC` (defaults to `$HOME`).
- Create a network bridge called `include0`, for tap-networking.
- Build the vmbuilder, which turns your service into a bootable image.
- Copy `vmbuild` and `qemu-ifup` from the repo, over to `$INCLUDEOS_HOME`.

Detailed installation instructions for Vagrant, Mac OS and Ubuntu are available, as well as instructions for building everything from source.
.. Se under Install.rst (Ubuntu B) Completely build everything from source)

Testing the installation
~~~~~~~~~~~~~~~~~~~~~~~~

A successful setup enables you to build and run a virtual machine. Running:

::

    $ ./test.sh

will build and run `this example service <https://github.com/hioa-cs/IncludeOS/blob/master/examples/demo_service/service.cpp>`__.

More information about testing the example service is available here: :ref:`Testing the example service`.
.. Testing the example service is further down on the page

Writing your first service
~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Copy the `./seed <https://github.com/hioa-cs/IncludeOS/tree/master/seed>`__ directory to a convenient location like `~/your_service`. Then, just start implementing the `Service::start` function in the `Service` class, located in `your_service/service.cpp <https://github.com/hioa-cs/IncludeOS/blob/master/seed/service.cpp>`__ (very simple example provided). This function will be called once the OS is up and running.
2. Enter the name of your service in the first line of the `seed Makefile <https://github.com/hioa-cs/IncludeOS/blob/master/seed/Makefile>`__. This will be the base for the name of the final disk image.

**Example:**

::

    $ cp -r seed ~/my_service
    $ cd ~/my_service
    $ emacs service.cpp
    ... add your code
    $ make
    $ ./run.sh my_service.img

Take a look at the `examples <https://github.com/hioa-cs/IncludeOS/tree/master/examples>`__ and the `tests <https://github.com/hioa-cs/IncludeOS/tree/master/test>`__ on GitHub. These all started out as copies of the same seed.

Helper scripts
~~~~~~~~~~~~~~

There's a convenience script, `./seed/run.sh <https://github.com/hioa-cs/IncludeOS/blob/master/seed/run.sh>`__, which has the "Make-vmbuild-qemu" sequence laid out, with special options for debugging (It will add debugging symbols to the elf-binary and start qemu in debugging mode, ready for connection with ``gdb``. More on this inside the script.). I use this script to run the code, where I'd normally just run the program from a shell. Don't worry, it's fast, even in nested/emulated mode.

Debugging with Bochs
~~~~~~~~~~~~~~~~~~~~

- If you want to debug the bootloader, or inspect memory, registers, flags etc. using a GUI, you need to install `Bochs <http://bochs.sourceforge.net/>`__. This is because ``gdb`` only works for objects with debugging symbols, which we don't have for our bootloader. See `./etc/bochs_installation.sh <https://github.com/hioa-cs/IncludeOS/blob/master/etc/bochs_installation.sh>`__ for build options, and `./etc/.bochsrc <https://github.com/hioa-cs/IncludeOS/blob/master/etc/.bochsrc>`__ for an example configuration file (which specifies a <1MB disk).

.. _Testing the example service:

Testing the example service
---------------------------

.. Bør konverteres til en oppskrift man kan følge (brukerperspektiv)
.. Oppdatere innholdet (VGA support: Jason?)

**Things to note**

- The default test script will only work on Linux, and uses Qemu (with KVM if available). To run IncludeOS directly on VirtualBox, see ``etc/vboxrun.sh``
- There is no shell! IncludeOS is a unikernel, and so it will only run one process. Think of an IncludeOS VM as a local process.
- There is no default VGA! So, nothing will show up on the "screen" if you're using a GUI (i.e. if you run IncludeOS directly in virtualbox). To enable VGA you will need to connect ``OS::set_rsprint_secondary`` to a lambda that calls ``ConsoleVGA::write`` (see: `api/kernel/vga.hpp <https://github.com/hioa-cs/IncludeOS/blob/master/api/kernel/vga.hpp>`__ and the `test service <https://github.com/hioa-cs/IncludeOS/blob/master/test/hw/integration/vga/vga.cpp>`__ for an example), as well as enable graphical output in the emulator, such as qemu. We'll add VGA support in the future, as a package.
- You should be able to ping the VM. Its IP-address will be stated in the boot-time output from IncludeOS.
- You should also be able to open a simple webpage on the VM, by entering the IP into a browser, inside the development machine.
- How to get out? The test script starts `qemu <http://wiki.qemu.org/Main_Page>`__ with the ``--nographics``-option. This will by default reroute stdin and stdout to the terminal. To exit the virtual machine, you can go via the `Qemu monitor <https://en.wikibooks.org/wiki/QEMU/Monitor#Virtual_machine>`__. The command for entering the monitor is ``Ctrl+a c``, or to exit directly, ``Ctrl+a x``.

    + *NOTE*: This keyboard shortcut may not work if you're interacting with your development environment is via a VirtualBox GUI, over putty, inside a ``screen`` etc. If you find a good solution for a certain platform (i.e. putty to VirtualBox on Windows), please let us know so we can update our wiki.
