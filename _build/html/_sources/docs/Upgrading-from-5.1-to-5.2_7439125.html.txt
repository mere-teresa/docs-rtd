#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Installation and Upgrade
   Guides <Installation-and-Upgrade-Guides_6292016.html>`__
#. `Upgrade <Upgrade_19234967.html>`__

eZ Publish Platform 5.<next> : Upgrading from 5.1 to 5.2
========================================================

Added by gaetano.giunta@ez.no , edited by ricardo.correia@ez.no on Dec
06, 2013

-  `Note on Paths <#Upgradingfrom5.1to5.2-NoteonPaths>`__
-  `Check for
   requirements <#Upgradingfrom5.1to5.2-Checkforrequirements>`__
-  `Upgrade steps <#Upgradingfrom5.1to5.2-Upgradesteps>`__

   -  `Step 1: Upgrade the distribution
      files <#Upgradingfrom5.1to5.2-Step1%3AUpgradethedistributionfiles>`__
   -  `Step 2: upgrade custom
      extensions <#Upgradingfrom5.1to5.2-Step2%3Aupgradecustomextensions>`__
   -  `Step 3: upgrade the
      database <#Upgradingfrom5.1to5.2-Step3%3Aupgradethedatabase>`__
   -  `Step 4: Apply 5.2 configuration
      changes <#Upgradingfrom5.1to5.2-Step4%3AApply5.2configurationchanges>`__

      -  `YAML files <#Upgradingfrom5.1to5.2-YAMLfiles>`__
      -  `ezpublish/EzPublishKernel.php <#Upgradingfrom5.1to5.2-ezpublish%2FEzPublishKernel.php>`__
      -  `composer.json <#Upgradingfrom5.1to5.2-composer.json>`__

   -  `Step 5: Update cluster data (if
      applicable) <#Upgradingfrom5.1to5.2-Step5%3AUpdateclusterdata%28ifapplicable%29>`__

      -  `Option 1: unclusterize, clear data, and
         reclusterize <#Upgradingfrom5.1to5.2-Option1%3Aunclusterize%2Ccleardata%2Candreclusterize>`__
      -  `Option 2: perform a background cleanup of the storage
         table <#Upgradingfrom5.1to5.2-Option2%3Aperformabackgroundcleanupofthestoragetable>`__

   -  `Step 6: Regenerate the autoload array for
      extensions <#Upgradingfrom5.1to5.2-Step6%3ARegeneratetheautoloadarrayforextensions>`__
   -  `Step 7: Link
      assets <#Upgradingfrom5.1to5.2-Step7%3ALinkassets>`__
   -  `Step 8: Update rewrite
      rules <#Upgradingfrom5.1to5.2-Step8%3AUpdaterewriterules>`__
   -  `Step 9: Clear the
      caches <#Upgradingfrom5.1to5.2-Step9%3AClearthecaches>`__
   -  `Step 10: Upgrade Extensions (site
      package) <#Upgradingfrom5.1to5.2-Step10%3AUpgradeExtensions%28sitepackage%29>`__

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

Step 1: Upgrade the distribution files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The easiest way to upgrade the distribution files is to unpack eZ
Publish 5.2 to a separate directory and then copy the directories that
contain site-specific files from the existing 5.1 installation into
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

Step 2: upgrade custom extensions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

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

    /<ezp5-root>/ezpublish_legacy/update/database/<mysql|postgresql>/5.2/dbupdate-5.1.0-to-5.2.0.sql

Step 4: Apply 5.2 configuration changes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

YAML files
''''''''''

Since default configuration files have been overwritten during step one,
the few additions to those files that were made in 5.2 need to be
applied manually to the configuration files. All of those changes
are \ **additions**, none of them replaces what you already have. For
most of them, at least one, if not all hierarchy elements (monolog,
handler, framework, router...) will already be there. All you have to do
is add the missing bits in the existing configuration blocks.

In \ **ezpublish/config/config\_dev.yml**, add the configuration for the
chromephp log handler:

.. code:: theme:

    monolog:
        handlers:
            chromephp:
                type: chromephp
                level: info

.. code:: wordwrap

    In ezpublish/config/config.yml, you need to add a few default values for the framework

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

In \ **routing\_dev.yml**, add the resource import for the
SensioDistributionBundle webconfigurator routes:

.. code:: theme:

    _configurator:
        resource: "@SensioDistributionBundle/Resources/config/routing/webconfigurator.xml"
        prefix: /_configurator

.. code:: wordwrap

    In ezpublish/config/security.yml, add the following:

.. code:: theme:

    access_control:    
        - { path: ^/login, roles: IS_AUTHENTICATED_ANONYMOUSLY, requires_channel: https }

If you have added anything to ``parameters.yml``, we suggest that you
add your custom settings to ``parameters.yml.dist``, so that the
composer post-update script handles those, and generates their values
correctly.

ezpublish/EzPublishKernel.php
'''''''''''''''''''''''''''''

It is not possible to just copy your old ``EzPublishKernel.php`` file
over from the previous installation, since quite a few changes were made
to this file in this release. We suggest that you simply reflect in the
new kernel file any changes you made in the previous version.

composer.json
'''''''''''''

If you had modified composer.json to add your own requirements, you must
re-apply those changes to the new version, and run composer install.

Step 5: Update cluster data (if applicable)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If your installation uses the DFS cluster, you are affected by
the \ `split DFS tables
feature <https://github.com/ezsystems/ezpublish-legacy/blob/master/doc/features/5.2/dfs_split_tables.md>`__ that
was added in 5.2. You can also check the \ `DFS setup
documentation <https://doc.ez.no/doc_hidden/eZ-Publish/Technical-manual/5.x/Features/Clustering/Setting-it-up-for-an-eZDFSFileHandler>`__ document
on steps 3 and 4 for additional details and usage examples about the
newly introduced configurations.

To use the feature, you need to update your DFS database structure. It
won't affect existing data, and doesn't require particular measures.
Import the following file into your\ *cluster* database (it should be
different from your eZ Publish database):

.. code:: wordwrap

    /<ezp5-root>/ezpublish_legacy/update/database/mysql/5.2/dbupdate-cluster-5.1.0-to-5.2.0.sql

The split table feature stores cache and storage into two different
tables. For now, your ezdfsfile table contains both cache and storage.
Starting from now, eZ Publish will use the newly created table,
ezdfsfile\_cache, to store cache. Since the table  is empty, it will
react as if there was no cache, and work without any changes.

However, we recommend that you remove entries related to cache from your
ezdfsfile table. There are several options.

Option 1: unclusterize, clear data, and reclusterize
                                                    

*This method requires that you shutdown the website completely* for a
little while. It mainly applies to small/medium websites (up to a couple
thousand content objects).

May require a lot of disk space

Icon

Since this method will create a local copy of every storage file (not
including cache) from your NFS to the local server, this method may
require a large amount of disk space. You can get an estimate of the
required space by running the following query on your cluster database:

.. code:: theme:

    SELECT SUM(size) from ezdfsfile WHERE name LIKE 'var/ezdemo\_site/storage/%';

It will give you the total size of storage items, in bytes. Remember to
escape \_ in your vardir.

Use the following commands from
your \ */<ezp5-root>/ezpublish\_legacy/* folder:

.. code:: theme:

    cd ezpublish_legacy
    bin/php/clusterize.php -u

Next, truncate the ezdfsfile table from your database:

.. code:: theme:

    TRUNCATE TABLE ezdfsfile

And finally, re-create cluster data based on local data:

.. code:: theme:

    cd ezpublish_legacy
    bin/php/clusterize.php

Option 2: perform a background cleanup of the storage table
                                                           

An upgrade script is provided that will let you cleanup  the ezdfsfile
table on a live website, even with a large cluster. It will delete a
configurable (by default 1000) batch of cache rows from ezdfsfile, and
sleep (by default for 100 ms) between batches. Run the following from
the legacy root:

.. code:: theme:

    cd ezpublish_legacy
    php update/common/scripts/5.2/cleanupdfscache.php -h

The script can be interrupted and restarted anytime without risks for
the system. We strongly suggest, if you execute it on a live website,
that you monitor your database server's performances, and increase the
sleep delay and/or decrease the limit if the SQL server's load is too
high.

Step 6: Regenerate the autoload array for extensions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To regenerate the autoload array, execute the following script from the
root of your eZ Publish Legacy directory:

.. code:: theme:

    cd ezpublish_legacy
    php bin/php/ezpgenerateautoloads.php --extension

Step 7: Link assets
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

Step 8: Update rewrite rules
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are two ways eZ Publish 5 can be installed, either the full
install with both the new Symfony stack and the legacy stack, or legacy
only. In latter case you only need to point your '4.7 like' rewrite
rules to \ */<ezp5-root>/ezpublish\_legacy/* and that's it.
Otherwise, update your virtual host according to \ `the eZ Publish 5.2
rewrite rules on
confluence <https://confluence.ez.no/display/EZP/Virtual+host+setup>`__ and
point your host configuration to \ */<ezp5-root>/web/*.

Step 9: Clear the caches
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

Step 10: Upgrade Extensions (site package)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Next, depending on if you originally installed eZ Flow, eZ Webin or eZ
Demo site, follow the steps mentioned in the \ `eZ
Webin <https://doc.ez.no/doc_hidden/Extensions/eZ-Publish-extensions/Website-Interface/Website-interface-5.2-upgrade>`__, \ `eZ
Flow <https://doc.ez.no/doc_hidden/Extensions/eZ-Publish-extensions/eZ-Flow/Upgrading-eZ-Flow/eZ-Flow-5.2-upgrade>`__ or `eZ
Demo <https://doc.ez.no/doc_hidden/Extensions/eZ-Publish-extensions/eZ-Demo/eZ-Demo-5.2-upgrade>`__ upgrade
documentation.

Comments:
---------

Option 1 (Step 4) should come with a diskspace warning, to make it clear
that it's not an options for large sites!

|image0| Posted by arnottg at Dec 04, 2013 13:56

This is a good point, \ `Gareth
Arnott <https://confluence.ez.no/display/~arnottg>`__. I'll add a
warning about that.

|image1| Posted by bertrand.dunogier@ez.no at Dec 04, 2013 14:15

What do you think, \ `Gareth
Arnott <https://confluence.ez.no/display/~arnottg>`__ ? Sufficient ?

|image2| Posted by bertrand.dunogier@ez.no at Dec 04, 2013 14:37

Looks good to me!

|image3| Posted by arnottg at Dec 04, 2013 14:50

Document generated by Confluence on Feb 12, 2014 16:43

.. |image0| image:: images/icons/contenttypes/comment_16.png
.. |image1| image:: images/icons/contenttypes/comment_16.png
.. |image2| image:: images/icons/contenttypes/comment_16.png
.. |image3| image:: images/icons/contenttypes/comment_16.png
