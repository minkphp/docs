SahiDriver
==========

SahiDriver provides a bridge for the `Sahi`_ browser controller. Sahi is
a new JS browser controller, that fast replaced old Selenium testing suite.
It's both easier to setup and to use than classical Selenium. It has a GUI
installer for each popular operating system out there and is able to control
every systems browser through a special bundled proxy server.

In order to talk with a real browser through Sahi, you should install and
configure Sahi first:

1. Download and run the Sahi jar from the `Sahi project website`_ and run
   it. It will run the installer, which will guide you through the installation
   process.

2. Run Sahi proxy before your test suites (you can start this proxy during
   system startup):

    .. code-block:: bash

        cd $YOUR_PATH_TO_SAHI/bin
        ./sahi.sh

After installing Sahi and running the Sahi proxy server, you will be able
to control it with ``Behat\Mink\Driver\SahiDriver``:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\SahiDriver('firefox');

.. note::

    Notice, that first argument of ``SahiDriver`` is always a browser name,
    `supported by Sahi`_.

If you want more control during the driver initialization, like for example
if you want to configure the driver to talk with a proxy on another machine,
use the more verbose version with a second client argument:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\SahiDriver(
        'firefox',
        new \Behat\SahiClient\Client(
            new \Behat\SahiClient\Connection($sid, $host, $port)
        )
    );

.. note::

    ``$sid`` is a Sahi session ID. It's a unique string, used by the driver
    and the Sahi proxy in order to be able to talk with each other. You should
    fill this with ``null`` if you want Sahi to start your browser automatically
    or with some unique string if you want to control an already started
    browser.

    ``$host`` simply defines the host on which Sahi is started. It is ``localhost``
    by default.

    ``$port`` defines a Sahi proxy port. The default one is ``9999``.

.. _Sahi: http://sahi.co.in/w/
.. _Sahi project website: http://sourceforge.net/projects/sahi/files/
.. _supported by Sahi: http://sahi.co.in/w/browser-types-xml
