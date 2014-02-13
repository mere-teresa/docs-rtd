#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Developing with eZ Publish
   5 <Developing-with-eZ-Publish-5_2720528.html>`__
#. `MVC and Application <MVC-and-Application_2719826.html>`__
#. `Configuration <Configuration_2720538.html>`__

eZ Publish Platform 5.<next> : Persistence cache configuration
==============================================================

Added by andre.romcke@ez.no , edited by sarah.haim-lubczanski@ez.no on
Feb 06, 2014

| 

-  `Introduction <#Persistencecacheconfiguration-Introduction>`__
-  `Configuration <#Persistencecacheconfiguration-Configuration>`__

   -  `Multi repository
      setup <#Persistencecacheconfiguration-Multirepositorysetup>`__

-  `Stash handlers
   configuration <#Persistencecacheconfiguration-Stashhandlersconfiguration>`__

   -  `General
      settings <#Persistencecacheconfiguration-Generalsettings>`__
   -  `FileSystem <#Persistencecacheconfiguration-FileSystem>`__

      -  `Available
         settings <#Persistencecacheconfiguration-Availablesettings>`__

   -  `APC <#Persistencecacheconfiguration-APC>`__
   -  `Memcache <#Persistencecacheconfiguration-Memcache>`__

      -  `Example with
         Memcache <#Persistencecacheconfiguration-ExamplewithMemcache>`__

Introduction
============

**Tech Note**

Current implementation uses a caching library called
`Stash <http://stash.tedivm.com/>`__, via
`Stash-bundle <https://github.com/tedivm/TedivmStashBundle>`__. If this
changes, then the configuration format will most likely change as
well. Stash supports the following cache handlers:
**FileSystem**, \ **Memcache**, **APC**, **Sqlite**, **XCache** and
**BlackHole**.

**Cache service**

The cache system is exposed as a "cache" service, and can be reused by
any other service as described on the
`Persistence cache <Persistence-cache_10158280.html>`__ page.

Configuration
=============

During Setup wizard and manually using ``ezpublish:configure`` console
command a default configuration is generated currently **using
FileSystem**, using ``%kernel.cache_dir%/stash`` to store cache files.

The configuration is placed in ezpublish/config/ezpublish.yml, and looks
like:

**Default ezpublish.yml**

.. code:: theme:

    stash:
        caches:
            default:
                handlers:
                    - FileSystem
                inMemory: false
                registerDoctrineAdapter: false

The default settings used during setup wizard as found in
``ezpublish/config/ezpublish_setup.yml``:

**ezpublish\_setup.yml**

.. code:: theme:

    stash:
        caches:
            default:
                handlers:
                    - BlackHole
                inMemory: true
                registerDoctrineAdapter: false

This setting works across all installs and just caches objects within
the same request thanks to the ``inMemory: true`` setting.

If you want to change to another handler, see in *Stash handlers
configuration* below for what kind of settings you have available.

Note for "inMemory" cache with long running scripts

Icon

Use of ``inMemory`` caching with BlackHole or any other cache handler
should not be used for long running scripts as it will over time return
stale data, inMemory cache is not shared across requests/processes, so
invalidation does not happen!

Multi repository setup
~~~~~~~~~~~~~~~~~~~~~~

New in 5.2 is the possibility to select a specific Stash cache pool on a
siteaccess or sitegroup level, the following example shows use in a
sitegroup:

**ezpublish.yml site group setting**

.. code:: theme:

            ezdemo_group:
                cache_pool_name: "default"
                database:
                    ...

| The "default" here refers to the name of the cache pool as specified
in the *stash* configuration block shown above, if your install has
several repositories (databases), then make sure every group of sites
using different repositories also uses a different cache pool to avoid
unwanted effects.
| NB: We plan to make this more native in the future, so this setting
will someday not be needed.

Stash handlers configuration
============================

General settings
~~~~~~~~~~~~~~~~

To check which cache settings are available for your installation, run
the following command in your terminal :

.. code:: theme:

    php ezpublish/console config:dump-reference stash

FileSystem
~~~~~~~~~~

This cache handler is using local filesystem, by default the Symfony
cache folder, as this is per server, it \ **does not support multi
server (cluster) setups**!

Icon

**We strongly discourage you from storing cache files on NFS**, as it
defeats the purpose of the cache: speed

**Available settings**
^^^^^^^^^^^^^^^^^^^^^^

::

    path

The path where the cache is placed, default is
``%kernel.cache_dir%/stash``, effectively
``ezpublish/cache/<env>/stash``

::

    dirSplit

Number of times the cache key should be split up to avoid having to many
files in each folder, default is 2.

::

    filePermissions

The permissions of the cache file, default is 0660.

::

    dirPermissions

The permission of the cache file directories (see dirSplit), default is
0770.

::

    memKeyLimit

Limit on how many key to path entries are kept in memory during
execution at a time to avoid having to recalculate the path on key
lookups, default 200.

::

    keyHashFunction

Algorithm used for creating paths, default md5. Use crc32 on
Windows\ ````

Issues with Microsoft Windows

Icon

If you are using a Windows OS, you may encounter an issue
regarding\ **long paths for cache directory name**. The paths are long
because Stash uses md5 to generate unique key that are sanitized really
quickly.

Solution is to **change the hash algorithm** used by Stash.

**Specifying key hash function**

.. code:: theme:

    stash:
        caches:
            default:
                handlers:
                    - FileSystem
                inMemory: true
                registerDoctrineAdapter: false
                FileSystem:
                    keyHashFunction: 'crc32'

| **This configuration is only recommended for Windows users**.
| Note : you can also define **the path** where you want the cache files
to be generated.

 

APC
~~~

This cache handler is using shard memory using APC's user cache feature,
as this is per server, it **does not support multi server (cluster)
setups**, unless you use PHP-FPM on a dedicated server (this way, user
cache can be shared among all the workers in one pool).

Limitation

Icon

| As APC user cache is not shared between processes, it is not possible
to clear the user cache from CLI, even if you set ``apc.enable_cli`` to
On.
| Hence publishing content from a command line script won't let you
properly clear SPI cache.

Please also note that the default value for \ ``apc.shm_size`` is 128MB.
However, 256MB is recommended for APC to work properly. For more details
please refer to the `APC configuration
manual <http://www.php.net/manual/en/apc.configuration.php#ini.apc.shm-size>`__.

 

**Available settings**

+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| ``ttl``         | The time to live of the cache in seconds, default set to 500 (8.3 minutes)                                                                       |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+
| ``namespace``   | A namespace to prefix cache keys with to avoid key conflicts with other eZ Publish sites on same eZ Publish installation, default is ``null``.   |
+-----------------+--------------------------------------------------------------------------------------------------------------------------------------------------+

Memcache
~~~~~~~~

This cache handler is using `Memcached, a distributed caching
solution <http://memcached.org/>`__, **this is the only supported cache
solution for multi server (cluster) setups**!

Note

Icon

Stash supports both the php `Memcache <http://php.net/memcache>`__ and
`Memcached <http://php.net/memcached>`__ extensions. **However Memcached
is recommended** as it is much more advanced and supports all settings
below.

If you have both extensions installed, **Stash will automatically choose
Memcached**.

``servers``

Array of Memcached servers, with host/IP, port and weight

| ``server``: Host or IP of your Memcached server
| ``port``: Port where Memcached is listening to (defaults to 11211)
| ``weight``: Weight of the server, when using several Memcached servers

``prefix_key``

| A namespace to prefix cache keys with to avoid key conflicts with
other eZ Publish sites on same eZ Publish installation (default is an
empty string).
| Must be the same on all server with the same installation. `See
Memcached prefix\_key
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-prefix-key>`__.

``compression``

default true. `See Memcached compression
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-compression>`__.

``libketama_compatible``

default false. `See Memcached libketama\_compatible
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-libketama-compatible>`__

``buffer_writes``

default false. `See Memcached buffer\_writes
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-buffer-writes>`__ 

``binary_protocol``

default false. `See Memcached binary\_protocol
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-binary-protocol>`__ 

``no_block``

default false. `See Memcached no\_block
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-no-block>`__ 

``tcp_nodelay``

default false. `See Memcached tcp\_nodelay
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-tcp-nodelay>`__ 

``connection_timeout``

default 1000. `See Memcached connection\_timeout
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-connection-timeout>`__ 

``retry_timeout``

default 0. `See Memcached retry\_timeout
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-memcached-timeout>`__ 

``send_timeout``

default 0. `See Memcached send\_timeout
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-send-timeout>`__ 

``recv_timeout``

default 0. `See Memcached recv\_timeout
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-recv-timeout>`__ 

``poll_timeout``

default 1000. `See Memcached poll\_timeout
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-poll-timeout>`__ 

``cache_lookups``

default false. `See Memcached cache\_lookups
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-cache-lookups>`__ 

**``server_failure_limit``**

| default 0. `See PHP Memcached
documentation <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-server-failure-limit>`__

**``socket_send_size ``
**

See `Memcached socket\_send\_size
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-socket-send-size>`__.

**``socket_recv_size``**

See `Memcached socket\_recv\_size
option. <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-socket-recv-size>`__

**``serializer``**

See `Memcached serializer
option <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-serializer>`__.

**``hash``**

See `Memcached hash
option. <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-hash>`__

**``distribution``**

Specifies the method of distributing item keys to the servers.
See\ `Memcached distribution
option. <http://www.php.net/manual/en/memcached.constants.php#memcached.constants.opt-distribution>`__

For in-depth information on the settings, see:
`http://php.net/Memcached <http://php.net/Memcached>`__

Icon

| When using Memcache handler, **it's strongly recommended to also use
``inMemory`` cache** (see example below).
| This will help reducing network traffic between your webserver and
your Memcached server.

Example with Memcache
^^^^^^^^^^^^^^^^^^^^^

.. code:: theme:

    stash:
        caches:
            default:
                handlers: [ Memcache ]
                inMemory: true
                registerDoctrineAdapter: false
                Memcache:
                    prefix_key: ezdemo_
                    retry_timeout: 1
                    servers:
                        -
                            server: 127.0.0.1
                            port: 11211

Note

Icon

This configuration is only recommended for Windows users, but does solve
this problem.

Document generated by Confluence on Feb 12, 2014 16:43
