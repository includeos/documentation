.. _The x86 boot process:

The x86 boot process
====================

1. When booting from a "hard drive", BIOS loads the first stage bootloader, either grub or `bootloader.asm <https://github.com/hioa-cs/IncludeOS/blob/master/src/platform/x86_pc/boot/bootloader.asm>`__, starting at ``_start``.
2. The bootloader - or Qemu with ``-kernel`` - sets up segments, switches to 32-bit protected mode, loads the service (an elf-binary ``your_service`` consisting of the OS classes, libraries and your service) from disk. For a multiboot compliant boot system (grub or qemu -kernel) the machine is now in the state `specified by multiboot <https://www.gnu.org/software/grub/manual/multiboot/multiboot.html#Machine-state>`__.
3. The bootloader hands over control to the OS, by jumping to the ``_start`` symbol inside `start.asm <https://github.com/hioa-cs/IncludeOS/blob/master/src/platform/x86_pc/start.asm#L61>`__. From there it will call architecture specific initialization and eventually `kernel_start.cpp <https://github.com/hioa-cs/IncludeOS/blob/master/src/platform/x86_pc/kernel_start.cpp>`__. Note that this can be overridden to make custom kernels, such as the minimal `x86_nano <https://github.com/hioa-cs/IncludeOS/blob/master/src/platform/x86_nano>`__ platform used for the chainloader.
4. The OS initializes ``.bss``, calls global constructors, and then calls ``OS::start`` in the `OS class <https://github.com/hioa-cs/IncludeOS/blob/master/api/kernel/os.hpp>`__.
5. The OS class sets up interrupts, initializes devices, plugins, drivers etc. etc.
6. Finally the OS class (still ``OS::start``) calls ``Service::start()`` (as for instance `here <https://github.com/hioa-cs/IncludeOS/blob/master/examples/demo_service/service.cpp>`__) or ``main()`` if you prefer that (such as `here <https://github.com/hioa-cs/IncludeOS/blob/master/examples/syslog/service.cpp>`__), either of which must be provided by your service.
7. Once your service is done initializing, e.g. having indirectly subscribed to certain events like incoming network packets by setting up a HTTP server, the OS resumes the `OS::event_loop() <https://github.com/hioa-cs/IncludeOS/blob/master/src/kernel/os.cpp>`__ which again drives your service.
