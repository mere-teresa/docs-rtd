#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Installation and Upgrade
   Guides <Installation-and-Upgrade-Guides_6292016.html>`__
#. `Installation <Installation_7438500.html>`__
#. `Virtual host setup <Virtual-host-setup_7438507.html>`__

eZ Publish Platform 5.<next> : Virtual host example
===================================================

Added by andrea.melo@ez.no , edited by ricardo.correia@ez.no on Sep 25,
2013

 

This example demonstrates how to set up a virtual host on the Apache web
server for an eZ Publish installation located in
"``/var/www/example``\ ". Let's say that we want to access eZ Publish by
using the following URLs:

-  http://www.example.com (actual website for public access)
-  http://admin.example.com (administration interface for the webmaster)

In order to achieve this, we need to set up both eZ Publish and the web
server so that they respond correctly to the different requests.

eZ Publish configuration: siteaccess settings
---------------------------------------------

eZ Publish needs to be configured to use the host access method. This
can be done from within the web based setup wizard or by manually
editing the global override for the site.ini configuration file:
"``<eZ Publish root>/ezpublish_legacy/settings/override/site.ini.append.php``\ ".
A typical legacy configuration would look something like this:

::

    ...
    [SiteAccessSettings]
    AvailableSiteAccessList[]
    AvailableSiteAccessList[]=example
    AvailableSiteAccessList[]=example_admin
    MatchOrder=host
     
    HostMatchMapItems[]=www.example.com;example
    HostMatchMapItems[]=admin.example.com;example_admin
    ...

This configuration tells eZ Publish that it should use the
"``example``\ " siteaccess if a request starts with
"``www.example.com``\ " and "``example_admin``\ " if the request starts
with "``admin.example.com``\ ". For more information about site
management in eZ Publish, please refer to the "`Site
management <http://doc.ez.no/eZ-Publish/Technical-manual/5.x/Concepts-and-basics/Configuration/Site-management>`__\ "
part of the "Concepts and basics" chapter.

Icon

The legacy configuration above will only work if the same is also
configured in Symfony, by adding our `available
siteaccesses <https://confluence.ez.no/display/EZP/Siteaccess+Matching#SiteaccessMatching-Configuration>`__
and `mapping our
hosts <https://confluence.ez.no/display/EZP/Siteaccess+Matching#SiteaccessMatching-Compoundsiteaccessmatcher>`__
to the desired siteaccess in **ezpublish/config/ezpublish.yml**. This
approach is recommended!

 

A Symfony based eZ Publish 5 configuration would look something like
this:

**ezpublish.yml**

.. code:: theme:

    ezpublish:
        siteaccess:
            default_siteaccess: ezdemo_site
            list:
                - ezdemo_site
                - eng
                - fre
                - fr_eng
                - ezdemo_site_admin
            groups:
                ezdemo_site_group:
                    - ezdemo_site
                    - eng
                    - fre
                    - fr_eng
                    - ezdemo_site_admin
            match:
                Map\URI:
                    ezdemo_site: ezdemo_site
                    fre: fre
                    ezdemo_site_admin: ezdemo_site_admin
                # The following mimics host_uri for 2 different configuration.
                Compound\LogicalAnd:
                    -
                        # Nested matchers, with their configuration.
                        # No need to precise their matching values (true will suffice).
                        matchers:
                            Map\URI:
                                the_front: true
                            Map\Host:
                                ezpublish.dev: true
                        # The siteaccess to match in the end
                        match: ezdemo_site 
                    -
                        matchers:
                            Map\URI:
                                the_admin: true
                            Map\Host:
                                ezpublish.dev: true
                        match: ezdemo_site_admin
                # Matching hosts as host: siteaccess
                Map\Host:
                    ezpublish.dev: ezdemo_site
                    admin.ezpublish.dev: ezdemo_site_admin

 

Apache configuration: virtual host settings
-------------------------------------------

Assuming that...

-  eZ Publish is located in "``/var/www/example``\ "
-  the server's IP address is ``128.39.140.28``
-  we wish to access eZ Publish using "``www.example.com``\ " and
   "``admin.example.com``\ "

...the following virtual host configuration needs to be added at the end
of "``http.conf``\ ":

::

    NameVirtualHost 128.39.140.28
     
    <VirtualHost 128.39.140.28>
        <Directory /var/www/example/web>
            Options FollowSymLinks
            AllowOverride None
        </Directory>

        # Environment.
        # Possible values: "prod" and "dev" out-of-the-box, other values possible with proper configuration
        # Defaults to "prod" if omitted
        SetEnv ENVIRONMENT "prod"

        # Whether to use Symfony's ApcClassLoader.
        # Possible values: 0 or 1
        # Defaults to 0 if omitted
        #SetEnv USE_APC_CLASSLOADER 0

        # Prefix used when USE_APC_CLASSLOADER is set to 1.
        # Use a unique prefix in order to prevent cache key conflicts
        # with other applications also using APC.
        # Defaults to "ezpublish" if omitted
        #SetEnv APC_CLASSLOADER_PREFIX "ezpublish"

        # Whether to use debugging.
        # Possible values: 0 or 1
        # Defaults to 0 if omitted, unless ENVIRONMENT is set to: "dev"
        #SetEnv USE_DEBUGGING 0

        # Whether to use Symfony's HTTP Caching.
        # Disable it if you are using an external reverse proxy (e.g. Varnish)
        # Possible values: 0 or 1
        # Defaults to 1 if omitted, unless ENVIRONMENT is set to: "dev"
        #SetEnv USE_HTTP_CACHE 1

        # Defines the proxies to trust.
        # Separate entries by a comma
        # Example: "proxy1.example.com,proxy2.example.org"
        # By default, no trusted proxies are set
        #SetEnv TRUSTED_PROXIES "127.0.0.1"
     
        <IfModule mod_php5.c>
            php_admin_flag safe_mode Off
            php_admin_value register_globals    0
            php_value magic_quotes_gpc  0
            php_value magic_quotes_runtime  0
            php_value allow_call_time_pass_reference 0
        </IfModule>
     
        DirectoryIndex index.php
     
        <IfModule mod_rewrite.c>
            RewriteEngine On
     
            # CVE-2012-6432
            RewriteRule ^/_internal - [R=403,L]
     
            # Uncomment in FastCGI mode, to get basic auth working.
            #RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
            # v1 rest API is on Legacy
            RewriteRule ^/api/[^/]+/v1/ /index_rest.php [L]
     
            # If using cluster, uncomment the following two lines:
            #RewriteRule ^/var/([^/]+/)?storage/images(-versioned)?/.* /index_cluster.php [L]
            #RewriteRule ^/var/([^/]+/)?cache/(texttoimage|public)/.* /index_cluster.php [L]
     
            RewriteRule ^/var/([^/]+/)?storage/images(-versioned)?/.* - [L]
            RewriteRule ^/var/([^/]+/)?cache/(texttoimage|public)/.* - [L]
            RewriteRule ^/design/[^/]+/(stylesheets|images|javascript|fonts)/.* - [L]
            RewriteRule ^/share/icons/.* - [L]
            RewriteRule ^/extension/[^/]+/design/[^/]+/(stylesheets|flash|images|lib|javascripts?)/.* - [L]
            RewriteRule ^/packages/styles/.+/(stylesheets|images|javascript)/[^/]+/.* - [L]
            RewriteRule ^/packages/styles/.+/thumbnail/.* - [L]
            RewriteRule ^/var/storage/packages/.* - [L]
     
            # Makes it possible to place your favicon at the root of your
            # eZ Publish instance. It will then be served directly.
            RewriteRule ^/favicon\.ico - [L]
            # Uncomment the line below if you want you favicon be served
            # from the standard design. You can customize the path to
            # favicon.ico by changing /design/standard/images/favicon\.ico
            #RewriteRule ^/favicon\.ico /design/standard/images/favicon.ico [L]
            RewriteRule ^/design/standard/images/favicon\.ico - [L]
     
            # Give direct access to robots.txt for use by crawlers (Google,
            # Bing, Spammers..)
            RewriteRule ^/robots\.txt - [L]
     
            # Platform for Privacy Preferences Project ( P3P ) related files
            # for Internet Explorer
            # More info here : http://en.wikipedia.org/wiki/P3p
            RewriteRule ^/w3c/p3p\.xml - [L]
     
            # Uncomment the following lines when using popup style debug in legacy
            #RewriteRule ^/var/([^/]+/)?cache/debug\.html.* - [L]
     
            # Following rule is needed to correctly display assets from eZ Publish5 / Symfony bundles
            RewriteRule ^/bundles/ - [L]
     
            # Additional Assetic rules for eZ Publish 5.1 / 2013.4 and higher:
            ## Don't forget to run php ezpublish/console assetic:dump --env=prod
            ## and make sure to comment these out in dev environment.
            RewriteRule ^/css/.*\.css - [L]
            RewriteRule ^/js/.*\.js - [L]
     
            RewriteRule .* /index.php
        </IfModule>
     
        DocumentRoot /var/www/example/web
        ServerName www.example.com
        ServerAlias admin.example.com
    </VirtualHost>

Icon

For instructions on how to set custom environments please refer to
the \ `Installing eZ Publish on a Linux-UNIX based
system <Installing-eZ-Publish-on-a-Linux-UNIX-based-system_7438581.html>`__
documentation page, under "Defining the desired environment".

Icon

Note that it isn't necessary to create a separate virtual host block for
"``admin.example.com``\ ", it can be added to the existing block using
the "``ServerAlias``\ " directive which can take several
values separated by a space.

Comments:
---------

I think this: \ ``RewriteRule ^/bundles/ - [L]``

Needs to become this \ ``RewriteRule ^bundles/ - [L]``

When apache2 is used. The first one seems to work for apache1

|image0| Posted by arnottg at Oct 30, 2013 10:29

Dear Gareth,

The virtual host configuration setting is correct as it is (with the
slash), and works properly on apache2.

Only in .htaccess configurations the slash should be removed.

Hope this helps on clarifying.

|image1| Posted by ricardo.correia@ez.no at Oct 30, 2013 11:00

Document generated by Confluence on Feb 12, 2014 16:43

.. |image0| image:: images/icons/contenttypes/comment_16.png
.. |image1| image:: images/icons/contenttypes/comment_16.png
