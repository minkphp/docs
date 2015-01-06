Controlling the Browser
=======================

Ok. Now we know how to create the browser driver to talk with a specific
browser emulator. Although we can use drivers directly to call some actions
on the emulator, Mink provides a better way - ``Session``:

.. code-block:: php

    // init session:
    $session = new \Behat\Mink\Session($driver);

    // start session:
    $session->start();

.. note::

  As you can see, the first argument to the session (``$driver``) is just
  a simple driver instance, which we created in the previous chapter.

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
    $session->visit('http://my_project.dev/second_page.php');

    // use history controls:
    $session->reload();
    $session->back();
    $session->forward();

    // evaluate JS expression:
    echo $session->evaluateScript(
        "return 'something from browser';"
    );

    // wait for n milliseconds or
    // till JS expression becomes true:
    $session->wait(
        5000,
        "$('.suggestions-results').children().length > 0"
    );

.. note::

    Although Mink does its best on removing browser differences between different
    browser emulators, it can't do much in some cases. For example, GoutteDriver
    can't evaluate JavaScript and Selenium2Driver can't get the response
    status code. In such cases, the driver will always throw a meaningful
    ``Behat\Mink\Exception\UnsupportedDriverActionException``.

Cookies and Headers management
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

With ``Behat\Mink\Session`` you can control your browsers cookies and headers:

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

    Headers handling is only supported in headless drivers, because there
    is no way browser controllers can get such information out of the browser.

HTTP Authentication
~~~~~~~~~~~~~~~~~~~

Also, Mink session has a special method to perform HTTP Basic authentication:

.. code-block:: php

    $session->setBasicAuth($user, $password);

.. note::

    Automatic HTTP authentication is only supported in headless drivers.
    Because HTTP authentication in browser requires manual user action, that
    can't be done remotely for browser controllers.

Resetting the Session
~~~~~~~~~~~~~~~~~~~~~

The primary aim for Mink is to provide a single consistent web browsing API
for acceptance tests. But most important part in testing is isolation. We
need a way to isolate our tests from each other. And Mink provides two very
useful methods for you to use in your ``teardown()`` methods:

.. code-block:: php

    // soft-reset:
    $session->reset();

    // hard-reset:
    $session->restart();

Both methods do exactly the same job for headless browsers, they clear browser's
cookies and history. The difference appears with browser controllers:

* ``$session->reset()`` will try to clean all available from browser side
  cookies. It's very fast and doesn't require the physical reload of the
  browser between tests, making them much faster. But it has a disadvantage:
  it clears only the cookies available browser-side. And we also have ``http-only``
  cookies. In such case, resetting simply won't work. Also, browsing history
  will state the same after this call. So, it's very fast, but limited in
  complex cases.

* ``$session->restart()`` will physically restart the browser. This action
  will physically clean **all** your cookies and browsing history by cost
  of browser reloading.

Taking all this into account, it would be the best way to use ``reset()``
by default and to call ``restart()`` in cases when we need really full isolation.
