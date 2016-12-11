The Build and Boot Process
==========================

The build process
~~~~~~~~~~~~~~~~~

1. Installing IncludeOS means building all the OS components, such as `IRQ manager <https://github.com/hioa-cs/IncludeOS/blob/master/api/kernel/irq_manager.hpp>`__, `PCI manager <https://github.com/hioa-cs/IncludeOS/blob/master/api/kernel/pci_manager.hpp>`__, `PIT-Timers <https://github.com/hioa-cs/IncludeOS/blob/master/api/hw/pit.hpp>`__ and `Device Driver(s) <https://github.com/hioa-cs/IncludeOS/blob/master/api/hw/nic.hpp>`__, combining them into a static library ``os.a`` using GNU ``ar``, and putting it in ``$INCLUDEOS_HOME`` along with all the public os-headers (the "IncludeOS API"). This is what you'll be including parts of, into the service.

2. When the service gets built it will turn into object files, which eventually gets statically linked with the os-library and other libraries. Only the objects actually needed by the service will be linked, turning it all into one minimal elf-binary, ``your_service``, with OS included.

3. The utility `vmbuild <https://github.com/hioa-cs/IncludeOS/tree/master/vmbuild>`__ combines the `bootloader <https://github.com/hioa-cs/IncludeOS/blob/master/src/boot/bootloader.asm>`__ and ``your_service`` binary into a disk image called ``your_service.img``. At this point the bootloader gets the size and location of the service hardcoded into it.

4. Now Qemu can start directly, with that image as hard disk, either directly from the command-line, using our convenience-script, or via libvirt/virsh.

5. To run with virtualbox, the image has to be converted into a supported format, such as ``vdi``. This is easily done in one command with the ``qemu-img``-tool, that comes with Qemu. We have a `script for that too <https://github.com/hioa-cs/IncludeOS/blob/master/etc/convert_image.sh>`__.

Inspect the `Makefile <https://github.com/hioa-cs/IncludeOS/blob/master/src/Makefile>`__ and `linker script, linker.ld <https://github.com/hioa-cs/IncludeOS/blob/master/src/linker.ld>`__ for more information about how the build happens, and `vmbuild/vmbuild.cpp <https://github.com/hioa-cs/IncludeOS/blob/master/vmbuild/vmbuild.cpp>`__ for how the image gets constructed.

The boot process
~~~~~~~~~~~~~~~~

1. BIOS loads `bootloader.asm <https://github.com/hioa-cs/IncludeOS/blob/master/src/boot/bootloader.asm>`__, starting at ``_start``.
2. The bootloader sets up segments, switches to 32-bit protected mode, loads the service (an elf-binary ``your_service`` consisting of the OS classes, libraries and your service) from disk.
3. The bootloader hands over control to the OS, by jumping to the ``_start`` symbol inside `kernel\_start.cpp <https://github.com/hioa-cs/IncludeOS/blob/master/src/kernel/kernel_start.cpp>`__.
4. The OS initializes ``.bss``, calls global constructors (``_init``), and then calls ``OS::start`` in `the OS class <https://github.com/hioa-cs/IncludeOS/blob/master/src/kernel/os.cpp>`__.
5. The OS class sets up interrupts, initializes devices, etc. etc.
6. Finally the OS class (still ``OS::start``) calls ``Service::start()``, inside your service, handing over control to you.
