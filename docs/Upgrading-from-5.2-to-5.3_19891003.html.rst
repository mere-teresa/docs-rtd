#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Installation and Upgrade
   Guides <Installation-and-Upgrade-Guides_6292016.html>`__
#. `Upgrade <Upgrade_19234967.html>`__

eZ Publish Platform 5.<next> : Upgrading from 5.2 to 5.3
========================================================

Added by jerome.vieilledent@ez.no , edited by jerome.vieilledent@ez.no
on Feb 03, 2014

-  `Note on Paths <#Upgradingfrom5.2to5.3-NoteonPaths>`__
-  `Check for
   requirements <#Upgradingfrom5.2to5.3-Checkforrequirements>`__
-  `Upgrade steps <#Upgradingfrom5.2to5.3-Upgradesteps>`__

   -  `Step 1: Upgrade the legacy distribution
      files <#Upgradingfrom5.2to5.3-Step1%3AUpgradethelegacydistributionfiles>`__
   -  `Step 2: upgrade custom legacy
      extensions <#Upgradingfrom5.2to5.3-Step2%3Aupgradecustomlegacyextensions>`__
   -  `Step 3: upgrade the
      database <#Upgradingfrom5.2to5.3-Step3%3Aupgradethedatabase>`__
   -  `Step 4: Apply 5.3 configuration
      changes <#Upgradingfrom5.2to5.3-Step4%3AApply5.3configurationchanges>`__

      -  `YAML files <#Upgradingfrom5.2to5.3-YAMLfiles>`__
      -  `Templates <#Upgradingfrom5.2to5.3-Templates>`__
      -  `ezpublish/EzPublishKernel.php <#Upgradingfrom5.2to5.3-ezpublish%2FEzPublishKernel.php>`__
      -  `composer.json <#Upgradingfrom5.2to5.3-composer.json>`__
      -  `Varnish (if
         applicable) <#Upgradingfrom5.2to5.3-Varnish%28ifapplicable%29>`__

   -  `Step 5: Regenerate the autoload array for
      extensions <#Upgradingfrom5.2to5.3-Step5%3ARegeneratetheautoloadarrayforextensions>`__
   -  `Step 6: Link
      assets <#Upgradingfrom5.2to5.3-Step6%3ALinkassets>`__
   -  `Step 7: Clear the
      caches <#Upgradingfrom5.2to5.3-Step7%3AClearthecaches>`__
   -  `Step 8: Upgrade Extensions (site
      package) <#Upgradingfrom5.2to5.3-Step8%3AUpgradeExtensions%28sitepackage%29>`__

| 

This section describes how to upgrade your existing eZ Publish 5.1
installation to version 5.2. Make sure that you have a working backup of
the site before you do the actual upgrade, and make sure that the
installation you are performing the upgrade on is offline.

Note on Paths
-------------

-  */<ezp5-root>/*: The root directory where eZ Publish 5 is installed
   in, examples: "*/home/myuser/www*/" or "*/var/sites/ezpublish/*\ "
-  */<ezp5-root>/ezpublish\_legacy/*:  Root directory of "Legacy" (aka
   "Legacy Stack", refers to the eZ Publish 4.x installation which is
   bundled with eZ Publish 5) normally inside "*ezpublish\_legacy*/",
   example: "*/home/myuser/www/ezpublish\_legacy/*\ "

Check for requirements
----------------------

php 5.3.3 and higher is needed. Further information regarding system
requirements can be found on \ `Requirements Documentation
Page <https://confluence.ez.no/display/EZP/Requirements>`__.

Upgrade steps
-------------

Step 1: Upgrade the legacy distribution files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The easiest way to upgrade the distribution files is to unpack eZ
Publish 5.3 to a separate directory and then copy the directories that
contain site-specific files from the existing 5.2 installation into
"*/<ezp5-root>/*\ ". Make sure you copy the following directories:

-  ``ezpublish_legacy/design/<your designs>`` (do NOT include built-in
   designs: admin, base, standard or admin2)
-  ``ezpublish_legacy/var/<your_var_dir>/storage``
-  ``ezpublish_legacy/var/storage/packages``
-  ``ezpublish_legacy/settings/siteaccess/<your_siteaccesses>``
-  ``ezpublish_legacy/settings/override/*``
-  ``ezpublish_legacy/extension/*`` (not including the built-in /
   standalone ones: ezflow, ezjscore, ezoe, ezodf, ezie, ezmultiupload,
   ezmbpaex, ez\_network, ezprestapiprovider, ezscriptmonitor, ezsi,
   ezfind)
-  src/
-  ``config.php`` & ``config.cluster.php``, if applicable
-  ``ezpublish/config/*``

***NB:** Since writable directories and files have been replaced /
copied, their permissions might have changed. You may have to
reconfigure webserver user permissions on specific folders as explained
in the file \ `permissions chapter of the installation
process <https://confluence.ez.no/pages/viewpage.action?pageId=7438581#InstallingeZPublishonaLinux%2FUNIXbasedsystem-Settingupfolderpermission>`__.*

Step 2: upgrade custom legacy extensions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you are using custom extensions, the sub-directories inside the
"extension" directory will also have to be copied from the existing 5.1
installation into "*/<ezp5-root>/ezpublish\_legacy/extension/*\ ".
However, make sure that you do not overwrite any extensions that are
included in eZ Publish distribution, which currently are (***Note:** As
of eZ Publish 5.2, these extensions have the same version number as eZ
Publish*):

Note that upgrading the distribution files will overwrite the autoload
arrays for extensions. You will need to re-generate the autoload arrays
for active extensions later.

***Important: **\ If you plan to upgrade your eZ Website Interface, eZ
Flow or eZ Demo site package as well, then additional extensions will be
updated and the step for re-generate the autoload arrays can be skipped
until that is done (links to documentation for upgrading these site
packages can be found in the last step of this page).*

Step 3: upgrade the database
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Import to your database the changes provided in

.. code:: wordwrap

    /<ezp5-root>/ezpublish_legacy/update/database/<mysql|postgresql>/5.3/dbupdate-5.2.0-to-5.3.0.sql

Step 4: Apply 5.3 configuration changes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

YAML files
^^^^^^^^^^

Since default configuration files have been overwritten during step one,
the few additions to those files that were made in 5.2 need to be
applied manually to the configuration files. All of those changes
are \ **additions**, none of them replaces what you already have. For
most of them, at least one, if not all hierarchy elements (monolog,
handler, framework, router...) will already be there. All you have to do
is add the missing bits in the existing configuration blocks.

In \ **ezpublish/config/config.yml**, you need to add a few default
values for the framework

.. code:: theme:

    framework:
        router:
            resource: "%kernel.root_dir%/config/routing.yml"
            strict_requirements: %kernel.debug%
        trusted_proxies: ~    
        http_method_override: true
     
    twig:    
        debug: %kernel.debug%
        strict_variables: %kernel.debug%

In \ **routing.yml**, add **new login routes** and
the \ ``_ezpublishRestOptionsRoutes`` route loader:

.. code:: theme:

    _ezpublishRestOptionsRoutes:
        resource: "@EzPublishRestBundle/Resources/config/routing.yml"
        prefix: %ezpublish_rest.path_prefix%
        type: rest_options
     
    login:
        path:   /login
        defaults:  { _controller: ezpublish.security.controller:loginAction }
    login_check:
        path:   /login_check
    logout:
        path:   /logout

In \ **ezpublish/config/security.yml**, under ``ezpublish_front``
firewall, update to fit the following (**be sure to remove
``ezpublish: true``**):

.. code:: theme:

    security:
        firewalls:
            ezpublish_front:
                pattern: ^/
                anonymous: ~
                form_login:
                    require_previous_session: false
                logout: ~

If you have added anything to ``parameters.yml``, we suggest that you
add your custom settings to ``parameters.yml.dist``, so that the
composer post-update script handles those, and generates their values
correctly.

Templates
^^^^^^^^^

In your templates, change your links pointing to ``/user/login`` and
``/user/logout`` to appropriate ``login ``/ ``login_check ``/ ``logout``
routes:

**Before**

.. code:: theme:

    <a href="{{ path( 'ez_legacy', {'module_uri': '/user/login'} ) }}">Login</a>
    <form action="{{ path( 'ez_legacy', {'module_uri': '/user/login'} ) }}" method="post">
    <a href="{{ path( 'ez_legacy', {'module_uri': '/user/logout'} ) }}">Logout</a>

**After**

.. code:: theme:

    <a href="{{ path( 'login' ) }}">Login</a>
    <form action="{{ path( 'login_check' ) }}" method="post">
    <a href="{{ path( 'logout' ) }}">Logout</a>

| 

ezpublish/EzPublishKernel.php
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

It is not possible to just copy your old ``EzPublishKernel.php`` file
over from the previous installation, since quite a few changes were made
to this file in this release. We suggest that you simply reflect in the
new kernel file any changes you made in the previous version.

composer.json
^^^^^^^^^^^^^

If you had modified ``composer.json`` to add your own requirements, you
must re-apply those changes to the new version, and run
``composer update``.

Varnish (if applicable)
^^^^^^^^^^^^^^^^^^^^^^^

Anonymous state of a user is not checked through presence of
``is_logged_in`` cookie any more. Therefore, when using Varnish, you
must change the following in your VCL file:

**Before**

.. code:: theme:

    # ez_user_hash sub-routine
    if (req.http.Cookie !~ "is_logged_in=" ) {
        # User don't have "is_logged_in" cookie => Set a hardcoded anonymous hash
        set req.http.X-User-Hash = "38015b703d82206ebc01d17a39c727e5";
    }

**After**

.. code:: theme:

    # ez_user_hash sub-routine
    if (req.http.Cookie !~ "eZSESSID" ) {
        # User don't have session cookie => Set a hardcoded anonymous hash
        set req.http.X-User-Hash = "38015b703d82206ebc01d17a39c727e5";
    }

 

Step 5: Regenerate the autoload array for extensions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To regenerate the autoload array, execute the following script from the
root of your eZ Publish Legacy directory:

.. code:: theme:

    cd ezpublish_legacy
    php bin/php/ezpgenerateautoloads.php --extension

Step 6: Link assets
~~~~~~~~~~~~~~~~~~~

Assets from the various bundles need to be made available for the
webserver through the web/ document root.

The following commands will first symlink eZ Publish 5 assets in
"Bundles" and the second will symlink assets (design files like images,
scripts and css, and files in var folder)  from eZ Publish Legacy:

.. code:: theme:

    php ezpublish/console assets:install --symlink
    php ezpublish/console ezpublish:legacy:assets_install --symlink
    php ezpublish/console assetic:dump --env=prod

Step 7: Clear the caches
~~~~~~~~~~~~~~~~~~~~~~~~

| Whenever an eZ Publish solution is upgraded, all caches must be
cleared in a proper way. This should be done from within a system shell:
| Navigate into the new eZ Publish directory.Run the script using the
following shell command:cd /<ezp5-root>/ezpublish\_legacy/php
bin/php/ezcache.php --clear-all --purgePurging ensures that the caches
are physically removed. When the "--purge" parameter is not specified,
the caches will be expired but not removed.
| Note: Sometimes the script is unable to clear all cache files because
of restrictive file/directory permission settings. Make sure that all
cache files have been cleared by inspecting the contents of the various
cache sub-directories within the "var" directory (typically the
"var/cache/" and "var/<name\_of\_siteaccess>/cache/" directories). If
there are any cache files left, you need to remove them manually.

Step 8: Upgrade Extensions (site package)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Next, depending on if you originally installed eZ Flow, eZ Webin or eZ
Demo site, follow the steps mentioned in the \ `eZ
Webin <https://doc.ez.no/doc_hidden/Extensions/eZ-Publish-extensions/Website-Interface/Website-interface-5.2-upgrade>`__, \ `eZ
Flow <https://doc.ez.no/doc_hidden/Extensions/eZ-Publish-extensions/eZ-Flow/Upgrading-eZ-Flow/eZ-Flow-5.2-upgrade>`__ or `eZ
Demo <https://doc.ez.no/doc_hidden/Extensions/eZ-Publish-extensions/eZ-Demo/eZ-Demo-5.2-upgrade>`__ upgrade
documentation.

Document generated by Confluence on Feb 12, 2014 16:43
