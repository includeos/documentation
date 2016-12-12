VirtualBox
==========

Using VirtualBox for development
--------------------------------

- VirtualBox does not support nested virtualization (a `ticket <https://www.virtualbox.org/ticket/4032>`__ has been open for 5 years). This means you can't use the kvm module to run IncludeOS from inside VirtualBox, but you can use Qemu directly, so developing for IncludeOS in a VirtualBox VM works. It will be slower, but a small VM still boots in no time. For this reason, this install script does not require kvm or nested virtualization.

- You might want to install VirtualBox vbox additions, if you want screen scaling. The above provides the prerequisites for this (compiler stuff).

Running IncludeOS images in VirtualBox
--------------------------------------

There are shell scripts provided for converting images into VirtualBox format and running them, but you can also use the VirtualBox GUI.

Use the "New" button in the main toolbar to create a new virtual machine as usual. But before starting your new virtual machine, you have to make some changes to the machine's settings:

Under **General** > **Basic**

Name: Your\_IncludeOS\_Service

Type: Linux

Version: Other Linux (32-bit)

Under **System** > **Motherboard**

Extended Features: *Enable* I/O APIC

Under **Network** > **Adapter 1** > **Advanced**

Adapter Type: Paravirtualized (virtio-net)

It is also very helpful to be able to see the serial output. To redirect it to a file and get useful info:

Under **Serial Ports** > **Port 1**

*Enable* Serial Port

Port Mode: Raw File

Path/Address:
C:\\Users\\\USERNAME\\Desktop\\Serial.txt

(This example assumes you are using Windows. Substitute own user name).
