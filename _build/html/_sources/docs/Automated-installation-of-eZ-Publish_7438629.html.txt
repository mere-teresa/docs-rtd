#. `eZ Publish Platform 5.<next> <index.html>`__
#. `eZ Publish Platform
   Documentation <eZ-Publish-Platform-Documentation_1114149.html>`__
#. `Installation and Upgrade
   Guides <Installation-and-Upgrade-Guides_6292016.html>`__
#. `Installation <Installation_7438500.html>`__
#. `Automated installation <Automated-installation_7438514.html>`__

eZ Publish Platform 5.<next> : Automated installation of eZ Publish
===================================================================

Added by andrea.melo@ez.no , edited by ricardo.correia@ez.no on Jun 17,
2013

 

Depending on the target system, please refer to either `"Installing eZ
Publish on a Linux/UNIX based
system" <Installing-eZ-Publish-on-a-Linux-UNIX-based-system_7438581.html>`__
or `"Installing eZ Publish on
Windows" <Installing-eZ-Publish-on-Windows_7438583.html>`__ for
information other steps needed to do an installation, the step below is
to be done before executing the setup wizard.

Configuring the kickstart system
--------------------------------

The behavior of the automated installation is controlled by the
"``kickstart.ini``\ " configuration file. This file makes it possible to
specify parameters for each installation step of the web based setup
wizard. For example, by providing the database connection parameters,
the corresponding setup wizard step will have the input forms
pre-filled. It is also possible to instruct the wizard to skip certain
steps.

Initialization
~~~~~~~~~~~~~~

Create a copy of the "``ezpublish_legacy/``\ kickstart.ini-dist" file
(located in the root of your eZ Publish installation) and make sure that
the copy is named "``ezpublish_legacy/``\ ``kickstart.ini``" (located in
the root of eZ Publish).

The following example shows how this can be done on a Linux/UNIX based
system:

#. Navigate into the eZ Publish directory:

   ::

       $ cd /path/to/ezpublish_legacy/

#. Copy and rename the configuration file:

   ::

       $ cp kickstart.ini-dist kickstart.ini

Security issues
^^^^^^^^^^^^^^^

The web server must have read access to the "``kickstart.ini``\ " file
during the installation process. This might become a security problem at
a later stage if the file contains usernames, passwords, etc. To prevent
this from happening, it is recommended to do one of the following:

-  Remove the file when the installation has completed.
-  Use rewrite rules to make sure that it is not readable from outside.

Configuration blocks
~~~~~~~~~~~~~~~~~~~~

The "``kickstart.ini``\ " file contains a configuration block for every
step of the setup wizard. The block names are encapsulated by square
brackets. The following list shows an overview of the available blocks.

-  ``[email_settings]``
-  ``[database_choice]``
-  ``[database_init]``
-  ``[language_options]``
-  ``[site_types]``
-  ``[site_access]``
-  ``[site_details]``
-  ``[site_admin]``
-  ``[security]``
-  ``[registration]``

In the default kickstart file, everything is commented out. The blocks
and the corresponding settings have to be uncommented in order to take
effect. This can be done by removing the hash ("#") characters from the
start of the lines that you should be activated. Make sure that there
are no leading whitespace characters at the start of the lines.

Configuration parameters
~~~~~~~~~~~~~~~~~~~~~~~~

Each parameter takes a text string as an input value. Some parameters
are able to handle an array of strings. The following examples
demonstrate the two parameter types.

-  Single parameter:

   ::

       Server=www.example.com

-  Array parameter:

   ::

       Title[]
       Title[news]=The news site
       Title[forums]=The forum site

Documentation and examples
~~~~~~~~~~~~~~~~~~~~~~~~~~

The "``kickstart.ini``\ " file contains documentation in the file
itself. Please refer to the embedded instructions and examples for a
detailed explanation of the steps. The following table shows how the
examples / inline instructions deal with required and optional
parameters.

**Syntax**

**Description**

``<value>``

Angle brackets indicate that the parameter value is required, example:

.. code:: wordwrap

    #Server=<hostname>

``[value]``

Squared brackets indicate that the parameter value is optional, example:

.. code:: wordwrap

    #FirstName=[string]

A parameter will only take effect if it has been uncommented. Remove the
leading hash ("#") and make sure that there ar no whitespace characters
at the start of the lines that include the uncommented parameters.

Skipping steps
~~~~~~~~~~~~~~

A step can be skipped by uncommenting and setting the "``Continue``\ "
parameter to "``true``\ ". This parameter can be used for each step /
block. The following table shows the outcome for the different
configurations of the "``Continue``\ " parameter.

**Assignment**

**Result**

``Continue=false``

The step will be shown and the input values will be pre-filled with the
values (if any) defined in the "kickstart.ini" configuration file. This
is the same as when the "``Continue``\ " parameter is missing or if it
has been commented out.

``Continue=true``

The system will automatically use the values defined in the kickstart
file and thus the step will not be shown. However, if something goes
wrong (missing or wrong values, etc.), the step will be shown.

Document generated by Confluence on Feb 12, 2014 16:43
