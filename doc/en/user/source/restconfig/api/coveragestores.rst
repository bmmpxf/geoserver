.. _rest_api_coveragestores:

Coverage stores
===============

A ``coverage store`` is a source of spatial data that is raster based.

``/workspaces/<ws>/coveragestores[.<format>]``
----------------------------------------------

Controls all coverage stores in a given workspace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - List all coverage stores in workspace ``ws``
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     - Create a new coverage store
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

* :download:`HTML <../representations/coveragestores_html.txt>`
* :download:`XML <../representations/coveragestores_xml.txt>`
* :download:`JSON <../representations/coveragestores_json.txt>`

``/workspaces/<ws>/coveragestores/<cs>[.<format>]``
---------------------------------------------------

Controls a particular coverage store in a given workspace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
     - Parameters
   * - GET
     - Return coverage store ``cs``
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
     - Modify coverage store ``cs``
     -
     -
     -
     -
   * - DELETE
     - Delete coverage store ``cs``
     -
     -
     -
     - :ref:`recurse <rest_api_coveragestores_recurse>`

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/coveragestore_html.txt>`
* :download:`XML <../representations/coveragestore_xml.txt>`
* :download:`JSON <../representations/coveragestore_json.txt>`

Exceptions
~~~~~~~~~~

.. list-table::
   :header-rows: 1

   * - Exception
     - Return Code
   * - GET for a coverage store that does not exist
     - 404
   * - PUT that changes name of coverage store
     - 403
   * - PUT that changes workspace of coverage store
     - 403
   * - DELETE against a coverage store that contains configured coverage
     - 403

Parameters
~~~~~~~~~~

.. _rest_api_coveragestores_recurse:

The ``recurse`` parameter is used to recursively delete all coverages contained by the specified coverage store. Allowable values for this parameter are "true" or "false". The default value is "false".

``/workspaces/<ws>/coveragestores/<cs>/file[.<extension>]``
-----------------------------------------------------------

This operation uploads a file containing spatial data into an existing coverage store, or will create a new coverage store if it doesn't already exist.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
     - Parameters
   * - GET
     - Get the underlying files for the coverage store as a zip file with 
       mime type ``application/zip``.
     - 200
     - 
     - 
     - 
   * - POST
     - 
     - 405
     - 
     - 
     - :ref:`recalculate <rest_api_coveragestores_recalculate>`
   * - PUT
     - Creates or overwrites the files for coverage store ``cs``.
     - 200
     - :ref:`See note below <rest_api_coveragestores_file_put>`
     - 
     - :ref:`configure <rest_api_coveragestores_configure>`, :ref:`coverageName <rest_api_coveragestores_coveragename>`
   * - DELETE
     -
     - 405
     -
     -
     -

.. _rest_api_coveragestores_file_put:

.. note::

   When the file for a coverage store is PUT, it can be as a standalone file, or as a zipped archive. The standalone file method is only applicable to coverage stores that work from a single file such as GeoTIFF. Coverage stores like Image mosaic must be sent as a zip archive.

   When uploading a standalone file the content type should be appropriately set based on the file type. When uploading a zip archive the ``Content-type`` should be set to ``application/zip``. 

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

The ``extension`` parameter specifies the type of coverage store. The
following extensions are supported:

.. list-table::
   :header-rows: 1

   * - Extension
     - Coverage store
   * - geotiff
     - GeoTIFF
   * - worldimage
     - Georeferenced image (JPEG, PNG, TIFF)
   * - imagemosaic
     - Image mosaic

.. _rest_api_coveragestores_configure:

The ``configure`` parameter is used to control how the coverage store is configured upon file upload. It can take one of the three values:

* ``first``—(*Default*) Only setup the first feature type available in the coverage store.
* ``none``—Do not configure any feature types.
* ``all``—Configure all feature types.

.. _rest_api_coveragestores_coveragename:

The ``coverageName`` parameter is used to specify the name of the coverage
within the coverage store. This parameter is only relevant if the ``configure``
parameter is not equal to "none". If not specified the resulting coverage will
receive the same name as its containing coverage store.

.. note::

   Currently the relationship between a coverage store and a coverage is one to
   one. However there is currently work underway to support multi-dimensional
   coverages, so in the future this parameter is likely to change.

.. _rest_api_coveragestores_recalculate:

The ``recalculate`` parameter specifies whether to recalculate any bounding boxes for a coverage. Some properties of coverages are automatically recalculated when necessary. In particular, the native bounding box is recalculated when the projection or projection policy are changed, and the lat/lon bounding box is recalculated when the native bounding box is recalculated, or when a new native bounding box is explicitly provided in the request. (The native and lat/lon bounding boxes are not automatically recalculated when they are explicitly included in the request.) In addition, the client may explicitly request a fixed set of fields to calculate, by including a comma-separated list of their names in the ``recalculate`` parameter. For example:

* ``recalculate=`` (empty parameter): Do not calculate any fields, regardless of the projection, projection policy, etc. This might be useful to avoid slow recalculation when operating against large datasets.
* ``recalculate=nativebbox``: Recalculate the native bounding box, but do not recalculate the lat/lon bounding box.
* ``recalculate=nativebbox,latlonbbox``: Recalculate both the native bounding box and the lat/lon bounding box.
