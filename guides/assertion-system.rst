Assertion system
================

The ``\Behat\Mink\WebAssert`` class provides a set of assertions. There are assertions
about the address of the page, the cookies, the status code, the response headers,
the content of the page, the page elements...

.. note::

    If an assertion evaluates to false, an exception ``Behat\Mink\Exception\ExpectationException``
    is thrown.

Mink initialisation
-------------------

.. code-block:: php

    // Choose a Mink driver.
    $driver = new \Behat\Mink\Driver\GoutteDriver();
    $session = new \Behat\Mink\Session($driver);
    $mink = new \Behat\Mink\Mink(array('goutte' => $session));
    $mink->setDefaultSessionName('goutte');

Checking address
----------------

``WebAssert::addressEquals``
    Checks that current session address is equals to provided one.

``WebAssert::addressNotEquals``
    Checks that current session address is not equals to provided one.

.. code-block:: php

    $mink->getSession()->visit('http://exemple.com');

    $mink->assertSession()->addressEquals('http://example.com/');
    $mink->assertSession()->addressEquals('/');

    $mink->assertSession()->addressNotEquals('http://example.com/not_found');
    $mink->assertSession()->addressNotEquals('/not_found');

``WebAssert::addressMatches``
    Checks that current session address matches regex.

.. code-block:: php

    $mink->getSession()->visit('http://example.com/script.php/sub/url/foo/bar');

    $mink->assertSession()->addressMatches('/sub.*bar/');

.. caution::
  Webassert compare only the path and the fragment (after the hashmark #). The scheme,
  the host, the query (after the question mark ?) are not taken into account.

  These examples didn't throw exception:

  .. code-block:: php

    $mink->getSession()->visit('http://example.com');
    // different scheme
    $mink->assertSession()->addressEquals('https://example.com');
    // different host
    $mink->assertSession()->addressEquals('https://another.com');


    $mink->getSession()->visit('http://example.com/script.php/sub/url?param=true#webapp/nav');
    // Without query string
    $mink->assertSession()->addressEquals('http://examyyyyyple.com/sub/url#webapp/nav');

Checking cookie
---------------

``WebAssert::cookieExists``
    Checks that specified cookie exists.

``WebAssert::cookieEquals``
    Checks that specified cookie exists and its value.

.. code-block:: php

    $mink->assertSession()->cookieExists('cookie_name');
    $mink->assertSession()->cookieEquals('cookie_name', 'foo_value');

Checking status code
--------------------

``WebAssert::statusCodeEquals``
    Checks that current response code equals to provided one.

``WebAssert::statusCodeNotEquals``
    Checks that current response code not equals to provided one.

.. code-block:: php

    $mink->assertSession()->statusCodeEquals(200);
    $mink->assertSession()->statusCodeNotEquals(500);

.. note::

    See the :ref:`driver-feature-support` to see which driver supports this feature.

Checking response headers
-------------------------

``WebAssert::responseHeaderEquals``
    Checks that current response header equals value.

``WebAssert::responseHeaderNotEquals``
    Checks that current response header does not equal value.

``WebAssert::responseHeaderContains``
    Checks that current response header contains value.

``WebAssert::responseHeaderNotContains``
    Checks that current response header does not contain value.

.. code-block:: php

    $mink->assertSession()->responseHeaderEquals('Content-Type', 'text/html;charset=utf-8');
    $mink->assertSession()->responseHeaderNotEquals('Content-Type', 'application/json');
    $mink->assertSession()->responseHeaderContains('Content-Type', 'charset=utf-8');
    $mink->assertSession()->responseHeaderNotContains('Content-Type', 'application/json');

``WebAssert::responseHeaderMatches``
    Checks that current response header matches regex.

``WebAssert::responseHeaderNotMatches``
    Checks that current response header does not match regex.

.. code-block:: php

    $mink->assertSession()->responseHeaderMatches('Content-Type', '/text.*charset.*/');
    $mink->assertSession()->responseHeaderNotMatches('Content-Type', '/application.*charset.*/');

.. note::

    See the :ref:`driver-feature-support` to see which driver supports this feature.

Checking response text content
------------------------------

WebAssert can checks the text content of current page. The comparison is case-insensitive.

``WebAssert::pageTextContains``
    Checks that current page contains text.

``WebAssert::pageTextNotContains``
    Checks that current page does not contains text.

.. code-block:: php

    $mink->assertSession()->pageTextContains('Example');
    $mink->assertSession()->pageTextNotContains('Examplefoobar');

``WebAssert::pageTextMatches``
    Checks that current page text matches regex.

``WebAssert::pageTextNotMatches``
    Checks that current page text does not matches regex.

.. code-block:: php

    $mink->assertSession()->pageTextMatches('/Example/');
    $mink->assertSession()->pageTextNotMatches('/Examplefoobar/');

Checking response HTML content
------------------------------

WebAssert can checks the HTML content of current page. The comparison is case-insensitive.

``WebAssert::responseContains``
    Checks that page HTML (response content) contains text.

``WebAssert::responseNotContains``
    Checks that page HTML (response content) does not contains text.

.. code-block:: php

    $mink->assertSession()->responseContains('<h1>Example Domain</h1>');
    $mink->assertSession()->responseNotContains('<h2>Example Domain </h2>');

``WebAssert::responseMatches``
    Checks that page HTML (response content) matches regex.

``WebAssert::responseNotMatches``
    Checks that page HTML (response content) does not matches regex.

.. code-block:: php

    $mink->assertSession()->responseMatches('/<h1>Example.*<\/h1>/');
    $mink->assertSession()->responseNotMatches('/<h1>ExampleFooBar.*<\/h1>/');

Checking elements
-----------------

``WebAssert::element*`` supports same :ref:`selectors <selectors>` that the
``ElementInterface::find`` and ``ElementInterface::findAll`` methods.

``WebAssert::elementsCount``
    Checks that there is specified number of specific elements on the page.

.. code-block:: php

    $mink->assertSession()->elementsCount('css', 'h1', 1);

``WebAssert::elementExists``
    Checks that specific element exists on the current page.

.. code-block:: php

    $titleNodeElement = $mink->assertSession()->elementExists('css', 'h1');

``WebAssert::elementNotExists``
    Checks that specific element does not exists on the current page.

.. code-block:: php

    $mink->assertSession()->elementNotExists('css', 'h5');

``WebAssert::elementTextContains``
    Checks that specific element contains text. The comparison is case-insensitive.

.. code-block:: php

    $mink->assertSession()->elementTextContains('css', 'h1', 'Example Domain');

``WebAssert::elementTextNotContains``
    Checks that specific element does not contains text. The comparison is case-insensitive.

.. code-block:: php

    $mink->assertSession()->elementTextNotContains('css', 'h1', 'ExampleFooBar');

``WebAssert::elementContains``
    Checks that specific element contains HTML. The comparison is case-insensitive.

.. code-block:: php

    $mink->assertSession()->elementContains('css', 'div', '<h1>Example Domain</h1>');

``WebAssert::elementNotContains``
    Checks that specific element does not contains HTML. The comparison is case-insensitive.

.. code-block:: php

    $mink->assertSession()->elementNotContains('css', 'div', '<h1>ExampleFooBar</h1>');

``Webassert::elementAttributeExists``
    Checks that an attribute exists in an element.

.. code-block:: php

    $mink->assertSession()->elementAttributeExists('css', 'a', 'href');

``Webassert::elementAttributeContains``
    Checks that an attribute of a specific elements contains text. The comparison is case-insensitive.

.. code-block:: php

    $mink->assertSession()->elementAttributeContains('css', 'a', 'href', 'http://');

``Webassert::elementAttributeNotContains``
    Checks that an attribute of a specific elements does not contain text.

.. code-block:: php

    $mink->assertSession()->elementAttributeNotContains('css', 'a', 'href', 'https://');

All ``Webassert::field*`` use the :ref:`field named selector <named-selector>`.

``Webassert::fieldExists``
    Checks that specific field exists on the current page.

.. code-block:: php

    $mink->assertSession()->fieldExists('username');

``Webassert::fieldNotExists``
    Checks that specific field does not exists on the current page.

.. code-block:: php

    $mink->assertSession()->fieldNotExists('not_exists');

``Webassert::fieldValueEquals``
    Checks that specific field have provided value. The comparison is case-insensitive.

.. code-block:: php

    $mink->assertSession()->fieldValueEquals('username', 'foo');

``Webassert::fieldValueNotEquals``
    Checks that specific field have provided value. The comparison is case-insensitive.

.. code-block:: php

    $mink->assertSession()->fieldValueEquals('username', 'foo');

All ``Webassert::checkbox*`` use the :ref:`checkbox named selector <named-selector>`.

``Webassert::checkboxChecked``
    Checks that specific checkbox is checked.

.. code-block:: php

    $mink->assertSession()->checkboxChecked('remember_me');

``Webassert::checkboxNotChecked``
    Checks that specific checkbox is unchecked.

.. code-block:: php

    $mink->assertSession()->checkboxNotChecked('remember_me');

Nested Traversing
-----------------

Every ``WebAssert::*Exists`` method return a ``Behat\Mink\Element\NodeElement``
(Except of course ``WebAssert::*NotExists`` methods).

``WebAssert::elementsCount``, ``WebAssert::elementExists``, ``WebAssert::elementNotExists``,
``Webassert::field*``, ``Webassert::checkbox*``methods support an ``ElementInterface``
argument in order to not search on all the page but only in an element of the page.

So, instead of that:

.. code-block:: php

    $registerForm = $page->find('css', 'form.register');

    if (null === $registerForm) {
        throw new \Exception('The element is not found');
    }

    $field = $registerForm->findField('Email');

    if (null === $field) {
        throw new \Exception('The element is not found');
    }

    $field->setValue('foo@example.com');

you can do:

.. code-block:: php

    // Throw exception when not found instead of returning null.
    $registerForm = $mink->assertSession()->elementExists('css', 'form.register');

    $field = $mink->assertSession()->fieldExists('Email', $registerForm);

    $field->setValue('foo@example.com');

This can improve code readability or avoid having a fatal error in method chaining.

Webassert and multisessions
---------------------------

You could use an another session by calling ``assertSession`` with a session name
or an instance of ``\Behat\Mink\Session``.

.. code-block:: php

    $mink->assertSession()->elementExists('css', 'form.register');
    $mink->assertSession('goutte')->elementExists('css', 'form.register');
    $mink->assertSession($mink->getSession('goutte'))->elementExists('css', 'form.register');
