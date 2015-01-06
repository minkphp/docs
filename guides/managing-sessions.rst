Managing Sessions
=================

Although the :doc:`session object </guides/session>` is already usable enough,
it's not as easy to write multisession (multidriver/multibrowser) code. Yep,
you've heard right, with Mink you can manipulate multiple browser emulators
simultaneously with a single consistent API:

.. code-block:: php

    // init sessions
    $session1 = new \Behat\Mink\Session($driver1);
    $session2 = new \Behat\Mink\Session($driver2);

    // start sessions
    $session1->start();
    $session2->start();

    $session1->visit('http://my_project.dev/chat.php');
    $session2->visit('http://my_project.dev/chat.php');

.. caution::

    The state of a session is actually managed by the driver. This means
    that each session must use a different driver instance.

Isn't it cool? But Mink makes it even cooler:

.. code-block:: php

    $mink = new \Behat\Mink\Mink();
    $mink->registerSession('goutte', $goutteSession);
    $mink->registerSession('sahi', $sahiSession);
    $mink->setDefaultSessionName('goutte');

With such configuration, you can talk with your sessions by name through
one single container object:

.. code-block:: php

    $mink->getSession('goutte')->visit('http://my_project.dev/chat.php');
    $mink->getSession('sahi')->visit('http://my_project.dev/chat.php');

.. note::

    Mink will even lazy-start your sessions when needed (on first ``getSession()``
    call). So, the browser will not be started until you really need it!

Or you could even omit the session name in default cases:

.. code-block:: php

    $mink->getSession()->visit('http://my_project.dev/chat.php');

This call is possible thanks to ``$mink->setDefaultSessionName('goutte')``
setting previously. We've set the default session, that would be returned
on ``getSession()`` call without arguments.

.. tip::

    The ``Behat\Mink\Mink`` class also provides an easy way to reset or restart
    your started sessions (and only started ones):

    .. code-block:: php

        // reset started sessions
        $mink->resetSessions();

        // restart started sessions
        $mink->restartSessions();
