Mink at a Glance
================

There's huge amount of browser emulators out there, like `Goutte`_, `Selenium`_,
`Sahi`_ and others. They all do the same job, but do it very differently.
They behave differently and have very different API's. But, what's more important,
there is actually 2 completely different types of browser emulators out there:

* Headless browser emulators
* Browser controllers

First type browsers are simple pure HTTP specification implementations, like
`Goutte`_. Those browser emulators send a real HTTP requests against an application
and parse the response content. They are very simple to run and configure,
because this type of emulators can be written in any available programming
language and can be run through console on servers without GUI. Headless
emulators have both advantages and disadvantages. Advantages are simplicity,
speed and ability to run it without the need of a real browser. But this
type of browsers has one big disadvantage, they have no JS/AJAX support.
So, you can't test your rich GUI web applications with headless browsers.

Second browser emulators type are browser controllers. Those emulators aims
to control the real browser. That's right, a program to control another program.
Browser controllers simulate user interactions on browser and are able to
retrieve actual information from current browser page. `Selenium`_ and `Sahi`_
are the two most famous browser controllers. The main advantage of browser
controllers usage is the support for JS/AJAX interactions on page. Disadvantage
is that such browser emulators require the installed browser, extra configuration
and are usually much slower than headless counterparts.

So, the easy answer is to choose the best emulator for your project and use
its API for testing. But as we've already seen, both browser types have both
advantages and disadvantages. If you choose headless browser emulator, you
will not be able to test your JS/AJAX pages. And if you choose browser controller,
your overall test suite will become very slow at some point. So, in real
world we should use both! And that's why you need a **Mink**.

**Mink** removes API differences between different browser emulators providing
different drivers (read in :doc:`/guides/drivers` chapter) for every browser
emulator and providing you with the easy way to control the browser (:doc:`/guides/session`),
traverse pages (:doc:`/guides/traversing-pages`), manipulate page elements
(:doc:`/guides/manipulating-pages`) or interact with them (:doc:`/guides/interacting-with-pages`).

.. _Goutte: https://github.com/FriendsOfPHP/Goutte
.. _Sahi: http://sahi.co.in/w/
.. _Selenium: http://seleniumhq.org/
