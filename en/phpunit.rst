Mink's TestCase for PHPUnit
===========================

Mink comes with special ``TestCase`` for `PHPUnit <http://www.phpunit.de>`_
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

You can configure your own sessions with ``setUp()`` OR ``registerSessions()``
methods override:

.. code-block:: php

    <?php
    
    require_once 'mink/autoload.php';
    
    use Behat\Mink\Mink,
        Behat\Mink\PHPUnit\TestCase;
    
    class MyApplicationTest extends TestCase
    {
        protected function setUp()
        {
            // don't forget to call parent setUp
            // in order to be able to use register
            // default sessions:
            parent::setUp();
        }

        protected function registerSessions(Mink $mink)
        {
            if (!$mink->hasSession('my_gotte')) {
                $zendParams = array();

                $mink->registerSession('my_gotte',
                    $this->initGoutteSession($zendParams)
                );
            }
        }

        public function testSomePage()
        {
            $this->getSession('my_gotte')
                 ->visit('http://dev.dev/page1.php');
        }
    }

Reset the Sessions
------------------

Mink will do ``reset()`` of all started sessions in ``teardown()`` by default. So
if you want to do ``restart()`` actually, you should do it manually:

.. code-block:: php

    public function testSomePage()
    {
        $this->getMink()->restartSessions();

        // ...
    }
