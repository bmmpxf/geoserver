.. _rest_api_services:

OWS Services
============

GeoServer includes several types of OGC services like WCS, WFS and WMS, commonly referred to as "OWS" services. These services can be global for the whole GeoServer instance or local to a particular workspace. In this last case, they are called :ref:`virtual services <virtual_services>`.

``/services/wcs/settings[.<format>]``
-------------------------------------

Controls Web Coverage Service settings.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Return global WCS settings
     - 200
     - XML, JSON
     - HTML
   * - POST
     -
     - 405
     - 
     - 
   * - PUT
     - Modify global WCS settings
     - 200
     - 
     - 
   * - DELETE
     -
     - 405
     - 
     - 

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/wcs_html.txt>`
* :download:`XML <../representations/wcs_xml.txt>`
* :download:`JSON <../representations/wcs_json.txt>`


``/services/wcs/<ws>/settings[.<format>]``
------------------------------------------

Controls Web Coverage Service settings for a given workspace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Return WCS settings for workspace ``ws``
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     - 
     - 405
     -
     -
   * - PUT
     - Create or modify WCS settings for workspace ``ws``
     - 200
     - XML,JSON
     - 
   * - DELETE
     - Delete WCS settings for workspace ``ws``
     - 200
     -
     -

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/wcsWS_html.txt>`
* :download:`XML <../representations/wcsWS_xml.txt>`
* :download:`JSON <../representations/wcsWS_json.txt>`


``/services/wfs/settings[.<format>]``
-------------------------------------

Controls Web Feature Service settings.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Return global WFS settings
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     - 
     - 405
     -
     -
   * - PUT
     - Modify global WFS settings
     - 200
     - XML,JSON
     - 
   * - DELETE
     - 
     - 405
     -
     -

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/wfs_html.txt>`
* :download:`XML <../representations/wfs_xml.txt>`
* :download:`JSON <../representations/wfs_json.txt>`


``/services/wfs/<ws>/settings[.<format>]``
------------------------------------------

Controls Web Feature Service settings for a given workspace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Return WFS settings for workspace ``ws``
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     - 
     - 405
     -
     -
   * - PUT
     - Modify WFS settings for workspace ``ws``
     - 200
     - XML,JSON
     - 
   * - DELETE
     - Delete WFS settings for workspace ``ws``
     - 200
     -
     -

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/wfsWS_html.txt>`
* :download:`XML <../representations/wfsWS_xml.txt>`
* :download:`JSON <../representations/wfsWS_json.txt>`


``/services/wms/settings[.<format>]``
-------------------------------------

Controls Web Map Service settings.


.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Return global WMS settings
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     - 
     - 405
     -
     -
   * - PUT
     - Modify global WMS settings
     - 200
     - XML,JSON
     - 
   * - DELETE
     - 
     - 405
     -
     -

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/wms_html.txt>`
* :download:`XML <../representations/wms_xml.txt>`
* :download:`JSON <../representations/wms_json.txt>`


``/services/wms/<ws>/settings[.<format>]``
------------------------------------------

Controls Web Map Service settings for a given workspace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Return WMS settings for workspace ``ws``
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     - 
     - 405
     -
     -
   * - PUT
     - Modify WMS settings for workspace ``ws``
     - 200
     - XML,JSON
     - 
   * - DELETE
     - Delete WMS settings for workspace ``ws``
     - 200
     -
     -

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/wmsWS_html.txt>`
* :download:`XML <../representations/wmsWS_xml.txt>`
* :download:`JSON <../representations/wmsWS_json.txt>`

.. todo:: WPS?
