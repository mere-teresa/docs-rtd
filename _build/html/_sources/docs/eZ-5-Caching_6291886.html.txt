#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Features <Features_12781009.html>`__

eZ Publish Platform 5.<next> : eZ 5 Caching
===========================================

Added by christian.bacher@ez.no , edited by andre.romcke@ez.no on Jan
20, 2013

-  `Cache Architecture for content
   repository <#eZ5Caching-CacheArchitectureforcontentrepository>`__

   -  `PUBLIC API Cache <#eZ5Caching-PUBLICAPICache>`__

      -  `Cached objects and methods using
         cache <#eZ5Caching-Cachedobjectsandmethodsusingcache>`__
      -  `The cache relevant write
         actions <#eZ5Caching-Thecacherelevantwriteactions>`__

   -  `Persistence SPI Cache <#eZ5Caching-PersistenceSPICache>`__

      -  `Cached objects and methods using
         cache <#eZ5Caching-Cachedobjectsandmethodsusingcache.1>`__
      -  `Cache relevant write
         actions <#eZ5Caching-Cacherelevantwriteactions>`__

-  `Cache SPI/API <#eZ5Caching-CacheSPI%2FAPI>`__

Cache Architecture for content repository
=========================================

The goal is to have transparent cache layers which do not interfere with
the implementation of the public API and storage engines and where multi
level caches with different cache strategies can be plugged in.

|image0|

A Level 1 cache could for example be a memory cache which is used only
in scope of a single request. Other levels could be APC based or
memcached based caches. Each cache level for PAPI or Persistence
respectively might be implemented with the same code by doing the actual
caching via a cache SPI. So the levels  of a cache themselves differ by
there usage of different cache SPI implementations.

As the caches implement the complete API each cache is able to
invalidate objects on the relevant write actions. But this implies that
no other relevant write actions  exist which do not use the public API.
If there are such modules which perform cache relevant write actions not
using the public API each cache must be able to be purged from "outside"
(e.g. via signal slot). But this should be avoided.

As higher level caches like http caches/reverse proxies/CDNs already use
TTLs for invalidating objects and the TTL must increase on the lower
cache levels the public API Cache is most useful if it has no TTL and
purges its objects if they change (as a large TTL anyway would imply
that the objects have to be purged on changes, too).

Under the hood the publicAPI caches are using the cache SPI (see below)
which abstracts underlying cache engines like APC or memcached or custom
cache implementations. This implies that we don't make here for now a
difference between clustered or standalone applications as the
underlying cache implementation is responsible for handling distributed
caching.

PUBLIC API Cache
~~~~~~~~~~~~~~~~

The public API Cache has to cache objects per user and reflects the
permission handling by caching UnauthorizedExceptions. In general the
most important user to handle is the anonymous user, nevertheless the
more a website provides user generated content or relies on personalized
views other users will become important as well.  The user specific
cache has to be cleared  if permissions change for a this user.  Note
that this is the main objective of the public API cache as the caching
of the persistence values to reduce load of database calls is done by a
persistence SPI cache (see below).

As of lack of concrete usage data the determination of to be cached
objects is based on guesses (that means we assume that methods using
cache are either often called or return objects which are seldom
changed) The following objects are a good point to start with:

Cached objects and methods using cache
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Content (which includes a published version - i.e. archived Content and
Drafts are not cached)

    ContentService::loadContent

    ContentService::loadContentByContentInfo

    ContentService::loadContentByVersionInfo

    ContentService::loadContentByRemoteId

ContentInfo

    ContentService::loadContentInfo

    ContentService::loadContentInfoByRemoteId

VersionInfo

    ContentService::loadVersionInfoById

    ContentService::loadVersionInfo

    ContentService::loadVersions

Location

    LocationService::loadLocation

    LocationService::loadLocationByRemoteId    

Relations of published content

    ContentService::loadRelations

Section

    SectionService::loadSection

    SectionService::loadSections

ContentType

    ContentTypeService::loadContentType

    ContentTypeService::loadContentTypeByIdentifier

    ContentTypeService::loadContentTypeByRemoteId    

User

    UserService::loadUser

    Repository::canUser

    Repostiory::hasAccess

    

The cache relevant write actions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The cache relevant write actions are those service calls which have
***at least*** to purge the listed cached objects:

Note: We assume for now that Content does not contain relations and
instead of ContentType the ContentTypeId (see deprecated list)

 

ContentService::publishVersion - invalidates ContentInfo [-> Location, 
Reverse-Relations ] , VersionInfos, Content

ContentService::updateContentMetaData - invalidates ContentInfo [ ->
Location, Content,  Reverse-Relations ]

SectionService::assignSectionToContent - invalidates ContentInfo [ ->
Location, Content,  Reverse-Relations ]

ContentService::deleteContent - invalidates all subtrees of content
including deleted content

 

LocationService::updateLocation - invalidates Location

LocationService::hideLocation - invalidates subtree

LocationService::unhideLocation - invalidates subtree

LocationService::moveSubtree - invalidates subtree

LocationService::deleteLocation - invalidates subtree

LocationService::swapLocation - invalidates both Locations

TrashService::trash - invalidates subtree

 

SectionService::updateSection - invalidates Sectionlist and Section

SectionService::deleteSection - invalidates Sectionlist and Section

UserService::updateUser - invalidates User, Content

UserService::deleteUser - invalidates User, Content

RoleService::addPolicy - invalidates canUser, hasAccess

RoleService::removePolicy - invalidates canUser, hasAccess

RoleService::assignRoleToUser - invalidates canUser, hasAccess

RoleService::assignRoleToUserGroup - invalidates canUser, hasAccess

RoleService::unassignRoleFromUser - invalidates canUser, hasAccess

RoleService::unassignRoleFromUserGroup - invalidates canUser, hasAccess

 

Persistence SPI Cache
~~~~~~~~~~~~~~~~~~~~~

The persistence cache is responsible for reducing database calls. There
is almost none object embedding so the dependencies for invalidation the
cache is much simpler:

Cached objects and methods using cache
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Content (which includes a published version - i.e. archived Content and
Drafts are not cached)

    Content\\Handler::load

ContentInfo

    Content\\Handler::loadContentInfo

VersionInfo

    Content\\Handler::loadVersionInfo

    Content\\Handler::loadVersions

Location

    Location\\Handler::load

    Location\\Handler::loadByRemoteId    

Relations of published content

    Content\\Handler::loadRelations

Section

    Section\\Handler::load

    Section\\Handler::loadByIdentifier

SectionList

    Section\\Handler::loadAll

ContentType (cached if published)

    ContentType\\Handler::load

    ContentType\\Handler::loadByIdentifier

    ContentType\\Handler::loadByRemoteId

ContentTypeGroup

    ContentType\\Handler::loadAllGroups

    ContentType\\Handler::loadGroup

    ContentType\\Handler::loadGroupByIdentifier

User

    User\\Handler::load

    User\\Handler::loadByLogin

Role

    User\\Handler::loadRole

    User\\Handler::loadRoleByIdentifer

    User\\Handler::loadRoles

    User\\Handler::loadRoleAssignmentsByGroupId

    User\\Handler::loadRoleAssignmentsByRoleId

PolicyList

    User\\Handler::loadPoliciesByUserId

UrlAlias

    UrlAlias\\Handler::loadUrlAlias

    UrlAlias\\Handler::lookup

UrlWildCard

    UrlWildCard\\Handler::load

ObjectState

    ObjectState\\Handler::getContentState

    ObjectState\\Handler::load

    ObjectState\\Handler::loadObjectStates

ObjectStateGroup

    ObjectState\\Handler::loadAllGroups

    ObjectState\\Handler::loadGroup

    ObjectState\\Handler::loadGroupByIdentifier

 

 

Cache relevant write actions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 

Content\\Handler::publish - invalidates ContentInfo, VersionInfos,
Content

Content\\Handler::updateMetaData - invalidates ContentInfo

Section\\Handler::assignSectionToContent - invalidates ContentInfo

Content\\Handler::deleteContent - invalidates all subtrees of content
including deleted content

Content\\Handler::setStatus - invalidates VersionInfo

Content\\Handler::deleteVersion - invalidates VersionInfo

 

LocationHandler::changeMainLocation - invalidates ContentInfo & all it's
Location's

Location\\Handler::updateLocation - invalidates Location

Location\\Handler::hideLocation - invalidates subtree

Location\\Handler::unhideLocation - invalidates subtree

Location\\Handler::moveSubtree - invalidates subtree

Location\\Handler::deleteLocation - invalidates subtree/removed content

Location\\Handler::swapLocation - invalidates both Locations

Trash\\Handler::trashSubtree - invalidates subtree

 

Section\\Handler::update - invalidates SectionList and Section

Section\\Handler::delete - invalidates SectionList and Section

Section\\Handler::create - invalidates SectionList

 

User\\Handler::update - invalidates User

User\\Handler::delete - invalidates User

User\\Handler::deleteRole - invalidates Role, PolicyList

User\\Handler::updateRole - invalidates Role

User\\Handler::addPolicy - invalidates PolicyList, Role

User\\Handler::removePolicy - invalidates PolicyList, Role

User\\Handler::assignRole - invalidates PolicyList, RoleAssignments

User\\Handler::unAssignRole - invalidates PolicyList, RoleAssignments

User\\Handler::updatePolicy - invalidates PolicyList, RoleAssignments

Cache invalidation of RoleAssigments does here not take changes to
locations into account(groups), this part of the Persistence SPI is in
need for a make over: 1. Needs to expose such functionality via
UserHandler & 2. in a cache friendly manner to avoid need to clear all
RoleAssignments on all location changes.

 

UrlAlias\\Handler::removeUrlAliases - invalidates URLAlias

UrlAlias\\Handler::locationMoved - invalidates URLAlias (subtree)

UrlAlias\\Handler::locationDeleted - invalidates URLAlias (subtree)

 

UrlWildcard\\Handler::remove - invalidates UrlWildcard

ObjectState\\Handler::delete - invalidates ObjectState, ContentState

ObjectState\\Handler::deleteGroup - invalidates ObjectStateGroup,
ObjectState, ContentState

ObjectState\\Handler::setContentState  - invalidates ContentState

ObjectState\\Handler::update - invalidates ObjectState

ObjectState\\Handler::updateGroup - invalidates ObjectStateGroup

 

Cache SPI/API
-------------

| It has been decided to not come up with our own cache spi, but instead
reuse existing solutions, for the time being
`Stash <http://stash.tedivm.com/>`__, and in the future PSR based cache
library.
| For use in API a simple `cache
pool <https://github.com/ezsystems/ezp-next/pull/194/files#diff-0>`__
for object instances is currently proposed, as it will in first round
only cache a limited amount of objects during request pr user as it
would otherwise need to know about permissions unlike spi level where
this is not a concern.

Future work can be one of (if needed):

-  Exposing cache api for use in application layer

   -  Should optimally wait until there is a accepted PSR cache standard
      so we can use this as backend for the API & use in SPI Persistence
      cache

-  Make a more complex persisted API cache that somehow deals with
   permissions in a efficient way 

 

 

 

 

 

Attachments:
------------

| |image1| `Folie1.jpg <attachments/6291886/6520850.jpg>`__ (image/jpeg)

Document generated by Confluence on Feb 12, 2014 16:43

.. |image0| image:: attachments/6291886/6520850.jpg
.. |image1| image:: images/icons/bullet_blue.gif
