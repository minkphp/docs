BrowserKitDriver
================

BrowserKitDriver provides a bridge for the `Symfony BrowserKit`_ component.
BrowserKit is a browser emulator provided by the `Symfony project`_.

Installation
------------

BrowserKitDriver is a pure PHP library available through Composer:

.. code-block:: bash

    $ composer require behat/mink-browserkit-driver

.. note::

    The BrowserKit component only provides an abstract implementation. The
    actual implementation are provided by other projects, like `Goutte`_
    or the `Symfony HttpKernel`_ component.

    If you are using Goutte, you should use the special :doc:`/drivers/goutte`
    which ensures full compatibility for Goutte due to an edge case in Goutte.

Usage
-----

In order to talk with BrowserKit, you should instantiate a
``Behat\Mink\Driver\BrowserKitDriver``:

.. code-block:: php

    $browserkitClient = // ...

    $driver = new \Behat\Mink\Driver\BrowserKitDriver($browserkitClient);

.. _Goutte: https://github.com/FriendsOfPHP/Goutte
.. _Symfony BrowserKit: http://symfony.com/components/BrowserKit
.. _Symfony HttpKernel: http://symfony.com/components/HttpKernel
.. _Symfony project: http://symfony.com
