#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Installation and Upgrade
   Guides <Installation-and-Upgrade-Guides_6292016.html>`__
#. `Installation <Installation_7438500.html>`__

eZ Publish Platform 5.<next> : Virtual host setup
=================================================

Added by andrea.melo@ez.no , edited by gaetano.giunta@ez.no on Jan 14,
2014

 

This section describes how to set up a virtual host for eZ Publish using
the Apache web-server. A virtual host setup is only needed if eZ Publish
has been configured to use the host access method, which is the
recommended, most secure method.

For an introduction and thorough description of the concept of virtual
hosts, you can refer to the `Apache
documentation <http://httpd.apache.org/docs/2.2/vhosts/>`__.

Do not forget to restart the web server after any change of the
configuration files.

Generic virtual host setup
--------------------------

Virtual hosts are usually defined at the bottom of "``httpd.conf``\ ",
which is the main configuration file for Apache, or in a separate
configuration file included from ``httpd.conf``.

Adding a virtual host for eZ Publish can be done by copying the
following lines and replacing the text encapsulated by the square
brackets with actual values. Please refer to the next section for a real
life example of using virtual hosts.

New in eZ Publish 5.2 / 2013.07

Icon

The front controller can now be altered with environment variables,
without the need to modify ``index.php``.

::

    NameVirtualHost [IP_ADDRESS]
     
    <VirtualHost [IP_ADDRESS]:[PORT]>
        <Directory [PATH_TO_EZPUBLISH]/web>
            Options FollowSymLinks
            AllowOverride None
            # depending on your global Apache settings, you may need to uncomment and adapt
            # for Apache 2.2 and earlier:
            #Allow from all
            # for Apache 2.4:
            #Require all granted
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
            php_admin_value register_globals 0
            php_value magic_quotes_gpc 0
            php_value magic_quotes_runtime 0
            php_value allow_call_time_pass_reference 0
        </IfModule>
     
        DirectoryIndex index.php
     
        <IfModule mod_rewrite.c>
            RewriteEngine On
     
            # Uncomment in FastCGI mode or when using PHP-FPM, to get basic auth working.
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
     
            # Additional Assetic rules for eZ Publish 5.1 / 2013.4 and higher.
            ## Don't forget to run php ezpublish/console assetic:dump --env=prod
            ## and make sure to comment these out in dev environment.
            RewriteRule ^/css/.*\.css - [L]
            RewriteRule ^/js/.*\.js - [L]
     
            RewriteRule .* /index.php
        </IfModule>
     
        DocumentRoot [PATH_TO_EZPUBLISH]/web/
        ServerName [SERVER_NAME]
        ServerAlias [SERVER_ALIAS]
     
    </VirtualHost>

Icon

For instructions on how to set custom environments please refer to
the \ `Installing eZ Publish on a Linux-UNIX based
system <Installing-eZ-Publish-on-a-Linux-UNIX-based-system_7438581.html>`__
documentation page, under "Defining the desired environment".

Reference

Description

[IP\_ADDRESS]

The IP address of the virtual host, for example "``128.39.140.28``\ ".
Apache allows the usage of a wildcards here ("\*").

[PORT]

The port on which the web server listens for incoming requests. This is
an optional setting, the default port is 80. The combination of an IP
address and a port is often referred to as a socket. Apache allows the
usage of a wildcards here ("\*").

[PATH\_TO\_EZPUBLISH]

Path to the directory that contains eZ Publish. This must be the full
path, for example "``/var/www/ezpublish-3.6.0``\ ".

[SERVER\_NAME]

The host or the IP address that Apache should look for. If a match is
found, the virtual host settings will be used.

[SERVER\_ALIAS]

Additional hosts/IP addresses that Apache should look for. If a match is
found, the virtual host settings will be used.

Icon

Please note that the "``mod_rewrite``\ " module must be enabled in
"``httpd.conf``\ " in order to use the Rewrite Rules.

NameVirtualHost
---------------

The "``NameVirtualHost``\ " setting might already exist in the default
configuration. Defining a new one will result in a conflict. If Apache
reports errors such as
"``NameVirtualHost [IP_ADDRESS] has no VirtualHosts``\ " or
"*``Mixing * ports and non-* ports with a NameVirtualHost address is not supported``*\ ",
try skipping the ``NameVirtualHost`` line. For more info about the
``NameVirtualHost`` directive, see
`http://httpd.apache.org/docs/2.2/mod/core.html#namevirtualhost <http://httpd.apache.org/docs/2.2/mod/core.html#namevirtualhost>`__.

SOAP and WebDAV
---------------

| If you would like to use the SOAP and/or the WebDAV features of eZ
Publish, you'll have to add the following lines in the virtual host
configuration (replace appropriately the host names used):

::

    RewriteCond %{HTTP_HOST} ^webdav\..*
    RewriteRule ^(.*) /webdav.php [L]
     
    RewriteCond %{HTTP_HOST} ^soap\..*
    RewriteRule ^(.*) /soap.php [L]
     
    ServerAlias soap.example.com
    ServerAlias webdav.example.com

Optional re-write rules to improve performance
----------------------------------------------

Using the Apache module ``mod_expires`` is recommended to improve site
performance, by preventing users from downloading too frequently static
assets. The following lines can added to the virtualhost configuration
to take advantage of it:

::

        # Everything below is optional to improve performance by forcing
        # clients to cache image and design files, change the expires time
        # to suite project needs.
        <IfModule mod_expires.c>
            <LocationMatch "^/var/[^/]+/storage/images/.*">
                # eZ Publish appends the version number to image URL (ezimage
                # datatype) so when an image is updated, its URL changes to
                ExpiresActive on
                ExpiresDefault "now plus 10 years"
            </LocationMatch>
     
            <LocationMatch "^/extension/[^/]+/design/[^/]+/(stylesheets|images|javascripts?|flash)/.*">
                # A good optimization if you don't change your design often
                ExpiresActive on
                ExpiresDefault "now plus 5 days"
            </LocationMatch>
     
            <LocationMatch "^/extension/[^/]+/design/[^/]+/lib/.*">
                # Libraries get a new url (version number) on updates
                ExpiresActive on
                ExpiresDefault "now plus 90 days"
            </LocationMatch>
     
            <LocationMatch "^/design/[^/]+/(stylesheets|images|javascripts?|lib|flash)/.*">
                # Same as above for bundled eZ Publish designs
                ExpiresActive on
                ExpiresDefault "now plus 7 days"
            </LocationMatch>
     
            <LocationMatch "^/share/icons/.*">
                # Icons as used by admin interface, barly change
                ExpiresActive on
                ExpiresDefault "now plus 7 days"
            </LocationMatch>
     
            # When ezjscore.ini/[Packer]/AppendLastModifiedTime=enabled
            # so that file names change when source files are modified
            #<LocationMatch "^/var/[^/]+/cache/public/.*">
                # Force ezjscore packer js/css files to be cached 30 days
                # at client side
                #ExpiresActive on
                #ExpiresDefault "now plus 30 days"
            #</LocationMatch>
            </IfModule>

 

 

::

     

Comments:
---------

Maybe we should add note for those who are migrating to apache 2.4.
Actually there are some changes on the Access Control part

`http://httpd.apache.org/docs/current/upgrading.html <http://httpd.apache.org/docs/current/upgrading.html>`__

|image0| Posted by desorden at Jan 11, 2014 17:40

Document generated by Confluence on Feb 12, 2014 16:43

.. |image0| image:: images/icons/contenttypes/comment_16.png
