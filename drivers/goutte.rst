GoutteDriver
============

GoutteDriver provides a bridge for the `Goutte`_ headless browser. Goutte
is a classical pure-php headless browser, written by the creator of the Symfony
framework Fabien Potencier.

Installation
------------

GoutteDriver is a pure PHP library available through Composer:

.. code-block:: bash

    $ composer require behat/mink-goutte-driver

.. note::

    GoutteDriver is compatible with both Goutte 1.x which relies on `Guzzle 3`_
    and Goutte 2.x which relies on `Guzzle 4+`_ for the underlying HTTP implementation.

    Composer will probably select Goutte 2.x by default.

Usage
-----

In order to talk with Goutte, you should instantiate a
``Behat\Mink\Driver\GoutteDriver``:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\GoutteDriver();

Also, if you want to configure Goutte more precisely, you could do the full
setup by hand:

.. code-block:: php

    $client = new \Goutte\Client();
    // Do more configuration for the Goutte client

    $driver = new \Behat\Mink\Driver\GoutteDriver($client);

.. _Goutte: https://github.com/FriendsOfPHP/Goutte
.. _Guzzle 3: http://guzzle3.readthedocs.org/en/latest/
.. _Guzzle 4+: http://docs.guzzlephp.org/en/latest/
