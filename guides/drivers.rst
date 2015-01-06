Drivers
=======

How does Mink provide a consistent API for very different browser library
types, often written in different languages? Through drivers! A Mink driver
is a simple class, that implements ``Behat\Mink\Driver\DriverInterface``.
This interface describes bridge methods between Mink and real browser emulators.
Mink always talks with browser emulators through its driver. It doesn't know
anything about how to start/stop or traverse page in that particular browser
emulator. It only knows what driver method it should call in order to do this.

Mink comes with five drivers out of the box:

.. toctree::
    :maxdepth: 1

    /drivers/goutte
    /drivers/selenium2
    /drivers/zombie
    /drivers/sahi
    /drivers/selenium

.. todo:: Build a table of the features supported by each driver
