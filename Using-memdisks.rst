.. _Using memdisks:

Using memdisks
==============

If your service needs to include files (configuration/settings, data files, static web content, etc), you can use a memdisk to store your files. A memdisk is an in-memory filesystem that gets baked into your service at build time.

Adding a memdisk
~~~~~~~~~~~~~~~~

In the directory where you're developing your service, add a subdirectory to hold your memdisk's files, and put any files (and folders) your service needs into this subdirectory. You can call the directory whatever you want, in this example I'll just use the name "disk". In your service's ``CMakeLists.txt`` file, add:

::

	diskbuilder(disk disk.img)

When you build your service, CMake will use the diskimagebuilder tool to collect all the files in your "disk" directory into a disk image (comparable to an .iso file), and add this disk image to your service binary.

Using a memdisk
~~~~~~~~~~~~~~~

The most flexible way to use a memdisk is to mount it in the virtual file system (VFS):

::

	// get the root of our memdisk
	auto disk = memdisk()->fs().stat("/");
	// mount it under "/etc"
	fs::mount("/etc", disk, "my memdisk");

(``memdisk()`` is a short helper function, included at the end of this document.)

Now, the files and folders from your memdisk are available using normal C/C++ file functions. Assuming you added a text file called 'hosts' to your memdisk, you can read it the same way you would in a normal C++ program.

::

	std::ifstream is("/etc/hosts");
	std::string line;
	while (std::getline(is, line)) {
	  // process line
	}

Traditional C-style file functions (``fopen()``, ``fgets()``, ``fread()`` and friends) as well as POSIX functions (``open()``, ``read()`` etc.) are also available.

It's also possible to use memdisks without mounting them in the Virtual File Systems and to use the native IncludeOS file system functionality, see `this example <https://github.com/hioa-cs/IncludeOS/tree/master/test/fs/integration/memdisk>`__ for more information.


Addendum: ``memdisk()`` helper function:

::

	fs::Disk_ptr& memdisk() {
	  static auto disk = fs::new_shared_memdisk();
	  if (not disk->fs_ready()) {
	    disk->init_fs([](fs::error_t err) {
	      if (err)
	        panic("error mounting disk"):
	    });
	  }
	  return disk;
	}
