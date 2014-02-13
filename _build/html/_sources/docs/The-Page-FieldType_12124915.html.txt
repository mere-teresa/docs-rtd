#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Reference <Reference_10158191.html>`__
#. `FieldTypes <FieldTypes_10158198.html>`__

eZ Publish Platform 5.<next> : The Page FieldType
=================================================

Added by jerome.vieilledent@ez.no , edited by andre.romcke@ez.no on Jan
26, 2014

-  `Description <#ThePageFieldType-Description>`__
-  `Configuration <#ThePageFieldType-Configuration>`__

   -  `Defining a zone layout <#ThePageFieldType-Definingazonelayout>`__
   -  `Available blocks <#ThePageFieldType-Availableblocks>`__
   -  `Block template
      selection <#ThePageFieldType-Blocktemplateselection>`__

      -  `Available matchers <#ThePageFieldType-Availablematchers>`__

-  `Displaying the Page
   content <#ThePageFieldType-DisplayingthePagecontent>`__

   -  `Variables passed to the layout
      template <#ThePageFieldType-Variablespassedtothelayouttemplate>`__

-  `Rendering blocks <#ThePageFieldType-Renderingblocks>`__

   -  `Using ez\_page:viewBlock <#ThePageFieldType-Usingez_page%3AviewBlock>`__

      -  `Available arguments <#ThePageFieldType-Availablearguments>`__
      -  `Variables exposed to the block
         template <#ThePageFieldType-Variablesexposedtotheblocktemplate>`__

   -  `Rendering Block items <#ThePageFieldType-RenderingBlockitems>`__

-  `The PageService object <#ThePageFieldType-ThePageServiceobject>`__

   -  `Main methods <#ThePageFieldType-Mainmethods>`__

-  `Example <#ThePageFieldType-ExampleExample>`__

Description
-----------

With the Page FieldType, in legacy part of "ezflow" extension, editors
can define a \ **layout** with **multiple zones** within a single front-
or landing-page.

Within each zone, editors create \ **blocks** that contain particular
content categories. Specific content can be added to these blocks, they
are called \ **block items**.

This is particularly useful for managing homepages/landing pages.

Icon

| The Page FieldType is currently **read-only** with the Public API.
| It's however still possible to edit content with it through the admin
interface (which runs through the legacy stack). 

Name

Internal name

Expected input

Output

``Page``

``ezpage``

``N/A``

``eZ\Publish\Core\FieldType\Page\Parts\Page ``

Configuration
-------------

Warning

Icon

You still need to define your available layouts and blocks in the legacy
part to get them available in the admin interface. Please refer to `eZ
Publish legacy
documentation <http://doc.ez.no/Extensions/eZ-Publish-extensions/eZ-Flow/eZ-Flow-2.5/Configuration/Configuring-the-eZ-Flow-block-system>`__
to learn how to do so.

Defining a zone layout
~~~~~~~~~~~~~~~~~~~~~~

A layout is a combination of zones that are placed on a page. The
placement of the zones is defined in a template that is specified as
part of the layout configuration. You can define as many layouts as you
need.

You can define a new layout and enable it in your main YAML
configuration:

**ezpublish.yml**

.. code:: theme:

    ezpublish:
        system:
            my_siteaccess:
                ezpage:
                    layouts:
                        myLayoutIdentifier:
                            name: "My über cool layout"
                            template: "AcmeDemoBundle:page/zonelayouts:my_template.html.twig"
                    enabledLayouts: [myLayoutIdentifier]

Then, when rendering a Page FieldType
using \ ``myLayoutIdentifier``, \ ``Resources/views/page/zonelayouts/my_template.html.twig`` from
AcmeDemoBundle will be used (see `how to use template identifiers in
Symfony
documentation <http://symfony.com/doc/master/book/controller.html#rendering-templates>`__).

Tip

Icon

You can specify a legacy template in your layout definition.

.. code:: theme:

    ezpublish:
        system:
            my_siteaccess:
                ezpage:
                    layouts:
                        myLegacyLayout:
                            name: My legacy layout
                            template: "design:zone/my_legacy_template.tpl"
                    enabledLayouts: [myLayoutIdentifier]

However, doing so will defer block display to the legacy templates as
well.

Available blocks
~~~~~~~~~~~~~~~~

The blocks need to be defined and enabled in the YAML configuration as
well:

**ezpublish.yml**

.. code:: theme:

    ezpublish:
        system:
            my_siteaccess:
                ezpage:
                    blocks:
                        myBlockIdentifier:
                            name: "My über cool block"
                        myBlockIdentifier2:
                            name: "My über cool block 2"
                    enabledBlocks: [myBlockIdentifier, myBlockIdentifier2]

Tip

Icon

To avoid issues and since the Page field are only in read only mode in
eZ Publish 5.1, it is recommended to synchronize the block configuration
between the legacy stack and the new stack.

Block template selection
~~~~~~~~~~~~~~~~~~~~~~~~

Icon

Template selection rules are applied only when you render a block with
the ``PageController`` (using ``ez_page:viewBlock`` from templates),
`see below <The-Page-FieldType_12124915.html>`__.

Like you are able to \ `define template selection rules when displaying
Location and Content
objects <View-provider-configuration_2720462.html>`__, you can also
define rules for blocks, with dedicated matchers.

Configuration is a hash built in the following way:

**ezpublish.yml**

.. code:: theme:

    ezpublish:
        system:
            my_siteaccess:
                block_view:
                    # A simple unique key for your matching ruleset
                    my_rule_set:
                        # The template identifier to load, following the Symfony bundle notation for templates
                        template: AcmeTestBundle:block:campaign.html.twig
                        # Hash of matchers to use, with their corresponding values to match against
                        match:
                            # Key is the matcher "identifier" (class name or service identifier)
                            # Value will be passed to the matcher's setMatchingConfig() method.
                            Type: Campaign
                    another_rule:
                        template: AcmeTestBundle:block:custom_block.html.twig
                        match:
                            Type: CustomBlock

Tip

Icon

You can define your template selection rules in a different
configuration file. `Read the cookbook recipe to learn more about
it <Import-settings-from-a-bundle_12125508.html>`__ .

Icon

Matchers for ``block_view`` follow the same behavior than `matchers for
regular location\_view /
content\_view <View-provider-configuration_2720462.html#Viewproviderconfiguration-Matchers>`__,
except that their relative namespace will be
``eZ\Publish\Core\MVC\Symfony\View\BlockViewProvider\Configured\Matcher``
.

Hence you can combine matchers with AND and OR capabilities (`see main
matchers' documentation
page <https://confluence.ez.no/display/EZP/View+provider+configuration#Viewproviderconfiguration-Matchers>`__).

Available matchers
^^^^^^^^^^^^^^^^^^

Identifier

Description

``Type``

Matches the unique **block identifier** defined in the
legacy \ ``block.ini`` file (`see legacy
documentation <http://doc.ez.no/Extensions/eZ-Publish-extensions/eZ-Flow/eZ-Flow-2.5/Configuration/Configuring-the-eZ-Flow-block-system/Defining-the-eZ-Flow-blocks/Defining-block-types>`__).

For example with the following configuration in legacy block.ini, it
will match against ``Manual3Items``:

.. code:: theme:

    [Manual3Items]
    Name=3 Column News

| 

``View``

Matches the **view’s unique identifier** defined in the block definition
in the legacy \ ``block.ini`` (see legacy documentation).

For example with the following configuration in legacy ``block.ini``, it
will match against ``3_items1``:

.. code:: theme:

    [Manual3Items]
    Name=3 Column News
    ViewList[]=3_items1

Icon

When no view is defined, the default value is **default**.

| 

``Id\Block``

Matches against the block ID, as stored in ``ezm_block`` table

``Id\Zone``

Matches against the zone ID a block belongs to, as stored in
``ezm_block`` table

 

Displaying the Page content
---------------------------

Icon

This section focuses on how to display blocks from zone/layout
templates.

Render of these templates are triggered when using ``ez_render_field()``
helper, like for any other field type.

`See field rendering
documentation <Content-view_8323263.html#Contentview-Contentview-UsingtheFieldType%27stemplateblock>`__
for more information.

Layout template

Goal of a \ **layout** template is to display \ **zones** for the given
layout, depending on your layout configuration.

Variables passed to the layout template
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Variable name

Description

Type

``zones``

Zone objects for this Page field

Array of ``eZ\Publish\Core\FieldType\Page\Parts\Zone`` objects

``zone_layout``

The layout identifier (e.g. "2ZonesLayout1")

``string``

``pageService``

The PageService object (read more below). 

``eZ\Bundle\EzPublishCoreBundle\FieldType\Page``

Rendering blocks
~~~~~~~~~~~~~~~~

Each zone contain blocks that hold your content as block items. To
render blocks from a layout template, you need to do a sub-request.

Tip

Icon

You can use a custom controller to display a block.

However, if you do so, you might need to get access to the
`PageService <The-Page-FieldType_12124915.html>`__. You can get it via
the service container with identifier
``ezpublish.fieldType.ezpage.pageService``.

Using \ ``ez_page:viewBlock``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This controller is responsible of choosing the right template for your
block, \ `depending on the rules you
defined <The-Page-FieldType_12124915.html>`__.

You can use this controller from templates with the following syntax:

.. code:: theme:

    {{ render( controller( "ez_page:viewBlock", {'block': myBlock} ) ) }}

Available arguments
'''''''''''''''''''

Name

Description

Type

Default value

``block``

The block object you want to render

``eZ\Publish\Core\FieldType\Page\Parts\Block``

N/A

``params``

Hash of variables you want to inject to sub-template, key being the
exposed variable name.

.. code:: theme:

    {{ render(
          controller( 
              "ez_page:viewBlock", 
              {
                  'block': myBlock,
                  'params': { 'some_variable': 'some_value' }
              }
          )
    ) }}

hash

empty

``cacheSettings``

Hash of cache settings to use by the sub-controller (useful if you use
ESI or Hinclude strategies).

.. code:: theme:

    {{ render_esi(
          controller( 
              "ez_page:viewBlock", 
              {
                  'block': myBlock,
                  'params': { 'some_variable': 'some_value' },
                  'cacheSettings': { 'smax-age': 600 }
              }
          )
    ) }}

hash (accepted keys are ``max-age`` and ``smax-age``)

empty

| 

Legacy BC

Icon

If no template selection rule is matched, the system will fallback to
the legacy kernel and will use rules you might have defined in legacy.
The result will be `the same as when using legacy ``block_view_gui``
function <http://doc.ez.no/Extensions/eZ-Publish-extensions/eZ-Flow/eZ-Flow-2.5/Configuration/Configuring-the-eZ-Flow-block-system/Defining-the-eZ-Flow-blocks/Defining-block-types#eztoc134744_0_2>`__.

However, additional variables (from the ``params`` argument) won't be
passed to the resulted template.

Variables exposed to the block template
'''''''''''''''''''''''''''''''''''''''

Variable name

Type

Description

``block``

``eZ\Publish\Core\FieldType\Page\Parts\Block``

The block to display

``valid_items``

Array of ``eZ\Publish\Core\FieldType\Page\Parts``

Displayable block items

``valid_contentinfo_items``

Array of ``eZ\Publish\API\Repository\Values\Content\ContentInfo``

Displayable block items, as ``ContentInfo`` objects.

``pageService`` (deprecated as of v5.2)

``eZ\Bundle\EzPublishCoreBundle\FieldType\Page\PageService``

`The PageService object <The-Page-FieldType_12124915.html>`__
(deprecated)

And of course, all the additional variables \ `you injected in
the \ ``params`` argument  <The-Page-FieldType_12124915.html>`__.

Icon

``valid_items`` and ``valid_contentinfo_items`` variables are available
as of **v5.2 / 2013.11**.

Usage of ``pageService`` is deprecated as of **v5.2 / 2013.11** and will
be removed in v6.0.

 

Rendering Block items
^^^^^^^^^^^^^^^^^^^^^

As said above, \ **a block holds your displayable content as block
items** which consists
of \ ``eZ\Publish\Core\FieldType\Page\Parts\Item`` objects. Among \ `the
available
properties <https://github.com/ezsystems/ezpublish-kernel/blob/master/eZ/Publish/Core/FieldType/Page/Parts/Item.php>`__,
you will find \ ``contentId`` and ``locationId`` which reference the
content/location you want to display. All you have to do then is
to \ **render it
view \ ``ez_content:viewLocation`` or ``ez_content:viewContent``** (`see
full example below <The-Page-FieldType_12124915.html>`__).

The PageService object
~~~~~~~~~~~~~~~~~~~~~~

The PageService object
(``eZ\Bundle\EzPublishCoreBundle\FieldType\Page\PageService``) is a
helper giving the possibility to get current zone/block definitions and
to retrieve block items.

Main methods
^^^^^^^^^^^^

Method name

Description

Return type

``getZoneDefinition()``

Returns zone definition (all defined zones for the current siteaccess)
as an array

``array``

``getZoneDefinitionByLayout()``

Returns a zone definition for a given layout. It consists of a
configuration array for the given layout.

``array``

``getBlockDefinition()``

Returns block definition as an array

``array``

``getBlockDefinitionByIdentifier()``

Returns a block definition for a given block identifier.

``array``

``getValidBlockItems()``

Returns valid items (that are to be displayed), for a given block.

``eZ\Publish\Core\FieldType\Page\Parts\Item[]``

``getLastValidBlockItem()``

Returns the last valid item, for a given block.

``eZ\Publish\Core\FieldType\Page\Parts\Item|null``

``getWaitingBlockItems()``

Returns queued items (the next to be displayed), for a given block.

``eZ\Publish\Core\FieldType\Page\Parts\Item[]``

``getArchivedBlockItems()``

Returns archived items (that were previously displayed), for a given
block.

``eZ\Publish\Core\FieldType\Page\Parts\Item[]``

``getValidBlockItemsAsContentInfo()``

Returns valid block items as content objects

``eZ\Publish\API\Repository\Values\Content\ContentInfo[] ``

Example
~~~~~~~

**2zoneslayout1.html.twig**

.. code:: theme:

    <h2>TWIG Template for 2zoneslayout1 zone</h2>
    <div class="zone-layout-{{ zone_layout|lower }} row">
        <div class="span8">
            <section class="content-view-block">
            {% if zones[0].blocks %}
                {# Rendering blocks with default PageController #}
                {% for block in zones[0].blocks %}
                    {{ render( controller( "ez_page:viewBlock", {'block': block} ) ) }}
                {% endfor %}
                <div class="block-separator"></div>
            {% endif %}
            </section>
        </div>
        <div class="span4">
            <aside>
                <section class="content-view-block content-view-aside">
                {% if zones[1].blocks %}
                    {# Still rendering with default PageController, but passing specific cache value (TTL of 100 seconds) and using ESI #}
                    {% for block in zones[1].blocks %}
                        {{ render_esi( controller( "ez_page:viewBlock", {'block': block, 'cacheSettings': {'smax-age': 100}} ) ) }}
                    {% endfor %}
                    <div class="block-separator"></div>
                {% endif %}
                </section>
            </aside>
        </div>
    </div>

**campaign\_block.html.twig**

.. code:: theme:

    <h3>TWIG Template for Campaign block type</h3>
    {% set validContentInfoItems = pageService.getValidBlockItemsAsContentInfo( block ) %}
    {% set validItems = pageService.getValidBlockItems( block ) %}
    <!-- BLOCK: START -->
    <div class="block-type-campaign">
        <div class="campaign">
            <a href="#" class="navig prev" style="opacity:0;"><span class="hide">&lt;</span></a>
            <a href="#" class="navig next"><span class="hide">&gt;</span></a>
            <ul class="indicator">
                {% for contentInfo in validContentInfoItems %}
                    <li><span>{{ contentInfo.name }} (#{{ contentInfo.id }})</span></li>
                {% endfor %}
            </ul>
            <ul class="images">
            {# Rendering valid items with regular view controller, with "block_item_campaign" view type #}
            {# Also passing an "image_class" parameter which will be available in sub-template. #}
            {% for item in validItems %}
                {{ render(
                    controller(
                        'ez_content:viewLocation',
                        {
                            'locationId': item.locationId,
                            'viewType': 'block_item_campaign',
                            'params': {'image_class': 'campaign'}
                        }
                    )
                ) }}
            {% endfor %}
            </ul>
        </div>
    </div>
    <!-- BLOCK: END -->

Comments:
---------

I have a question about configuration part. when you define layout
template in yaml file like

*``template: ``\ ``"AcmeDemoBundle:page/zonelayouts/my_template.html.twig"``*

Do i suppose that i need to create that file under Resource/views in my
bundle? Or do i need something more?

Thanks!. 

 

|image0| Posted by desorden at May 07, 2013 07:01

Carlos: Yes (updated the doc). Your comment also pointed out that the
template identifier was wrong. You must use:

.. code:: theme:

    template: "AcmeDemoBundle:page/zonelayouts:my_template.html.twig"

|image1| Posted by jerome.vieilledent@ez.no at May 07, 2013 07:49

Thanks Jerome. Now i know what it wasn't working for me last
night \ |(smile)|

|image3| Posted by desorden at May 07, 2013 07:50

Two things about the provided samples:

In the zone definition sample, i think there's "wrong order". It says

``ezpublish:``

``    ``\ ``system:``

``        ``\ ``my_siteaccess:``

``            ``\ ``layouts:``

``                ``\ ``ezpage:``

where it should say (i think )

``ezpublish:``

``    ``\ ``system:``

``        ``\ ``my_siteaccess:``

``            ezpage``\ ``:``

``                layouts``\ ``:``

And about defining the zone with a legacy template,

``ezpublish:``

``    ``\ ``system:``

``        ``\ ``my_siteaccess:``

``            ``\ ``ezpage:``

``                ``\ ``myLegacyLayout:``

don't wee need to add "layouts" between ezpage: and myLegacyLayout?

Thanks

|image4| Posted by desorden at May 08, 2013 20:45

You're right Carlos, fixing.

|image5| Posted by jerome.vieilledent@ez.no at May 08, 2013 20:52

The warning at the top of the page says that bock and zone definition is
still needed for the backoffice to work. But it seems that if there is
no list of enabled layouts in the eZ5 configuration, an exception is
thrown (or if a content with a Page field which uses a layout which is
not defined as enabled in ez5 configuration).

Should this be filed as bug? (the best thing to do would be imho to
avoid as much as possible to have to do double-configuration, as long as
the ez-4 part is still mandatory)

|image6| Posted by gaetano.giunta@ez.no at Jan 24, 2014 14:20

Document generated by Confluence on Feb 12, 2014 16:43

.. |image0| image:: images/icons/contenttypes/comment_16.png
.. |image1| image:: images/icons/contenttypes/comment_16.png
.. |(smile)| image:: images/icons/emoticons/smile.png
.. |image3| image:: images/icons/contenttypes/comment_16.png
.. |image4| image:: images/icons/contenttypes/comment_16.png
.. |image5| image:: images/icons/contenttypes/comment_16.png
.. |image6| image:: images/icons/contenttypes/comment_16.png
