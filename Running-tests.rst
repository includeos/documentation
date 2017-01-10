.. _Running tests:

Running tests
=============

.. More user perspective - what the user should do, not Jenkins
.. Move what's relevant for Jenkins under Deeper-understanding.rst (text about Jenkins there)

In order to run the tests that are created for IncludeOS there are some dependencies.

Dependencies
------------

-  httperf - Used for bombardment of http
-  arping - Used for bombardment of arp
-  python-jsonchema - Python json support
-  dnsmasq - Used for setting up a local dhcp server that answers the dhcp test
-  g++ - Gnu compiler used for running the unit tests

In order to install these packages quickly a puppet script has been created:
`includeos-tools/puppet <https://github.com/includeos/includeos-tools/tree/master/puppet>`__

Running the tests
-----------------

In order to run all the tests issue the following commands:

::

    $ cd IncludeOS/test
    $ ./test.py

This will run the integration tests, unit tests, stress test and some miscellaneous tests.

Running only a certain test category
------------------------------------

If only a certain test category is needed then this can be specified in the following way:

::

    $ ./test.py -t net

The tests are split into folders where the folder structure is as follows:

::

	IncludeOS/test
	+--test_category
	| +--test_type
	| +--test_type
	|
	+--net
	| +--integration
	| +--unit
