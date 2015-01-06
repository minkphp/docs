Traversing Pages
================

Now you know how to control the browser itself. But what about traversing
the current page content? Mink talks to its drivers with `XPath selectors`_,
but you also have access to `named selectors`_ and `css selectors`_. Mink
will transform such selectors into XPath queries internally for you.

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
* ``button`` - for searching a button by its name, id, value, img alt or
  title
* ``link_or_button`` - for searching for both, links and buttons
* ``content`` - for searching a specific page content (text)
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

It's like a proxy method, which will return the same expression you give
to it. It's used internally in `find* methods`_.

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
and ``findAll()`` will return an array of such instances. The fun part is
you can make same old traversing on such elements too:

.. code-block:: php

    $registerForm = $page->find('css', 'form.register');

    // find some field INSIDE form with class="register"
    $field = $registerForm->findField('id|name|value|label');
