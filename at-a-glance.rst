Mink at a Glance
================

Mink provides you with a consistent API that can be used with multiple browser
testing suites in order to choose the best tool for the task at hand - without
rewriting your tests.

There's a huge number of browser emulators out there, like `Goutte`_, `Selenium`_,
`Sahi`_ and others. They all do a similar job, but approach the problem differently
and have very different APIs. There are two main types of browser emulators out there:

* Headless browser emulators
* Browser controllers

The first, browser emulators, are pure HTTP specification implementations like
`Goutte`_. Those browser emulators send a real HTTP requests against an application
and parse the response content. They are very simple to run and configure,
because this type of emulators can be written in any available programming
language and can be run in the console on servers without a GUI. Headless
emulators have both advantages and disadvantages. The advantages are simplicity,
speed and ability to run it without the need of a real browser. But, this
type of emulator has one big disadvantage. They have no JavaScript/AJAX support.
So, you can't test your rich GUI web applications with headless browsers.

The second type of browser emulators are browser controllers. These emulators aim
to control a real browser. That's right, a program to control another program.
Browser controllers simulate user interactions in browser and are able to
retrieve actual information from the current browser page. `Selenium`_ and `Sahi`_
are the two most famous browser controllers. The main advantage of browser
controllers is the support for JavaScipt/AJAX interactions on a page. The disadvantage
is that such browser emulators require an installed browser as well as extra configuration
and are usually much slower than their headless counterparts.

So, the easy answer is to choose the best emulator for your project and use
its API for testing. However, as we've already seen, both types have 
advantages and disadvantages. If you choose a headless browser emulator, you
will not be able to test your JavaScript/AJAX pages. And if you choose a browser controller,
your overall test suite will become very slow at some point. So, in the real
world we should use both! And that's why you need a **Mink**.

**Mink** removes API differences between different browser emulators providing
different drivers (read in :doc:`/guides/drivers` chapter) for every browser
emulator and providing you with an easy way to control the browser (:doc:`/guides/session`),
traverse pages (:doc:`/guides/traversing-pages`), manipulate page elements
(:doc:`/guides/manipulating-pages`) or interact with them (:doc:`/guides/interacting-with-pages`).

.. _Goutte: https://github.com/FriendsOfPHP/Goutte
.. _Sahi: http://sahi.co.in/w/
.. _Selenium: http://seleniumhq.org/
