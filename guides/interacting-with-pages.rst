Interacting with Pages
======================

Most usages of Mink will involve working with the page opened in your browser.
The Mink Element API lets you interact with elements of the page.

Interacting with Links and Buttons
----------------------------------

The ``NodeElement::click`` and ``NodeElement::press`` methods let you click
the links and press the buttons on the page.

.. note::

    These methods are actually equivalent internally (pressing a button involves
    clicking on it). Having both methods allows to keep the code more readable.

.. _interacting-with-forms:

Interacting with Forms
----------------------

The ``NodeElement`` class has a set of methods allowing to interact with
forms:

``NodeElement::getValue``
    gets the value of a form field. The value depends on the type of field:

    - the value of the selected option for single select boxes (or ``null``
      when none are selected);
    - an array of selected option values for multiple select boxes;
    - the value of the checkbox field when checked, or ``null`` when not
      checked;
    - the value of the selected radio button in the radio group for radio
      buttons;
    - the value of the field for textual fields and textareas;
    - an undefined value for file fields (because of browser limitations).

``NodeElement::setValue``
    sets the value of a form field

    - for a file field, it should be the absolute path to the file;
    - for a checkbox, it should be a boolean indicating whether it is checked;
    - for other fields, it should match the behavior of ``getValue``.

``NodeElement::isChecked``
    reports whether a radio button or a checkbox is checked.

``NodeElement::isSelected``
    reports whether an ``<option>`` element is selected.

``NodeElement::check``
    checks a checkbox field.

``NodeElement::uncheck``
    unchecks a checkbox field.

``NodeElement::selectOption``
    select an option in a select box or in a radio group.

``NodeElement::attachFile``
    attaches a file in a file input.

``NodeElement::submit``
    submits the form.

Interacting with the Mouse
--------------------------

The ``NodeElement`` class offers a set of methods allowing to interact with
the mouse:

``NodeElement::click``
    performs a click on the element.

``NodeElement::doubleClick``
    performs a double click on the element.

``NodeElement::rightClick``
    performs a right click on the element.

``NodeElement::mouseOver``
    moves the mouse over the element.

Interacting with the Keyboard
-----------------------------

Mink lets you interact with the keyboard thanks to the ``NodeElement::keyDown``,
``NodeElement::keyPress`` and ``NodeElement::keyUp`` methods.

Manipulating the Focus
----------------------

The ``NodeElement`` class lets you give and remove focus on the element thanks
to the ``NodeElement::focus`` and ``NodeElement::blur`` methods.

Drag'n'Drop
-----------

Mink supports drag'n'drop of one element onto another:

.. code-block:: php

    $dragged = $page->find(...);
    $target = $page->find(...);

    $dragged->dragTo($target);

Shortcut Methods
----------------

The ``TraversableElement`` class provides a few shortcut methods allowing
to find a child element on the page and perform an action on it immediately:

``TraversableElement::clickLink``
    Looks for a link (see findLink) and clicks on it.

``TraversableElement::pressButton``
    Looks for a button (see findButton) and presses on it.

``TraversableElement::fillField``
    Looks for a field (see findField) and sets a value in it.

``TraversableElement::checkField``
    Looks for a checkbox (see findField) and checks it.

``TraversableElement::uncheckField``
    Looks for a checkbox (see findField) and unchecks it.

``TraversableElement::selectFieldOption``
    Looks for a select or radio group (see findField) and selects a choice in it.

``TraversableElement::attachFileToField``
    Looks for a file field (see findField) and attach a file to it.

.. note::

    All these shortcut methods are throwing an ``ElementNotFoundException``
    in case the child element cannot be found.
