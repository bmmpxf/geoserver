.. _rest_api_namespaces:

Namespaces
==========

A ``namespace`` is a uniquely identifiable grouping of feature types. It is identified by a prefix and a URI.

``/namespaces[.<format>]``
--------------------------

Controls all namespaces.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - List all namespaces
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     - Create a new namespace
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

* :download:`HTML <../representations/namespaces_html.txt>`
* :download:`XML <../representations/namespaces_xml.txt>`
* :download:`JSON <../representations/namespaces_json.txt>`


``/namespaces/<ns>[.<format>]``
-------------------------------

Controls a particular namespace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Returns namespace ``ns``
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     -
     - 405
     -
     -
   * - PUT
     - 200
     - Modify namespace ``ns``
     - XML, JSON
     -
   * - DELETE
     - 200
     - Delete namespace ``ns``
     - XML, JSON
     -

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/namespace_html.txt>`
* :download:`XML <../representations/namespace_xml.txt>`
* :download:`JSON <../representations/namespace_json.txt>`

Exceptions
~~~~~~~~~~

.. list-table::
   :header-rows: 1

   * - Exception
     - Return Code
   * - GET for a namespace that does not exist
     - 404
   * - PUT that changes prefix of namespace
     - 403
   * - DELETE against a namespace whose corresponding workspace is non-empty
     - 403

``/namespaces/default[.<format>]``
----------------------------------

Controls the default namespace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Returns default namespace
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     -
     - 405
     -
     -
   * - PUT
     - 200
     - Set default namespace
     - XML, JSON
     -
   * - DELETE
     -
     - 405
     -
     -

.. todo:: No representation?
