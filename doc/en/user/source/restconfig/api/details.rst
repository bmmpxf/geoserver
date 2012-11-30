.. _rest_api_details:

API details
===========

This page contains information on how the REST API architecture operates.

.. todo:: This section needs a better intro.

Authentication
--------------

Requests that modify resources (POST, PUT, and DELETE operations) require the client to be authenticated. Currently the only supported method of authentication is Basic authentication. See the :ref:`examples <rest_examples>` section for examples of how to perform authentication with various clients and environments.

.. todo:: Aren't there other ways to authenticate now that there is a new security subsystem?

Return codes
------------

An HTTP request uses a "return code" (status code) to relay the outcome of the request to the client. Different status codes are used for various purposes through out this document. These codes are described in detail by the `HTTP specification <http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html>`_.

.. todo:: In addition to linking to the above, should have examples of the common REST codes (200, 201, 405)

Formats and representations
---------------------------

.. todo:: What's the difference between a format and a representation?

A ``format`` specifies how a particular resource should be represented. A format is used:

* In an operation to specify what representation should be returned to the client
* In a POST or PUT operation to specify the representation being sent to the server

In a **GET** operation the format can be specified in two ways. The first is with the ``Accept`` header. For instance, setting the header to ``"Accept: text/xml"`` would specify the desire to have the resource returned as XML. The second method of specifying the format is via file extension. For example, given a resource ``foo``, to request a representation of ``foo`` as XML, the request URI would end with ``/foo.xml``. To request a representation as JSON, the request URI would end with ``/foo.json``. When no format is specified the server will use its own internal format, usually HTML.

In a **POST** or **PUT** operation the format specifies both the representation of the content being sent to the server, and the representation of the response to be sent back. The representation of content being sent to the server is specified with the ``Content-type`` header. For example, to send a representation in XML, use ``"Content-type: text/xml"`` or ``"Content-type: application/xml"``. The representation of content being sent to the server is specified with the ``Accept`` header as with the GET request.

The following table defines the ``Content-type`` values for each format: 

.. list-table::
   :header-rows: 1

   * - Format
     - Content-type
   * - XML
     - ``application/xml``
   * - JSON
     - ``application/json``
   * - HTML
     - ``application/html``
   * - SLD
     - ``application/vnd.ogc.sld+xml``

.. todo:: What about Accept?  What about text/xml, as mentioned above?

