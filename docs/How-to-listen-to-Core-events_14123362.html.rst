#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Developing with eZ Publish
   5 <Developing-with-eZ-Publish-5_2720528.html>`__
#. `Developer Cookbook <Developer-Cookbook_11403951.html>`__

eZ Publish Platform 5.<next> : How to listen to Core events
===========================================================

Added by jerome.vieilledent@ez.no , edited by jerome.vieilledent@ez.no
on Oct 25, 2013

Version compatibility

Icon

This recipe is compatible with \ **eZ Publish 5.2 / 2013.07**

-  `Description <#HowtolistentoCoreevents-Description>`__
-  `Registering a Slot for a given
   Signal <#HowtolistentoCoreevents-RegisteringaSlotforagivenSignal>`__
-  `Using a basic Symfony event
   listener <#HowtolistentoCoreevents-UsingabasicSymfonyeventlistener>`__

   -  `Simple example <#HowtolistentoCoreevents-Simpleexample>`__

Description
-----------

When you interact with the Public API, and with the content Repository
in particular, **Signals** may be sent out, allowing you to react on
actions triggered by the Repository. Those signals can be received by
dedicated services called **Slots**.

Icon

To learn more about SignalSlot in eZ Publish, please refer to the
`dedicated documentation page <Signal-Slot_3506264.html>`__.

`Signals reference <Signals-reference_14123454.html>`__

This recipe will describe how to register a Slot for a dedicated Signal.

Registering a Slot for a given Signal
-------------------------------------

As described in the `SignalSlot
documentation <Signal-Slot_3506264.html>`__, a Slot is roughly like an
event listener and must extend \ ``eZ\Publish\Core\SignalSlot\Slot``.

A typical implementation is the following:

**OnPublishSlot**

.. code:: theme:

    namespace Acme\TestBundle\Slot;
     
    use eZ\Publish\Core\SignalSlot\Slot as BaseSlot;
    use eZ\Publish\API\Repository\Repository;
    use eZ\Publish\SignalSlot\Signal;
     
    class OnPublishSlot extends BaseSlot
    {
        /**
         * @var \eZ\Publish\API\Repository\ContentService
         */
        private $contentService;

        public function __construct( ContentService $contentService )
        {
            $this->contentService = $contentService;
        }
     
        public function receive( Signal $signal )
        {
            if ( !$signal instanceof Signal\ContentService\PublishVersionSignal )
            {
                return;
            }

            // Load published content
            $content = $this->contentService->loadContent( $signal->contentId, null, $signal->versionNo );
            // Do stuff with it...
        }
    }

``OnPublishSlot`` now needs to be registered as a service in the
ServiceContainer and identified as a valid Slot:

**services.yml (in your bundle)**

.. code:: theme:

    parameters:
        my.onpublish_slot.class: Acme\TestBundle\Slot\OnPublishSlot
     
    services:
        my.onpublish_slot:
            class: %my.onpublish_slot.class%
            arguments: [@ezpublish.api.service.content]
            tags:
                - { name: ezpublish.api.slot, signal: ContentService\PublishVersionSignal }

Service tag **``ezpublish.api.slot``** identifies your service as a
valid Slot. The signal part (mandatory) says that this slot is listening
to **``ContentService\PublishVersionSignal``** (shortcut for
``\eZ\Publish\Core\SignalSlot\Signal\ContentService\PublishVersionSignal``).

Icon

Internal signals emitted by Repository services are always relative to
``eZ\Publish\Core\SignalSlot\Signal`` namespace.

Hence ``ContentService\PublishVersionSignal`` means
``eZ\Publish\Core\SignalSlot\Signal\ContentService\PublishVersionSignal``.

Tip

Icon

You can register a slot for any kind of signal by setting ``signal`` to
``*`` in the service tag.

Using a basic Symfony event listener
------------------------------------

eZ Publish comes with a generic slot that converts signals (including
ones defined by user code) to regular event objects and expose them via
the EventDispatcher. This makes it possible to implement a simple event
listener/subscriber if you're more comfortable with this approach.

All you need to do is to implement an event listener or subscriber and
register it.

Simple example
~~~~~~~~~~~~~~

This very simple example will just log the received signal.

**services.yml (in your bundle)**

.. code:: theme:

    parameters:
        my.signal_listener.class: Acme\TestBundle\EventListener\SignalListener
     
    services:
        my.signal_listener:
            class: %my.signal_listener.class%
            arguments: [@logger]
            tags:
                - { name: kernel.event_subscriber }

.. code:: theme:

    <?php
    namespace Acme\TestBundle\EventListener;

    use eZ\Publish\Core\MVC\Symfony\Event\SignalEvent;
    use eZ\Publish\Core\MVC\Symfony\MVCEvents;
    use Psr\Log\LoggerInterface;
    use Symfony\Component\EventDispatcher\EventSubscriberInterface;

    class SignalListener implements EventSubscriberInterface
    {
        /**
         * @var \Psr\Log\LoggerInterface
         */
        private $logger;

        public function __construct( LoggerInterface $logger )
        {
            $this->logger = $logger;
        }

        public function onAPISignal( SignalEvent $event )
        {
            $signal = $event->getSignal();
            // You may want to check the signal type here to react accordingly
            $this->logger->debug( 'Received Signal: ' . print_r( $signal, true ) );
        }

        public static function getSubscribedEvents()
        {
            return array(
                MVCEvents::API_SIGNAL => 'onAPISignal'
            );
        }
    }

 

 

Document generated by Confluence on Feb 12, 2014 16:43
