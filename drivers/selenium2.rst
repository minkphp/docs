Selenium2Driver
===============

Selenium2Driver provides a bridge for the `Selenium2 (webdriver)`_ tool.
If you just love Selenium2, you can now use it right out of the box too.

Installation
------------

Selenium2Driver is available through Composer:

.. code-block:: bash

    $ composer require behat/mink-selenium2-driver

In order to talk with selenium server, you should install and configure it
first:

1. Download the Selenium Server from the `project website`_.

2. Run the server with the following command (update the version number to
   the one you downloaded):

   .. code-block:: bash

        $ java -jar selenium-server-standalone-2.44.0.jar

.. tip::

    The Selenium2Driver actually relies on the WebDriver protocol defined
    by Selenium2. This means that it is possible to use it with other implementations
    of the protocol. Note however that other implementations may have some
    bugs.

    The testsuite of the driver is run against the `Phantom.js implementation`_
    but it still triggers some failures because of bugs in their implementation.

Usage
-----

That's it, now you can use Selenium2Driver:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\Selenium2Driver('firefox');

.. _Phantom.js implementation: http://phantomjs.org/
.. _project website: http://seleniumhq.org/download/
.. _Selenium2 (webdriver): http://seleniumhq.org/

Integration with BrowserStack
-----------------------------

When you want to perform web testing on multiple browsers and operating systems, it can be quite complicated to maintain machines for each of the target environments. BrowserStack provides "remote web browsers as a service",
making it easy to do this sort of matrix testing without having to maintain the multiple browser installations yourself.

Check out `BrowserStack's documentation`_  for a list of all capabilites. 

Check out `BrowserStack's repository`_ for an example integration with BrowserStack.

.. _BrowserStack's documentation: https://www.browserstack.com/automate/capabilities

.. _BrowserStack's repository: https://github.com/browserstack/behat-mink-browserstack


