Drivers
=======

How does Mink provide a consistent API for very different browser library
types, often written in different languages? Through drivers! A Mink driver
is a simple class, that implements ``Behat\Mink\Driver\DriverInterface``.
This interface describes bridge methods between Mink and real browser emulators.
Mink always talks with browser emulators through its driver. It doesn't know
anything about how to start/stop or traverse pages in that particular browser
emulator. It only knows what driver method it should call in order to do this.

Mink comes with six drivers out of the box:

.. toctree::
    :maxdepth: 1

    /drivers/goutte
    /drivers/browserkit
    /drivers/selenium2
    /drivers/zombie
    /drivers/sahi
    /drivers/selenium

.. _driver-feature-support:

Driver Feature Support
----------------------

Although Mink does its best to remove browser differences between different
browser emulators, it can't do much in some cases. For example, BrowserKitDriver
cannot evaluate JavaScript and Selenium2Driver cannot get the response status
code. In such cases, the driver will always throw a meaningful
``Behat\Mink\Exception\UnsupportedDriverActionException``.

======================  =================  =========  ======  ========  ====
Feature                 BrowserKit/Goutte  Selenium2  Zombie  Selenium  Sahi
======================  =================  =========  ======  ========  ====
Page traversing         Yes                Yes        Yes     Yes       Yes
Form manipulation       Yes                Yes        Yes     Yes       Yes
HTTP Basic auth         Yes                No         Yes     No        No
Windows management      No                 Yes        No      Yes       Yes
iFrames management      No                 Yes        No      Yes       No
Request headers access  Yes                No         Yes     No        No
Response headers        Yes                No         Yes     No        No
Cookie manipulation     Yes                Yes        Yes     Yes       Yes
Status code access      Yes                No         Yes     No        No
Mouse manipulation      No                 Yes        Yes     Yes       Yes
Drag'n Drop             No                 Yes        No      Yes       Yes
Keyboard actions        No                 Yes        Yes     Yes       Yes
Element visibility      No                 Yes        No      Yes       Yes
JS evaluation           No                 Yes        Yes     Yes       Yes
Window resizing         No                 Yes        No      No        No
Window maximizing       No                 Yes        No      Yes       No
======================  =================  =========  ======  ========  ====
