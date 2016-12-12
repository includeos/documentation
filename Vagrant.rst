.. _Vagrant:

Vagrant
=======

`Vagrant <https://www.vagrantup.com/>`__ is an awesome tool for creating and configuring virtual development environments.

Building with Vagrant
---------------------

You can use :ref:`Vagrant` to set up a virtual machine with the correct environment for building IncludeOS. The following commands will build and install IncludeOS into your home directory (``~/IncludeOS_install/``). The directory is mapped as a shared folder into the virtual machine vagrant creates.

::

        $ git clone https://github.com/hioa-cs/IncludeOS.git
        $ cd IncludeOS
        $ vagrant up
        $ vagrant ssh --command=/IncludeOS/etc/install_from_bundle.sh

You can now log in to the vagrant build environment and build and run a test service like so:

::

        $ vagrant ssh
        $ ./test.sh
