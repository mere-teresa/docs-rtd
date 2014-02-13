#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Developing with eZ Publish
   5 <Developing-with-eZ-Publish-5_2720528.html>`__
#. `MVC and Application <MVC-and-Application_2719826.html>`__
#. `Configuration <Configuration_2720538.html>`__

eZ Publish Platform 5.<next> : Legacy configuration
===================================================

Added by jerome.vieilledent@ez.no , edited by ricardo.correia@ez.no on
May 21, 2013

 

-  `LegacyConfigResolver <#Legacyconfiguration-LegacyConfigResolver>`__

   -  `Example <#Legacyconfiguration-Example>`__

-  `Best practices for
   migration <#Legacyconfiguration-Bestpracticesformigration>`__

   -  `Note for extension
      developers <#Legacyconfiguration-Noteforextensiondevelopers>`__

      -  `Example for
         SQLIImport <#Legacyconfiguration-ExampleforSQLIImport>`__

As eZ Publish legacy is part of eZ Publish 5, you might want to access
its settings (i.e. not migrated extension). Regarding this, a
**LegacyConfigResolver** has been implemented, allowing the access to
the legacy settings.

LegacyConfigResolver
--------------------

The ``LegacyConfigResolver`` works in the same way the main
``ConfigResolver`` presented in the `configuration
documentation <Configuration_2720538.html>`__. However, it has its own
specification.

-  **Parameter name** must be the combination of the INI block and
   parameter name
   (**``<iniGroupName>.<paramName>``**, e.g. ``DebugSettings.DebugOutput``).
-  **Namespace** is the **INI file name**, without .ini suffix (e.g.
   **image** for *image.ini*).
-  **Scope** can still be a specific siteaccess to look into (still
   defaults to current siteaccess)

Example
~~~~~~~

.. code:: theme:

    // Inside a controller

    $legacyResolver = $this->get( 'ezpublish_legacy.config.resolver' );
    // Get [DebugSettings].DebugOutput from site.ini
    $debugOutput = $legacyResolver->getParameter( 'DebugSettings.DebugOutput' );
    // Get [ImageMagick].ExecutablePath from image.ini
    $imageMagickPath = $legacyResolver->getParameter( 'ImageMagick.ExecutablePath', 'image' );
    // Get [DatabaseSettings].Database from site.ini, for ezdemo_site_admin siteaccess
    $databaseName = $legacyResolver->getParameter( 'DatabaseSettings.Database', 'site', 'ezdemo_site_admin' );

    // Note that the examples above are also applicable for hasParameter().

Best practices for migration
----------------------------

It might happen that your eZ Publish application still relies on legacy
extensions (yours or from the community) that have not been migrated yet
to eZ Publish 5. In this case the **LegacyConfigResolver** can be
useful, but the problem is that once the extension is migrated you will
then need to change your code to use the main config resolver, which can
be a bit tedious on large applications.

Actually, the *main* config resolver is set in such a way that **it
chains the different available resolvers** (i.e. the main one and the
legacy one). The **ChainConfigResolver** always checks the main
``ConfigResolver`` first, and if no parameter is found, it fallbacks to
the ``LegacyConfigResolver``.

So the example described above could be done in the following way:

.. code:: theme:

    // Inside a controller

    // Get the (chain) ConfigResolver
    $configResolver = $this->getConfigResolver();
    // Get [DebugSettings].DebugOutput from site.ini
    $debugOutput = $configResolver->getParameter( 'DebugSettings.DebugOutput' );
    // Get [ImageMagick].ExecutablePath from image.ini
    $imageMagickPath = $configResolver->getParameter( 'ImageMagick.ExecutablePath', 'image' );
    // Get [DatabaseSettings].Database from site.ini, for ezdemo_site_admin siteaccess
    $databaseName = $configResolver->getParameter( 'DatabaseSettings.Database', 'site', 'ezdemo_site_admin' );

The main difference is that **we don't explicitly call the
LegacyConfigResolver**. The benefit is that once the settings are
migrated to work with eZ Publish 5 (and assuming that these settings are
named in the same way), there will be absolutely no change to do in your
code! The **ChainConfigResolver** will seamlessly use the new
configuration files.

Note for extension developers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Considering what was explained above, the best practice for extension
migration regarding configuration is to keep a compatible format.

Example for SQLIImport
^^^^^^^^^^^^^^^^^^^^^^

**Legacy INI settings:**

**sqliimport.ini for ezdemo\_site siteaccess**

.. code:: theme:

    [ImportSettings]
    # User ID of the XML Import Robot
    # If none provided, site.ini [UserSettings]/AnonymousUserID will be used
    RobotUserID=14

    # Max time to wait for each Stream (in seconds)
    StreamTimeout=50 

**Would become:**

.. code:: theme:

    parameters:
        # Namespace is sqliimport (formerly sqliimport.ini), scope is defined to ezdemo_site siteaccess
        sqliimport.ezdemo_site.ImportSettings.RobotUserID: 14
        sqliimport.ezdemo_site.ImportSettings.StreamTimeout: 50

**Usage in a controller:**

.. code:: theme:

    // Assuming that current siteaccess is ezdemo_site
    // The following code will work regardless using the legacy extension or the migrated one.
    $resolver = $this->getConfigResolver();
    $robotUserId = $resolver->getParameter( 'ImportSettings.RobotUserID', 'sqliimport' );
    $streamTimeout = $resolver->getParameter( 'ImportSettings.StreamTimeout', 'sqliimport' );

 

 

Document generated by Confluence on Feb 12, 2014 16:43
