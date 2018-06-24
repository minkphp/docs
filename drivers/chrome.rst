ChromeDriver
===============

ChromeDriver allows Mink to control Chrome without the overhead of Selenium.

It communicates directly with chrome over HTTP and WebSockets, which allows it to work at least twice as fast as Chrome with Selenium.

For Chrome 59+ it supports headless mode, eliminating the need to install a display server, and the overhead that comes with it.

Installation
------------

ChromeDriver is available through Composer:

.. code-block:: bash

    $ composer require dmore/chrome-mink-driver

Usage
-----

Run Chromium or Google Chrome with remote debugging enabled:

   .. code-block:: bash

        $ google-chrome-stable --remote-debugging-address=0.0.0.0 --remote-debugging-port=9222

or headless (59+):

   .. code-block:: bash

        $ google-chrome-stable --disable-gpu --headless --remote-debugging-address=0.0.0.0 --remote-debugging-port=9222

Configure Mink to use ChromeDriver:

.. code-block:: php

    use Behat\Mink\Mink;
    use Behat\Mink\Session;
    use DMore\ChromeDriver\ChromeDriver;

    $mink = new Mink(array(
        'browser' => new Session(new ChromeDriver('http://localhost:9222', null, 'http://www.google.com'))
    ));

That's it!

For more details, see `the official documentation`_

.. _the official documentation: https://gitlab.com/DMore/chrome-mink-driver/blob/master/README.md
