SeleniumDriver
==============

SeleniumDriver provides a bridge for the famous `Selenium`_ tool. If you
need legacy Selenium, you can use it right out of the box in your Mink test
suites.

.. caution::

    The SeleniumRC protocol used by this driver is deprecated and does not
    support all Mink features. For this reason, the SeleniumDriver is deprecated
    in favor of the :doc:`/drivers/selenium2`, which is based on the new
    protocol and is more powerful.

Installation
------------

SeleniumDriver is available through Composer:

.. code-block:: bash

    $ composer require behat/mink-selenium2-driver

In order to talk with the selenium server, you should install and configure
it first:

1. Download the Selenium Server from the `project website`_.

2. Run the server with the following command (update the version number to
   the one you downloaded):

   .. code-block:: bash

        $ java -jar selenium-server-standalone-2.44.0.jar

Usage
-----

That's it, now you can use SeleniumDriver:

.. code-block:: php

    $client = new \Selenium\Client($host, $port);
    $driver = new \Behat\Mink\Driver\SeleniumDriver(
        'firefox', 'base_url', $client
    );

.. _project website: http://seleniumhq.org/download/
.. _Selenium: http://seleniumhq.org/
