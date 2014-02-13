#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Developing with eZ Publish
   5 <Developing-with-eZ-Publish-5_2720528.html>`__
#. `MVC and Application <MVC-and-Application_2719826.html>`__
#. `Siteaccess <Siteaccess_2719828.html>`__

eZ Publish Platform 5.<next> : Siteaccess Matching
==================================================

Added by jerome.vieilledent@ez.no , edited by andre.romcke@ez.no on Dec
18, 2013

-  `Configuration <#SiteaccessMatching-Configuration>`__
-  `Available matchers <#SiteaccessMatching-Availablematchers>`__
-  `Compound siteaccess
   matcher <#SiteaccessMatching-Compoundsiteaccessmatcher>`__
-  `Matching by request
   header <#SiteaccessMatching-Matchingbyrequestheader>`__
-  `Matching by environment
   variable <#SiteaccessMatching-Matchingbyenvironmentvariable>`__
-  `URILexer and
   semanticPathinfo <#SiteaccessMatching-URILexerandsemanticPathinfo>`__

Siteaccess matching is done through
**eZ\\Publish\\MVC\\SiteAccess\\Matcher** objects. You can configure
this matching and even develop custom matchers.

Configuration
-------------

You can configure siteaccess matching in your main
**ezpublish/config/ezpublish.yml** :

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

 

You need to set several parameters:

-  **ezpublish.siteaccess.default\_siteaccess**
-  **ezpublish.siteaccess.list**
-  *(optional)*\ **ezpublish.siteaccess.groups**
-  **ezpublish.siteaccess.match**

**ezpublish.siteaccess.default\_siteaccess** is the default siteaccess
that will be used if matching was not successful. This ensures that a
siteaccess is always defined.

**ezpublish.siteaccess.list** is the list of all available siteaccesses
in your website.

*(optional)* **ezpublish.siteaccess.groups** defines which groups
siteaccesses are member of. This is useful when you want to mutualize
settings between several siteaccesses and avoid config duplication.
Siteaccess groups are considered as regular siteaccesses as far as
configuration is concerned.

Icon

A siteaccess can be part of several groups.

A siteaccess configuration has always precedence on the group
configuration.

**ezpublish.siteaccess.match** holds the matching configuration. It
consists in a hash where the key is the name of the matcher class. If
the matcher class doesn't start with a **\\**, it will be considered
relative to ``eZ\Publish\MVC\SiteAccess\Matcher`` (e.g. ``Map\Host``
will refer to \ ``eZ\Publish\MVC\SiteAccess\Matcher\Map\Host``)

Icon

Every **custom matcher** can be specified with a **fully qualified class
name** (e.g. ``\My\SiteAccess\Matcher``) or by a **service identifier
prefixed by @** (e.g. ``@my_matcher_service``).

-  In the case of a fully qualified class name, the matching
   configuration will be passed in the constructor.
-  In the case of a service, it must implement
   ``eZ\Bundle\EzPublishCoreBundle\SiteAccess\Matcher``. The matching
   configuration will be passed to ``setMatchingConfiguration()``.

Icon

Make sure to type matcher in correct case, if wrong case like "Uri"
instead of "URI" it will happily work on systems like Mac OS X because
of case in sensitive file system, while it will fail when you deploy it
to a linux server. This is a known artifact of `PSR-0
autoloading <http://www.php-fig.org/psr/psr-0/>`__ of PHP classes.

Available matchers
------------------

Name

Description

Configuration

Example

``URIElement``

| Maps a URI element to a siteaccess.
| This is the default matcher used when choosing
| URI matching in setup wizard. 

The element number you want to match (starting from 1).

.. code:: theme:

    ezpublish:
        siteaccess:
            match:
                URIElement: 1

| **Important:** When using a value > 1,
| it will concatenate the elements with \_

**URI:** ``/ezdemo_site/foo/bar``

| Element number: 1
| Matched siteaccess: ezdemo\_site

| Element number: 2
| Matched siteaccess: ezdemo\_site\_foo 

``URIText``

| Matches URI using *pre* and/or *post* sub-strings
| in the first URI segment

The prefix and/or suffix (none are required)

.. code:: theme:

    ezpublish:
        siteaccess:
            match:
                URIText:
                    prefix: foo
                    suffix: bar

**URI:** ``/footestbar/my/content``

| Prefix: foo
| Suffix: bar
| Matched siteaccess: test 

``HostElement``

Maps an element in the host name to a siteaccess.

The element number you want to match (starting from 1).

.. code:: theme:

    ezpublish:
        siteaccess:
            match:
                HostElement: 2

**Host name:** ``www.example.com``

| Element number: 2
| Matched siteaccess: example 

``HostText``

| Matches a siteaccess in the host name,
| using *pre* and/or *post* sub-strings.

The prefix and/or suffix (none are required)

.. code:: theme:

    ezpublish:
        siteaccess:
            match:
                HostText:
                    prefix: www.
                    suffix: .com

**Host name**: ``www.foo.com``

| Prefix: www.
| Suffix: .com
| Matched siteaccess: foo 

``Map\Host``

Maps a host name to a siteaccess.

A hash map of host/siteaccess

.. code:: theme:

    ezpublish:
        siteaccess:
            match:
                Map\Host:
                    www.foo.com: foo_front
                    adm.foo.com: foo_admin
                    www.stuff.fr: bar_front
                    adm.stuff.fr: bar_admin

**Map**:

-  www.foo.com => foo\_front
-  admin.foo.com => foo\_admin 

**Host name**: www.example.com

Matched siteaccess: foo\_front

``Map\URI``

Maps a URI to a siteaccess

A hash map of URI/siteaccess

.. code:: theme:

    ezpublish:
        siteaccess:
            match:
                Map\URI:
                    something: ezdemo_site
                    foobar: ezdemo_site_admin

**URI:** ``/something/my/content``

Map:

-  something => ezdemo\_site
-  foobar => ezdemo\_site\_admin

Matched siteaccess: ezdemo\_site

``Map\Port``

Maps a port to a siteaccess

A has map of Port/siteaccess

.. code:: theme:

    ezpublish:
        siteaccess:
            match:
                Match\Port:
                    80: foo
                    8080: bar

**URL:** ``http://ezpublish.dev:8080/my/content``

Map:

-  80: foo
-  8080: bar

Matched siteaccess: bar

``Regex\Host``

Matches against a regexp and extract a portion of it

| The regexp to match against
| and the captured element to use

.. code:: theme:

    ezpublish:
        siteaccess:
            match:
                Regex\Host:
                    regex: "^(\\w+_sa)$"
                    # Default is 1
                    itemNumber: 1

**Host name:** ``example_sa``

| regex: ``^(\\w+)_sa$``
| itemNumber: 1

Matched siteaccess: example

``Regex\URI``

Matches against a regexp and extract a portion of it

| The regexp to match against
| and the captured element to use

.. code:: theme:

    ezpublish:
        siteaccess:
            match:
                Regex\URI:
                    regex: "^/foo(\\w+)bar"
                    # Default is 1
                    itemNumber: 1

| 

**URI:** ``/footestbar/something``

| regex: ^/foo(\\\\w+)bar
| itemNumber: 1

Matched siteaccess: test 

 

Compound siteaccess matcher
---------------------------

The Compound siteaccess matcher allows to combine several matchers
together, this can most commonly be used to performed a legacy
***host\_uri*** match.

For that, the following matchers can be used in your siteaccess match:

-  ``Compound\LogicalAnd``
-  ``Compound\LogicalOr``

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
                    my_compound_rule_1:
                        # Nested matchers, with their configuration.
                        # No need to precise their matching values (true will suffice).
                        matchers:
                            Map\URI:
                                the_front: true
                            Map\Host:
                                ezpublish.dev: true
                        # The siteaccess to match in the end
                        match: ezdemo_site 
                    my_compound_rule_2:
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

Matching by request header
--------------------------

It is possible to define which siteaccess to use by setting a
**X-Siteaccess** header in your request. This can be useful for REST
requests.

In such case, **X-Siteaccess** must be the **siteaccess name** (e.g.
*ezdemo\_site*).

Matching by environment variable
--------------------------------

It is also possible to define which siteaccess to use directly via an
**EZPUBLISH\_SITEACCESS** environment variable.

This is recommended if you want to get **performance gain** since no
matching logic is done in this case.

You can define this environment variable directly from your web server
configuration:

**Apache VirtualHost example**

.. code:: theme:

    # This configuration assumes that mod_env is activated
    <VirtualHost *:80>
        DocumentRoot "/path/to/ezpublish5/web/folder"
        ServerName example.com
        ServerAlias www.example.com
        SetEnv EZPUBLISH_SITEACCESS ezdemo_site
    </VirtualHost>

Icon

This can also be done via PHP-FPM configuration file, if you use it.
See \ `PHP-FPM
documentation <http://php.net/manual/en/install.fpm.configuration.php#example-60>`__ for
more information.

Note about precedence

Icon

 The precedence order for siteaccess matching is the following (the
first matched wins):

#. Request header
#. Environment variable
#. Configured matchers

 

URILexer and semanticPathinfo
-----------------------------

In some cases, after matching a siteaccess, it is neecessary to modify
the original request URI. This is for example needed with URI-based
matchers since the siteaccess is contained in the original URI and it is
not part of the route itself.

The problem is addressed by *analyzing* this URI and by modifying it
when needed through the **URILexer** interface.

**URILexer interface**

.. code:: theme:

    /**
     * Interface for SiteAccess matchers that need to alter the URI after matching.
     * This is useful when you have the siteaccess in the URI like "/<siteaccessName>/my/awesome/uri"
     */
    interface URILexer
    {
        /**
         * Analyses $uri and removes the siteaccess part, if needed.
         *
         * @param string $uri The original URI
         * @return string The modified URI
         */
        public function analyseURI( $uri );
        /**
         * Analyses $linkUri when generating a link to a route, in order to have the siteaccess part back in the URI.
         *
         * @param string $linkUri
         * @return string The modified link URI
         */
        public function analyseLink( $linkUri );
    }

Once modified, the URI is stored in the ***semanticPathinfo*** request
attribute, and the original pathinfo is not modified.

 

Comments:
---------

Nice work ! Just a question about host/uri match: how could it work with
the matcher? it ll be include in ez5 or should we develop it ?

|image0| Posted by philippe.vincent-royol@ez.no at Sep 04, 2012 11:04

Thanks.

It's a pending task. We will introduce matcher combinators for that. So
it will be more powerful \ |(smile)|

|image2| Posted by jerome.vieilledent@ez.no at Sep 04, 2012 11:35

The configuration doc. seems outdated. From what I could tell, It is now
set in (and imported from) **parameters.yml**, using a different
structure:

**parameters.yml**

.. code:: theme:

    ezpublish:
        siteaccess:
            # Available siteaccesses
            list:
                - ezdemo_site
                - ezdemo_site_admin
            # Siteaccess groups. Use them to group common settings.
            groups:
                ezdemo_group: [ezdemo_site, ezdemo_site_admin]

|image3| Posted by joao.inacio@ez.no at Sep 18, 2012 22:43

You're right \ |(smile)|. I just didn't have time to update the
documentation yet \ |(wink)|

|image5| Posted by jerome.vieilledent@ez.no at Sep 20, 2012 17:14

Document generated by Confluence on Feb 12, 2014 16:43

.. |image0| image:: images/icons/contenttypes/comment_16.png
.. |(smile)| image:: images/icons/emoticons/smile.png
.. |image2| image:: images/icons/contenttypes/comment_16.png
.. |image3| image:: images/icons/contenttypes/comment_16.png
.. |(wink)| image:: images/icons/emoticons/wink.png
.. |image5| image:: images/icons/contenttypes/comment_16.png
