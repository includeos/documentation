.. _Testing the example service:

Testing the example service
===========================

**Things to note**

- The default test script will only work on Linux, and uses Qemu (with KVM if available). To run IncludeOS directly on VirtualBox, see ``etc/vboxrun.sh``
- There is no shell! IncludeOS is a unikernel, and so it will only run one process. Think of an IncludeOS VM as a local process.
- There is no default VGA! So, nothing will show up on the "screen" if you're using a GUI (i.e. if you run IncludeOS directly in virtualbox). To enable VGA you will need to connect ``OS::set_rsprint_secondary`` to a lambda that calls ``ConsoleVGA::write`` (see: `api/kernel/vga.hpp <https://github.com/hioa-cs/IncludeOS/blob/master/api/kernel/vga.hpp>`__ and the `test service <https://github.com/hioa-cs/IncludeOS/blob/master/test/hw/integration/vga/vga.cpp>`__ for an example), as well as enable graphical output in the emulator, such as qemu. We'll add VGA support in the future, as a package.
- You should be able to ping the VM. Its IP-address will be stated in the boot-time output from IncludeOS.
- You should also be able to open a simple webpage on the VM, by entering the IP into a browser, inside the development machine.
- How to get out? The test script starts `qemu <http://wiki.qemu.org/Main_Page>`__ with the ``--nographics``-option. This will by default reroute stdin and stdout to the terminal. To exit the virtual machine, you can go via the `Qemu monitor <https://en.wikibooks.org/wiki/QEMU/Monitor#Virtual_machine>`__. The command for entering the monitor is ``Ctrl+a c``, or to exit directly, ``Ctrl+a x``.

	+ *NOTE*: This keyboard shortcut may not work if you're interacting with your development environment is via a VirtualBox GUI, over putty, inside a ``screen`` etc. If you find a good solution for a certain platform (i.e. putty to VirtualBox on Windows), please let us know so we can update our wiki.
