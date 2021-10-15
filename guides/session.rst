Controlling the Browser
=======================

In Mink, the entry point to the browser is called the session. Think about
it as being your browser window (some drivers even let you switch tabs!).

First, start your session (it's like opening your browser tab). Nothing can
be done with it before starting it.

.. code-block:: php

    // Choose a Mink driver. More about it in later chapters.
    $driver = new \Behat\Mink\Driver\GoutteDriver();

    $session = new \Behat\Mink\Session($driver);

    // start the session
    $session->start();

.. note::

    The first argument to the session constructor is a driver object. Drivers
    are the way the Mink abstraction layer works. You will discover more
    about the available drivers in a :doc:`later chapter </guides/drivers>`.

.. caution::

    Although Mink does its best to remove differences between the different
    drivers, each driver has unique features and shortcomings. See the :ref:`driver-feature-support`
    to see which features are supported by each driver.

Basic Browser Interaction
-------------------------

Now that your session is started, you'll want to open a page with it. Just
after starting, the session is not on any page (in a real browser, you would
be on the ``about:blank`` page), and calling any other action is likely to fail.

.. code-block:: php

    $session->visit('http://my_project.dev/some_page.php');

.. note::

    Mink is primarily designed to be used for testing websites. To allow
    you to browse and test error pages, the ``Session::visit`` method does
    not consider error status codes as invalid. It will *not* throw an exception
    in this case. You will need to check the status code (or certain text
    on the page) to know if the response was successful or not.

Interacting with the Page
-------------------------

The session gives you access to the page through the ``Session::getPage``
method. This allows you to :doc:`traverse the page </guides/traversing-pages>`,
:doc:`manipulate page elements </guides/traversing-pages>` and
:doc:`interact </guides/interacting-with-pages>` with them. The next chapters
cover the page API in depth. Most of what you'll do with Mink will use this
object, but you can continue reading to learn more about the Session.

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

Cookie Management
-----------------

The session can manipulate cookies available in the browser.

.. code-block:: php

    // set cookie:
    $session->setCookie('cookie name', 'value');

    // get cookie:
    echo $session->getCookie('cookie name');

    // delete cookie:
    $session->setCookie('cookie name', null);

.. note::

    With drivers that use JavaScript to control the browser - like Sahi -
    you may be restricted to accessing/setting all, but `HttpOnly cookies`_ .

Status Code Retrieval
---------------------

The session lets you retrieve the HTTP status code of the response:

.. code-block:: php

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

    Headers handling is only supported in headless drivers (e.g. Goutte).
    Browser controllers (e.g. Selenium2) cannot access that information.

HTTP Authentication
-------------------

The session has a special method to perform HTTP Basic authentication:

.. code-block:: php

    $session->setBasicAuth($user, $password);

The method can also be used to reset a previous authentication:

.. code-block:: php

    $session->setBasicAuth(false);

.. note::

    Automatic HTTP authentication is only supported in headless drivers.
    Because HTTP authentication in the browser requires manual user action, that
    can't be done remotely for browser controllers.

Javascript Evaluation
---------------------

The session allows you to execute or evaluate Javascript.

.. code-block:: php

    // Execute JS
    $session->executeScript('document.body.firstChild.innerHTML = "";');

    // evaluate JS expression:
    echo $session->evaluateScript(
        "return 'something from browser';"
    );

.. note::

    The difference between these methods is that ``Session::evaluateScript``
    returns the result of the expression. When you don't need to get a return
    value, using ``Session::executeScript`` is better.

You can also wait until a given JS expression returns a truthy value or the
timeout is reached:

.. code-block:: php

    // wait for n milliseconds or
    // till JS expression becomes truthy:
    $session->wait(
        5000,
        "$('.suggestions-results').children().length"
    );

.. note::

    The ``Session::wait`` method returns ``true`` when the evaluation becomes
    truthy. It will return ``false`` when the timeout is reached.

Resetting the Session
---------------------

The primary aim for Mink is to provide a single consistent web browsing API
for acceptance tests. But a very important part in testing is isolation.  

So Mink provides two very useful methods to isolate tests, which can be used
in your tests' ``teardown`` methods:

.. code-block:: php

    // soft-reset:
    $session->reset();

    // hard-reset:
    $session->stop();
    // or if you want to start again at the same time
    $session->restart();

Stopping the session is the best way to reset the session to its initial
state. It will close the browser entirely. To use the session again, you
need to start the session before any other action. The ``Session::restart``
shortcut allows you to do these 2 steps in a single call.

The drawback of closing the browser and starting it again is that it takes
time. In many cases, a shallower level of isolation is enough in favor of a 
faster resetting. The ``Session::reset`` method covers this use case. It 
will try to clear the cookies, reset the request headers and clear the 
browser history - though there are some driver limitations that mean this 
isn't always effective.  For example:

* Selenium will not allow cookies to be cleared from anything other than 
  the current domain.  So a test that runs across different domains (for 
  example your site and PayPal's sandbox) cannot be properly isolated using 
  ``Session::reset()``.

Taking all this into account, it is recommended to use ``Session::reset()``
by default and to call ``Session::stop()`` when you need really full isolation.

.. _HttpOnly cookies: http://en.wikipedia.org/wiki/HTTP_cookie#HttpOnly_cookie
