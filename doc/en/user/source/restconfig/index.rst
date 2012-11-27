.. _rest:

REST Configuration
==================

GeoServer provides a `RESTful <http://en.wikipedia.org/wiki/Representational_state_transfer>`_ interface through which clients can configure an instance through simple HTTP calls. Using the REST interface, clients can programatically configure GeoServer without the need to use the :ref:`web_admin`.

REST is an acronym for "`REpresentational State Transfer <http://en.wikipedia.org/wiki/Representational_state_transfer>`_".  The basic idea of REST is to rely on a fixed set of operations on named resources, where the representation of each resource is the same for retrieving and setting information. In other words, one can retrieve (read) data in an XML format and can also send data back to the server in the same XML format in order to set (write) changes to the system.

Operations on resources are implemented with the standard primitives of HTTP:  GET, DELETE, PUT, POST, HEAD, etc. Each
resource is represented as a standard URI, such as ``http://GEOSERVER_HOME/rest/workspaces/topp``.

For details about the REST API, proceed to the :ref:`rest_api` section.  For some hands on examples, process to the :ref:`rest_examples` section.
 
.. toctree::
   :maxdepth: 2

   api
   examples/index

