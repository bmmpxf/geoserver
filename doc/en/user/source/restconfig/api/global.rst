.. _rest_api_global:

Global settings
===============

Allows accessing of GeoServer global settings.

``/settings[.<format>]``
------------------------

Controls all global settings.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - List all global settings
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     - 
     - 405
     - 
     - 
   * - PUT
     - Update global settings
     - 200
     - XML, JSON
     -
   * - DELETE
     -
     - 405
     -
     -

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/settings_html.txt>`
* :download:`XML <../representations/settings_xml.txt>`
* :download:`JSON <../representations/settings_json.txt>`


``/settings/contact[.<format>]``
--------------------------------

Controls global contact information only.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - List global contact information
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     - 
     - 405
     - 
     - 
   * - PUT
     - Update global contact
     - 200
     - XML, JSON
     -
   * - DELETE
     -
     - 405
     -
     -

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/contact_html.txt>`
* :download:`XML <../representations/contact_xml.txt>`
* :download:`JSON <../representations/contact_json.txt>`

