.. _Getting started:

Getting started
===============

Set custom location and compiler
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default the project is installed to /usr/local/includeos.

However, it is recommended to choose a custom location as well as select the compiler we want clang to find.

To do this we can edit ~/.bashrc (in the home folder), adding these lines at the end of the file:

::

    export CC=/usr/bin/clang-3.8
    export CXX=/usr/bin/clang++-3.8
    export INCLUDEOS_PREFIX=<HOME FOLDER>/includeos
    export PATH=$PATH:$INCLUDEOS_PREFIX/bin

This will also crucially make the boot program visible globally, so that you can simply run :code:`boot <myservice>` inside any service folder.

Install libraries
~~~~~~~~~~~~~~~~~

**NOTE:** The script will install packages and create a network bridge.

::

	$ git clone https://github.com/hioa-cs/IncludeOS
	$ cd IncludeOS
	$ ./install.sh

**The script will:**

- Install the required dependencies: :code:`curl make clang-3.8 nasm bridge-utils qemu`.
- Create a network bridge called :code:`bridge43`, for tap-networking.
- Build IncludeOS with CMake:
    + Download the latest binary release bundle from github together with the required git submodules.
    + Unzip the bundle to the current build directory.
    + Build several tools used with IncludeOS, including vmbuilder, which turns your service into a bootable image.
    + Install everything in :code:`$INCLUDEOS_PREFIX/includeos` (defaults to :code:`/usr/local`).

Configuration of your IncludeOS installation can be done inside :code:`build/` with :code:`ccmake ..`.

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

1. Copy the `./seed <https://github.com/hioa-cs/IncludeOS/tree/master/seed>`__ directory to a convenient location like :code:`~/your_service`. Then, just start implementing the :code:`Service::start` function in the :code:`Service` class, located in `your_service/service.cpp <https://github.com/hioa-cs/IncludeOS/blob/master/seed/service.cpp>`__ (very simple example provided). This function will be called once the OS is up and running.
2. Update the `CMakeLists.txt <https://github.com/hioa-cs/IncludeOS/blob/master/seed/CMakeLists.txt>`__ to specify the name of your project, enable any needed drivers or plugins, etc.

**Example:**

::

    $ cp -r seed ~/my_service
    $ cd ~/my_service
    $ emacs service.cpp
    ... add your code
    $ mkdir build && cd build
    $ cmake ..
    $ make
    $ boot my_service
    or
    $ ../run.sh my_service

Take a look at the `examples <https://github.com/hioa-cs/IncludeOS/tree/master/examples>`__ and the `tests <https://github.com/hioa-cs/IncludeOS/tree/master/test>`__ on GitHub. These all started out as copies of the same seed.

.. _Testing the example service:

Testing the example service
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Should be converted to a recipe (user perspective)
.. Update the content (VGA support: Jason Turner)

**Things to note**

- The default test script will only work on Linux, and uses Qemu (with KVM if available). To run IncludeOS directly on VirtualBox, see ``etc/vboxrun.sh``
- There is no shell! IncludeOS is a unikernel, and so it will only run one process. Think of an IncludeOS VM as a local process.
- There is no default VGA! So, nothing will show up on the "screen" if you're using a GUI (i.e. if you run IncludeOS directly in virtualbox). To enable VGA you will need to connect ``OS::set_rsprint_secondary`` to a lambda that calls ``ConsoleVGA::write`` (see: `api/kernel/vga.hpp <https://github.com/hioa-cs/IncludeOS/blob/master/api/kernel/vga.hpp>`__ and the `test service <https://github.com/hioa-cs/IncludeOS/blob/master/test/hw/integration/vga/vga.cpp>`__ for an example), as well as enable graphical output in the emulator, such as qemu. We'll add VGA support in the future, as a package.
- You should be able to ping the VM. Its IP-address will be stated in the boot-time output from IncludeOS.
- You should also be able to open a simple webpage on the VM, by entering the IP into a browser, inside the development machine.
- How to get out? The test script starts `qemu <http://wiki.qemu.org/Main_Page>`__ with the ``--nographics``-option. This will by default reroute stdin and stdout to the terminal. To exit the virtual machine, you can go via the `Qemu monitor <https://en.wikibooks.org/wiki/QEMU/Monitor#Virtual_machine>`__. The command for entering the monitor is ``Ctrl+a c``, or to exit directly, ``Ctrl+a x``.

    + *NOTE*: This keyboard shortcut may not work if you're interacting with your development environment via a VirtualBox GUI, over putty, inside a ``screen`` etc. If you find a good solution for a certain platform (i.e. putty to VirtualBox on Windows), please let us know so we can update the documentation.
