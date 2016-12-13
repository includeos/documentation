.. _Deeper understanding:

Deeper understanding
====================

.. Ha en egen innholdsfortegnelse (hvis blir mye)

.. The build process
.. The boot process
.. Fun with Guns and Knives
.. Jenkins CI

.. Mer fokus på hvordan brukere faktisk bruker IncludeOS
.. Er nå altfor detaljert
.. Ta inn det som er aktuelt om CMake
.. Hvordan kan brukeren komme i gang

.. Ang. The boot process
.. Vet ikke om denne er 100 % oppdatert

.. Ang. Fun with Guns and Knives
.. 

.. --------------------------------- The build process -----------------------------

The build process
~~~~~~~~~~~~~~~~~

The build system will:

- link your service and only the necessary OS objects into a single binary

- attach a boot loader

- combine everything into a self-contained bootable disk image, ready to run on a modern hypervisor.

In other words, it's a `Unikernel <https://en.wikipedia.org/wiki/Unikernel>`__ written from scratch, employing x86 hardware virtualization, with no dependencies except for the virtual hardware.

.. figure:: _static/IncludeOS_build_system_overview.png
    :alt: Build system overview

1. Installing IncludeOS means building all the OS components, such as `IRQ manager <https://github.com/hioa-cs/IncludeOS/blob/master/api/kernel/irq_manager.hpp>`__, `PCI manager <https://github.com/hioa-cs/IncludeOS/blob/master/api/kernel/pci_manager.hpp>`__, `PIT-Timers <https://github.com/hioa-cs/IncludeOS/blob/master/api/hw/pit.hpp>`__ and `Device Driver(s) <https://github.com/hioa-cs/IncludeOS/blob/master/api/hw/nic.hpp>`__, combining them into a static library ``os.a`` using GNU ``ar``, and putting it in ``$INCLUDEOS_HOME`` along with all the public os-headers (the "IncludeOS API"). This is what you'll be including parts of, into the service.

2. When the service gets built it will turn into object files, which eventually gets statically linked with the os-library and other libraries. Only the objects actually needed by the service will be linked, turning it all into one minimal elf-binary, ``your_service``, with OS included.

3. The utility `vmbuild <https://github.com/hioa-cs/IncludeOS/tree/master/vmbuild>`__ combines the `bootloader <https://github.com/hioa-cs/IncludeOS/blob/master/src/boot/bootloader.asm>`__ and ``your_service`` binary into a disk image called ``your_service.img``. At this point the bootloader gets the size and location of the service hardcoded into it.

4. Now Qemu can start directly, with that image as hard disk, either directly from the command-line, using our convenience-script, or via libvirt/virsh.

5. To run with virtualbox, the image has to be converted into a supported format, such as ``vdi``. This is easily done in one command with the ``qemu-img``-tool, that comes with Qemu. We have a `script for that too <https://github.com/hioa-cs/IncludeOS/blob/master/etc/convert_image.sh>`__.

Inspect the `Makefile <https://github.com/hioa-cs/IncludeOS/blob/master/src/Makefile>`__ and `linker script, linker.ld <https://github.com/hioa-cs/IncludeOS/blob/master/src/linker.ld>`__ for more information about how the build happens, and `vmbuild/vmbuild.cpp <https://github.com/hioa-cs/IncludeOS/blob/master/vmbuild/vmbuild.cpp>`__ for how the image gets constructed.

.. --------------------------------- The boot process -------------------------------

The boot process
~~~~~~~~~~~~~~~~

1. BIOS loads `bootloader.asm <https://github.com/hioa-cs/IncludeOS/blob/master/src/boot/bootloader.asm>`__, starting at ``_start``.
2. The bootloader sets up segments, switches to 32-bit protected mode, loads the service (an elf-binary ``your_service`` consisting of the OS classes, libraries and your service) from disk.
3. The bootloader hands over control to the OS, by jumping to the ``_start`` symbol inside `kernel\_start.cpp <https://github.com/hioa-cs/IncludeOS/blob/master/src/kernel/kernel_start.cpp>`__.
4. The OS initializes ``.bss``, calls global constructors (``_init``), and then calls ``OS::start`` in `the OS class <https://github.com/hioa-cs/IncludeOS/blob/master/src/kernel/os.cpp>`__.
5. The OS class sets up interrupts, initializes devices, etc. etc.
6. Finally the OS class (still ``OS::start``) calls ``Service::start()``, inside your service, handing over control to you.

.. --------------------------------- Fun with Guns and Knives -----------------------------

.. _Fun with Guns and Knives:

Fun with Guns and Knives
~~~~~~~~~~~~~~~~~~~~~~~~

.. Skrive litt mer seriøst

What's in an address
--------------------

-  Try writing to that damn well protected address 0! Just remember that you're overwriting a 16-bit heirloom. You have enough powers to dig it up and see that it still works - if you start early enough in the bootloader. (It's only 512 *bytes* - you can read it!)

Make sure nobody can call you, then go to sleep until somebody calls...
-----------------------------------------------------------------------

-  Try to ``asm("cli;hlt")`` in a userspace program - then try it in IncludeOS, directly on KVM/VirtualBox. Explain to the duck exactly what's going on - and he'll tell you why Intel made VT-x (Yes IBM was way behind Alan Turing). That's a virtualization gold nugget in reward of your mischief.

-  Now try it in IncludeOS inside a virtualBox VM. Explain *that* to the duck!

Throw frying pans
-----------------

-  Ever wondered what would happen if you ``throw FryingPan`` inside a (real) interrupt handler - or better yet - a std::Exception inside a CPU Exception handler? (Hey - maybe that way we could ``catch`` division by zero Exception?) You have the means to try!

A new segment
-------------

What if we protect the memory segment of the .rodata of the OS?

Interrupt please!
-----------------

.. ----------------------------------- Jenkins ---------------------------------

.. Oppdatere

Jenkins CI
----------

`Jenkins.includeos.org <https://jenkins.includeos.org>`__
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Getting your personal build to build on the jenkins server
----------------------------------------------------------

If you want to get your personal fork of IncludeOS to build with every commit this procedure will show you what steps to go through.

Things to take note off:

- Will by default build on your dev branch. This will be easier to change at a later date.

- Will look for the repo: ``https://github.com/<github-username>/IncludeOS``

- Does not merge with upstream dev automatically as of this date.

Follow these steps to get it to work:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Go to the settings page for your **personal fork**

|Settings menu|

2. Navigate to the *Webhooks & Services* section and press the *Add webhook* button. Then enter the following url into the *Payload URL* section ``https://jenkins.includeos.org/github-webhook/``. Then press *Add webhook*

|Payload URL|

3. To make sure this works, go back to the webhooks page and make sure you see the green checkmark next to the url. This might take a few seconds, so refresh the page.

|Checkmark|

Then when I create the tests results will be available on `Jenkins.includeos.org <https://jenkins.includeos.org>`__

.. |Settings menu| image:: http://i.imgur.com/wfoYcaD.png
.. |Payload URL| image:: http://i.imgur.com/g0gEcBq.png
.. |Checkmark| image:: http://i.imgur.com/yUTIwZ1.png?1
