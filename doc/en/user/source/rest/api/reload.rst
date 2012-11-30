.. _rest_api_reload:

Reloading configuration
=======================

Reloads the GeoServer catalog and configuration from disk. This operation is used in cases where an external tool has modified the on-disk configuration. This operation will also force GeoServer to drop any internal caches and reconnect to all data stores.

``/reload``
-----------

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     -
     - 405
     - 
     - 
   * - POST
     - Reloads the configuration from disk
     - 200
     - 
     - 
   * - PUT
     - Reloads the configuration from disk
     - 200
     - 
     - 
   * - DELETE
     -
     - 405
     -
     -
     
.. todo:: No representation.