#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Installation and Upgrade
   Guides <Installation-and-Upgrade-Guides_6292016.html>`__
#. `Installation <Installation_7438500.html>`__

eZ Publish Platform 5.<next> : The setup wizard
===============================================

Added by andrea.melo@ez.no , edited by ricardo.correia@ez.no on Sep 26,
2013

 

This section contains a comprehensive guide through the web-based setup
wizard of eZ Publish. The setup wizard is designed to ease the initial
configuration of the system. It can be started using a web browser when
the necessary installation steps (described in the previous sections)
are completed. The setup wizard will automatically start the first time
the "index.php" file (located in the root of the eZ Publish directory)
is accessed/browsed.

The setup wizard does not store or modify any data before the final
step; thus, it can be safely restarted by reloading the URL containing
only the "index.php" part. The back button (located at the bottom) can
be used to jump back to previous steps in order to modify settings. A
typical setup cycle consists of 13 steps:

-  1 `Welcome page <#Thesetupwizard-Welcomepage>`__
-  2 `System finetuning <#Thesetupwizard-Systemfinetuning>`__
-  3 `System check <#Thesetupwizard-Systemcheck>`__
-  4 `Outgoing Email <#Thesetupwizard-OutgoingEmail>`__
-  5 `Database type <#Thesetupwizard-Databasetype>`__
-  6 `Database
   initialization <#Thesetupwizard-Databaseinitialization>`__
-  7 `Language support <#Thesetupwizard-Languagesupport>`__
-  8 `Site selection <#Thesetupwizard-Siteselection>`__
-  9 `Access method <#Thesetupwizard-Accessmethod>`__
-  10 `Site details <#Thesetupwizard-Sitedetails>`__
-  11 `Site security <#Thesetupwizard-Sitesecurity>`__
-  12 `Site registration <#Thesetupwizard-Siteregistration>`__
-  13 `Finished <#Thesetupwizard-Finished>`__

***Note:** Some of the steps will be omitted when an eZ Publish bundle
is being installed.*

Welcome page
------------

| |image0|
| :sup:`*Setup Wizard Welcome Page*`

This is the initial page of the setup wizard. This step allows the user
to select which language will be used during the installation process.
In addition, the wizard also checks the system configuration and
displays a note if it is not optimal (in this case, an additional button
called "Finetune" will be available at the bottom of the page).

The system automatically pre-selects one of the languages according to
your browser's language settings. You can choose another language by
selecting the desired language using the drop-down list. (The list of
available languages is built using the INI files located in the
"share/locale" directory).

After you click "Finetune" (if available), the wizard will load the
"System finetuning" page, which contains information about configuration
issues. The following screenshot shows an example of this page.

System finetuning
-----------------

| |image1|
| :sup:`*Setup Wizard Finetuning*`

After you click "Next", the wizard will either load the "System check"
page (if some critical issues need to be fixed) or the "Outgoing Email"
page (if everything is okay).

System check
------------

| |image2|
| :sup:`*Setup Wizard System Check*`

This page usually appears if critical issues/problems are detected. The
setup wizard will display information about the issues that need to be
fixed and suggestions describing how they can be fixed.

Issues
~~~~~~

There may be several issues/problems. A suggestion to each problem is
presented below the description of the problem itself. The setup wizard
will probably suggest the execution of miscellaneous shell commands (in
order to fix ownerships, permissions, etc.). These commands must be
executed using a system shell. Simply copy the commands from the browser
window and paste them into an open shell. The setup wizard will run the
system check again when the "Next" button is clicked. The "System check"
page will keep reappearing until all issues have been fixed (or ignored,
see the next section). Once everything is okay, the setup wizard will
display the next step.

Ignoring tests
~~~~~~~~~~~~~~

***Important:** Some issues/problems may be ignored using a check-box
labeled "Ignore this test". **However, it is recommended to fix all
issues rather than ignoring them.***

Outgoing Email
--------------

| |image3|
| :sup:`*Setup Wizard Outgoing Email*`

eZ Publish uses Email to send out miscellaneous notices. This step is
used to configure how eZ Publish delivers outgoing Email. There are two
options:

-  Direct delivery through sendmail (must be available on the server)
-  Indirect delivery using an SMTP (Simple Mail Transfer Protocol) relay
   server

On Linux/UNIX: try to use sendmail; use SMTP if sendmail is unavailable.
On Windows: use the SMTP setting.

Sendmail
~~~~~~~~

Mail is delivered directly using the sendmail transfer agent. The agent
must be running on the same host as the webserver is running on. The
sendmail binary is usually available on most Linux/UNIX systems. If
sendmail is not available then SMTP should be used.

SMTP
~~~~

Mail is delivered through an SMTP server. At the minimum, the hostname
of the SMTP server must be specified.

Database type
-------------

| |image4|
| :sup:`*Setup Wizard Database Type*`

The setup will automatically detect database support that has been made
available for the PHP scripting engine. If both MySQL and PostgreSQL are
supported, the database choice dialog will appear. If PHP is setup only
to support one type of database, eZ Publish will automatically use it
and thus the database choice dialog will not be displayed.

Note that if the `MySQLi extension <http://php.net/mysqli>`__ is enabled
in PHP, the "MySQL Improved" option will be available on the list. If
you are going to use a MySQL database, it is recommended to select
"MySQL Improved" instead of "MySQL".

Database initialization
-----------------------

| |image5|
| :sup:`*Setup Wizard Database Initialization*`

Information about the host name of the server running the database
engine, and a user name/password combination needs to be provided. After
you click "Next", if MySQL or MySQL Improved are used, the setup wizard
will attempt to connect to the database. The setup will only continue if
it is able to connect to the specified MySQL server with the specified
user name/password combination. PostgreSQL parameters are tested at a
later stage during the setup wizard. (Note that even if the eZ Publish
Extension for Oracle® Database is installed, the setup wizard will not
let you use an Oracle database. The configuration must be done manually
as described in the `documentation of the database
extension <http://doc.ez.no/Extensions/eZ-Publish-extensions/eZ-Publish-Extension-for-Oracle-R-database>`__.)

***Known issue with running PHP5.3 on MySQL:** Some people (like Windows
users with both IPv4 and IPv6 installed ) experience problems connecting
to the database server using host names like "localhost"... If you
experience problems, try using IPv4 address like "127.0.0.1". This is
due to a connectivity problem when running PHP5.3 on MySQL. So, please
replace the database server name "localhost" with the IP address of the
machine, or "127.0.0.1", which is reserved for the local host.*

Language support
----------------

| |image6|
| :sup:`*Setup Wizard Language Support*`

This step allows the user to choose a language configuration for the
site that is being installed. The setup wizard automatically pre-selects
one of the languages according to your browser language settings. Use
the radio buttons to choose the default language (required), and the
check-boxes to choose the additional languages (optional). All the
selected languages will be added to the system and put on the list of
`prioritized
languages <https://confluence.ez.no/doc.ez.no/eZ-Publish/Technical-manual/5.x/Features/Multi-language>`__.
You will be able to use any of these languages for creating and
translating your content after the setup wizard is finished.

Note that choosing the default language at this step will determine
`default
language <http://doc.ez.no/eZ-Publish/Technical-manual/5.x/Features/Multi-language>`__,
`system
locale <http://doc.ez.no/eZ-Publish/Technical-manual/5.x/Features/Multi-language/Configuring-your-site-locale>`__
and the most prioritized language for your site. If you select for
example "German" as default language, then both locale and default
language will be set to "ger-DE", your administration interface will be
translated into German, and this language will be recorded as the most
prioritized one for your site. Languages can be reconfigured at any time
(even when a site is up and running) using the administration interface.

***Important:** Note that regardless of the selected language
configuration, the site will be created using UTF-8 as the character
set.*

Icon

As of eZ Publish 5.2 the maximum number of languages supported for 64
bit servers \ `has been
improved <https://confluence.ez.no/display/EZP/5.2-beta1+Release+Notes#id-5.2-beta1ReleaseNotes-Supportformorethan30languages>`__,
allowing up to 62 languages simultaneously. For 32 bit servers the
maximum number of simultaneous languages supported is 30.

*
*

Site selection
--------------

| |image7|
| :sup:`*Setup Wizard Site Selection*`

This step allows the user to select one of the standard `site
packages <http://doc.ez.no/eZ-Publish/Technical-manual/5.x/Features/Packages/Package-types>`__.
These packages are intended to provide basic examples mostly for the
purpose of demonstration and learning. However, it is possible to use
them as a basic framework which you can extend/tweak in order to make it
suitable for a specific purpose. A demo site usually contains some
artwork (images), CSS code, actual content and template files. The plain
type should be used when starting from scratch.

The setup wizard automatically fetches the list of available site
packages from
`remote <http://doc.ez.no/eZ-Publish/Technical-manual/5.x/Features/Packages#remote_repository>`__
and
`internal <http://doc.ez.no/eZ-Publish/Technical-manual/5.x/Features/Packages#internal_repository>`__
repositories and asks the user to choose one. The default remote
repository is
`http://packages.ez.no/ezpublish/ <http://packages.ez.no/ezpublish/>`__
(you can browse packages in this repository, to find the ones applicable
to the new release).

Icon

Note that, as of eZ Publish 5.2, the setup wizard only retrieves the
`Demo
Site <http://doc.ez.no/eZ-Publish/Technical-manual/5.x/Features/Front-End-Demo-Design>`__
design.

However, the following other design are still available for download in
the repository at
`http://packages.ez.no/ezpublish/ <http://packages.ez.no/ezpublish/>`__
for manual installation:

-  Plain site
-  `Website
   Interface <http://doc.ez.no/Extensions/eZ-Publish-extensions/Website-Interface>`__
-  eZ Flow

*Older site packages such as "News site", "Shop site" and "Gallery site"
are currently not available for eZ Publish 4.*

The wizard will automatically download the selected site package and all
its dependent packages, import them to the system and display a list of
successfully imported packages as shown in the following screenshot.
(This step will be omitted if all these packages are already stored
under internal repositories.)

***Note:** You may download and extract these packages and required
files for offline installation. Read the requirements for such an
offline installation by clicking on this
`link <Requirements-for-doing-an-automated-installation_7438626.html>`__.*

All dependent packages except for the site style package will be
automatically installed.

Package language options
~~~~~~~~~~~~~~~~~~~~~~~~

| |image8|
| :sup:`*Setup Wizard Package Language Options*`

If the language configuration selected at the "Language support" step
doesn't match the languages used in the packages being installed, the
"Package language options" interface will appear as shown on the
screenshot above. For example, the "Website interface" site package
makes it possible to have demo content created in 2 languages: English
(United Kingdom) and French. If the same languages are selected at the
"Language support" step, the packages will be installed silently.
Otherwise, the user will need to specify how the system should act
towards the "superfluous" languages (i.e. languages that exist in the
package but aren't present in the selected language configuration for
the site). Possible actions are:

-  Skip content in this language
-  Create language (extend the language configuration of the site and
   create demo content in this language)
-  Map to another language (use demo data to create content in another
   language)

If the web server is not able to contact the remote repository (due to
firewall rules for example), the setup wizard will display an error
message at the "Site selection" step. To fix this, allow outbound
connections to `http://packages.ez.no <http://packages.ez.no>`__ in your
firewall (port 80) or download the packages manually.

 

Outbound connections via proxy
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you allow only outbound connections via a proxy server, then you need
to configure eZ Publish in the following way:

#. Create a file called "site.ini.append.php" in the "settings/override"
   directory and make sure it contains the following lines:

   ::

       [ProxySettings]
       ProxyServer=proxy.example.com:3128
       User=myuser
       Password=secret

    

   Replace "``proxy.example.com:3128``\ " with the actual address and
   port number that can be used to access the web through the proxy
   server. If the proxy server requires authentication, you will also
   need to provide a valid username/password combination.

#. Restart the setup wizard.

*Note that `CURL <http://www.php.net/curl>`__ support must be enabled in
PHP, otherwise outbound connections via proxy will not work.*

Manual download of packages
^^^^^^^^^^^^^^^^^^^^^^^^^^^

If the wizard fails to connect the external packages repository, you can
manually download the desired site package and all the dependent
packages it requires and then upload/import them via the setup wizard.
The following instructions reveal how this can be done.

#. Go to the `packages download
   page <http://packages.ez.no/ezpublish/>`__. The "Sites" section of
   this page contains the list of available site packages including the
   following information for each of them:

   -  Name
   -  Description
   -  Dependencies (if any)

   Click on the name of the desired site package to download it. (A
   package is downloaded as an "``.ezpkg``\ " file.)

#. Download all the dependent packages required by this site package
   (these are listed under "Dependencies"). You can download a package
   by clicking on its name. The packages are downloaded as
   "``.ezpkg``\ " files.
#. Use the package import interface located at the bottom of the page in
   the setup wizard to upload/import the downloaded site package (click
   the "Choose" button, select the downloaded "``ezpkg``\ " file that
   contains the site package and click the "Upload" button). The
   imported site package will appear on the list.
#. Upload/import all the dependent packages using the same import
   interface.

***Note:** it is also possible to download packages manually from the
remote repository. The following instructions reveal how this can be
done.*

#. Go to the `packages
   repository <http://packages.ez.no/ezpublish/4.6/4.6.0/>`__, find the
   desired site package and download it manually. (A package is
   downloaded as an "``.ezpkg``\ " file.)
#. Unpack the "``ezpkg``\ " file into a temporary folder and view the
   "``package.xml``\ " file in order to figure out which dependent
   packages are required (these are listed between the
   ``<dependencies>`` and ``</dependencies>`` XML tags as described
   `here <http://doc.ez.no/eZ-Publish/Technical-manual/5.x/Features/Packages/package.xml-format>`__).
   Download all the dependent packages that are required.

Additional functionality
~~~~~~~~~~~~~~~~~~~~~~~~

In eZ Publish 3.7 and earlier versions, the setup wizard included one
more step called "Site functionality" that allowed to select additional
features that should be installed. This step is no longer used.
Additional functionality can be added after the setup wizard is finished
by downloading the desired packages from the "Content objects" section
of the `packages download page <http://packages.ez.no/ezpublish/>`__,
`importing <http://doc.ez.no/eZ-Publish/Technical-manual/5.x/Features/Packages/Importing-packages-to-the-system>`__
the packages and
`installing <http://doc.ez.no/eZ-Publish/Technical-manual/5.x/Features/Packages/Installing-packages>`__
them.

Access method
-------------

| |image9|
| :sup:`*Setup Wizard Siteaccess Configuration*`

This step allows the configuration of the access method that should be
used when eZ Publish receives a request. There are three options:

-  URL
-  Port
-  Hostname

URL
~~~

When the URL access method is used, eZ Publish selects the site that
should be accessed based on the contents of the URL (in particular the
part that comes right after "``index.php``\ "). This is the default and
most generic option. It doesn't require any additional configuration.
Use this setting when installing eZ Publish for the first time.

Port
~~~~

When the port access method is used, eZ Publish selects the site that
should be accessed based on a port number that is specified in the URL.
The port number must be appended to the host name of the web server:
"``http://www.example.com:81/index.php``\ ". This option requires
additional web server and firewall configuration. Use this setting only
if you know what you're doing.

Host name
~~~~~~~~~

When this access method is used, each site is assigned a unique host
name. For example, "``www.example.com``\ " and "``admin.example.com``\ "
can be assigned to the public and the administration interface
respectively. This option requires additional web and DNS server
configuration. Use this setting only if you know what you're doing.

Site details
------------

|image10|\ *
:sup:`Setup Wizard Site Details`*

This step allows the modification of settings related to the site that
is being installed. Note that the "User path" and "Admin path" access
values depend on which access method you choose. When the port access
method is used these values are port numbers. If you use the URL access
method then "User path" and "Admin path" should only contain letters,
digits and underscores. If the host name access method is used then some
additional symbols like dashes, dots and colons are allowed whereas
underscores aren't.

The available databases will be displayed in the database drop-down
menu. The "Refresh" button can be used to update the list (if a database
is being created at this point). It is required that the database uses
UTF-8 as character set.

If the selected database already contains data, the "Site Details" page
will reappear and ask what to do. Possible actions are:

-  Leave the data and add new
-  Remove existing data
-  Leave the data and do nothing
-  I've chosen a new database

Use the last option if another database has been chosen.

Site security
-------------

|image11|\ *
:sup:`Setup Wizard Site administrator`*

This step suggests some basic modifications that should be carried out
in order to secure the site being installed. The suggested security
tweak protects the configuration files from unwanted access. Don't worry
about this unless you're setting up a site for public use.

Note that the administrator's user name (log-in) is set to "admin" by
default and can not be changed. If you need another user name for site
administrator, you can install eZ Publish, create a new administrator
user, log in as this user and remove the old one.

Site registration
-----------------

|image12|\ :sup:`*
Setup Wizard Site Registration*`

On this step the Setup Wizard will ask the user for the following
information:

-  First name
-  Last name
-  Email
-  Country
-  Company

***Note:**\ First name, last name, email and country are mandatory
fields, thus must be filled. *

If you are installing eZ Publish for the first time, you should check
the following option: **Is this the first time you are installing eZ
Publish?**

If you also checks this option: **Is it ok that some simple technical
information is included?**

the following information will also be sent to eZ Systems:

-  System details (OS type, etc)
-  The test results
-  The type of database that is being used
-  The name of the site
-  The URL of the site
-  The languages that were chosen

***Important Note:** All the information above will be sent to eZ
Systems in an information Email and will be used internally for
statistics and for improving eZ Publish. No confidential data will be
transmitted and eZ Systems will not misuse or sell these details.*

Finished
--------

| |image13|
| :sup:`*Setup Wizard Finished*`

The setup wizard has finished. eZ Publish is ready for use. Click on one
of the links to access the various interfaces (public site,
administration interface, etc.).

Note that it is possible to restart the installation wizard after its
successful finishing by specifying "``CheckValidity=true``\ " in the
"``<eZ_Publish_root_path>/ezpublish_legacysettings/override/site.ini.append.php``\ "
file so that the setup wizard will be initiated when trying to access
the site.

Attachments:
------------

| |image14| `Setup Wizard\_1.jpg <attachments/7438516/7798807.jpg>`__
(image/jpeg)
|  |image15| `Setup Wizard\_2.jpg <attachments/7438516/7798808.jpg>`__
(image/jpeg)
|  |image16| `Setup Wizard\_3.jpg <attachments/7438516/7798809.jpg>`__
(image/jpeg)
|  |image17| `Setup Wizard\_4.jpg <attachments/7438516/7798810.jpg>`__
(image/jpeg)
|  |image18| `Setup Wizard\_5.jpg <attachments/7438516/7798811.jpg>`__
(image/jpeg)
|  |image19| `Setup Wizard\_6.jpg <attachments/7438516/7798812.jpg>`__
(image/jpeg)
|  |image20| `Setup Wizard\_7.jpg <attachments/7438516/7798813.jpg>`__
(image/jpeg)
|  |image21| `Setup Wizard\_8.jpg <attachments/7438516/7798814.jpg>`__
(image/jpeg)
|  |image22| `Setup Wizard\_9.jpg <attachments/7438516/7798815.jpg>`__
(image/jpeg)
|  |image23| `Setup Wizard\_10.jpg <attachments/7438516/7798816.jpg>`__
(image/jpeg)
|  |image24| `Setup Wizard\_11.jpg <attachments/7438516/7798817.jpg>`__
(image/jpeg)
|  |image25| `Setup Wizard\_13.jpg <attachments/7438516/7798818.jpg>`__
(image/jpeg)
|  |image26| `Setup Wizard\_14.jpg <attachments/7438516/7798819.jpg>`__
(image/jpeg)

Document generated by Confluence on Feb 12, 2014 16:43

.. |image0| image:: attachments/16286504/16679153.png
.. |image1| image:: attachments/16286504/16679163.png
.. |image2| image:: download/attachments/16286504/899722085
.. |image3| image:: attachments/16286504/16679164.png
.. |image4| image:: attachments/16286504/16679165.png
.. |image5| image:: attachments/16286504/16679166.png
.. |image6| image:: attachments/16286504/16679167.png
.. |image7| image:: attachments/16286504/16679154.png
.. |image8| image:: attachments/16286504/16679155.png
.. |image9| image:: attachments/16286504/16679156.png
.. |image10| image:: attachments/16286504/16679157.png
.. |image11| image:: attachments/16286504/16679159.png
.. |image12| image:: attachments/16286504/16679158.png
.. |image13| image:: attachments/16286504/16679160.png
.. |image14| image:: images/icons/bullet_blue.gif
.. |image15| image:: images/icons/bullet_blue.gif
.. |image16| image:: images/icons/bullet_blue.gif
.. |image17| image:: images/icons/bullet_blue.gif
.. |image18| image:: images/icons/bullet_blue.gif
.. |image19| image:: images/icons/bullet_blue.gif
.. |image20| image:: images/icons/bullet_blue.gif
.. |image21| image:: images/icons/bullet_blue.gif
.. |image22| image:: images/icons/bullet_blue.gif
.. |image23| image:: images/icons/bullet_blue.gif
.. |image24| image:: images/icons/bullet_blue.gif
.. |image25| image:: images/icons/bullet_blue.gif
.. |image26| image:: images/icons/bullet_blue.gif
