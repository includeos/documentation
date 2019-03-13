.. _Contributing to IncludeOS:

Contributing to IncludeOS
=========================

Clone, edit and send us a pull request on GitHub
------------------------------------------------

IncludeOS is being developed on GitHub. Clone the `repository <https://github.com/hioa-cs/IncludeOS>`__, send us a `pull request <https://help.github.com/articles/using-pull-requests>`__ and `chat with us on Slack <https://goo.gl/NXBVsc>`__.

Send any and all pull requests to the `dev-branch <https://github.com/hioa-cs/IncludeOS/tree/dev>`__. It's ok if it comes from your master branch.

Guidelines
~~~~~~~~~~

**1. Do "one thing" per pull request**

This makes it possible to quickly see and understand what you've done.

**2. License**

IncludeOS is licensed under the APL 2.0.

**3. State what you have done in the commit message**

Avoid general terms like "minor changes". Commit messages should also be short so pt. 1 is important. Commit messages should also indicate which major component the commit makes changes to. Good commit messages have a subject line that starts with the name of the major component that is modified by the commit:

   -  virtionet: Increased buffers for packets
   -  test: Update bufferstore test
   -  crt: Manually realign heap to at least 16 byte boundary

**4. Avoid lots of reformatting along with program changes in the same commit**

If you're making drastic changes to a file, but mostly adding comments, reformatting, cleaning up etc., please do this in a separate commit and mark it as *reformatting* or *documentation*. Otherwise GitHub will light up the whole file and people that mostly/only care about the actual program changes will have a hard time finding them.

**5. Please don't redo the folder-structure**

If you have suggestions for this, just post an `issue <https://github.com/hioa-cs/IncludeOS/issues>`__ explaining the benefits of your suggested structure.

Code formatting
~~~~~~~~~~~~~~~

- Indent using 2 spaces. Don't use tabs.
- Return early, don't use `else` after `return`.

::

    // Do:
    if (condition)
      return 42;
    return -1;

    // Don't:
    if (condition)
      return 42;
    else
      return -1;

- Add :code:`// -*-C++-*-` as the first line of extensionless header files.
- Use the following style for multiline comments:

::

	/**
	 * My very important comment
	 */

- Each class needs a short comment above it to show up in Doxygen generated documentation.

::

	/** Description of class */
	class Logger {
	...
	};

(For single-line comments on classes/interfaces, **don't** use :code:`//` as this does not get picked up by Doxygen.)

- Avoid unnecessary whitespace or decorations

::

	// Do:
	namespace fs {
	  struct File_system;

	  /** Generic structure for directory entries */
	  struct Dirent {

	    /** Constructor */
	    explicit Dirent(File_system* fs, const Enttype t = INVALID_ENTITY, const std::string& n = "",
	                    const uint64_t blk   = 0, const uint64_t pr    = 0,
	                    const uint64_t sz    = 0, const uint32_t attr  = 0,
	                    const uint32_t modt = 0)
	    : fs_ {fs}, ftype {t}, fname_ {n},
	      block_ {blk}, parent_ {pr},
	      size_{sz}, attrib_ {attr},
	      modif {modt}
	    {}

	    Enttype type() const noexcept
	    { return ftype; }

	    const std::string& name() const noexcept
	    { return fname_; }

	    uint64_t block() const noexcept
	    { return block_; }
	  };
	}

	// Don't:
	namespace fs {
	  ///////////////////////////////////////////////////////////////////////////////
	  struct File_system;

	  ///////////////////////////////////////////////////////////////////////////////
	  struct Dirent {
	
	    ///////////////////////////////////////////////////////////////////////////////
	    explicit Dirent(File_system* fs, const Enttype t = INVALID_ENTITY, const std::string& n = "",
	                  const uint64_t blk   = 0, const uint64_t pr    = 0,
	                  const uint64_t sz    = 0, const uint32_t attr  = 0,
	                  const uint32_t modt = 0)
	    : fs_ {fs}, ftype {t}, fname_ {n},
	      block_ {blk}, parent_ {pr},
	      size_{sz}, attrib_ {attr},
	      modif {modt}
	    {}

	    ///////////////////////////////////////////////////////////////////////////////
	    Enttype type() const noexcept
	    { return ftype; }

	    ///////////////////////////////////////////////////////////////////////////////
	    const std::string& name() const noexcept
	    { return fname_; }

	    ///////////////////////////////////////////////////////////////////////////////
	    uint64_t block() const noexcept
	    { return block_; }
	  };
	}

- Use UTF-8 encoding, LF line endings.

- If your editor supports :code:`.editorconfig`, use it.

Issue tracker
-------------

Post any issues not already mentioned, in the `issue tracker on GitHub <https://github.com/hioa-cs/IncludeOS/issues>`__. You can also post questions not answered by editing the :ref:`FAQ` on GitHub.

Gitter chat
-----------

We are usually present in our `public gitter channel <https://gitter.im/hioa-cs/IncludeOS>`__ for any kinds of questions.
