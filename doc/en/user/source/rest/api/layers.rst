.. _rest_api_layers:

Layers
======

A ``layer`` is a *published* resource (feature type or coverage).

``/layers[.<format>]``
----------------------

Controls all layers.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Return all layers
     - 200
     - HTML, XML, JSON
     - HTML
   * - POST
     -
     - 405
     - 
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

* :download:`HTML <../representations/layers_html.txt>`
* :download:`XML <../representations/layers_xml.txt>`
* :download:`JSON <../representations/layers_json.txt>`

``/layers/<l>[.<format>]``
--------------------------

Controls a particular layer.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
     - Parameters
   * - GET
     - Return layer ``l``
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
     - Modify layer ``l`` 
     - 200
     - XML,JSON
     -
     - 
   * - DELETE
     - Delete layer ``l``
     - 200
     -
     -
     - :ref:`recurse <rest_api_layers_recurse>`

Representations
~~~~~~~~~~~~~~~

* :download:`HTML <../representations/layer_html.txt>`
* :download:`XML <../representations/layer_xml.txt>`
* :download:`JSON <../representations/layer_json.txt>`

Exceptions
~~~~~~~~~~

.. list-table::
   :header-rows: 1

   * - Exception
     - Return Code
   * - GET for a layer that does not exist
     - 404
   * - PUT that changes name of layer
     - 403
   * - PUT that changes resource of layer
     - 403

Parameters
~~~~~~~~~~

.. _rest_api_layers_recurse:

The ``recurse`` parameter recursively deletes all layers referenced by the specified layer. Allowed values for this parameter are "true" or "false". The default value is "false".

``/layers/<l>/styles[.<format>]``
---------------------------------

Controls all styles in a given layer.

.. list-table::
   :header-rows: 1

   * - Method
     - Action
     - Return Code
     - Formats
     - Default Format
   * - GET
     - Return all styles for layer ``l``
     - 200
     - SLD, HTML, XML, JSON
     - HTML
   * - POST
     - Add a new style to layer ``l``
     - 201, with ``Location`` header
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

.. todo:: No representation.
