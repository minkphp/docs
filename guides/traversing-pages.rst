Traversing Pages
================

Most usages of Mink will involve working with the page opened in your browser.
This is done thanks to the powerful Element API. This API allows to traverse
the page (similar to the DOM in Javascript), :doc:`manipulate page elements </guides/traversing-pages>`
and to :doc:`interact with them </guides/interacting-with-pages>`, which
will be covered in the next chapters.

DocumentElement and NodeElement
-------------------------------

The Element API consists of 2 main classes. The ``DocumentElement`` instance
represents the page being displayed in the browser, while the ``NodeElement``
class is used to represent any element inside the page. Both class are sharing
a common set of methods to traverse the page (defined in ``TraversableElement``).

The ``DocumentElement`` instance is accessible through the ``Session::getPage`` method:

.. code-block:: php

    $page = $session->getPage();

    // You can now manipulate the page.

.. note::

    The ``DocumentElement`` instance represents the ``<html>`` node in the
    DOM. It is equivalent to ``document.documentElement`` in the Javascript
    DOM API.

Traversal Methods
-----------------

Elements have 2 main traversal methods: ``ElementInterface::findAll`` returns
an array of ``NodeElement`` instances matching the provided :ref:`selector <selectors>`
inside the current element while ``ElementInterface::find`` returns the first
match or ``null`` when there is none.

The ``TraversableElement`` class also provides a bunch of shortcut methods
on top of ``find()`` to make it easier to achieve many common use cases:

``ElementInterface::has``
    Checks whether a child element matches the given selector but without
    returning it.

``TraversableElement::findById``
    Looks for a child element with the given id.

``TraversableElement::findLink``
    Looks for a link with the given text, title, id or ``alt`` attribute
    (for images used inside links).

``TraversableElement::findButton``
    Looks for a button with the given text, title, id, ``name`` attribute
    or ``alt`` attribute (for images used inside links).

``TraversableElement::findField``
    Looks for a field (``input``, ``textarea`` or ``select``) with the given
    label, placeholder, id or ``name`` attribute.

.. note::

    These shortcuts are returning a single element. If you need to find all
    matches, you will need to use ``findAll`` with the :ref:`named selector <named-selector>`.

Nested Traversing
~~~~~~~~~~~~~~~~~

Every ``find*()`` method will return a ``Behat\Mink\Element\NodeElement`` instance
and ``findAll()`` will return an array of such instances. The fun part is
that you can make same old traversing on such elements as well:

.. code-block:: php

    $registerForm = $page->find('css', 'form.register');

    if (null === $registerForm) {
        throw new \Exception('The element is not found');
    }

    // find some field INSIDE form with class="register"
    $field = $registerForm->findField('Email');

.. _selectors:

Selectors
---------

The ``ElementInterface::find`` and ``ElementInterface::findAll`` methods
support several kinds of selectors to find elements.

CSS Selector
~~~~~~~~~~~~

The ``css`` selector type lets you use CSS expressions to search for elements
on the page:

.. code-block:: php

    $title = $page->find('css', 'h1');

    $buttonIcon = $page->find('css', '.btn > .icon');

XPath Selector
~~~~~~~~~~~~~~

The ``xpath`` selector type lets you use XPath queries to search for elements
on the page:

.. code-block:: php

    $anchorsWithoutUrl = $page->findAll('xpath', '//a[not(@href)]');

.. caution::

    This selector searches for an element inside the current node (which
    is ``<html>`` for the page object). This means that trying to pass it
    the XPath of and element retrieved with ``ElementInterface::getXpath``
    will not work (this query includes the query for the root node). To check
    whether an element object still exists on the browser page, use ``ElementInterface::isValid``
    instead.

.. _named-selector:

Named Selectors
~~~~~~~~~~~~~~~

Named selectors provide a set of reusable queries for common needs. For conditions
based on the content of elements, the named selector will try to find an
exact match first. It will then fallback to partial matching in case there
is no result for the exact match. The ``named_exact`` selector type can be
used to force using only exact matching. The ``named_partial`` selector type
can be used to apply partial matching without preferring exact matches.

For the named selector type, the second argument of the ``find()`` method
is an array with 2 elements: the name of the query to use and the value to
search with this query:

.. code-block:: php

    $escapedValue = $session->getSelectorsHandler()->xpathLiteral('Go to top');

    $topLink = $page->find('named', array('link', $escapedValue);

.. caution::

    The named selector requires escaping the value as XPath literal. Otherwise
    the generated XPath query will be invalid.

The following queries are supported by the named selector:

``id``
    Searches for an element by its id.
``id_or_name``
    Searches for an element by its id or name.
``link``
    Searches for a link by its id, title, img alt, rel or text.
``button``
    Searches for a button by its name, id, text, img alt or title.
``link_or_button``
    Searches for both links and buttons.
``content``
    Searches for a specific page content (text).
``field``
    Searches for a form field by its id, name, label or placeholder.
``select``
    Searches for a select field by its id, name or label.
``checkbox``
    Searches for a checkbox by its id, name, or label.
``radio``
    Searches for a radio button by its id, name, or label.
``file``
    Searches for a file input by its id, name, or label.
``optgroup``
    Searches for an optgroup by its label.
``option``
    Searches for an option by its content or value.
``fieldset``
    Searches for a fieldset by its id or legend.
``table``
    Searches for a table by its id or caption.

Custom Selector
~~~~~~~~~~~~~~~

Mink lets you register your own selector types through implementing the ``Behat\Mink\Selector\SelectorInterface``.
It should then be registered in the ``SelectorsHandler`` which is the registry
of available selectors.

The recommended way to register a custom selector is to do it when building
your ``Session``:

.. code-block:: php

    $selector = new \App\MySelector();

    $handler = new \Behat\Mink\Selector\SelectorsHandler();
    $handler->registerSelector('mine', $selector);

    $driver = // ...

    $session = new \Behat\Mink\Session($driver, $handler);
