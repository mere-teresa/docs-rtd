#. `eZ Publish Platform 5.<next> <index.html>`__

|Home Page| eZ Publish Platform 5.<next> : eZ Publish Platform Documentation
============================================================================

Added by andre.romcke@ez.no , edited by gaetano.giunta@ez.no on Jan 14,
2014

What is eZ Publish?
===================

eZ Publish is an Enterprise Open-Source Content Management
System/Framework
(`CMS <http://en.wikipedia.org/wiki/Content_management_system>`__/`F <http://en.wikipedia.org/wiki/Content_management_framework>`__)
at its core. It is highly extendable so with extensions it provides
"`WCMS <http://en.wikipedia.org/wiki/Web_content_management_system>`__\ "
capabilities. Its default Enterprise offering as of version 5, "eZ
Publish Platform", is extended to provide full set of Customer
Experience Management
(`CXM <http://en.wikipedia.org/wiki/Customer_experience>`__)
capabilities.

What is eZ Publish 5.x?
-----------------------

Version 5 represents a big step forward for eZ Publish, as it introduces
a whole new architecture and embraces the full Symfony2 stack and
community around it. As is often the case with architecture, most of it
is under the hood in 5.x releases, and there is no new User Interfaces
(UI) yet running on the new kernel; however with our "Dual-kernel"
approach all existing UI's are still here running on legacy kernel, and
same goes for all extensions and solutions made for eZ Publish 4.x.

When 5.x is considered "feature complete" enough to be able to stand on
its own both in terms of kernel and user interfaces, then it will become
"6.x" (working title) and will ship without legacy stack (kernel &
extensions).

From high level 5.x is:

-  “Ready for the future, data compatible with the past”: Content
   Repository accessible via \ `Public
   API <eZ-Publish-Public-API_1736723.html>`__ & `REST API
   v2 <eZ-Publish-REST-API_6292277.html>`__  (full read & write support)
-  “Full Symfony2
   access”: `Symfony2 <http://symfony.com/about>`__ Full-stack as
   the framework, extended with eZ Publish features 
-  DemoBundle front-end on top of eZ's Symfony2 stack (read only)
-  “Dual kernel: Full code and data backwards compatibility”: 5.x stack
   is not fully functional on its own, it's combined with 4.x ("legacy
   stack") in a "Dual-kernel" setup
   (see `developer <6291674.html>`__ section for more) to be able to
   provide all features you would expect, this is fully exposed to
   website developers who want/need to take advantage of legacy stack,
   5.x stack or both. 

For more information on the status eZ Publish 5.x, known issues, change
logs and architecture updates, see the `latest release
notes <eZ-Publish-Platform-Releases_12781017.html>`__.

Quick links
-----------

-  `Installing eZ Publish 5 <Installation_7438500.html>`__ (includes
   detailed specification of requirements)
-  `Upgrading to eZ Publish
   5 <http://doc.ez.no/eZ-Publish/Upgrading/>`__
-  `Architecture overview of eZ Publish 5 <11403666.html>`__
-  Public API `basics <Public-API-basics_6293122.html>`__ &
   `Cookbook <Public-API-Cookbook_5046311.html>`__
-  `eZ Publish REST API <eZ-Publish-REST-API_6292277.html>`__

 

 

Recently Updated
----------------

-  `Content view <Content-view_8323263.html>`__
   about 3 hours ago • updated by `Jérôme
   Vieilledent </display/~jerome.vieilledent@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=8323263&selectedPageVersions=38&selectedPageVersions=37>`__
-  `Parameters injection in content
   views <Parameters-injection-in-content-views_8323330.html>`__
   Feb 10, 2014 • updated by `Jérôme
   Vieilledent </display/~jerome.vieilledent@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=8323330&selectedPageVersions=2&selectedPageVersions=1>`__
-  `Persistence cache
   configuration <Persistence-cache-configuration_12781293.html>`__
   Feb 06, 2014 • updated by `Sarah
   Haïm-Lubczanski </display/~sarah.haim-lubczanski@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=12781293&selectedPageVersions=18&selectedPageVersions=17>`__
-  `Authentication <Authentication_19891028.html>`__
   Feb 03, 2014 • updated by `Sarah
   Haïm-Lubczanski </display/~sarah.haim-lubczanski@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=19891028&selectedPageVersions=7&selectedPageVersions=6>`__
-  `MVC and Application <MVC-and-Application_2719826.html>`__
   Feb 03, 2014 • updated by `Sarah
   Haïm-Lubczanski </display/~sarah.haim-lubczanski@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=2719826&selectedPageVersions=16&selectedPageVersions=15>`__
-  `Setting the Index Page <Setting-the-Index-Page_19890549.html>`__
   Feb 03, 2014 • updated by `Sarah
   Haïm-Lubczanski </display/~sarah.haim-lubczanski@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=19890549&selectedPageVersions=13&selectedPageVersions=12>`__
-  `Upgrading from 5.2 to
   5.3 <Upgrading-from-5.2-to-5.3_19891003.html>`__
   Feb 03, 2014 • created by `Jérôme
   Vieilledent </display/~jerome.vieilledent@ez.no>`__
-  `2. Browsing, finding, viewing <6292980.html>`__
   Jan 31, 2014 • updated by `Ricardo
   Correia </display/~ricardo.correia@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=6292980&selectedPageVersions=71&selectedPageVersions=70>`__
-  `2. Browsing, finding, viewing <19890973.html>`__
   Jan 31, 2014 • updated by `André
   Rømcke </display/~andre.romcke@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=6292980&selectedPageVersions=70&selectedPageVersions=69>`__
-  `Requirements <Requirements_7438502.html>`__
   Jan 30, 2014 • updated by `André
   Rømcke </display/~andre.romcke@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=7438502&selectedPageVersions=37&selectedPageVersions=36>`__
-  `Settings and
   Configurations <Settings-and-Configurations_14123147.html>`__
   Jan 30, 2014 • updated by `André
   Rømcke </display/~andre.romcke@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=14123147&selectedPageVersions=7&selectedPageVersions=6>`__
-  `Using eZ Publish as a Service
   Platform <Using-eZ-Publish-as-a-Service-Platform_2720526.html>`__
   Jan 30, 2014 • updated by `André
   Rømcke </display/~andre.romcke@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=2720526&selectedPageVersions=9&selectedPageVersions=8>`__
-  `Using Varnish <Using-Varnish_12124722.html>`__
   Jan 30, 2014 • updated by `Jérôme
   Vieilledent </display/~jerome.vieilledent@ez.no>`__ • `view
   change </pages/diffpagesbyversion.action?pageId=12124722&selectedPageVersions=11&selectedPageVersions=10>`__
-  `Using
   Varnish </display/EZP/Using+Varnish?focusedCommentId=19890870#comment-19890870>`__
   Jan 30, 2014 • commented by `Jérôme
   Vieilledent </display/~jerome.vieilledent@ez.no>`__
-  `Using
   Varnish </display/EZP/Using+Varnish?focusedCommentId=19890851#comment-19890851>`__
   Jan 29, 2014 • commented by `Carlos Revillo </display/~desorden>`__

`Show
More </plugins/recently-updated/changes.action?theme=concise&pageSize=15&startHandle=com.atlassian.confluence.pages.Comment-19890845&spaceKeys=EZP&contentType=-mail,page,comment,blogpost,attachment,userinfo,spacedesc,personalspacedesc,status,space,draft,custom>`__
|Please wait|

 

Navigate space
              

Document generated by Confluence on Feb 12, 2014 16:43

.. |Home Page| image:: images/icons/contenttypes/home_page_16.png
.. |Please wait| image:: images/icons/wait.gif
