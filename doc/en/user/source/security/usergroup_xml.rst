.. _usergroup_xml:

XML
===

Default user/group service that persists the user/group database in an XML file.

The xml user/group service represents the user database as XML corresponding to this :download:`XML schema`. The file is 
named ``users.xml`` and is located under the ``security`` directory at a path of ``usergroup/<name>/users.xml`` where
``<name>`` is the name of the user group service.

The following is the contents of ``users.xml`` that ships with an out of the box GeoServer configuration:

.. code-block:: xml

      <userRegistry version="1.0" xmlns="http://www.geoserver.org/security/users">
          <users>
              <user enabled="true" name="admin" password="crypt1:5WK8hBrtrte9wtImg5i5fjnd8VeqCjDB"/>
          </users>
          <groups/>
      </userRegistry>
  
It contains a single user named "admin" and no groups. User passwords are stored encrypted by default using the 
:ref:`weak PBE <passwd_encryption>` method.

Configuration
-------------

.. figure:: images/usergroup_xml.jpg

*Password encryption* is the method to use to encrypt user passwords. See :ref:`passwd_encryption` for more details.

*Password policy* is the policy to use to enforce constraints on user passwords. See :ref:`passwd_policy` for more details.

*XML Filename* is the name of the file used to persist the XML user/group database. If left unspecified the default 
filename ``users.xml`` is used.

*Enable schema validation* forces schema validation to occur every time the user/group xml file is read. This
option is useful when editing the file by hand.

*File reload interval* defines the time interval in which GeoServer will check for changes to the user/group XML file. When
the file is found to be modified the file will be re-read, and the user/group database recreated. This value is meant to be
set in cases where the XML file contents might change "out of process", and not directly through the web admin interface. 
The value is specified in milliseconds, for example a value of 5000 would cause a check to be made very 5 seconds. A value
of 0 disables any checking of the file.

