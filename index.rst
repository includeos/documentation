IncludeOS
=========

**IncludeOS** is an includeable, minimal library operating system for C++ services running in the cloud. Starting a program with ``#include <os>``, will literally include a whole little operating system into your service during link-time.

The build system will:
- link your service and only the necessary OS objects into a single binary
- attach a boot loader
- combine everything into a self-contained bootable disk image, ready to run on a modern hypervisor.

In other words, it's a `Unikernel <https://en.wikipedia.org/wiki/Unikernel>`__ written from scratch, employing x86 hardware virtualization, with no dependencies except for the virtual hardware.

.. figure:: _static/IncludeOS_build_system_overview.png
    :alt: Build system overview

.. TODO Update url

`Visit our website <http://www.includeos.org>`__

Check out the project on `GitHub <https://github.com/hioa-cs/IncludeOS>`__

Or come chat with us on `Gitter <https://gitter.im/hioa-cs/IncludeOS>`__

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   The-Build-and-Boot-process
   Features
   Fun-with-Guns-and-Knives
   Roadmap
   Contributing-to-IncludeOS
   FAQ
   Getting-started
   Ubuntu
   Vagrant
   OS-X
   Testing-the-example-service
   Setting-up-a-development-environment
   Virtualbox
   Acknowledgements
   Publications
   Jenkins-CI
   Running-tests
..   Output-latest-nightly-test
..   Output-latest-test
..   About-test-output
