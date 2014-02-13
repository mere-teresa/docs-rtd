#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Developing with eZ Publish
   5 <Developing-with-eZ-Publish-5_2720528.html>`__
#. `Extending eZ Publish 5 <Extending-eZ-Publish-5_1736733.html>`__

eZ Publish Platform 5.<next> : Content and Location view providers
==================================================================

Added by jerome.vieilledent@ez.no , edited by patrick.allaert@ez.no on
Aug 26, 2013

-  `View\\Manager &
   View\\Provider <#ContentandLocationviewproviders-View%5CManager%26View%5CProvider>`__

   -  `Provided View\\Provider
      implementations <#ContentandLocationviewproviders-ProvidedView%5CProviderimplementations>`__

-  `Custom
   View\\Provider <#ContentandLocationviewproviders-CustomView%5CProvider>`__

   -  `Difference
      between View\\Provider\\Location and View\\Provider\\Content <#ContentandLocationviewproviders-DifferencebetweenView%5CProvider%5CLocationandView%5CProvider%5CContent>`__
   -  `When to develop a custom
      View\\Provider\\(Location\|Content) <#ContentandLocationviewproviders-WhentodevelopacustomView%5CProvider%5C%28Location%7CContent%29>`__
   -  `Example <#ContentandLocationviewproviders-Example>`__

View\\Manager & View\\Provider
==============================

The role of the ``(eZ\Publish\Core\MVC\Symfony\)View\Manager`` is to
select the right template for displaying a given content or location. It
aggregates objects called *content and location view providers* which
respectively
implement \ ``eZ\Publish\Core\MVC\Symfony\View\Provider\Content`` and ``eZ\Publish\Core\MVC\Symfony\View\Provider\Location`` interfaces.

Each time a content is to be displayed through the
``Content\ViewController``, the ``View\Manager`` iterates over the
registered content or location \ ``View\Provider`` objects and
calls \ ``getView``\ ``()``.

Provided View\\Provider implementations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Name

Usage

``View provider configuration``

| Based on application configuration.
| Formerly known as *Template override system*.

``eZ\Publish\Core\MVC\Legacy\View\Provider\Content``

``eZ\Publish\Core\MVC\Legacy\View\Provider\Location``

| Forwards view selection to the legacy kernel by running the old
content/view module.
| Pagelayout used is the one configured in
``ezpublish_legacy.<scope>.view_default_layout``.
| For more details about the ``<scope>`` please refer to the `scope
configuration <https://confluence.ez.no/display/EZP/Configuration#Configuration-Scope>`__
documentation.

Custom View\\Provider
---------------------

Difference between \ ``View\Provider\Location`` and ``View\Provider\Content``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  A \ ``View\Provider\Location`` only deals with \ ``Location`` objects
   and
   implements \ ``eZ\Publish\Core\MVC\Symfony\View\Provider\Location`` interface.
-  A \ ``View\Provider\Content`` only deals
   with \ ``ContentInfo`` objects and
   implements \ ``eZ\Publish\Core\MVC\Symfony\View\Provider\Content`` interface.

When to develop a custom ``View\Provider\(Location|Content)``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  You want a custom template selection based on a very specific state
   of your application
-  You depend on external resources for view selection
-  You want to override the default one (based on configuration) for
   some reason

``View\Provider`` objects need to be properly registered in the service
container with the ``ezpublish.location_view_provider``
or \ ``ezpublish.content_view_provider`` service tag.

.. code:: theme:

    parameters:
        acme.location_view_provider.class: Acme\DemoBundle\Content\MyLocationViewProvider

    services:
        acme.location_view_provider:
            class: %ezdemo.location_view_provider.class%
            tags:
                - {name: ezpublish.location_view_provider, priority: 30}

Tag attribute name

Usage

priority

An integer giving the priority to the
``View\Provider\(Content|Location)`` in the ``View\Manager``.

The priority range is **from -255 to 255**

Example
~~~~~~~

**Custom View\\Provider\\Location**

.. code:: theme:

    <?php

    namespace Acme\DemoBundle\Content;

    use eZ\Publish\Core\MVC\Symfony\View\ContentView;
    use eZ\Publish\Core\MVC\Symfony\View\Provider\Location as LocationViewProvider;
    use eZ\Publish\API\Repository\Values\Content\Location;

    class MyLocationViewProvider implements LocationViewProvider
    {
        /**
         * Returns a ContentView object corresponding to $location, or void if not applicable
         *
         * @param \eZ\Publish\API\Repository\Values\Content\Location $location
         * @param string $viewType
         * @return \eZ\Publish\Core\MVC\Symfony\View\ContentView|null
         */
        public function getView( Location $location, $viewType )
        {
            // Let's check location Id
            switch ( $location->id )
            {
                // Special template for home page, passing "foo" variable to the template
                case 2:
                    return new ContentView( "AcmeDemoBundle:$viewType:home.html.twig", array( 'foo' => 'bar' ) );
            }
     
            // ContentType identifier (formerly "class identifier")
            switch ( $contentInfo->contentType->identifier )
            {
                // For view full, it will load AcmeDemoBundle:full:small_folder.html.twig
                case 'folder':
                    return new ContentView( "AcmeDemoBundle:$viewType:small_folder.html.twig" );
            }
        }
    }

 

Comments:
---------

Hello,

Example Custom ContentViewProvider is not working. Path
to eZ\\Publish\\MVC\\View\\ContentViewProvider is wrong with eZ 5.0
platform 

|image0| Posted by philippe.vincent-royol@ez.no at Dec 07, 2012 14:35

`Philippe
Vincent-Royol <https://confluence.ez.no/display/~philippe.vincent-royol@ez.no>`__:
Fixed, thanks \ |(smile)|

|image2| Posted by jerome.vieilledent@ez.no at Jan 29, 2013 14:40

Document generated by Confluence on Feb 12, 2014 16:43

.. |image0| image:: images/icons/contenttypes/comment_16.png
.. |(smile)| image:: images/icons/emoticons/smile.png
.. |image2| image:: images/icons/contenttypes/comment_16.png
