#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Development & Administration Guides <6291674.html>`__
#. `Reference <Reference_10158191.html>`__

eZ Publish Platform 5.<next> : Signals reference
================================================

Added by jerome.vieilledent@ez.no , edited by ricardo.correia@ez.no on
Jul 23, 2013

This page references **all available signals**, triggered by Public API,
you can listen to.

Icon

For more information, check `SignalSlot
documentation <Signal-Slot_3506264.html>`__ and the `cookbook recipe on
how to listen to
signals <How-to-listen-to-Core-events_14123362.html>`__.

**Signals are ordered by Repository service:**

-  `ContentService <#Signalsreference-ContentService>`__
-  `ContentTypeService <#Signalsreference-ContentTypeService>`__
-  `LanguageService <#Signalsreference-LanguageService>`__
-  `LocationService <#Signalsreference-LocationService>`__
-  `ObjectStateService <#Signalsreference-ObjectStateService>`__
-  `RoleService <#Signalsreference-RoleService>`__
-  `SectionService <#Signalsreference-SectionService>`__
-  `TrashService <#Signalsreference-TrashService>`__
-  `URLAliasService <#Signalsreference-URLAliasService>`__
-  `URLWildcardService <#Signalsreference-URLWildcardService>`__
-  `UserService <#Signalsreference-UserService>`__

Icon

All signals are relative to ``eZ\Publish\Core\SignalSlot\Signal``
namespace.

 

ContentService
--------------

Signal type

Properties

Triggered by

When

``ContentService\AddRelationSignal``

-  ``srcContentId`` (source contentId, aka referrer)
-  ``srcVersionNo``
-  ``dstContentId`` (destination contentId, aka target)

``ContentService::addRelation()``

After

``ContentService\AddTranslationInfoSignal``

N/A

``ContentService::addTranslationInfo()``

After

``ContentService\CopyContentSignal``

-  ``srcContentId`` (original content ID)
-  ``srcVersionNo``
-  ``dstContentId`` (contentId of the copy)
-  ``dstVersionNo``
-  ``dstParentLocationId`` (locationId where the content has been
   copied)

``ContentService::copyContent()``

After

``ContentService\CreateContentDraftSignal``

-  ``contentId``
-  ``versionNo``
-  ``userId`` (Id of user used to create the draft, or null - current
   user)

``ContentService::createContentDraft()``

After

``ContentService\CreateContentSignal``

-  ``contentId``
-  ``versionNo``

``ContentService::createContent()``

After

``ContentService\DeleteContentSignal``

-  ``contentId``

``ContentService::deleteContent()``

After

``ContentService\DeleteRelationSignal``

-  ``srcContentId``
-  ``srcVersionNo``
-  ``dstContentId``

``ContentService::deleteRelation()``

After

``ContentService\DeleteVersionSignal``

-  ``contentId``
-  ``versionNo``

``ContentService::deleteVersion()``

After

``ContentService\PublishVersionSignal``

-  ``contentId``
-  ``versionNo``

``ContentService:: publishVersion()``

After

``ContentService\TranslateVersionSignal``

-  ``contentId``
-  ``versionNo``
-  ``userId``

``ContentService::translationVersion()``

After

``ContentService\UpdateContentMetadataSignal``

-  ``contentId``

``ContentService::updateContentMetadata()``

After

``ContentService\UpdateContentSignal``

-  ``contentId``
-  ``versionNo``

``ContentService::updateContent()``

After

ContentTypeService
------------------

Signal type

Properties

Triggered by

When

``ContentTypeService\AddFieldDefinitionSignal``

-  contentTypeDraftId

``ContentTypeService::addFieldDefinition()``

After

``ContentTypeService\AssignContentTypeGroupSignal``

-  ``contentTypeId``
-  ``contentTypeGroupId``

``ContentTypeService::assignContentTypeGroup()``

After

``ContentTypeService\CopyContentTypeSignal``

-  ``contentTypeId``
-  ``userId``

``ContentTypeService::copyContentType()``

After

``ContentTypeService\CreateContentTypeDraftSignal``

-  ``contentTypeId``

``ContentTypeService::createContentTypeDraft()``

After

``ContentTypeService\CreateContentTypeGroupSignal``

-  ``groupId``

``ContentTypeService::createContentTypeGroup()``

After

``ContentTypeService\CreateContentTypeSignal``

-  ``contentTypeId``

``ContentTypeService::createContentType()``

After

``ContentTypeService\DeleteContentTypeGroupSignal``

-  ``contentTypeGroupId``

``ContentTypeService::deleteContentTypeGroup()``

After

``ContentTypeService\DeleteContentTypeSignal``

-  ``contentTypeId``

``ContentTypeService::deleteContentType()``

After

``ContentTypeService\PublishContentTypeDraftSignal``

-  ``contentTypeDraftId``

``ContentTypeService::publishContentTypeDraft()``

After

``ContentTypeService\RemoveFieldDefinitionSignal``

-  ``contentTypeDraftId``
-  ``fieldDefinitionId``

``ContentTypeService::removeFieldDefinition()``

After

``ContentTypeService\UnassignContentTypeGroupSignal``

-  ``contentTypeId``
-  ``contentTypeGroupId``

``ContentTypeService::unassignContentTypeGroup()``

After

``ContentTypeService\UpdateContentTypeDraftSignal``

-  ``contentTypeDraftId``

``ContentTypeService::updateContentTypeDraft()``

After

``ContentTypeService\UpdateContentTypeGroupSignal``

-  ``contentTypeGroupId``

``ContentTypeService::updateContentTypeGroup()``

After

``ContentTypeService\UpdateFieldDefinitionSignal``

-  ``contentTypeDraftId``
-  ``fieldDefinitionId``

``ContentTypeService::updateFieldDefinition()``

After

LanguageService
---------------

Signal type

Properties

Triggered by

When

``LanguageService\CreateLanguageSignal``

-  ``languageId``

``LanguageService::createLanguage()``

After

``LanguageService\DeleteLanguageSignal``

-  ``languageId``

``LanguageService::deleteLanguage()``

After

``LanguageService\DisableLanguageSignal``

-  ``languageId``

``LanguageService::disableLanguage()``

After

``LanguageService\EnableLanguageSignal``

-  ``languageId``

``LanguageService::enableLanguage()``

After

``LanguageService\UpdateLanguageNameSignal``

-  ``languageId``
-  ``newName``

``LanguageService::updateLanguageName()``

After

LocationService
---------------

Signal type

Properties

Triggered by

When

``LocationService\CopySubtreeSignal``

-  ``subtreeId`` (top locationId of the subtree to be copied)
-  ``targetParentLocationId``

``LocationService::copySubtree()``

After

``LocationService\CreateLocationSignal``

-  ``contentId``
-  ``locationId``

``LocationService::createLocation()``

After

``LocationService\DeleteLocationSignal``

-  ``contentId``
-  ``locationId``

``LocationService::deleteLocation()``

After

``LocationService\HideLocationSignal``

-  ``contentId``
-  ``locationId``

``LocationService::hideLocation()``

After

``LocationService\UnhideLocationSignal``

-  ``contentId``
-  ``locationId``

``LocationService::unhideLocation()``

After

``LocationService\MoveSubtreeSignal``

-  ``subtreeId``
-  ``newParentLocationId``

``LocationService::moveSubtree()``

After

``LocationService\SwapLocationSignal``

-  ``content1Id``
-  ``location1Id``
-  ``content2Id``
-  ``location2Id``\ ````

``LocationService::swapLocation()``

After

``LocationService\UpdateLocationSignal``

-  ``contentId``
-  ``locationId``

``LocationService::updateLocation()``

After

ObjectStateService
------------------

Signal type

Properties

Triggered by

When

``ObjectStateService\CreateObjectStateGroupSignal``

-  ``objectStateGroupId``

``ObjectStateService::createObjectStateGroup()``

After

``ObjectStateService\CreateObjectStateSignal``

-  ``objectStateGroupId``
-  ``objectStateId``

``ObjectStateService::createObjectState()``

After

``ObjectStateService\DeleteObjectStateGroupSignal``

-  ``objectStateGroupId``

``ObjectStateService::deleteObjectStateGroup()``

After

``ObjectStateService\DeleteObjectStateSignal``

-  ``objectStateId``

``ObjectStateService::deleteObjectState()``

After

``ObjectStateService\SetContentStateSignal``

-  ``contentId``
-  ``objectStateGroupId``
-  ``objectStateId``

``ObjectStateService::setContentState()``

After

``ObjectStateService\SetPriorityOfObjectStateSignal``

-  ``objectStateId``
-  ``priority``

``ObjectStateService::setPriorityOfObjectState()``

After

``ObjectStateService\UpdateObjectStateGroupSignal``

-  ``objectStateGroupId``

``ObjectStateService::updateObjectStateGroup()``

After

``ObjectStateService\UpdateObjectStateSignal``

-  ``objectStateId``

``ObjectStateService::updateObjectState()``

After

RoleService
-----------

Signal type

Properties

Triggered by

When

``RoleService\AddPolicySignal``

-  ``roleId``
-  ``policyId``

``RoleService::addPolicy()``

After

``RoleService\AssignRoleToUserGroupSignal``

-  ``roleId``
-  ``userGroupId``
-  ``roleLimitation``

``RoleService::assignRoleToUserGroup()``

After

``RoleService\AssignRoleToUserSignal``

-  ``roleId``
-  ``userId``
-  ``roleLimitation``

``RoleService::assignRoleToUser()``

After

``RoleService\CreateRoleSignal``

-  ``roleId``

``RoleService::createRole()``

After

``RoleService\DeleteRoleSignal``

-  ``roleId``

``RoleService::deleteRole()``

After

``RoleService\RemovePolicySignal``

-  ``roleId``
-  ``policyId``

``RoleService::removePolicy()``

After

``RoleService\UnassignRoleFromUserGroupSignal``

-  ``roleId``
-  ``userGroupId``

``RoleService::unassignRoleFromUserGroup()``

After

``RoleService\UnassignRoleFromUserSignal``

-  ``roleId``
-  ``userId``

``RoleService::unassignRoleFromUser()``

After

``RoleService\UpdatePolicySignal``

-  ``policyId``

``RoleService::updatePolicy()``

After

``RoleService\UpdateRoleSignal``

-  ``roleId``

``RoleService::updateRole()``

After

SectionService
--------------

Signal type

Properties

Triggered by

When

``SectionService\AssignSectionSignal``

-  ``contentId``
-  ``sectionId``

``SectionService::assignSection()``

After

``SectionService\CreateSectionSignal``

-  ``sectionId``

``SectionService::createSection()``

After

``SectionService\DeleteSectionSignal``

-  ``sectionId``

``SectionService::deleteSection()``

After

``SectionService\UpdateSectionSignal``

-  ``sectionId``

``SectionService::updateSection()``

After

TrashService
------------

Signal type

Properties

Triggered by

When

``TrashService\DeleteTrashItemSignal``

-  ``trashItemId``

``TrashService::deleteTrashItem()``

After

``TrashService\EmptyTrashSignal``

N/A

``TrashService::emptyTrash()``

After

``TrashService\RecoverSignal``

-  ``trashItemId``
-  ``newParentLocationId``
-  ``newLocationId``

``TrashService::recover()``

After

``TrashService\TrashSignal``

-  ``locationId``

``TrashService::trash()``

After

URLAliasService
---------------

Signal type

Properties

Triggered by

When

``URLAliasService\CreateGlobalUrlAliasSignal``

-  ``urlAliasId``

``URLAliasService::createGlobalUrlAlias()``

After

``URLAliasService\CreateUrlAliasSignal``

-  ``urlAliasId``

``URLAliasService::createUrlAlias()``

After

``URLAliasService\RemoveAliasesSignal``

-  ``aliasList``

``URLAliasService::removeAliases()``

After

URLWildcardService
------------------

Signal type

Properties

Triggered by

When

``URLWildcardService\CreateSignal``

-  ``urlWildcardId``

``URLWildcardService::create()``

After

``URLWildcardService\RemoveSignal``

-  ``urlWildcardId``

``URLWildcardService::remove()``

After

``URLWildcardService\TranslateSignal``

-  ``url``

``URLWildcardService::translate()``

After

UserService
-----------

Signal type

Properties

Triggered by

When

``UserService\AssignUserToUserGroupSignal``

-  ``userId``
-  ``userGroupId``

``UserService::assignUserToUserGroup()``

After

``UserService\CreateUserGroupSignal``

-  ``userGroupId``

``UserService::createUserGroup()``

After

``UserService\CreateUserSignal``

-  ``userId``

``UserService::createUser()``

After

``UserService\DeleteUserGroupSignal``

-  ``userGroupId``

``UserService::deleteUserGroup()``

After

``UserService\DeleteUserSignal``

-  ``userId``

``UserService::deleteUser()``

After

``UserService\MoveUserGroupSignal``

-  ``userGroupId``
-  ``newParentId``

``UserService::moveUserGroup()``

After

``UserService\UnAssignUserFromUserGroupSignal``

-  ``userId``
-  ``userGroupId``

``UserService::unAssignUserFromUserGroup()``

After

``UserService\UpdateUserGroupSignal``

-  ``userGroupId``

``UserService::updateUserGroup()``

After

``UserService\UpdateUserSignal``

-  ``userId``

``UserService::updateUser()``

After

 

 

Document generated by Confluence on Feb 12, 2014 16:43
