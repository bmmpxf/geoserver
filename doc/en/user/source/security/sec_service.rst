.. _sec_service:

Service Security
================

GeoServer allows for access control at the service level, allowing for the locking down of service operations to only 
authenticated users who have been granted a particular role. There are two main categories of services present in GeoServer. The first is :ref:`OGC <services>` services such as WMS and WFS. The second are RESTful services, such 
:ref:`RESTConfig <rest_extension>`.

.. note::

   Service security and :ref:`sec_layer` cannot be combined.  For example, it is not possible to specify access to a specific OGC service on one specific layer.

OGC services
------------

Security for OGC services allow for setting access constraints globally to a particular service, or to a specific operation
within that service. A few examples might include:

* Securing the entire WFS service so only authenticated users have access to all WFS operations
* Allowing anonymous access to read-only WFS operations such as GetCapabilities, DescribeFeatureType, GetFeature, etc... 
  but securing write operations such as Transaction
* Disabling the WFS service by securing all operations and not handing out the necessary roles to any users

OGC service security access rules are specified in a file named ``services.properties``, located in the ``security`` directory in the GeoServer data directory. The file contains a list of rules mapping service operations to defined roles. The syntax for specifying rules is as follows. 

``[]`` denote optional parameters, ``|`` means "or"

::

   <service>.<operation|*>=<role>[,<role2>,...]

where:

* **service** is the identifier of an OGC service, examples include ``wfs``, ``wms``, or ``wcs``
* **operation** can be any operation supported by the service, examples include ``GetFeature`` for WFS, ``GetMap`` for WMS, ``*`` indicates operations
* **role[,role2,...]** is a list of predefined role names

.. note::

   It is important that roles specified are actually linked to a user, otherwise the whole service/operation will be 
   accessible to no one. However in some cases this may be the desired effect.

The default service security configuration in GeoServer contains no rules, and allows any anonymous user to access any operation of any service. Let us consider the examples mentioned above and determine what the corresponding rules would 
look like.

Securing the entire WFS service
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

   wfs.*=ROLE_WFS
   
This rule only allows any WFS operation to authenticated users who have been granted the ``ROLE_WFS`` role.


Allowing anonymous WFS access only for read operations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

   wfs.Transaction=ROLE_WFS_WRITE

This rule allows for anonymous access to GetCapabilities, GetFeature, etc... but a Transaction request will only succeed
if the authenticated user has been granted the ``ROLE_WFS_WRITE``.

Securing WFS operations that access actual data, and further securing write operations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

::

   wfs.GetFeature=ROLE_WFS_READ
   wfs.Transaction=ROLE_WFS_WRITE

These rules allow anonymous access to GetCapabilities and DescribeFeatureType but forces the user to authenticate for 
the GetFeature operation. Furthermore, a second role is required for users performing transactions.

REST services
-------------

.. note::

   RESTful security configuration is available in GeoServer versions greater than 2.0.1.

In addition to providing the ability to secure OGC style services GeoServer also allows for the securing of RESTful services.

REST service security access rules are specified in a file named ``rest.properties``, located in the ``security`` directory of the GeoServer data directory. The file contains a list of rules mapping request uris to defined roles. The rule syntax 
is as follows.

``[]`` denote optional parameters

::

   <uriPattern>;<method>[,<method>,...]=<role>[,<role>,...]

where:

* **uriPattern** is the :ref:`ant pattern <ant_patterns>` that matches a set of request uri's 
* **method** is an HTTP request method, one of ``GET``, ``POST``, ``PUT``, ``POST``, ``DELETE``, or ``HEAD``
* **role** is the name of a predefined role. The wildcard '* is used to indicate the permission is applied to all users, including anonymous users.

A few things to note:

* uri patterns should account for the first component of the rest path, usually ``rest`` or ``api``
* method and role lists should **not** contain any spaces

.. _ant_patterns:

Ant patterns
^^^^^^^^^^^^

Ant patterns are a commonly used syntax for pattern matching directory and file paths. The :ref:`examples <examples>` section contains some basic examples. The apache ant `user manual <http://ant.apache.org/manual/dirtasks.html>`_ contains more sophisticated cases.

Consider the following examples, most of which are specific to the :ref:`rest configuration extension <rest_extension>` but any RESTful GeoServer service can be configured in the same manner.

Allowing only authenticated access to services
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The most secure of configurations is one that forces any request to be authenticated. The following will lock down access to all requests::

   /**;GET,POST,PUT,DELETE=ROLE_ADMINISTRATOR

A slightly less restricting configuration locks down access to operations under the path ``/rest``, but will allow anonymous access to requests that fall under other paths (for example ``/api``)::

   /rest/**;GET,POST,PUT,DELETE=ROLE_ADMINISTRATOR

The following configuration is like the previous except it grants access to a specific role rather than the administrator::

   /**;GET,POST,PUT,DELETE=ROLE_TRUSTED

Where ``ROLE_TRUSTED`` is a pre-defined role.

Providing anonymous read-only access
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following configuration allows anonymous access when the ``GET`` (read) method is used but forces authentication for a ``POST``, ``PUT``, or ``DELETE`` (write)::

   /**;GET=IS_AUTHENTICATED_ANONYMOUSLY
   /**;POST,PUT,DELETE=TRUSTED_ROLE

Securing a specific resource
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following configuration forces authentication for access to a particular resource (in this case a feature type)::

  /rest/**/states*;GET=TRUSTED_ROLE
  /rest/**;POST,PUT,DELETE=TRUSTED_ROLE

The following secures access to a set of resources (in this case all data stores)::

  /rest/**/datastores/*;GET=TRUSTED_ROLE
  /rest/**/datastores/*.*;GET=TRUSTED_ROLE
  /rest/**;POST,PUT,DELETE=TRUSTED_ROLE