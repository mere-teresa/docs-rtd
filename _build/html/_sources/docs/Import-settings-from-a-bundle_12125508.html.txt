#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Developing with eZ Publish
   5 <Developing-with-eZ-Publish-5_2720528.html>`__
#. `Developer Cookbook <Developer-Cookbook_11403951.html>`__

eZ Publish Platform 5.<next> : Import settings from a bundle
============================================================

Added by jerome.vieilledent@ez.no , edited by jerome.vieilledent@ez.no
on Dec 17, 2013

Icon

The following recipe is valid for any type of settings supported by
Symfony framework.

Use case

Usually, you develop your website using one or several custom bundles as
this is a best practice. However, dealing with core bundles semantic
configuration can be a bit tedious if you maintain it in the main
``ezpublish/config/ezpublish.yml`` configuration file.

This recipe will show you how to import configuration from a bundle the
manual way and the implicit way.

The manual way
--------------

This is the simplest way of doing and it has the advantage to be
explicit. The idea is to use the ``imports`` statement in your main
``ezpublish.yml``:

**ezpublish/config/ezpublish.yml**

.. code:: theme:

    imports:
        # Let's import our template selection rules that reside in our custom bundle.
        # MyCustomBundle is the actual bundle name
        - {resource: "@AcmeTestBundle/Resources/config/templates_rules.yml"}
     
    ezpublish:
        # ...

**templates\_rules.yml, placed under Resources/config folder in
AcmeTestBundle**

.. code:: theme:

    # Here I need to reproduce the right configuration tree.
    # It will be merged with the main one
    ezpublish:
        system:
            my_siteaccess:
                ezpage:
                    layouts:
                        2ZonesLayout1:
                            name: "2 zones (layout 1)"
                            template: "AcmeTestBundle:zone:2zoneslayout1.html.twig"
        
                location_view:
                    full:
                        article_test:
                            template: "AcmeTestBundle:full:article_test.html.twig"
                            match:
                                Id\Location: [144,149]
                        another_test:
                            template: "::another_test.html.twig"
                            match:
                                Id\Content: 142
        
                block_view:
                    campaign:
                        template: "AcmeTestBundle:block:campaign.html.twig"
                        match:
                            Type: "Campaign"

Icon

During the merge process, if the imported configuration files contain
entries that are already defined in the main configuration file, **they
will override them**.

Tip

Icon

If you want to import configuration for development use only, you can do
so in your ``ezpublish_dev.yml`` |(wink)|

The implicit way
----------------

Icon

**Compatible with eZ Publish 5.1+ / Symfony 2.2+** as it uses
``Symfony\Component\DependencyInjection\Extension\PrependExtensionInterface``
which is available as of Symfony 2.2 (`more info on Symfony
cookbook <http://symfony.com/doc/current/cookbook/bundles/prepend_extension.html>`__).

The following example will show you **how to implicitly load settings to
be configure eZ Publish kernel**. Note that this is also valid for any
bundle !

We assume here that you're aware of `service container
extensions <http://symfony.com/doc/current/book/service_container.html#importing-configuration-via-container-extensions>`__.

**Acme/TestBundle/DependencyInjection/AcmeTestExtension**

.. code:: theme:

    <?php
     
    namespace Acme\TestBundle\DependencyInjection;
     
    use Symfony\Component\DependencyInjection\ContainerBuilder;
    use Symfony\Component\Config\FileLocator;
    use Symfony\Component\DependencyInjection\Extension\PrependExtensionInterface;
    use Symfony\Component\HttpKernel\DependencyInjection\Extension;
    use Symfony\Component\DependencyInjection\Loader;
    use Symfony\Component\Yaml\Yaml;
     
    /**
     * This is the class that loads and manages your bundle configuration
     *
     * To learn more see {@link http://symfony.com/doc/current/cookbook/bundles/extension.html}
     */
    class AcmeTestExtension extends Extension implements PrependExtensionInterface
    {
        // ...
     
        /**
         * Allow an extension to prepend the extension configurations.
         * Here we will load our template selection rules.

         *
         * @param ContainerBuilder $container
         */
        public function prepend( ContainerBuilder $container )
        {
            // Loading our YAML file containing our template rules
            $config = Yaml::parse( __DIR__ . '/../Resources/config/template_rules.yml' );
            // We explicitly prepend loaded configuration for "ezpublish" namespace.
            // So it will be placed under the "ezpublish" configuration key, like in ezpublish.yml.
            $container->prependExtensionConfig( 'ezpublish', $config );
        }
    }

**AcmeTestBundle/Resources/config/template\_rules.yml**

.. code:: theme:

    # We explicitly prepend config for "ezpublish" namespace in service container extension, 
    # so no need to repeat it here
    system:
        ezdemo_frontend_group:
            ezpage:
                layouts:
                    2ZonesLayout1:
                        name: "2 zones (layout 1)"
                        template: "AcmeTestBundle:zone:2zoneslayout1.html.twig"
     
            location_view:
                full:
                    article_test:
                        template: "AcmeTestBundle:full:article_test.html.twig"
                        match:
                            Id\Location: 144
                    another_test:
                        template: "::another_test.html.twig"
                        match:
                            Id\Content: 142
     
            block_view:
                campaign:
                    template: "AcmeTestBundle:block:campaign.html.twig"
                    match:
                        Type: "Campaign"

| 

Regarding performance

Icon

Service container extensions are called only when the container is being
compiled, so there is nothing to worry about regarding performance.

Icon

In dev environment, each time you modify files loaded this way, **you'll
need to manually clear** the cache as Symfony doesn't check modification
time on them.

Icon

Loading configuration this way lets you override settings in your main
``ezpublish.yml`` file.

 

 

Comments:
---------

If I have 20 siteaccesses, one for each language, do I have to define
the rules 20 times in the templates\_rules.yml?

|image1| Posted by xrow at Jul 15, 2013 07:59

No, you can simply create a group with the 20 siteaccesses and add the
general definitions to the group

|image2| Posted by pedro.resende@ez.no at Jul 15, 2013 09:45

Document generated by Confluence on Feb 12, 2014 16:43

.. |(wink)| image:: images/icons/emoticons/wink.png
.. |image1| image:: images/icons/contenttypes/comment_16.png
.. |image2| image:: images/icons/contenttypes/comment_16.png
