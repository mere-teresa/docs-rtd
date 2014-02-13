#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `eZ Publish Public API <eZ-Publish-Public-API_1736723.html>`__

eZ Publish Platform 5.<next> : Public API basics
================================================

Added by bertrand.dunogier@ez.no , edited by ricardo.correia@ez.no on
Aug 02, 2013

eZ Publish API Repository
~~~~~~~~~~~~~~~~~~~~~~~~~

This entity is the entry point to everything you will do with the Public
API.

It will allow you to create, retrieve, update and delete all the eZ
Publish objects, as well as content types, sections, content states. It
is always obtained through the service container.

**Obtaining the eZ Publish Repository via the service container**

.. code:: theme:

    /** @var $repository \eZ\Publish\API\Repository\Repository
    $repository = $container->get( 'ezpublish.api.repository' );

 By itself, the repository doesn't do much. It allows three type of
operations: user authentication (getting / changing the current user),
issuing transactions, and obtaining services. 

Inline objects documentation

Icon

Pay attention to the inline phpdoc block in this code stub. It tells
your IDE that \ ``$repository`` is an instance
of \ ``\eZ\Publish\API\Repository\Repository``. If your IDE supports
this feature, you will get code completion on
the \ ``$repository`` object. This helper is a huge time saver when it
comes to learning about the eZ Publish API.

The service container
~~~~~~~~~~~~~~~~~~~~~

The above code snippet implies that the `service
container <http://symfony.com/doc/2.0/book/service_container.html>`__ is
available in the context you are writing your code in.

In controllers, this generally is done by extending the
Symfony \ ``Controller`` class. It comes with a \ ``get()`` method that
calls the service container. In command line scripts, it requires that
you extend
the \ ```ContainerAwareCommand`` <http://api.symfony.com/2.1/Symfony/Bundle/FrameworkBundle/Command/ContainerAwareCommand.html>`__ base
class instead of \ ``Controller``. This class provides you with
a \ ``getContainer()`` method that returns the service container.

Getting the repository from eZ Publish 5 controllers

Icon

In order to make it even easier to obtain the repository from
controllers code, eZ Publish 5 controllers extend a
custom \ `Controller <http://apidoc.ez.no/sami/trunk/NS/html/eZ/Bundle/EzPublishCoreBundle/Controller.html>`__
class that provides a
```getRepository()`` <http://apidoc.ez.no/sami/trunk/NS/html/eZ/Bundle/EzPublishCoreBundle/Controller.html#method_getRepository>`__
method which directly returns the repository from the service container.

You can and should of course do the same in your custom controllers.

Authentication
~~~~~~~~~~~~~~

One of the responsibilities of the Repository is user authentication.
Every action will be executed \ *as* a user. In the context of a normal
eZ Publish execution, the logged in user will of course be the current
one, identified via one of the available authentication methods. This
user's permissions will affect the behavior of the Repository. The user
may f.e. not be allowed to create Content, or view a particular section.

Logging in to the Repository is covered in the recipes of the Cookbook.

Services
~~~~~~~~

The main entry point to the repository's features are services. The
Public API breaks down access to Content, User, Content Types, etc
features into various services. Those services are obtained via
the \ `Repository <http://apidoc.ez.no/sami/trunk/NS/html/eZ/Publish/API/Repository/Repository.html>`__,
using getServiceName() like
methods: \ ``getContentService()``, \ ``getUserService()``...

Throughout the Cookbook, you will be guided through the various
capabilities those services have, and how you can use them to implement
your projects.

Value objects
~~~~~~~~~~~~~

While Services provide interaction with the repository, the elements
(Content, Users) they provide interaction with are provided as read
only \ `Value
Objects <http://apidoc.ez.no/doxygen/trunk/NS/html/namespaceeZ_1_1Publish_1_1Core_1_1Repository_1_1Values.html>`__,
in the \ ``eZ\Publish\Core\Repository\Values`` namespace. Those objects
are broken down into
sub-namespaces: \ ``Content``, \ ``ContentType``, \ ``User`` and ``ObjectState``,
each sub-namespace containing a set of value objects, such
as \ ```Content\Content`` <https://github.com/ezsystems/ezp-next/blob/master/eZ/Publish/Core/Repository/Values/Content/Content.php>`__ or ```User\Role`` <https://github.com/ezsystems/ezp-next/blob/master/eZ/Publish/Core/Repository/Values/User/Role.php>`__.

These objects are read-only by design. They are only meant to be used in
order to fetch data from the repository. They come with their own
properties, such as ``$content->id``, ``$location->hidden``, but also
with methods that provide access to more, related information, such as
``Relation::getSourceContentInfo()`` or ``Role::getPolicies()``. By
design, a value object will only give you access to data that is very
closely related to it. More complex retrieval operations will require
you to use the appropriate Service, using information from your Value
Object.

Value info objects
^^^^^^^^^^^^^^^^^^

Some complex Value Objects have an Info counterpart,
like \ ``ContentInfo``, the counterpart for \ ``Content``. These objects
are specific, and provide you with lower level information. For
instance, \ ``ContentInfo`` will provide you with currentVersionNo or
remoteId, while Content will let you retrieve Fields, the ContentType,
or previous Versions.

They are provided by the API, but are \ **read only**, can't be modified
and sent back. Creation and modification of Repository values are done
using Create structs and Update structs.

Create and update structs
~~~~~~~~~~~~~~~~~~~~~~~~~

In order to update or create elements in the Repository, you will use
structs. They are usually provided by the Service that manages the Value
Objects you want to alter or create. For instance, the Content service
has a \ ``getContentCreateStruct()`` method that returns a
new \ ``ContentCreateStruct`` object. Equivalent methods exist for
UpdateStruct objects as well, and for most Value Objects.

Using them is also covered in the Cookbook.

Document generated by Confluence on Feb 12, 2014 16:43
