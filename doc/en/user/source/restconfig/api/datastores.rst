.. _rest_api_datastores:

Data stores
===========

A ``data store`` is a source of spatial data that is vector based. It can be a file (such as a shapefile), a database (such as PostGIS), or a server (such as a :ref:`remote Web Feature Service <data_external_wfs>`).

``/workspaces/<ws>/datastores[.<format>]``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Controls all data stores in a given workspace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - List all data stores in workspace ``ws``
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     - Create a new data store
     - 201 with ``Location`` header 
     - XML, JSON
     - 
   * - PUT
     -
     - 405
     -
     -
   * - DELETE
     -
     - 405
     -
     -

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/datastores_html.txt>`
* :download:`XML <../representations/datastores_xml.txt>`
* :download:`JSON <../representations/datastores_json.txt>`

``/workspaces/<ws>/datastores/<ds>[.<format>]``
-----------------------------------------------

Controls a particular data store in a given workspace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
     - Parameters
   * - GET
     - Return data store ``ds``
     - 200
     - HTML, XML, JSON
     - HTML
     -
   * - POST
     - 
     - 405
     - 
     -
     - 
   * - PUT
     - Modify data store ``ds``
     -
     -
     -
     -
   * - DELETE
     - Delete data store ``ds``
     -
     -
     -
     - :ref:`recurse <rest_api_datastores_recurse>`

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/datastore_html.txt>`
* :download:`XML <../representations/datastore_xml.txt>`
* :download:`JSON <../representations/datastore_json.txt>`

Exceptions
~~~~~~~~~~

.. list-table::
   :header-rows: 1

   * - Exception
     - Return Code
   * - GET for a data store that does not exist
     - 404
   * - PUT that changes name of data store
     - 403
   * - PUT that changes workspace of data store
     - 403
   * - DELETE against a data store that contains configured feature types
     - 403

Parameters
~~~~~~~~~~

.. _rest_api_datastores_recurse:

The ``recurse`` parameter is used to recursively delete all feature types contained by the specified data store. Allowable values for this parameter are "true" or "false". The default value is "false".



``/workspaces/<ws>/datastores/<ds>/file[.<extension>]``
-------------------------------------------------------

``/workspaces/<ws>/datastores/<ds>/url[.<extension>]``
------------------------------------------------------

``/workspaces/<ws>/datastores/<ds>/external[.<extension>]``
-----------------------------------------------------------

These three operations upload a file containing spatial data into an existing data store, or will create a new data store if it doesn't already exist.

The ``file``, ``url``, and ``external`` endpoints are used to specify the method that is used to upload the file.

* The ``file`` method is used to directly upload a file from a local source. The body of the request is the file itself.
* The ``url`` method is used to indirectly upload a file from an remote source. The body of the request is a URL pointing to the file to upload. This URL must be visible from the server. 
* The ``external`` method is used to use an existing file on the server. The body of the request is the absolute path to the existing file.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
     - Parameters
   * - GET
     - *Deprecated*. Retrieve the underlying files for the data store as a zip file with mime type ``application/zip``. 
     - 200
     - 
     - 
     - 
   * - POST
     - 
     - 405
     - 
     - 
     -
   * - PUT
     - Uploads files to the data store ``ds``, creating it if necessary.
     - 200
     - See :ref:`notes <rest_api_datastores_file_put>` below.
     - 
     - :ref:`configure <rest_api_datastores_configure>`, :ref:`target <rest_api_datastores_target>`, :ref:`update <rest_api_datastores_update>`, :ref:`charset <rest_api_datastores_charset>`
   * - DELETE
     -
     - 405
     -
     -
     -


Exceptions
~~~~~~~~~~

.. list-table::
   :header-rows: 1

   * - Exception
     - Return Code
   * - GET for a data store that does not exist
     - 404
   * - GET for a data store that is not file based
     - 404


Parameters
~~~~~~~~~~

.. _rest_api_datastores_extension:

The ``extension`` parameter specifies the type of data being uploaded. The following extensions are supported:

.. list-table::
   :header-rows: 1

   * - Extension
     - Datastore
   * - shp
     - Shapefile
   * - properties
     - Property file
   * - h2
     - H2 Database
   * - spatialite
     - SpatiaLite Database

.. _rest_api_datastores_file_put:

When executing a PUT request with a file data store, it can be as a standalone file or a zipped archive. The standalone file method is only applicable to data stores that work from a single file (for example GML). Data stores that require multiple files (such as shapefiles) must be sent as an archive.

When uploading a standalone file the content type should be appropriately set based on the file type. When uploading an archive the ``Content-type`` should be set to ``application/zip``. 

.. _rest_api_datastores_configure:

The ``configure`` parameter is used to control how the data store is configured upon file upload. It can take one of the three values:

* ``first``—(*Default*) Only setup the first feature type available in the data store.
* ``none``—Do not configure any feature types.
* ``all``—Configure all feature types.

.. _rest_api_datastores_target:

The ``target`` parameter is used to control the type of data store that is created on the server when the data store that is the target of the PUT request does not exist. The allowable values for this parameter are the same as for the :ref:`extension parameter <rest_api_datastores_extension>`. 

.. _rest_api_datastores_update:

The ``update`` parameter is used to control how existing data is handled when the file is PUT into a data store that already exists and already contains a schema that matches the content of the file. It can take one of the two values:

* ``append``—Data being uploaded is appended to the existing data. This is the default.
* ``overwrite``—Data being uploaded replaces any existing data.

.. _rest_api_datastores_charset:

The ``charset`` parameter is used to specify the character encoding of the file being uploaded (such as "ISO-8559-1"). 
