.. _Acknowledgements:

Acknowledgements
================

.. Master's students:
.. Update thesis - publications

SanOS
~~~~~

It's hard to create an operating system from scratch - especially the parts where you talk directly to hardware. I don't think I could have done this without `SanOS <http://www.jbox.dk/sanos/>`__, (C) Michael Ringgaard. Especially the `nicely annotated code <http://www.jbox.dk/sanos/source/>`__, with cross-links, was an invaluable resource.

**Note:** We initially planned to just port a lot of stuff from SanOS, and indeed certain pieces of code such as PIC- and PCI-code was included in the IncludeOS repository. However, that code should now all be removed. Partly because we're now trying to make our codebase conform to
the `C++ Core Guidelines <https://github.com/isocpp/CppCoreGuidelines>`__ and partly because the design of IncludeOS is radically different from SanOS, making it harder and harder to get the pieces to fit.

Nevertheless, a big thanks to Michael Ringgaard and SanOS.

Oslo and Akershus University College of Applied Science
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Both the faculty of Technology, Art and Design and the central research administration have provided funding for the project. Tor-Einar Edvardsen and Anne Bjørtuft have been especially helpful and supportive.

We've also received continuous support from the Dept. of Computer Science, which we are lucky is being led by Laurence Habib.

Contributors
~~~~~~~~~~~~

The `GitHub contributors list <https://github.com/hioa-cs/IncludeOS/graphs/contributors>`__ speaks for itself. Alf André Walla (fwsGonzo) probably deserves a medal.

Some contributions are not directly visible in code. The `NETSYS research group <http://www.hioa.no/eng/Forskning-og-utvikling/Hva-forsker-HiOA-paa/Forskning-og-utvikling-ved-Fakultet-for-teknologi-kunst-og-design/node_73129/Nettverks-og-systemadministrasjon-NETSYS>`__ has been supporting IncludeOS form the beginning, with everything from equipment, to system administration, to statistical advice and paper reviews. A very special thanks to these guys:

- Hårek Haugerud
- Kyrre Begnum
- Paal Engelstad
- Aniz Yazidi
- Hugo Hammer

Master's students
~~~~~~~~~~~~~~~~~

There has also been a lot of master students at the NETSYS masters program working on projects related to IncludeOS. We'll try to make a list here, as their thesis take shape.

Others
~~~~~~

Thanks to `Bjarne Stroustrup <http://www.stroustrup.com/>`__ for very encouraging words early on. IncludeOS has a long way to go when it comes to meeting all the `C++ Core Guidelines <https://github.com/isocpp/CppCoreGuidelines>`__, but we're working on getting there.
