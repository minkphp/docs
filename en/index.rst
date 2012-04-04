Web acceptance testing
======================

One of the most important parts in the web is a browser. Browser is the window,
through which web users interact with web applications and other users. Users
are always talks with web applications through browsers.

So, in order to test, that our web application behaves correctly, we need a way
to simulate this interaction between browser and web application in our tests.
We need a **Mink**.

Mink is an open source acceptance test framework for web applications, written
in PHP 5.3.

Installation
------------

Mink is a php 5.3 library that you'll use inside your test suites or project.
Before you begin, ensure that you have at least PHP 5.3.1 installed.

Method #1 (Composer)
~~~~~~~~~~~~~~~~~~~~

The simplest way to install Mink with all its dependencies is through Composer:

Create ``composer.json`` file in the project root:

.. code-block:: js

    {
        "require": {
            "behat/mink": ">=1.3.2"
        },

        "repositories": {
            "behat/mink-deps": { 
                "type": "composer",
                "url": "behat.org"
            }
        }
    }

Then download ``composer.phar`` and run ``install`` command:

.. code-block:: bash

    $ wget -nc http://getcomposer.org/composer.phar
    $ php composer.phar install

Everything will be installed inside ``vendor`` folder.
Finally, include Composer autoloading script to your project:

.. code-block:: php

    require_once 'vendor/.composer/autoload.php';

.. note::

    By default, Mink will be configured with only zombie.js driver. In order to
    be able to use additional drivers, you should install (through composer) their 
    dependencies. Add apropriate dependencies to your ``composer.json`` ``require``
    block:

    - GoutteDriver - ``"fabpot/goutte": "*"``
    - SeleniumDriver - ``"alexandresalome/php-selenium": "*"``
    - WebDriver - ``"facebook/php-webdriver": "*"``
    - SahiDriver - ``"behat/sahi-client": "*"``

    In order to install Mink with all available drivers, use this ``composer.json``:

    .. code-block:: js

        {
            "require": {
                "behat/mink":  "*",
                "fabpot/goutte": "*",
                "alexandresalome/php-selenium": "*",
                "facebook/php-webdriver": "*",
                "behat/sahi-client": "*"
            },

            "repositories": {
                "behat/mink-deps": {
                    "type": "composer",
                    "url":  "behat.org"
                }
            }
        }


Method #2 (PEAR)
~~~~~~~~~~~~~~~~

Another way to install Mink is through PEAR:

.. code-block:: bash

    $ pear channel-discover pear.symfony.com
    $ pear channel-discover pear.behat.org
    $ pear install behat/mink

Now, you can use Mink in your projects simply by including it:

.. code-block:: php

    require_once 'mink/autoload.php';

Method #3 (PHAR)
~~~~~~~~~~~~~~~~

Also, you can use mink phar package:

.. code-block:: bash

    $ wget https://github.com/downloads/Behat/Mink/mink.phar

Now you can require phar package in your project:

.. code-block:: php

    require_once 'mink.phar';

Method #4 (Git)
~~~~~~~~~~~~~~~

You can also clone the Mink with Git by running:

.. code-block:: bash

    $ git clone git://github.com/Behat/Mink.git && cd Mink

Then download ``composer.phar`` and run ``install`` command:

.. code-block:: bash

    $ wget -nc http://getcomposer.org/composer.phar
    $ php composer.phar install

Now, you can use Mink in your projects simply by including it:

.. code-block:: php

    require_once '/path/to/Mink/vendor/.composer/autoload.php';

Understanding the Mink
----------------------

There's huge amount of browser emulators out there, like
`Goutte <https://github.com/fabpot/goutte>`_, `Selenium <http://seleniumhq.org/>`_,
`Sahi <http://sahi.co.in/w/>`_ and others. They all do the same job, but do it
very differently. They behave differently and have very different API's. But,
what's more important - there's actually 2 completely different types of
browser emulators out there:

* Headless browser emulators
* Browser controllers

First type browsers are simple pure HTTP specification implementations, like
`Goutte <https://github.com/fabpot/goutte>`_. Those browser emulators send
a real HTTP requests against an application and parse the response content. They
are very simple to run and configure, because this type of emulators can be
written in any available programming language and can be run through console on
servers without GUI. Headless emulators have both, advantages and disadvantages.
Advantages are simplicity, speed and ability to run it without the need in real
browser. But this type of browsers have one big disadvantage - they have no
JS/AJAX support. So, you can't test your rich GUI web applications with
headless browsers.

Second browser emulators type are browser controllers. Those emulators
aims to control the real browser. That's right, a program to control another
program. Browser controllers simulate user interactions on browser and are able
to retrieve actual information from current browser page. `Selenium <http://seleniumhq.org/>`_
and `Sahi <http://sahi.co.in/w/>`_ are two most famous browser controllers.
The main advantage of browser controllers usage is the support for JS/AJAX
interactions on page. Disadvantage is that such browser emulators require
installed browser, extra configuration are usually much slower than headless
counterparts.

So, the easy answer is to choose the best emulator for your project and use
its API for testing. But as we've already seen, both browser types have both,
advantages and disadvantages. If you choose headless browser emulator - you'll
not be able to test your JS/AJAX pages. And if you choose browser controller -
your overall test suite will become very slow at some point. So, in real world
we should use both! And that's why you need a **Mink**.

**Mink** removes API differences between different browser emulators providing
different drivers (read in "`Different Browsers - Drivers`_" chapter) for every
browser emulator and providing you with the easy way to control the browser
("`Control the Browser - Session`_"), traverse pages ("`Traverse the Page - Selectors`_")
or manipulate page elements ("`Manipulate the Page - NodeElement`_").

Different Browsers - Drivers
----------------------------

How does Mink provide a consistent API for very different browser library types, often
written in different languages? Through drivers! Mink driver is a simple class,
that implements ``Behat\Mink\Driver\DriverInterface``. This interface describes
bridge methods between Mink and real browser emulators. Mink always talks with
browser emulators through its driver - it doesn't know anything about how to
start/stop or traverse page in that particular browser emulator - it only knows
what driver method it should call in order to do this.

Mink v1.2 comes with four drivers out of the box:

* ``GoutteDriver`` - provides a bridge for `Goutte <https://github.com/fabpot/goutte>`_
  headless browser. Goutte is a classical pure-php headless browser, written by
  the creator of the Symfony framework - Fabien Potencier.

* ``SahiDriver`` - provides a bridge for `Sahi <http://sahi.co.in/w/>`_ browser
  controller. Sahi is a new JS browser controller, that fastly replaced old
  Selenium testing suite. It's both, easier to setup and to use than classical
  Selenium. It has a GUI installer for each popular operating system out there
  and is able to control every systems browser through a special bundled proxy
  server.

.. versionadded:: 1.1

* ``ZombieDriver`` - provides a bridge for `Zombie.js <http://zombie.labnotes.org/>`_
  browser emulator. Zombie.js is a headless browser emulator, written on
  node.js. It supports all JS interactions that Sahi does and works almost as
  fast as Goutte does. It's best of both worlds, actually, but still limited to
  only one browser type (Chromium), also it's still slower than Goutte and
  requires node.js and npm to be installed on the system.

.. versionadded:: 1.2

* ``SeleniumDriver`` - provides a bridge for famous `Selenium <http://seleniumhq.org/>`_
  tool. If you need legacy Selenium - you can use it right out of the box
  in your Behat/Mink test suites.

.. versionadded:: 1.3

* ``Selenium2Driver`` - provides a bridge for `Selenium2 (webdriver) <http://seleniumhq.org/>`_
  tool. If you just love Selenium2 - you can now use it right out of the box too.

GoutteDriver
~~~~~~~~~~~~

In order to talk with Goutte, you should instantiate a
``Driver\GoutteDriver`` class:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\GoutteDriver();

Also, if you want to configure Goutte more precisely, you could do the full
setup by hands:

.. code-block:: php

    $zendOptions   = array();
    $serverOptions = array();

    $driver = new \Behat\Mink\Driver\GoutteDriver(
        new \Goutte\Client($zendOptions, $serverOptions)
    );

.. tip::

    ``$zendOptions`` is an array of parameters for Zend HTTP client, which
    Goutte uses internally. You can read about Zend client parameters `here <http://framework.zend.com/manual/en/zend.http.client.html>`_.

SahiDriver
~~~~~~~~~~

In order to talk with a real browser through Sahi, you should install and
configure Sahi first:

1. Download and run the Sahi jar from the `<http://sourceforge.net/projects/sahi/files/>`_
   and run it. It will run the installer, which will guide you through the
   installation process.

2. Run Sahi proxy before your test suites (you can start this proxy during
   system startup):

    .. code-block:: bash

        cd $YOUR_PATH_TO_SAHI/bin
        ./sahi.sh

After installing Sahi and running Sahi proxy server, you'll be able to control
it with ``Driver\SahiDriver``:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\SahiDriver('firefox');

.. note::

    Notice, that first argument to ``SahiDriver`` is always a browser name,
    `supported by Sahi <http://sahi.co.in/w/browser-types-xml>`_.

If you want more control during driver initialization, like for example if you
want to configure the driver to talk with a proxy on another machine - use the
more verbose version with a second client arugment:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\SahiDriver('firefox',
        new \Behat\SahiClient\Client(
            new \Behat\SahiClient\Connection($sid, $host, $port)
        )
    );

.. note::

    ``$sid`` is a Sahi session ID. It's a unique string, used by the driver and
    Sahi proxy in order to be able to talk with each other. You should fill
    this with ``null`` if you want Sahi to start your browser automatically
    or with some uniqe string if you want to control an already started browser.

    ``$host`` simply defines the host on which Sahi is started. It's
    ``localhost`` by default.

    ``$port`` defines a Sahi proxy port. Default one is ``9999``.

ZombieDriver
~~~~~~~~~~~~

.. versionadded:: 1.1

In order to talk with zombie.js server, you should install and configure
zombie.js first:

1. Install node.js by following instructions from the official site:
   `<http://nodejs.org/>`_.

2. Install npm (node package manager) by following instructions from the
   `<http://npmjs.org/>`_.

3. Install zombie.js with npm:

    .. code-block:: bash

        $ npm install -g zombie

After installing npm and zombie.js, you'll need to add npm libs to your
``NODE_PATH``. The easiest way to do this is to add:

    .. code-block:: bash

        export NODE_PATH="/PATH/TO/NPM/node_modules"

into your ``.bashrc``.

After that, you'll be able to just use ``ZombieDriver`` without manual
server setup - the driver will do all that for you automatically:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\ZombieDriver();

If you want more control during driver initialization, like for example if you
want to configure the driver to init the server on a specific port - use the
more verbose version with 2 arguments:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\ZombieDriver(
        new \Behat\Mink\Driver\Zombie\Connection($host, $port),
        new \Behat\Mink\Driver\Zombie\Server($host, $port, $nodeBin, $script)
    );

.. note::

    ``$host`` simply defines the host on which zombie.js will be started. It's
    ``127.0.0.1`` by default.

    ``$port`` defines a zombie.js port. Default one is ``8124``.

    ``$nodeBin`` defines full path to node.js binary. Default one is just ``node``.

    ``$script`` defines a node.js script to start zombie.js server. If you pass
    a ``null`` the default script will be used. Use this option carefully!

SeleniumDriver
~~~~~~~~~~~~~~

.. versionadded:: 1.2

In order to talk with selenium server, you should install and configure it first:

1. Download Selenium Server (formerly the Selenium RC Server) from here:
   `<http://seleniumhq.org/download/>`_.

2. Run server with command:

   .. code-block:: bash

        $ java -jar selenium-server-standalone-2.11.0.jar

That's it, now you can use ``SeleniumDriver``:

.. code-block:: php

    $client = new \Selenium\Client($host, $port);
    $driver = new \Behat\Mink\Driver\SeleniumDriver(
        'firefox', 'base_url', $client
    );

Selenium2Driver
~~~~~~~~~~~~~~~

.. versionadded:: 1.3

In order to talk with selenium server, you should install and configure it first:

1. Download Selenium Server (formerly the Selenium RC Server) from here:
   `<http://seleniumhq.org/download/>`_.

2. Run server with command:

   .. code-block:: bash

        $ java -jar selenium-server-standalone-2.11.0.jar

That's it, now you can use ``Selenium2Driver``:

.. code-block:: php

    $driver = new \Behat\Mink\Driver\Selenium2Driver(
        'firefox', 'base_url'
    );

Control the Browser - Session
-----------------------------

Ok. Now we know how to create the browser driver to talk with a specific browser
emulator. Although we can use drivers directly to call some actions on the
emulator, Mink provides a better way - ``Session``:

.. code-block:: php

    // init session:
    $session = new \Behat\Mink\Session($driver);

    // start session:
    $session->start();

.. note::

  As you can see, the first argument to the session (``$driver``) is just a
  simple driver instance, which we created in the previous chapter.

``start()`` call is required in order to configure the browser emulator or
controller to be fully functional.

Basic Browser Interaction
~~~~~~~~~~~~~~~~~~~~~~~~~

After you've instantiated the ``$session`` object, you can control the actual
browser emulator with it:

.. code-block:: php

    // open some page in browser:
    $session->visit('http://my_project.dev/some_page.php');

    // get the current page URL:
    echo $session->getCurrentUrl();

    // get the response status code:
    echo $session->getStatusCode();

    // get page content:
    echo $session->getPage()->getContent();

    // open another page:
    $session->visit('http://my_project.dev/second_page.php')

    // use history controlls:
    $session->reload();
    $session->back();
    $session->forward();

    // evaluate JS expression:
    echo $session->evaluateScript(
        "(function(){ return 'something from browser'; })()"
    );

    // wait for n milliseconds or
    // till JS expression becomes true:
    $session->wait(5000,
        "$('.suggestions-results').children().length > 0"
    );

.. note::

    Although Mink does its best on removing browser differences between
    different browser emulators - it can't do much in some cases. For example,
    ``GoutteDriver`` can't evaluate JavaScript and ``SahiDriver`` can't get
    the response status code. In such cases, the driver will always throw
    meaningful ``Behat\Mink\Exception\UnsupportedDriverActionException``.

Cookies and Headers management
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

With ``Mink\Session`` you can controll your browsers cookies and headers:

.. code-block:: php

    // setting browser language:
    $session->setRequestHeader('Accept-Language', 'fr');

    // retrieving response headers:
    print_r($session->getResponseHeaders());

    // set cookie:
    $session->setCookie('cookie name', 'value');

    // get cookie:
    echo $session->getCookie('cookie name');

    // delete cookie:
    $session->setCookie('cookie name', null);

.. note::

    Headers handling is not supported by ``Driver\SahiDriver``. Because there's
    no way Sahi can get such information out of the browser.

HTTP Authentication
~~~~~~~~~~~~~~~~~~~

Also, Mink session has a special method to perform HTTP Basic authentication:

.. code-block:: php

    $session->setBasicAuth($user, $password);

.. note::

    Automatic HTTP authentication is not supported by ``SahiDriver``. Because
    HTTP authentication in browser requires manual user action, that can't
    be done remotely.

Resetting the Session
~~~~~~~~~~~~~~~~~~~~~

The primary aim for Mink is to provide a single consistent web browsing API for
acceptance tests. But most important part in testing is isolation. We need a
way to isolate our tests from each other. And Mink provides two very useful
methods for you to use in your ``teardown()`` methods:

.. code-block:: php

    // soft-reset:
    $session->reset();

    // hard-reset:
    $session->restart();

Both methods do exactly the same job for headless browsers - they clear
browser's cookies and history. The difference appears with ``Driver\SahiDriver``:

* ``$session->reset()`` will try to clean all available from browser side
  cookies. It's very fast and doesn't require the physical reload of the browser
  between tests, making them much faster. But it has a disadvantage - it clears
  only the cookies, available to clean from browser side. And we also have
  ``http-only`` cookies. In such case, resetting simply won't work. Also,
  browsing history will state the same after this call. So, it's very fast, but
  limited in complex cases.

* ``$session->restart()`` will physically restart the browser. This action will
  physically clean **all** your cookies and browsing history by cost of browser
  reloading.

Taking all this into account, it would be the best way to use ``reset()`` by
default and to call ``restart()`` in cases when we need really full isolation.

Sessions Manager
~~~~~~~~~~~~~~~~

Although ``$session`` object is already usable enough, it's not as easy to
write multisession (multidriver/multibrowser) code. Yep, you've heard me right,
with Mink you can manipulate multiple browser emulators simultaneously with a
single consistent API:

.. code-block:: php

    // init sessions
    $session1 = new \Behat\Mink\Session($driver1);
    $session2 = new \Behat\Mink\Session($driver2);

    // start sessions
    $session1->start();
    $session2->start();

    $session1->visit('http://my_project.dev/chat.php');
    $session2->visit('http://my_project.dev/chat.php');

Isn't it cool? But Mink makes it even cooler:

.. code-block:: php

    $mink = new \Behat\Mink\Mink();
    $mink->registerSession('goutte', $goutteSession);
    $mink->registerSession('sahi', $sahiSession);
    $mink->setDefaultSessionName('goutte');

With such configuration, you can talk with your sessions by name through one
single container object:

.. code-block:: php

    $mink->getSession('goutte')->visit('http://my_project.dev/chat.php');
    $mink->getSession('sahi')->visit('http://my_project.dev/chat.php');

.. note::

    Mink will even lazy-start your sessions when needed (on first ``getSession()``
    call). So, the browser will not be started till you really need it!

Or you could even omit the session name in default cases:

.. code-block:: php

    $mink->getSession()->visit('http://my_project.dev/chat.php');

This call is possible thanks to ``$mink->setDefaultSessionName('goutte')``
setting previously. We've set the default session, that would be returned on
``getSession()`` call without arguments.

.. tip::

    ``Mink`` class also provides an easy way to reset or restart your started
    sessions (and only started ones):

    .. code-block:: php

        // reset started sessions
        $mink->resetSessions();

        // restart started sessions
        $mink->restartSessions();

Traverse the Page - Selectors
-----------------------------

Now you know how to control the browser itself. But what about traversing the
current page content? Mink talks to its drivers with `XPath selectors`_, but
you also have access to `named selectors`_ and `css selectors`_. Mink will
transform such selectors into XPath queries internally for you.

The main class of Mink's selectors engine is ``Behat\Mink\Selector\SelectorsHandler``.
It handles different selector types, which implements ``Behat\Mink\Selector\SelectorInterface``:

.. code-block:: php

    $cssSelector = new \Behat\Mink\Selector\CssSelector();

    // generate XPath query out of CSS:
    echo $cssSelector->translateToXPath('h1 > a');

    $handler = new \Behat\Mink\Selector\SelectorsHandler();
    $handler->registerSelector('css', $cssSelector);

    // generate XPath query out of CSS:
    echo $handler->selectorToXpath('css', 'h1 > a');

When you initialize ``Selector\SelectorsHandler`` it already has `XPath selectors`_,
`named selectors`_ and `css selectors`_ registered in it.

You can provide a custom selectors handler as a second argument to your session
instances:

.. code-block:: php

    $session = new \Behat\Mink\Session($driver,
        new \Behat\Mink\Selector\SelectorsHandler()
    );

Mink will use this handler internally in `find* methods`_.

Named Selectors
~~~~~~~~~~~~~~~

Named selectors provide a way to get named XPath queries:

.. code-block:: php

    $selector = new \Behat\Mink\Selector\NamedSelector();
    $handler  = new \Behat\Mink\Selector\SelectorsHandler(array(
        'named' => $selector
    ));

    // XPath query to find the fieldset:
    $xpath1 = $selector->translateToXPath(
        array('fieldset', 'id|legend')
    );
    $xpath1 = $handler->selectorToXpath('named',
        array('fieldset', 'id|legend')
    );

    // XPath query to find the field:
    $xpath2 = $selector->translateToXPath(
        array('field', 'id|name|value|label')
    );
    $xpath2 = $handler->selectorToXpath('named',
        array('field', 'id|name|value|label')
    );

There's whole lot more named selectors for you to use:

* ``link`` - for searching a link by its href, id, title, img alt or value
* ``button`` - for searching a button by its name, id, value, img alt or title
* ``link_or_button`` - for searching for both, links and buttons
* ``content`` - for sarching a specific page content (text)
* ``select`` - for searching a select field by its id, name or label
* ``checkbox`` - for searching a checkbox by its id, name, or label
* ``radio`` - for searching a radio button by its id, name, or label
* ``file`` - for searching a file input by its id, name, or label
* ``optgroup`` - for searching optgroup by its label
* ``option`` - for searching an option by its content
* ``table`` - for searching a table by its id or caption

CSS Selectors
~~~~~~~~~~~~~

With ``Selector\CssSelector``, you can use CSS expressions to search page
elements:

.. code-block:: php

    $selector = new \Behat\Mink\Selector\CssSelector();
    $handler  = new \Behat\Mink\Selector\SelectorsHandler(array(
        'css' => $selector
    ));

    // XPath query to find the link by ID:
    $xpath1 = $selector->translateToXPath('a#ID');
    $xpath1 = $handler->selectorToXpath('css', 'a#ID');

XPath Selectors
~~~~~~~~~~~~~~~

And of course, you can use clean XPath queries:

.. code-block:: php

    $xpath = $handler->selectorToXpath('xpath', '//html');

It's like a proxy method, which will return the same expression you give to it.
It's used internally in `find* methods`_.

``find*`` Methods
~~~~~~~~~~~~~~~~~

So, now we know how to generate XPath queries for specific elements search.
But how we actually make this search? The answer is ``find*`` methods,
available on ``DocumentElement`` object. You can get this object from session:

.. code-block:: php

    $page = $session->getPage();
    $page = $mink->getSession('sahi')->getPage();

This object provides two very useful traversing methods:

* ``find()`` - evaluates specific selector on the page content and returns
  the last matched element or ``null``:

  .. code-block:: php

    $fieldElement = $page->find('named',
        array('field', 'id|name|value|label')
    );
    $elementByCss = $page->find('css', 'h3 > a');

* ``findAll()`` - evaluates specific selector on the page content and returns
  an array of matched elements:

  .. code-block:: php

    $fieldElements = $page->findAll('named',
        array('field', 'id|name|value|label')
    );
    $elementsByCss = $page->findAll('css', 'h3 > a');

Also, there's a bunch of shortcut methods:

* ``findById()`` - will search for an element by its ID
* ``findLink()`` - will search for a link with ``link`` named selector
* ``findButton()`` - will search for a button with ``button`` named selector
* ``findField()`` - will search for a field with ``field`` named selector

Nested Traversing
~~~~~~~~~~~~~~~~~

Every ``find*()`` method will return ``Behat\Mink\Element\NodeElement`` instance
and ``findAll()`` will return an array of such instances. The fun part is you
can make same old traversing on such elements too:

.. code-block:: php

    $registerForm = $page->find('css', 'form.register');

    // find some field INSIDE form with class="register"
    $field = $registerForm->findField('id|name|value|label');

Manipulate the Page - ``NodeElement``
-------------------------------------

Ok, you've got an interesting page element. Now you'll want to do something with
it. ``Behat\Mink\Element\NodeElement`` provides bunch a of useful methods for
you:

.. code-block:: php

    $el = $page->find('css', '.something');

    // get tag name:
    echo $el->getTagName();

    // check that element has href attribute:
    $el->hasAttribute('href');

    // get element's href attribute:
    echo $el->getAttribute('href');

Element Content and Text
~~~~~~~~~~~~~~~~~~~~~~~~

To retrieve HTML content or plain text from out of the element, you can use:

.. code-block:: php

    $plainText = $el->getText();
    $html = $el->getHtml();

.. note::

    ``getText()`` will strip tags and unprinted characters out of the response,
    including newlines. So it'll basically return the text, that user sees on
    the page.

Form Field Maniupaltions
~~~~~~~~~~~~~~~~~~~~~~~~

You can fill form fields/retrieve its values with form manipulation actions:

.. code-block:: php

    // check/unchech checkbox:
    if ($el->isChecked()) {
        $el->uncheck();
    }
    $el->check();

    // select option in select:
    $el->selectOption('optin value');

    // attach file to file input:
    $el->attachFile('/path/to/file');

    // get input value:
    echo $el->getValue();

    // set intput value:
    $el->setValue('some val');

    // press the button:
    $el->press();


Mouse Manipulations
~~~~~~~~~~~~~~~~~~~

You can perform mouse manipulations on an element:

.. code-block:: php

    $el->click();
    $el->doubleClick();
    $el->rightClick();
    $el->mouseOver();
    $el->focus();
    $el->blur();

.. note::

    All methods except ``click()`` are not supported by ``Driver\GoutteDriver``,
    because there's no way how it can perform them without actual browser window.

Drag'n'Drop
~~~~~~~~~~~

Mink even supports drag'n'drop of one field onto another:

.. code-block:: php

    $el1 = $page->find(...);
    $el2 = $page->find(...);

    $el1->dragTo($el2);

.. note::

    Drag'n'drop is not supported by ``Driver\GoutteDriver``, because there's no
    way how it can perform this action without actual browser window.

Testing Tools Integration
-------------------------

Mink comes with `PHPUnit <http://www.phpunit.de>`_, `Behat <http://behat.org>`_
and `Symfony2 <http://symfony.com>`_ support out of the box. You can read next
articles in order to understand how you can use them with Mink:

* :doc:`phpunit`
* `Developing Web Applications with Behat and Mink <http://docs.behat.org/cookbook/behat_and_mink.html>`_
* :doc:`bundle/index`

Mink API
--------

Find out all available functions in `Mink API <http://mink.behat.org/api/>`_.
