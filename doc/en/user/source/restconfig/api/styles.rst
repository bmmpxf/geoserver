.. _rest_api_styles:

Styles
======

A ``style`` describes how a resource (feature type or coverage) should be symbolized or rendered by the Web Map Service. In GeoServer styles are specified with :ref:`SLD <styling>`.

``/styles[.<format>]``
----------------------

Controls all styles.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
     - Parameters
   * - GET
     - Return all styles
     - 200
     - HTML, XML, JSON
     - HTML
     -
   * - POST
     - Create a new style
     - 201 with ``Location`` header
     - SLD, XML, JSON
       :ref:`See note below <rest_api_styles_post_put>`
     -
     - :ref:`name <rest_api_styles_name>`
   * - PUT
     - 
     - 405
     - 
     - 
     -
   * - DELETE
     - 
     - 405
     -
     -
     - :ref:`purge <rest_api_styles_purge>`

.. _rest_api_styles_post_put:

When executing a POST or PUT request with an SLD style, the ``Content-type`` header should be set to ``application/vnd.ogc.sld+xml``.

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/styles_html.txt>`
* :download:`XML <../representations/styles_xml.txt>`
* :download:`JSON <../representations/styles_json.txt>`

Parameters
~~~~~~~~~~

.. _rest_api_styles_name:

The ``name`` parameter specifies the name to be given to the style. This option is most useful when executing a POST request with a style in SLD format, and an appropriate name can be not be inferred from the SLD itself.


``/styles/<s>[.<format>]``
--------------------------

Controls a given style.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Return style ``s``
     - 200
     - SLD, HTML, XML, JSON
     - HTML
   * - POST
     - 
     - 405
     -
     -
   * - PUT
     - Modify style ``s`` 
     - 200
     - SLD, XML, JSON, :ref:`See note above <rest_api_styles_post_put>`
     - 
   * - DELETE
     - Delete style ``s``
     - 200
     -
     -

Representations
~~~~~~~~~~~~~~~

* :download:`SLD <../representations/style_sld.txt>`
* :download:`HTML <../representations/style_html.txt>`
* :download:`XML <../representations/style_xml.txt>`
* :download:`JSON <../representations/style_json.txt>`

Exceptions
~~~~~~~~~~

.. list-table::
   :header-rows: 1

   * - Exception
     - Return Code
   * - GET for a style that does not exist
     - 404
   * - PUT that changes name of style
     - 403
   * - DELETE against style which is referenced by existing layers
     - 403

Parameters
~~~~~~~~~~

.. _rest_api_styles_purge:

The ``purge`` parameter specifies whether the underlying SLD file for the style should be deleted on disk. Allowable values for this parameter are "true" or "false". When set to ``true`` the underlying file will be deleted. 


``/workspaces/<ws>/styles[.<format>]``
--------------------------------------

Controls all styles in a given workspace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
     - Parameters
   * - GET
     - Return all styles within workspace ``ws``
     - 200
     - HTML, XML, JSON
     - HTML
     -
   * - POST
     - Create a new style within workspace ``ws``
     - 201 with ``Location`` header
     - SLD, XML, JSON, :ref:`See note above <rest_api_styles_post_put>`
     -
     - :ref:`name <rest_api_styles_name>`
   * - PUT
     - 
     - 405
     - 
     - 
     -
   * - DELETE
     - 
     - 405
     -
     -
     - :ref:`purge <rest_api_styles_purge>`

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/styles_html.txt>`
* :download:`XML <../representations/styles_xml.txt>`
* :download:`JSON <../representations/styles_json.txt>`


``/workspaces/<ws>/styles/<s>[.<format>]``
------------------------------------------

Controls a particular style in a given workspace.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Return style ``s`` within workspace ``ws``
     - 200
     - SLD, HTML, XML, JSON
     - HTML
   * - POST
     - 
     - 405
     -
     -
   * - PUT
     - Modify style ``s`` within workspace ``ws``
     - 200
     - SLD, XML, JSON
       :ref:`See note above <rest_api_styles_post_put>`
     - 
   * - DELETE
     - Delete style ``s`` within workspace ``ws``
     - 200
     -
     -

Representations
~~~~~~~~~~~~~~~

 * :download:`SLD <../representations/style_sld.txt>`
 * :download:`HTML <../representations/style_html.txt>`
 * :download:`XML <../representations/style_xml.txt>`
 * :download:`JSON <../representations/style_json.txt>`

