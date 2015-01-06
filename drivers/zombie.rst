ZombieDriver
============

ZombieDriver provides a bridge for the `Zombie.js`_ browser emulator. Zombie.js
is a headless browser emulator, written in node.js. It supports all JS interactions
that Sahi and Selenium do and works almost as fast as Goutte does.
It's best of both worlds, actually, but still limited to only one browser
type (Webkit). Also it's still slower than Goutte and requires node.js and
npm to be installed on the system.

In order to talk with zombie.js server, you should install and configure
zombie.js first:

1. Install node.js by following instructions from the official site:
   `<http://nodejs.org/>`_.

2. Install npm (node package manager) by following instructions from `<http://npmjs.org/>`_.

3. Install zombie.js with npm:

    .. code-block:: bash

        $ npm install -g zombie

After installing npm and zombie.js, you'll need to add npm libs to your ``NODE_PATH``.
The easiest way to do this is to add:

.. code-block:: bash

    export NODE_PATH="/PATH/TO/NPM/node_modules"

into your ``.bashrc``.

After that, you'll be able to just use ZombieDriver without manual server
setup. The driver will do all that for you automatically:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\ZombieDriver(
        new \Behat\Mink\Driver\NodeJS\Server\ZombieServer()
    );

If you want more control during driver initialization, like for example if
you want to configure the driver to init the server on a specific port, use
the more verbose version:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\ZombieDriver(
        new \Behat\Mink\Driver\Zombie\Server($host, $port, $nodeBin, $script)
    );

.. note::

    ``$host`` simply defines the host on which zombie.js will be started. It's
    ``127.0.0.1`` by default.

    ``$port`` defines a zombie.js port. Default one is ``8124``.

    ``$nodeBin`` defines full path to node.js binary. Default one is just ``node``.

    ``$script`` defines a node.js script to start zombie.js server. If you pass
    a ``null`` the default script will be used. Use this option carefully!

.. _Zombie.js: http://zombie.labnotes.org/
