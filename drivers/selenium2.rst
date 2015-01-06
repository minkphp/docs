Selenium2Driver
===============

Selenium2Driver provides a bridge for the `Selenium2 (webdriver)`_ tool.
If you just love Selenium2, you can now use it right out of the box too.

In order to talk with selenium server, you should install and configure it
first:

1. Download the Selenium Server from the `project website`_.

2. Run the server with the following command (update the version number to
   the one you downloaded):

   .. code-block:: bash

        $ java -jar selenium-server-standalone-2.44.0.jar

That's it, now you can use Selenium2Driver:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\Selenium2Driver('firefox');

.. _project website: http://seleniumhq.org/download/
.. _Selenium2 (webdriver): http://seleniumhq.org/
