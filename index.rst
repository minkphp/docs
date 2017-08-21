Welcome to the Mink documentation!
==================================

One of the most important parts in the web is a browser. A browser is the window
through which web users interact with web applications and other users. Users
are always talking with web applications through browsers.

So, in order to test that our web application behaves correctly, we need
a way to simulate this interaction between the browser and the web application
in our tests. We need **Mink**.

Mink is an open source browser controller/emulator for web applications, written
in PHP 5.3.

Read :doc:`/at-a-glance` to learn more about Mink and why you need it.

Installation
------------

Mink is a php 5.3 library that you'll use inside your test suites or project.
Before you begin, ensure that you have at least PHP 5.3.1 installed.

The recommended way to install Mink with all its dependencies is through
`Composer`_:

.. code-block:: bash

    $ composer require behat/mink

.. note::

    For local installations of composer you must call it like this:
    ``$ php composer.phar require behat/mink`` .
    In this case you must use the different call
    ``php composer.phar`` everywhere instead of the simple command ``composer``.

Everything will be installed inside the ``vendor`` folder.
Finally, include the Composer autoloading script to your project:

.. code-block:: php

    require_once 'vendor/autoload.php';

.. note::

    By default, Mink will be installed with no drivers. In order to
    be able to use additional drivers, you should install them (through composer).
    Require the appropriate dependencies:

    - GoutteDriver - ``behat/mink-goutte-driver``
    - Selenium2Driver - ``behat/mink-selenium2-driver``
    - BrowserKitDriver - ``behat/mink-browserkit-driver``
    - ZombieDriver - ``behat/mink-zombie-driver``
    - SeleniumDriver - ``behat/mink-selenium-driver``
    - SahiDriver - ``behat/mink-sahi-driver``
    - WUnitDriver - ``behat/mink-wunit-driver``
    - PhantomJSDriver - ``jcalderonzumba/mink-phantomjs-driver`` (third-party driver)

    If you're a newcomer or just don't know what to choose, you should probably
    start with the GoutteDriver and the Selenium2Driver (you will be able
    to tune it up later):

Guides
------

Learn Mink with the topical guides:

.. toctree::
    :maxdepth: 1

    at-a-glance
    guides/session
    guides/traversing-pages
    guides/manipulating-pages
    guides/interacting-with-pages
    guides/drivers
    guides/third-party-drivers
    guides/managing-sessions
    contributing

Testing Tools Integration
-------------------------

Mink has integrations for several testing tools:

- `Behat`_ through the `Behat MinkExtension`_
- `PHPUnit`_ through the `phpunit-mink package`_

.. _Behat: http://behat.org
.. _Behat MinkExtension: https://github.com/Behat/MinkExtension
.. _Composer: https://getcomposer.org
.. _PHPUnit: http://www.phpunit.de
.. _phpunit-mink package: https://github.com/minkphp/phpunit-mink
