PhantomJSDriver
======================

PhantomJSDriver provides a bridge for the `PhantomJS`_ headless browser, it uses native `APIs`_ to interact with the browser which makes it generally faster than `Selenium2 (webdriver)`_.

If you want to use a headless browser in your automation tests, with PhantomJSDriver you can.

Installation
------------

PhantomJSDriver is available through Composer:

.. code-block:: bash

    $ composer require jcalderonzumba/mink-phantomjs-driver

In order to talk with phantomjs, you must install and configure it
first:

1. Download the phantomjs browser from the `project website`_.

2. Install PhantomJSDriver via Composer.

3. Run the browser with the following command:

   .. code-block:: bash

        $ phantomjs --ssl-protocol=any --ignore-ssl-errors=true vendor/jcalderonzumba/gastonjs/src/Client/main.js 8510 1024 768 2>&1 >> /tmp/gastonjs.log &

.. tip::

    The PhantomJSDriver relies on the `GastonJS`_ project to talk to phantomjs, this means you can do more things that are outside of the scope of the Mink Driver interface.

    The testsuite of the driver is still triggering some failures because of bugs in phantomJS or in GastonJS that need to be fixed.

Usage
------------

That's it, now you can use PhantomJSDriver:

.. code-block:: php

    $driver = new Zumba\Mink\Driver\PhantomJSDriver('http://localhost:8510');

PhantomJSDriver Feature Support
---------------------------------

======================  =================
Feature                 PhantomJSDriver
======================  =================
Page traversing         Yes
Form manipulation       Yes
HTTP Basic auth         Yes
Windows management      Yes
iFrames management      Yes
Request headers access  Yes
Response headers        Yes
Cookie manipulation     Yes
Status code access      Yes
Mouse manipulation      Yes
Drag'n Drop             Yes
Keyboard actions        Yes
Element visibility      Yes
JS evaluation           Yes
Window resizing         Yes
Window maximizing       No
======================  =================

FAQ
---------

1. Is this a selenium based driver?

  **NO**, it has nothing to do with Selenium it's inspired on the `Poltergeist project`_.

2. What features does this driver implements?

  **ALL** of the features defined in Mink DriverInterface. maximizeWindow is the only one not implemented since is a headless browser it does not make sense to implement it.

.. _PhantomJS: http://phantomjs.org/
.. _APIs: http://phantomjs.org/api/webpage/
.. _Selenium2 (webdriver): http://seleniumhq.org/
.. _project website: http://phantomjs.org/download.html
.. _GastonJS: http://gastonjs.readthedocs.io/en/latest/
.. _Poltergeist project: https://github.com/teampoltergeist/poltergeist
