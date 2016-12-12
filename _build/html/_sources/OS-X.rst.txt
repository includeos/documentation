.. _OS X:

OS X
====

Building on Mac OS X
--------------------

You can build IncludeOS (from bundle only) directly on a Mac by running `./etc/install\_osx.sh <https://github.com/hioa-cs/IncludeOS/blob/master/etc/install_osx.sh>`__. The following dependencies are utilized by the script:

- homebrew (OS X package manager - https://brew.sh)
- /usr/local directory with write access
- /usr/local/bin added to your PATH
- Xcode CTL (Command Line Tools)

If you wish to quickly test the installation perform the following steps:

::

        $ cd IncludeOS/examples/demo_service
        $ make
        $ ../../etc/vboxrun.sh Demo_Service.img

Developing on Mac OS X
----------------------

Building services
'''''''''''''''''

To build services and run tests, LD\_INC need to be set:

::

    $ export LD_INC=$INCLUDEOS_INSTALL/bin/ld

Rebuilding IncludeOS
''''''''''''''''''''

To rebuild IncludeOS, AR\_INC and OBJCOPY\_INC need to be set (in addtion to LD\_INC):

::

    $ export AR_INC=$INCLUDEOS_INSTALL/bin/ar
    $ export OBJCOPY_INC=$INCLUDEOS_INSTALL/bin/objcopy

**NOTE:** ``$INCLUDEOS_INSTALL`` is used internally by the install scripts, and defaults to ``~/IncludeOS_install`` unless specified by you prior to the installation. We don't set the variable system-wide.
