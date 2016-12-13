.. _The boot process:

The boot process
~~~~~~~~~~~~~~~~

.. Vet ikke om denne er 100 % oppdatert
.. Tenk brukervennlighet
.. Liten intro?

1. BIOS loads `bootloader.asm <https://github.com/hioa-cs/IncludeOS/blob/master/src/boot/bootloader.asm>`__, starting at ``_start``.
2. The bootloader sets up segments, switches to 32-bit protected mode, loads the service (an elf-binary ``your_service`` consisting of the OS classes, libraries and your service) from disk.
3. The bootloader hands over control to the OS, by jumping to the ``_start`` symbol inside `kernel\_start.cpp <https://github.com/hioa-cs/IncludeOS/blob/master/src/kernel/kernel_start.cpp>`__.
4. The OS initializes ``.bss``, calls global constructors (``_init``), and then calls ``OS::start`` in `the OS class <https://github.com/hioa-cs/IncludeOS/blob/master/src/kernel/os.cpp>`__.
5. The OS class sets up interrupts, initializes devices, etc. etc.
6. Finally the OS class (still ``OS::start``) calls ``Service::start()``, inside your service, handing over control to you.
