#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Developing with eZ Publish
   5 <Developing-with-eZ-Publish-5_2720528.html>`__
#. `MVC and Application <MVC-and-Application_2719826.html>`__

eZ Publish Platform 5.<next> : Content view
===========================================

Added by jerome.vieilledent@ez.no , edited by jerome.vieilledent@ez.no
on Feb 12, 2014

-  `The ViewController <#Contentview-TheViewController>`__
-  `View selection <#Contentview-Viewselection>`__
-  `Location view template <#Contentview-Locationviewtemplate>`__

   -  `Available variables <#Contentview-Availablevariables>`__
   -  `Template inheritance <#Contentview-Templateinheritance>`__
   -  `Rendering content's
      fields <#Contentview-Renderingcontent%27sfields>`__

      -  `Getting raw Field value <#Contentview-GettingrawFieldvalue>`__
      -  `Using the FieldType's template
         block <#Contentview-UsingtheFieldType%27stemplateblock>`__

   -  `Rendering Content name <#Contentview-RenderingContentname>`__

      -  `Name property in
         ContentInfo <#Contentview-NamepropertyinContentInfo>`__
      -  `Translated name <#Contentview-Translatedname>`__

   -  `Exposing additional
      variables <#Contentview-Exposingadditionalvariables>`__

-  `Making links to other
   locations <#Contentview-Makinglinkstootherlocations>`__
-  `Render embedded content
   objects <#Contentview-Renderembeddedcontentobjects>`__

   -  `Using ez\_content
      controller <#Contentview-Usingez_contentcontroller>`__

      -  `Available arguments <#Contentview-Availablearguments>`__

The ViewController
------------------

eZ Publish comes with a native controller to display your content, known
as the **``ViewController``**. It is called each time you try to reach a
content from its **Url Alias** (*good looking* URI generated for any
content) and is able to render any content previously edited in the
admin interface or via the `eZ Publish Public
API <eZ-Publish-Public-API_1736723.html>`__.

It can also be called directly by its direct URI :
``/content/location/<locationId>``

A content can also have different **view types** (full page, abstract in
a list, block in a landing page...). By default the view type is
**full** (for full page), but it can be anything (*line*, *block*...).

Important note regarding visibility

Icon

Location visibility flag, which you can change with hide/unhide in
admin, is not permission based and thus acts as a simple potential
filter. **It is not meant to restrict access to content**.

If you need to restrict access to a given content, use **Sections or
Object states**, which are permission based.

 

View selection
--------------

To display a content, the ViewController uses a view manager which
selects the appropriate template depending on matching rules.

Icon

For more information about the **view provider configuration**, please
`refer to the dedicated
page <View-provider-configuration_2720462.html>`__.

Icon

You can also `use your own custom controller to render a
content/location <How-to-use-a-custom-controller-to-display-a-content-or-location_13468497.html>`__.

Location view template
----------------------

A content view template is like any other template, with several
specific aspects.

Available variables
~~~~~~~~~~~~~~~~~~~

Variable name

Type

Description

**``location``**

`eZ\\Publish\\Core\\Repository\\Values\\Content\\Location <https://github.com/ezsystems/ezp-next/blob/master/eZ/Publish/Core/Repository/Values/Content/Location.php>`__\ ` <https://github.com/ezsystems/ezp-next/blob/master/eZ/Publish/Core/Repository/Values/Content/Content.php>`__

| The location object. Contains meta information on the content
(`ContentInfo <https://github.com/ezsystems/ezp-next/blob/master/eZ/Publish/Core/Repository/Values/Content/ContentInfo.php>`__)
| (only when accessing a location) 

``content``

`eZ\\Publish\\Core\\Repository\\Values\\Content\\Content <https://github.com/ezsystems/ezp-next/blob/master/eZ/Publish/Core/Repository/Values/Content/Content.php>`__

The content object, containing all fields and version information
(`VersionInfo <https://github.com/ezsystems/ezp-next/blob/master/eZ/Publish/Core/Repository/Values/Content/VersionInfo.php>`__)

``noLayout``

Boolean

| If true, indicates if the content/location is to be displayed without
any pagelayout (i.e. AJAX, sub-requests...).
| It's generally ``false`` when displaying a content in view type
**full**. 

``viewBaseLayout``

String

The base layout template to use when the view is requested to be
generated outside of the pagelayout (when ``noLayout`` is true).

Template inheritance
~~~~~~~~~~~~~~~~~~~~

Like any template, a content view template can use `template
inheritance <http://symfony.com/doc/current/book/templating.html#template-inheritance-and-layouts>`__.
However keep in mind that your content can be also requested via
`sub-requests <http://symfony.com/doc/current/book/templating.html#embedding-controllers>`__
(see below how to render embedded content objects). In this case your
template should probably not extend your main layout.

In this regard, it is recommended to use inheritance this way:

.. code:: theme:

    {% extends noLayout ? viewbaseLayout : "AcmeDemoBundle::pagelayout.html.twig" %}

    {% block content %}
    ...
    {% endblock %}

Rendering content's fields
~~~~~~~~~~~~~~~~~~~~~~~~~~

As stated above, a view template receives the requested Content object,
holding all fields.

In order to display the fields' value the way you want, you can either
manipulate the Field Value object itself or use a template.

Getting raw Field value
^^^^^^^^^^^^^^^^^^^^^^^

Having access to the Content object in the template, you can use `its
public
methods <https://github.com/ezsystems/ezpublish-kernel/blob/master/eZ/Publish/Core/Repository/Values/Content/Content.php>`__
to access to all the information you need. You can also
use \ `ez\_field\_value <ez_field_value_17105557.html>`__ helper to get
the Field value in the current language if translation is available.

.. code:: theme:

    {# With the following, myFieldValue will be in the content's main language, regardless the current language #}
    {% set myFieldValue = content.getFieldValue( 'some_field_identifier' ) %}
     
    {# Here myTranslatedFieldValue will be in the current language if a translation is available. If not, the content's main language will be used #}
    {% set myTranslatedFieldValue = ez_field_value( content, 'some_field_identifier' ) %}

Using the FieldType's template block
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

All built-in FieldTypes come with `a piece of Twig template
code <https://github.com/ezsystems/ezpublish-kernel/blob/master/eZ/Bundle/EzPublishCoreBundle/Resources/views/content_fields.html.twig>`__
you can take advantage of by calling ``ez_render_field()`` helper.

.. code:: theme:

    {{ ez_render_field( content, 'some_field_identifier' ) }}

Refer to ```ez_render_field()`` reference
page <ez_render_field_12779554.html>`__ for further information.

Icon

As this makes use of reusable templates, **using ``ez_render_field()``
is the recommended way and is to be considered as a best practice**.

Rendering Content name
~~~~~~~~~~~~~~~~~~~~~~

The **name** of a content is its generic "title", generated by the
repository considering several rules in the FieldDefinition. It usually
consists in the normalized value of the first field.

There are 2 different ways to access to this special property:

-  Through the name property of ContentInfo (not translated).
-  Through VersionInfo with the TranslationHelper (translated).

Name property in ContentInfo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This property is the actual content name, but **in main language only**
(so it is not translated).

.. code:: theme:

    <h2>Content name: {{ content.contentInfo.name }}</h2>

.. code:: theme:

    $contentName = $content->contentInfo->name;

Translated name
^^^^^^^^^^^^^^^

Icon

The TranslationHelper service is available as of version 5.2 / 2013.09

The *translated name* is held in ``VersionInfo`` object, in the names
property which consists of hash indexed by locale. You can easily
retrieve it in the right language via the ``TranslationHelper`` service.

.. code:: theme:

    <h2>Translated content name: {{ ez_content_name( content ) }}</h2>
    <h3>Also works from ContentInfo : {{ ez_content_name( content.contentInfo ) }}</h3>

You can refer to `ez\_content\_name() reference
page <ez_content_name_17105551.html>`__ for further information.

 

.. code:: theme:

    // Assuming we're in a controller action
    $translationHelper = $this->get( 'ezpublish.translation_helper' );
     
    // From Content
    $translatedContentName = $translationHelper->getTranslatedContentName( $content );
    // From ContentInfo
    $translatedContentName = $translationHelper->getTranslatedContentNameByContentInfo( $contentInfo );

Icon

The helper will respect the prioritized languages. 

If there is no translation for your prioritized languages, the helper
will always return the name in the main language.

You can also **force a locale** in a 2nd argument:

.. code:: theme:

    {# Force fre-FR locale. #}
    <h2>{{ ez_content_name( content, 'fre-FR' ) }}</h2>

.. code:: theme:

    // Assuming we're in a controller action
    $translatedContentName = $this->get( 'ezpublish.translation_helper' )->getTranslatedName( $content, 'fre-FR' );

Exposing additional variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is possible to expose additional variables in a content view
template. See \ `parameters injection in content
views <Parameters-injection-in-content-views_8323330.html>`__ or `use
your own custom controller to render a
content/location <How-to-use-a-custom-controller-to-display-a-content-or-location_13468497.html>`__.

Making links to other locations
-------------------------------

Linking to other locations is fairly easy and is done with
`native \ ``path()`` Twig
helper <http://symfony.com/doc/2.3/book/templating.html#linking-to-pages>`__
(or ``url()`` if you want to generate absolute URLs). You just have to
pass it the Location object and ``path()`` will generate the URLAlias
for you.

.. code:: theme:

    {# Assuming "location" variable is a valid eZ\Publish\API\Repository\Values\Content\Location object #}
    <a href="{{ path( location ) }}">Some link to a location</a>

If you don't have the Location object, but only its ID, you can generate
the URLAlias the following way:

.. code:: theme:

    <a href="{{ path( "ez_urlalias", {"locationId": 123} ) }}">Some link to a location, with its Id only</a>

Under the hood

Icon

In the backend, ``path()`` uses the Router to generate links.

This makes also easy to generate links from PHP, via the ``router``
service.

 

Render embedded content objects
-------------------------------

Rendering an embedded content from a Twig template is pretty straight
forward as you just need to **do a subrequest with ``ez_content``
controller**.

Using \ ``ez_content`` controller
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This controller is exactly the same as \ `the ViewController presented
above <Content-view_8323263.html>`__ and has 2 main actions:

-  **``viewLocation``** to render a location (same as when accessing a
   content through an URLAlias)
-  **``viewContent``** to render a content

You can use this controller from templates with the following syntax:

**eZ Publish 5.1+ / Symfony 2.2+**

.. code:: theme:

    {{ render( controller( "ez_content:viewLocation", {"locationId": 123, "viewType": "line"} ) ) }}

 

The example above allows you to render a Location which ID is **123**,
with the view type \ **line**.

Icon

Reference of ``ez_content`` controller follow the syntax of *controllers
as a service*, `as explained in Symfony
documentation <http://symfony.com/doc/current/cookbook/controller/service.html>`__.

Available arguments
^^^^^^^^^^^^^^^^^^^

As any controller, you can pass arguments to ``ez_content:viewLocation``
or ``ez_content:viewContent`` to fit your needs.

Name

Description

Type

Default value

``locationId``

| Id of the location you want to render.
| **Only for ``ez_content:viewLocation``** 

integer

N/A

``contentId``

| Id of the content you want to render.
| **Only for ``ez_content:viewContent``** 

integer

N/A

``viewType``

| The view type you want to render your content/location in.
| Will be used by the ViewManager to select corresponding template,
according to defined rules. 

Example: full, line, my\_custom\_view, ...

string

full

``layout``

Indicates if the sub-view needs to use the main layout (see `available
variables in a view template <Content-view_8323263.html>`__)

 

boolean

false

``params``

Hash of variables you want to inject to sub-template, key being the
exposed variable name.

Icon

Available as of eZ Publish 5.1

.. code:: theme:

    {{ render(
          controller( 
              "ez_content:viewLocation", 
              {
                  "locationId": 123,
                  "viewType": "line",
                  "params": { "some_variable": "some_value" }
              }
          )
    ) }}

hash

empty hash

 

ESI

Just as for regular Symfony controllers, you can take advantage of ESI
and use different cache levels:

**Using ESI (eZ Publish 5.1+ / Symfony 2.2+)**

.. code:: theme:

    {{ render_esi( controller( "ez_content:viewLocation", {"locationId": 123, "viewMode": "line"} ) ) }}

 

 

 Asynchronous rendering

Symfony also supports asynchronous content rendering with the help
of \ `hinclude.js <http://mnot.github.com/hinclude/>`__ library.

**Asynchronous rendering (eZ Publish 5.1+ / Symfony 2.2+)**

.. code:: theme:

    {{ render_hinclude( controller( "ez_content:viewLocation", {"locationId": 123, "viewMode": "line"} ) ) }}

 

 

Icon

`hinclude.js <http://mnot.github.com/hinclude/>`__ needs to be properly
included in your layout to work.

Please `refer to Symfony
documentation <http://symfony.com/doc/current/book/templating.html#asynchronous-content-with-hinclude-js>`__
for all available options.

Document generated by Confluence on Feb 12, 2014 16:43
