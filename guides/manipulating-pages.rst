Manipulating Pages
==================

Ok, you've got an interesting page element. Now you'll want to do something
with it. ``Behat\Mink\Element\NodeElement`` provides bunch a of useful methods
for you:

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
    including newlines. So it'll basically return the text, that user sees
    on the page.

Form Field Manipulations
~~~~~~~~~~~~~~~~~~~~~~~~

You can fill form fields/retrieve its values with form manipulation actions:

.. code-block:: php

    // check/uncheck checkbox:
    if ($el->isChecked()) {
        $el->uncheck();
    }
    $el->check();

    // select option in select:
    $el->selectOption('option value');

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
    because there is no way how it can perform them without actual browser
    window.

Drag'n'Drop
~~~~~~~~~~~

Mink even supports drag'n'drop of one field onto another:

.. code-block:: php

    $el1 = $page->find(...);
    $el2 = $page->find(...);

    $el1->dragTo($el2);

.. note::

    Drag'n'drop is not supported by ``Driver\GoutteDriver``, because there
    is no way how it can perform this action without actual browser window.

