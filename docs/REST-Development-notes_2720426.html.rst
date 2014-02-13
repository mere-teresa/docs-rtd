#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Using eZ Publish as a Service
   Platform <Using-eZ-Publish-as-a-Service-Platform_2720526.html>`__
#. `Hacking the REST API <Hacking-the-REST-API_2720423.html>`__

eZ Publish Platform 5.<next> : REST Development notes
=====================================================

Added by toby.schlitt@ez.no , edited by ricardo.correia@ez.no on May 21,
2013

SDK
===

The REST SDK realizes an incarnation of the Public API on basis of the
REST Server.

Related objects
---------------

The REST API supports having related objects embedded into the returned
response for a certain requested object.

**For example**, when requesting a ``Content`` object via a ``GET``
request to ``/content/objects/23/versions/2``, in addition to the
requested data, the related ``VersionInfo`` object is included in the
response.

The SDK must handle such embedded objects properly. Since embedded and
referenced objects may change over time, **the client acts by default as
if no objects are embedded at all, when responding through the server**.

Therefore, loaded value objects that reference other value objects need
to receive a lazy loading mechanism in any case (e.g. a ``VersionInfo``
to load the related ``ContentInfo`` object). Lazy loading is achieved by
setting a closure into a corresponding (internal) property of the value
object that is executed on request of the related object.

**For example**, a loaded ``VersionInfo`` object receives a closure
during creation that can be used to retrieve the related ``VersionInfo``
object. This closure is then used either when the ``getContentInfo()``
method is called, or when the virtual ``$contentInfo`` property is
accessed.

Note that the value object **must not** handle the caching of their
related instances, i.e. they must not execute the closure only once and
keep the returned instance. Instead, the corresponding service
encapsulated in the closure handle the loading of an object only once.

Creating a deferred loading
~~~~~~~~~~~~~~~~~~~~~~~~~~~

When creating a deferred loading, 2 components are essentially affected:

#. **The value object**
   The value object needs to handle the loading of its related objects
   through one (or more) of the available services. To store the
   information for loading the related object, the value object must
   provide the respective properties. For example, a ``VersionInfo``
   object could hold the ID (aka URL) of the related ``ContentInfo`` in
   the ``$contentInfoId`` property. In order to load the related
   ``ContentInfo`` object, it needs only the ``ContentService``, which
   must be provided during construction. The call to
   ``VersionInfo->getContentInfo()`` will then just indirect to
   ``ContentService->loadContentInfo()`` and return its result.
#. **The parser for the value object**
   The parser must handle the necessary properties set into the value
   object. In addition to that, it must guarantee that all potential
   embedded objects are parsed accordingly.

**Note**: If there is no suitable method on one of the services that
allow you to load the deferred object, you can realize this by doing
either of the following:

-  

   -  create a method on the suitable service that realizes what you
      need and mark it with ``@access protected``
   -  handle the corresponding operations in the value object (not
      recommended, since hardly testable)

Parsing embedded objects
~~~~~~~~~~~~~~~~~~~~~~~~

The REST API is based on the principle that referred objects might
optionally be embedded into the response. For example, the list of
Relations is embedded into the Content response. However, a consuming
client may not rely on such embedding, since objects might change from
embedded to not embedded (and vice-versa) in the future. This must also
be reflected in the SDK implementation.

**Hint**: An object reference can be detected by its ``href`` and
``media-type`` attributes. If both are available, the element is an
object reference. If the corresponding tag is not empty, the referenced
object is also embedded.

To not rely on embedded objects, only a deferred load must be created,
for every referenced object . In addition to that, the potentially
embedded object might be parsed and put into the cache. In order to
allow such caching of embedded objects, the parsing process must still
be triggered, regardless if the parsing result is used or not.

**For example**, when parsing a ``VersionInfo``, the currently embedded
``ContentInfo`` must be triggered for parsing, using the
``ParsingDispatcher``, although a deferred loading closure is put into
the ``VersionInfo`` instance.

The ``ParserTools`` object has a convenience method that allows you to
trigger parsing of potentially embedded objects easily.

Object caching
--------------

In order to realize the lazy loading mechanism described above, a simple
ObjectCache is used by the services and parsers in the SDK. The
ObjectCache handles the storing of references to all already loaded
objects, during a single usage cycle of the REST SDK.

**Note**: The ObjectCache is not meant to be replaced by a more
persistent caching mechanism, later on.  Instead, such a mechanism needs
to work far more integrated, using ETags and friends.

**Warning**: ObjectCaching is not active in the REST Client, yet. It
still needs to be introduced!

The object cache is used in 2 different areas of the SDK:

-  **The parsers**:
   A parser must store a created object into the cache as soon as it is
   created
-  **The services**
   A service must, first, attempt to restore a requested object from the
   cache, before loading it from the REST server

In order to use the cache, the parsers and services will receive it via
dependency injection at construction time.

Document generated by Confluence on Feb 12, 2014 16:43
