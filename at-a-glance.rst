Mink at a Glance
================

There are a huge number of browser emulators out there, like `Goutte`_, `Selenium`_,
`Sahi`_ and others. They all do the same job but they behave differently and have very different API's. What's more important is that there are actually two completely different types of browser emulators out there:

* Headless browser emulators
* Browser controllers

The first type of browser emulators are simple pure HTTP specification implementations, like
`Goutte`_. These browser emulators run real HTTP requests against an application
and parse the response content. They are very simple to run and configure,
because these type of emulators can be written in any available programming
language and can be run via the command line on servers without GUI. Headless
emulators have both advantages and disadvantages. The advantages are simplicity,
speed and the ability to run it without the need of a real browser. These
types of emulator have one big disadvantage in that they have no JS/AJAX support.
This means that you can't test your rich GUI web applications with headless browsers.

The second type of browser emulators are browser controllers. These emulators aim
to manipulate the real browser. That's right, a program to control another program.
Browser controllers simulate user interactions on a browser and are able to
retrieve actual information from current browser page. `Selenium`_ and `Sahi`_
are the two most famous browser controllers. The main advantage of browser
controller usage is the support for JS/AJAX interactions on a page. The disadvantages
are that such browser emulators require the installed browser, extra configuration
and are usually much slower than their headless counterparts.

So, the easy answer is to choose the best emulator for your project and use
its API for testing. But as we've already seen, both browser emulator types have both
advantages and disadvantages. If you choose headless browser emulators, you
will not be able to test your JS/AJAX pages, and if you choose browser controllers then
your overall test suite will become very slow at some point. So, in the real
world we should use both! And that's why you need a **Mink**.

**Mink** removes the API differences between different browser emulators by providing
different drivers (read in :doc:`/guides/drivers` chapter) for every browser
emulator and providing you with an easy way to control the browser (:doc:`/guides/session`),
traverse pages (:doc:`/guides/traversing-pages`), manipulate page elements
(:doc:`/guides/manipulating-pages`) or interact with them (:doc:`/guides/interacting-with-pages`).

.. _Goutte: https://github.com/FriendsOfPHP/Goutte
.. _Sahi: http://sahi.co.in/w/
.. _Selenium: http://seleniumhq.org/
