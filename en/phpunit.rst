Mink's TestCase for PHPUnit
===========================

Mink comes with a special ``TestCase`` for the `PHPUnit <http://www.phpunit.de>`_
framework. You can use this test case in your PHPUnit acceptance tests:

.. code-block:: php

    <?php
    
    require_once 'mink/autoload.php';
    
    use Behat\Mink\PHPUnit\TestCase;
    
    class MyApplicationTest extends TestCase
    {
        public function testSomePage()
        {
            $this->getSession('goutte')
                 ->visit('http://dev.dev/page1.php');
            // or:
            $this->getSession()->visit('http://dev.dev/page1.php');
        }
    }

Register Custom Sessions
------------------------

You can configure your own sessions with ``registerSessions()`` method override:

.. code-block:: php

    <?php
    
    require_once 'mink/autoload.php';
    
    use Behat\Mink\Mink,
        Behat\Mink\PHPUnit\TestCase;
    
    class MyApplicationTest extends TestCase
    {
        protected static function registerSessions(Mink $mink)
        {
            $zendParams = array();

            $mink->registerSession('my_gotte',
                $this->initGoutteSession($zendParams)
            );

            // register all default sessions
            parent::registerSessions();
        }

        public function testSomePage()
        {
            $this->getSession('my_gotte')
                 ->visit('http://dev.dev/page1.php');
        }
    }

Reset the Sessions
------------------

Mink will do a ``reset()`` of all started sessions in ``teardown()`` by default.
So if you want to do ``restart()`` actually, you should do it manually:

.. code-block:: php

    public function testSomePage()
    {
        $this->getMink()->restartSessions();

        // ...
    }
