#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Using eZ Publish as a Service
   Platform <Using-eZ-Publish-as-a-Service-Platform_2720526.html>`__
#. `SPI <SPI_1736729.html>`__

eZ Publish Platform 5.<next> : Solr Search Service Implementation Notes
=======================================================================

Added by kore.nordmann , edited by ricardo.correia@ez.no on May 21, 2013

Defining A Solr Schema
======================

The default Solr schema that is provided at
``eZ/Publish/Core/Persistence/Solr/Content/Search/schema.xml``
works.Therefore, neither custom field types or custom content types will
require changes in the ``schema.xml``.

This is implemented by using the *dynamic field types*, which are
provided by Solr. A dynamic field type allows all fields that match a
certain pattern to be mapped to a type:

.. code:: literal-block

    <dynamicField name="*_s" type="string" indexed="true" stored="true"/>

The given line tells Solr that every field which name ends with ``_s``
should be considered a string type, *if no dedicated type definition
exists*. Thus you can always overwrite the default type definition with
custom, optimized type definitions in your ``schema.xml``.

We mostly use predefined ``dynamicField`` definitions that come by
default with Solr, with just one additional ``dynamicField``:

.. code:: literal-block

    <fieldType name="identifier" class="solr.StrField" sortMissingLast="true" />
    <dynamicField name="*_id" type="identifier" indexed="true" stored="true"/>

This field is required to differentiate identifiers  from common
strings. Common strings are also used to build full text indexes, while
we do not want identifiers to end up there. Since the API stack does not
limit identifiers to integer numbers, this is the only sane way to go
here.

Indexed Data
------------

In general per content object a bunch of data will be indexed:

-  Content object base data

   -  IDs
   -  Types
   -  Meta information

-  Field dependent data

   -  Might include multiple values per field

The content object base data includes all the data for the most common
filtering and search operations. It is also used by most of the facets.
On the other hand, it is irrelevant for full text searches or field
dependent queries. It consists mostly of ID values plus some date
values.

The field dependent data is used by the full text search and field
specific queries and facets.

Full Text Index
---------------

To build up a full text index all string and text fields are used. This
is accomplished using the following schema definition:

.. code:: literal-block

    <field name="text" type="text" indexed="true" multiValued="true" stored="false"/>

    <copyField source="*_t" dest="text" />
    <copyField source="*_s" dest="text" />
    <copyField source="*_h" dest="text" />

This defines that every field that matches the common textual patterns
(text, html and string types) is copied into the full text index field.
Since one single document may have multiple "text" fields (for example
the article text and the title) this needs to be multivalued.

Schema Extensibility
--------------------

As mentioned before, you can always write a custom ``field`` definition,
if you do not want the field to be handled by the ``dynamicField``
definitions. Just check the name of the field in the Solr documents and
define the field.

You may also use types other then the defined set of types, which are:

-  ``*_i``: int
-  ``*_id``: identifier
-  ``*_s``: string
-  ``*_l``: long
-  ``*_t``: text
-  ``*_h``: html
-  ``*_b``: boolean
-  ``*_f``: float
-  ``*_d``: double
-  ``*_dt``: date
-  ``*_p``: location
-  ``*_c``: currency

In this case the type name will not be mapped, and the field will be
called something like ``my_field_my_type``, that can then be handled by
custom type definitions. The above mentioned default types should be
sufficient for most cases, though.

Field Type Integration
======================

To integrate field types with the Solr Search (or any other future
search back end), the field types must implement an additional
interface: ``eZ\Publish\SPI\FieldType\Indexable`` and the implementation
must be registered with the field type registry of the currently used
search back end. This interface enforces the field type to return the
field data and structural index information in a processable manner.
Each field type may return any amount of index data.

Misc
----

The search is supposed to return content objects. Most of the required
data is probably available in the search back end, but given the
involved value and data conversions, currently only IDs are fetched from
the search back end. The ``ContentHandler`` is then used to fetch the
actual content objects.

Setup
=====

#. Download Solr:
   `http://apache.mirror.digionline.de/lucene/solr/3.6.0/apache-solr-3.6.0.tgz <http://apache.mirror.digionline.de/lucene/solr/3.6.0/apache-solr-3.6.0.tgz>`__

   -  Extract
   -  We only need the example/directory -- you can remove the rest.

#. Link custom schema

   -  In the ``solr/conf/`` sub-directory, remove the original schema
      and link or copy our custom schema instead
   -  It can be found at:
      ``eZ/Publish/Core/Persistence/Solr/Content/Search/schema.xml``

#. Start Solr: ``java -jar start.jar``
#. You can try this out by running the Solr Search tests:

   -  Go to: ``eZ/Publish/API/Repository/Tests/``
   -  Run ``phpunit -c phpunit-legacy-solr.xml --group search ./``

 

Document generated by Confluence on Feb 12, 2014 16:43
