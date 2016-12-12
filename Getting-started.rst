Getting started
===============

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

Detailed installation instructions for :ref:`Vagrant`, :ref:`OS X` and :ref:`Ubuntu` are available, as well as instructions for building everything from source: :ref:`building everything from source`.

Testing the installation
~~~~~~~~~~~~~~~~~~~~~~~~

A successful setup enables you to build and run a virtual machine. Running:

::

    $ ./test.sh

will build and run `this example service <https://github.com/hioa-cs/IncludeOS/blob/master/examples/demo_service/service.cpp>`__.

More information about testing the example service is available here: :ref:`Testing the example service`.

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
