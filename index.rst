IncludeOS Documentation
=====================================

**IncludeOS** is an includeable, minimal library operating system for C++ services running in the cloud. Starting a program with ``#include <os>``, will literally include a whole little operating
system into your service during link-time. The build system will link your service and only the necessary OS objects into a single binary, attach a boot loader and combine all that into a self-contained bootable disk image, ready to run on a modern hypervisor. In other words, it's a `Unikernel <https://en.wikipedia.org/wiki/Unikernel>`__ written from
scratch, employing x86 hardware virtualization, with no dependencies except for the virtual hardware.

.. figure:: https://github.com/hioa-cs/IncludeOS/blob/master/doc/figures/IncludeOS_build_system_overview.png
   :alt: Build system overview

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   Roadmap
..   Acknowledgements
..   Contributing-to-IncludeOS
..   Creating-your-first-IncludeOS-service
..   FAQ
..   Features
..   Fun-with-Guns-and-Knives
..   Jenkins-CI
..   OS-X
..   Output-latest-nightly-test
..   Output-latest-test
..   Publications
..   Roadmap
..   Running-tests
..   Setting-up-a-development-environment
..   Testing-the-example-service
..   The-Build-and-Boot-process
..   Ubuntu
..   Vagrant
..   Virtualbox
..   About-test-output

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
