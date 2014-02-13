#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Developing with eZ Publish
   5 <Developing-with-eZ-Publish-5_2720528.html>`__
#. `MVC and Application <MVC-and-Application_2719826.html>`__
#. `Templating <Templating_8323395.html>`__

eZ Publish Platform 5.<next> : Twig Helper
==========================================

Added by jerome.vieilledent@ez.no , edited by jerome.vieilledent@ez.no
on Oct 07, 2013

eZ Publish **5.1+** comes with a Twig helper as a `global
variable <http://symfony.com/doc/master/cookbook/templating/global_variables.html>`__
named **``ezpublish``**.

This helper is accessible from all Twig templates and allows you to
easily retrieve useful information.

Reference
---------

Property

Description

``ezpublish.siteaccess``

Returns the current siteaccess.

``ezpublish.rootLocation``

Returns the root Location object

``ezpublish.requestedUriString``

Returns the requested URI string (also known as semanticPathInfo).

``ezpublish.systemUriString``

Returns the "system" URI string. System URI is the URI for internal
content controller. If current route is not an URLAlias, then the
current Pathinfo is returned.

``ezpublish.viewParameters``

Returns the view parameters as a hash.

``ezpublish.viewParametersString``

Returns the view parameters as a string.

``ezpublish.legacy``

Returns legacy information.

 

Legacy
------

Icon

``ezpublish.legacy`` is only available **when viewing content in legacy
fallback** (e.g. no corresponding Twig templates)

 

The ``ezpublish.legacy`` property returns an object of
type \ `ParameterBag <http://api.symfony.com/2.0/Symfony/Component/HttpFoundation/ParameterBag.html>`__,
which is a container for key/value pairs, and contains additional
properties to retrieve/handle legacy information.

Property

Description

``ezpublish.legacy.all``

Returns all the parameters, with all the contained information.

``ezpublish.legacy.keys``

Returns the parameter keys only.

``ezpublish.legacy.get``

Returns a parameter by name.

``ezpublish.legacy.has``

Returns true if the parameter is defined.

 

Listing the available parameters
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can list the available parameters in ``ezpublish.legacy`` by using
the ``ezpublish.legacy.keys`` property, as shown in the following
example:

**Example on retrieving the available parameters under
ezpublish.legacy**

.. code:: theme:

    {{ dump(ezpublish.legacy.keys()) }}

which will give a result similar to:

::

    array
      0 => string 'view_parameters' (length=15)
      1 => string 'path' (length=4)
      2 => string 'title_path' (length=10)
      3 => string 'section_id' (length=10)
      4 => string 'node_id' (length=7)
      5 => string 'navigation_part' (length=15)
      6 => string 'content_info' (length=12)
      7 => string 'template_list' (length=13)
      8 => string 'cache_ttl' (length=9)
      9 => string 'is_default_navigation_part' (length=26)
      10 => string 'css_files' (length=9)
      11 => string 'js_files' (length=8)
      12 => string 'css_files_configured' (length=20)
      13 => string 'js_files_configured' (length=19)

Retrieving legacy information
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Legacy information is accessible by using the ``ezpublish.legacy.get``
property, which will allow you to access all data contained in
`$module\_result <http://doc.ez.no/eZ-Publish/Technical-manual/4.x/Templates/The-pagelayout/Variables-in-pagelayout#module_result>`__,
from the legacy kernel.

This allows you to import information directly into twig templates. For
more details please check the available examples on using the
``ezpublish.legacy.get`` property for retrieving \ `persistent
variables <https://confluence.ez.no/display/EZP/Legacy+template+fallback#Legacytemplatefallback-Persistentvariable>`__
and
`assets <https://confluence.ez.no/display/EZP/Legacy+template+fallback#Legacytemplatefallback-Assets>`__.

| 

As a usage example, if you want to access the legacy information related
to 'content\_info' you can do it, as shown in the following example:

**Example on accessing 'content\_info' under ezpublish.legacy**

.. code:: theme:

    {{ ezpublish.legacy.get('content_info') }}

The previous call will return the contents on the 'content\_info' as an
``array``, and if we ``dump`` it the result will be similar to the
following:

::

    array
      'object_id' => string '57' (length=2)
      'node_id' => string '2' (length=1)
      'parent_node_id' => string '1' (length=1)
      'class_id' => string '23' (length=2)
      'class_identifier' => string 'landing_page' (length=12)
      'remote_id' => string '8a9c9c761004866fb458d89910f52bee' (length=32)
      'node_remote_id' => string 'f3e90596361e31d496d4026eb624c983' (length=32)
      'offset' => boolean false
      'viewmode' => string 'full' (length=4)
      'navigation_part_identifier' => string 'ezcontentnavigationpart' (length=23)
      'node_depth' => string '1' (length=1)
      'url_alias' => string '' (length=0)
      'current_language' => string 'eng-GB' (length=6)
      'language_mask' => string '3' (length=1)
      'main_node_id' => string '2' (length=1)
      'main_node_url_alias' => boolean false
      'persistent_variable' => 
        array
          'css_files' => 
            array
              0 => string 'video.css' (length=9)
          'js_files' => 
            array
              0 => string 'video.js' (length=8)
      'class_group' => boolean false
      'state' => 
        array
          2 => string '1' (length=1)
      'state_identifier' => 
        array
          0 => string 'ez_lock/not_locked' (length=18)
      'parent_class_id' => string '1' (length=1)
      'parent_class_identifier' => string 'folder' (length=6)
      'parent_node_remote_id' => string '629709ba256fe317c3ddcee35453a96a' (length=32)
      'parent_object_remote_id' => string 'e5c9db64baadb82ab8db54f0e2192ec3' (length=32)

Additionally, for retrieving information contained in 'content\_info'
such as the current language of the content in the page you can do it
like in the following example:

**Example on retrieving 'current\_language'**

.. code:: theme:

    {{ ezpublish.legacy.get('content_info')['current_language'] }}

Document generated by Confluence on Feb 12, 2014 16:43
