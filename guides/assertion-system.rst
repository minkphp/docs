Assertion system
================

The ``\Behat\Mink\WebAssert`` class provides a set of assertions. There are assertions
about the address of the page, the cookies, the status code, the response headers,
the content of the page, the page elements...

API with exceptions
-------------------

An assertion can improve code readability and avoid having a fatal error in method chaining.

For example, instead of:

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


Webassert and multisessions
---------------------------

You could use an another session by calling ``assertSession`` with a session name
or an instance of ``\Behat\Mink\Session``.

.. code-block:: php

    $mink->assertSession()->elementExists('css', 'form.register');
    $mink->assertSession('goutte')->elementExists('css', 'form.register');
    $mink->assertSession($mink->getSession('goutte'))->elementExists('css', 'form.register');
