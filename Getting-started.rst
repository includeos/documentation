.. _Getting started:

Getting started
===============

Set custom location and compiler
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

By default the project is installed to /usr/local/includeos.

However, it is recommended to choose a custom location as well as select the compiler we want clang to find.

To do this we can edit ~/.bashrc, adding these lines at the end of the file:

::

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

- Install the required dependencies: :code:`curl make clang-5.0 nasm bridge-utils qemu`.
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

.. Testing the example service is further down on the page

Writing your first service
~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Create a blank directory.
2. Create a minimal ```service.cpp```
3. Running "boot ." will add a CMakeList.txt based on the `./seed <https://github.com/hioa-cs/IncludeOS/tree/master/seed>`__.
4. Update the `CMakeLists.txt <https://github.com/hioa-cs/IncludeOS/blob/master/seed/CMakeLists.txt>`__ to specify the name of your project, enable any needed drivers or plugins, etc.

**Example:**

::

    $ mkdir ~/my_service
    $ cd ~/my_service
    $ emacs service.cpp
    ... add your code
    $ boot .

Take a look at the `examples <https://github.com/hioa-cs/IncludeOS/tree/master/examples>`__ and the `tests <https://github.com/hioa-cs/IncludeOS/tree/master/test>`__ on GitHub. These all started out as copies of the same seed.

Testing the demo service
~~~~~~~~~~~~~~~~~~~~~~~~

A suitable service to test your installation is the Demo Service, found in ```examples/demo_service```. It contains a simplistic web server that will serve out a single, static page. 


::

    $ cd examples/demo_service
    $ boot --create-bridge .

Now the service should be running. In another shell session you can try to ping the service to see if responds.

::
    $ ping -c 3 10.0.0.42
    PING 10.0.0.42 (10.0.0.42): 56 data bytes
    64 bytes from 10.0.0.42: icmp_seq=0 ttl=64 time=0.379 ms
    64 bytes from 10.0.0.42: icmp_seq=1 ttl=64 time=0.370 ms
    64 bytes from 10.0.0.42: icmp_seq=2 ttl=64 time=0.639 ms
    $

Great. The final step is to see if we get a web page from the service. 

::

    $ links -dump http://10.0.0.42/
    IncludeOS  The C++ Unikernel

    You have successfully booted an IncludeOS TCP service with simple http.
    For a more sophisticated example, take a look at Acorn.

