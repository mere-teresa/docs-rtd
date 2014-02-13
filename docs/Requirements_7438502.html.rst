#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Installation and Upgrade
   Guides <Installation-and-Upgrade-Guides_6292016.html>`__

eZ Publish Platform 5.<next> : Requirements
===========================================

Added by andrea.melo@ez.no , edited by andre.romcke@ez.no on Jan 30,
2014

eZ Publish software is built to rely on existing technologies and
standards, mainly:

-  PHP scripting language (5.3/5.4/5.5, minimum 5.3.3)

-  SQL database

-  XHTML version 1.0 / HTML 5

-  XML 1.0

-  Java JRE 1.6 or higher (Oracle-Sun/OpenJDK) when eZ Find search
   engine is used

These are the main components, for more details, please consult
documentation for in-depth information.

To answer the question "Is my platform supported by eZ Publish", several
things must be taken into consideration in order to determining whether
a platform is supported or not, and how. The following tables gives you
the details of the platform requirements for the latest eZ Publish
release.

Supported stacks
----------------

Reference Stack
~~~~~~~~~~~~~~~

The most  safe and  optimized stack for eZ Publish Platform. The stack
is supported and certified for eZ Publish Platform 5. This means that
our QA certification team has successfully run intensive certification
tests on the platform, and that we can fully support the platform in the
scope of an eZ Publish Subscription.

The reference stack for eZ Publish Platform 5 is based on Red Hat
Enterprise Linux 6. The detail of the stack can be seen in the table
below.

 

**Stack**

Single mode

Cluster mode

**OS**

-  Red Hat RHEL 6.x latests stable (tested on 6.4)

-  Red Hat RHEL 6.x latests stable (tested on 6.4)

**Web Server**

-  Apache 2.2.15 (pre-fork mode) (rhel6 package)

-  Apache 2.2.15 (pre-fork mode) (rhel6 package)

**Symfony Web Framework (\* )**

-  Latest stable version of Symfony 2.3.x with Twig

-  Latest stable version of Symfony 2.3.x with Twig

**DBMS**

-  MySQL 5.1 (rhel6 package)

-  MySQL 5.1 (rhel6 package)

**PHP (mod\_php) + PHP CLI**

-  Official PHP 5.3.3 (rhel6 packages)

-  Official PHP 5.3.3 (rhel6 packages)

**PHP + PHP CLI extensions**

-  APC 3.1.9 (rhel6 packages)
-  bz2
-  Curl
-  dom
-  exif
-  fileinfo
-  ftp
-  gd
-  Iconv
-  json
-  mbstring
-  Database:

   -  MySQL:

      -  mysqli (LS)
      -  pdo-mysql (5.x)

-  pecl\_http

   -  Required by ezphttprequest used by ezodoscope & ezrecommender

-  pcntl
-  pcre
-  posix
-  reflection
-  simplexml
-  spl
-  ssl
-  xmlreader
-  xsl
-  zlib
-  php-intl

-  APC 3.1.9 (rhel6 packages)
-  bz2
-  Curl
-  dom
-  exif
-  fileinfo
-  ftp
-  gd
-  Iconv
-  json
-  mbstring
-  memcache
-  Database:

   -  MySQL:

      -  mysqli (LS)
      -  pdo-mysql (5.x)

-  pecl\_http

   -  Required by ezphttprequest used by ezodoscope & ezrecommender

-  pcntl
-  pcre
-  posix
-  reflection
-  simplexml
-  spl
-  ssl
-  xmlreader
-  xsl
-  zlib
-  php-intl

**Graphic Handler**

-  ImageMagick 6.5.4 (rhel6 packages)

-  ImageMagick 6.5.4 (rhel6 packages)

**Other**

-  OpenOffice 3.2.1 (rhel6 packages)

-  OpenOffice 3.2.1 (rhel6 packages)

**eZ Publish extensions**

-  eZ Online Editor LS 5.2.0
-  eZ Website Interface LS 5.2.0
-  eZ Flow LS 5.2.0
-  eZ Find LS 5.2.0
-  eZ Google Maps Location LS 5.2.0
-  eZ Star Rating LS 5.2.0
-  eZ Website Toolbar LS 5.2.0
-  eZ Openoffice.org LS 5.2.0
-  eZ MB Password Expiry LS 5.2.0
-  eZ Multiupload LS 5.2.0
-  eZ Survey LS 5.2.0
-  eZ Comments LS 5.2.0
-  eZ JSCore LS 5.2.0
-  eZ Script Monitor LS 5.2.0
-  eZ SI LS 5.2.0
-  eZ Style Editor LS 5.2.0
-  eZ XML Export LS 5.2.0
-  eZ Image Editor LS 5.2.0
-  eZ Network LS 5.2.0
-  eZ Form Token LS 5.2.0
-  eZ Content Staging LS 5.2.0
-  eZ Autosave LS 5.2.0
-  eZ REST API Provider LS 5.2.0

-  eZ Online Editor LS 5.2.0
-  eZ Website Interface LS 5.2.0
-  eZ Flow LS 5.2.0
-  eZ Find LS 5.2.0
-  eZ Google Maps Location LS 5.2.0
-  eZ Star Rating LS 5.2.0
-  eZ Website Toolbar LS 5.2.0
-  eZ Openoffice.org LS 5.2.0
-  eZ MB Password Expiry LS 5.2.0
-  eZ Multiupload LS 5.2.0
-  eZ Survey LS 5.2.0
-  eZ Comments LS 5.2.0
-  eZ JSCore LS 5.2.0
-  eZ Script Monitor LS 5.2.0
-  eZ SI LS 5.2.0
-  eZ Style Editor LS 5.2.0
-  eZ XML Export LS 5.2.0
-  eZ Image Editor LS 5.2.0
-  eZ Network LS 5.2.0
-  eZ Form Token LS 5.2.0
-  eZ Content Staging LS 5.2.0
-  eZ Autosave LS 5.2.0
-  eZ REST API Provider LS 5.2.0

**Cluster mode**

 

-  eZDFSFileHandler (mysqli) + Linux NFS
-  Persistence cache configured with Memcache

**File system**

-  Linux ext4

-  Linux ext4

Approved stacks
~~~~~~~~~~~~~~~

These stacks are also tested, but not as extensively as our reference
stacks. These stacks still benefit from the full support and maintenance
guarantees provided with the eZ Publish Platform Subscription, but more
issues might occur during normal operations, performance might be lower
and issues take longer to resolve.

The supported non-reference stacks for eZ Publish Platform 5 are mainly:

-  Debian 7
-  CentOS (with same version and configuration as our reference Red Hat
   stack above)

The details of the Debian stack can be seen in the table below. 

 

Single Mode

Cluster Mode

**Operating system**

-  Debian 7

-  Debian 7

**Web Server**

-  Apache 2.2.x (prefork mode)

-  Apache 2.2.x (prefork mode)

**Symfony Web Framework (\* )**

-  Latest stable version of Symfony 2.3.x with Twig

-  Latest stable version of Symfony 2.3.x with Twig

**DBMS**

-  MySQL 5.5.x
-  PostgreSQL 9.1.x

-  MySQL 5.5.x

**PHP (mod\_php) + PHP CLI + apache**

-  PHP 5.3.3+ (tested on 5.4.4)

-  PHP 5.3.3+ (tested on 5.4.4)

**PHP**

-  APC 3.1.9+ (tested on 3.1.13 debian package)
-  bz2
-  Curl
-  dom
-  exif
-  fileinfo
-  ftp
-  gd
-  Iconv
-  json
-  mbstring
-  Database:

   -  MySQL:

      -  mysqli (LS)
      -  pdo-mysql (5.x)

   -  Postgres:

      -  pgsql (LS)
      -  pdo-pgsql (5.x)

-  pecl\_http

   -  Required by ezphttprequest used by ezodoscope & ezrecommender

-  pcntl
-  pcre
-  posix
-  reflection
-  simplexml
-  spl
-  ssl
-  xmlreader
-  xsl
-  zlib
-  php-intl

-  APC 3.1.9+ (tested on 3.1.13 debian package)
-  bz2
-  Curl
-  dom
-  exif
-  fileinfo
-  ftp
-  gd
-  Iconv
-  json
-  mbstring
-  memcache OR memcached
-  Database:

   -  MySQL:

      -  mysqli (LS)
      -  pdo-mysql (5.x)

-  pecl\_http

   -  Required by ezphttprequest used by ezodoscope & ezrecommender

-  pcntl
-  pcre
-  posix
-  reflection
-  simplexml
-  spl
-  ssl
-  xmlreader
-  xsl
-  zlib
-  php-intl

**Graphic Handler**

-  ImageMagick >= 6.4.x
-  GD2 ( PHP extension )

-  ImageMagick >= 6.4.x
-  GD2 ( PHP extension )

**eZ Publish extensions**

-  similar as Reference platform (see above)

**Cluster mode**

 

-  eZDFSFileHandler (mysqli) + Linux NFS
-  Persistence cache configured with Memcache[d]

**Filesystem**

-  Linux ext3 / ext4

-  Linux ext3 / ext4

| \*: to ease developer and administrator life, the latest stable
version of the Symfony framework is bundled with the eZ Publish release.
| eZ support eZ Publish only when used with the latest maintenance
version of Symfony within the version specified above, new
maintenance versions are announced by
`Symfony <http://symfony.com/blog/category/releases>`__ and provided via
composer. Symfony is not supported directly by eZ within eZ Publish
Enterprise Subscriptions, however contact your eZ Systems representative
for alternatives.

Compatible platforms
--------------------

eZ Publish can run and execute on many more platforms than the ones
listed above, including (but not limited to) the operating systems
listing below if they pass
the \ `Symfony requirements <http://symfony.com/doc/current/reference/requirements.html>`__.

**However, eZ Systems doesn't insure and guarantee quality operation of
an eZ Publish Platform installation if it is running on any platform not
listed as supported.** eZ Publish Enterprise Subscriptions are still
available for compatible platforms, but the guarantee and the product
support will not apply and although you will receive various maintenance
releases and services, no bug fix guarantee will apply to issues related
to the platform. Maintenance and monitoring tools will not be available.
eZ Systems does not advise merely compatible platforms for production
use.

Compatible platform:

-  Most Linux operating system (Fedora, CentOS, Ubuntu...)
-  Solaris 
-  OpenSolaris
-  Windows Vista/7/2008  (some issue might occur related to third party
   composer libraries, contact these for fixes if that is the case)
-  Mac OS X server

Supported browsers
==================

eZ Publish is developed to work properly and support the following
browser configurations for administrator users:

-  Mozilla® Firefox®, most recent stable version (tested on Firefox 25).
   eZ makes every effort to test and support the most recent version of
   Firefox.  

-  Google Chrome™, most recent stable version (tested on chrome
   30). Chrome applies updates automatically; eZ makes every effort to
   test and support the most recent version.

-  Microsoft® Internet Explorer® versions 9, and 10. We recommend using
   the latest version.

-  Apple® Safari® 6.1 or higher on Mac OS X. Apple Safari on iOS isn’t
   supported for admin backend.

Please note that the interface will display and behave optimally in any
browser that supports HTML 5.0, CSS 3.0 and ECMAScript 5. If
these technologies are not supported the system will gracefully appear
with simpler design/layout but will still be accessible through
standard/default HTML elements.

 

 

Document generated by Confluence on Feb 12, 2014 16:43
