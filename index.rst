IncludeOS Documentation
=====================================

**IncludeOS** is an includeable, minimal library operating system for C++ services running in the cloud. Starting a program with ``#include <os>``, will literally include a whole little operating
system into your service during link-time. The build system will link your service and only the necessary OS objects into a single binary, attach a boot loader and combine all that into a self-contained bootable disk image, ready to run on a modern hypervisor. In other words, it's a `Unikernel <https://en.wikipedia.org/wiki/Unikernel>`__ written from
scratch, employing x86 hardware virtualization, with no dependencies except for the virtual hardware.

.. figure:: _static/IncludeOS_build_system_overview.png
    :alt: Build system overview

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   The-Build-and-Boot-process
   Features
   Fun-with-Guns-and-Knives
   Roadmap
..   Acknowledgements
..   Contributing-to-IncludeOS
..   Creating-your-first-IncludeOS-service
..   FAQ
..   Jenkins-CI
..   OS-X
..   Output-latest-nightly-test
..   Output-latest-test
..   Publications
..   Running-tests
..   Setting-up-a-development-environment
..   Testing-the-example-service
..   Ubuntu
..   Vagrant
..   Virtualbox
..   About-test-output

Indices and tables
==================

* :ref:`genindex`
* :ref:`search`
