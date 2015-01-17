Controlling the Browser
=======================

In Mink, the entry point to the browser is called the session. Think about
it as being your browser window (some drivers even allow to deal with switching
tabs).

The first thing to do with your session is to start it. Nothing can be done
with it before starting it.

.. code-block:: php

    // choose a Mink driver. More about it in later chapters
    $driver = new \Behat\Mink\Driver\GoutteDriver();

    $session = new \Behat\Mink\Session($driver);

    // start the session
    $session->start();

.. note::

    The first argument to the session constructor is a driver object, which
    is how the Mink abstraction layer works. You will discover more about
    the available drivers in a :doc:`later chapter </guides/drivers>`.


.. note::

    Although Mink does its best on removing browser differences between different
    browser emulators, it can't do much in some cases. See the :ref:`driver-feature-support`
    to see which features are supported by each driver.

Basic Browser Interaction
-------------------------

The first thing to do to use your session is to open a page with it. Just
after starting, the session is not on any page (in a real browser, you would
on ``about:blank``), and calling any other action is likely to fail.

.. code-block:: php

    $session->visit('http://my_project.dev/some_page.php');

.. note::

    Mink is primarily designed to be used for testing websites. To allow
    covering error pages as well, ``Session::visit`` does not consider that
    error status codes are invalid. It will not throw an exception in this
    case. You will need to check whether the response was a success or an
    error. It will only throw an exception when Mink cannot load the page
    (network error, ...).

Interacting with the Page
-------------------------

The session gives you access to the page through the ``Session::getPage``
method. This allows you to :doc:`traverse it </guides/traversing-pages>` and
:doc:`manipulate it </guides/manipulating-pages>`. The next chapters are
covering the page API in depth.

Using the Browser History
-------------------------

The session gives you access to the browser history:

.. code-block:: php

    // get the current page URL:
    echo $session->getCurrentUrl();

    // use history controls:
    $session->reload();
    $session->back();
    $session->forward();

Cookies Management
------------------

The session can manipulate cookies available in the browser.

.. code-block:: php

    // set cookie:
    $session->setCookie('cookie name', 'value');

    // get cookie:
    echo $session->getCookie('cookie name');

    // delete cookie:
    $session->setCookie('cookie name', null);

.. note::

    In browser controllers, the access to http-only cookies may be restricted
    as they cannot be accessed in Javascript.

Status Code Retrieval
---------------------

The session lets you retrieve the status code of the response:

.. code-block:: php

    // get the response status code:
    echo $session->getStatusCode();

Headers Management
------------------

The session lets you manipulate request headers and access response headers:

.. code-block:: php

    // setting browser language:
    $session->setRequestHeader('Accept-Language', 'fr');

    // retrieving response headers:
    print_r($session->getResponseHeaders());

.. note::

    Headers handling is only supported in headless drivers, because there
    is no way browser controllers can get such information out of the browser.

HTTP Authentication
-------------------

The Mink session has a special method to perform HTTP Basic authentication:

.. code-block:: php

    $session->setBasicAuth($user, $password);

The method can also be used to reset a previous authentication:

.. code-block:: php

    $session->setBasicAuth(false);

.. note::

    Automatic HTTP authentication is only supported in headless drivers.
    Because HTTP authentication in browser requires manual user action, that
    can't be done remotely for browser controllers.

Javascript Evaluation
---------------------

The session allows you to execute or evaluate Javascript.

.. code-block:: php

    // Execute JS
    $session->evaluateScript('document.body.firstChild.innerHtml = "";');

    // evaluate JS expression:
    echo $session->evaluateScript(
        "return 'something from browser';"
    );

.. note::

    The difference between these methods is that ``Session::evaluateScript``
    returns the result of the expression. When you don't need to get a return
    value, using ``Session::executeScript`` is better.

You can also wait until a give JS expression returns a truthy value or the
timeout is reached:

.. code-block:: php

    // wait for n milliseconds or
    // till JS expression becomes true:
    $session->wait(
        5000,
        "$('.suggestions-results').children().length"
    );

.. note::

    The ``Session::wait`` method returns the result of the evaluation. It
    will return ``null`` when the timeout is reached.

Resetting the Session
---------------------

The primary aim for Mink is to provide a single consistent web browsing API
for acceptance tests. But a very important part in testing is isolation.

Mink provides two very useful methods to isolate tests, to be used in your
``teardown`` methods:

.. code-block:: php

    // soft-reset:
    $session->reset();

    // hard-reset:
    $session->stop();
    // or if you want to start again at the same time
    $session->restart();

Stopping the session is the best way to reset the session to its initial
state. It will close the browser entirely. Using the session again requires
starting the session before any other action. The ``Session::restart`` shortcut
allows to do these 2 steps in a single actions.

The drawback of closing the browser and starting it again is that it takes
time. In many cases, a lower level of isolation is enough in favor of a faster
resetting. The ``Session::reset`` method covers this use case. It will try
to clear the cookies and reset the request headers and the browser history
in the limit of the driver possibilities.

Taking all this into account, it is recommended to use ``Session::reset()``
by default and to call ``Session::stop()`` in cases when we need really full
isolation.
