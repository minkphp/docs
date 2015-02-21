Manipulating Pages
==================

Once you :doc:`get a page element </guides/traversing-pages>`, you will want
to manipulate it. You can also interact with the page, which is covered in
the :doc:`next chapter </guides/interacting-with-pages>`.

Getting the tag name
--------------------

The ``NodeElement::getTagName`` method allows you to get the tag name of
the element. This tag name is always returned lowercased.

.. code-block:: php

    $el = $page->find('css', '.something');

    // get tag name:
    echo $el->getTagName(); // displays 'a'

Accessing HTML attributes
-------------------------

The ``NodeElement`` class gives you access to HTML attributes of the element.

``NodeElement::hasAttribute``
    Checks whether the element has a given attribute.

``NodeElement::getAttribute``
    Gets the value of an attribute.

``NodeElement::hasClass``
    Checks whether the element has the given class (convenience wrapper around
    ``getAttribute('class')``).

.. code-block:: php

    $el = $page->find('css', '.something');

    if ($el->hasAttribute('href') {
        echo $el->getAttribute('href');
    } else {
        echo 'This anchor is not a link. It does not have an href.';
    }

Element Content and Text
------------------------

The ``Element`` class provides access to the content of elements.

``Element::getHtml``
    Gets the inner HTML of the element, i.e. all children of the element.

``Element::getOuterHtml``
    Gets the outer HTML of the element, i.e. including the element itself.

``Element::getText``
    Gets the text of the element.

.. note::

    ``getText()`` will strip tags and unprinted characters out of the response,
    including newlines. So it'll basically return the text, that user sees
    on the page.

Checking Element Visibility
---------------------------

The ``NodeElement::isVisible`` methods allows to checks whether the element
is visible.

Accessing Form State
--------------------

The ``NodeElement`` class allows to access the state of form elements:

``NodeElement::getValue``
    Gets the value of the element. See :ref:`interacting-with-forms`.

``NodeElement::isChecked``
    Checks whether the checkbox or radio button is checked.

``NodeElement::isSelected``
    Checks whether the ``<option>`` element is selected.

Shortcut methods
~~~~~~~~~~~~~~~~

The ``TraversableElement`` class provides a few shortcut methods allowing
to find a child element in the page and checks the state of it immediately:

``TraversableElement::hasCheckedField``
    Looks for a checkbox (see findField) and checks whether it is checked.

``TraversableElement::hasUncheckedField``
    Looks for a checkbox (see findField) and checks whether it is not checked.
