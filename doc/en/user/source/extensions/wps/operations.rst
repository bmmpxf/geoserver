.. _wps_operations:

WPS operations
==============

WPS defines three operations for the discovery and execution of geospatial processes.  

The operations are:

* GetCapabilities
* DescribeProcess
* ExecuteProcess

.. _wps_getcaps:

GetCapabilities
---------------

The **GetCapabilities** operation requests details of the service offering, including service metadata and metadata describing the available processes. The response is an XML document called the **capabilities document**.

The required parameters, as in all OGC GetCapabilities requests, are ``service=WPS``, ``version=1.0.0`` and ``request=GetCapabilities``.

An example of a GetCapabilities request is::

  http://localhost:8080/geoserver/ows?
    service=WPS&
    version=1.0.0&
    request=GetCapabilities

DescribeProcess
---------------

The **DescribeProcess** operation requests a description of a WPS process available through the service.

The parameter ``identifier`` specifies the process to describe. Multiple processes can be requested, separated by commas (for example: ``identifier=JTS:buffer``). At least one process must be specified.

.. note:: As with all OGC parameters, the keys (``request``, ``version`` etc.) are case-insensitive, and the values (``GetCapabilities``, ``JTS:buffer`` etc.) are case-sensitive. GeoServer is generally more relaxed about case, but it is best to follow the specification.

The response is an XML document containing metadata about each requested process, including the following:
 
* Process name, title and abstract
* For each input and output parameter: identifier, title, abstract, multiplicity, and supported datatype and format

An example request for the process ``JTS:buffer`` is::

  http://localhost:8080/geoserver/ows?
    service=WPS&
    version=1.0.0&
    request=DescribeProcess&
    identifier=JTS:buffer

The response XML document contains the following information:

.. list-table:: 
   :widths: 20 80 

   * - **Title**
     - "Buffers a geometry using a certain distance"
   * - **Inputs**
     - **geom**: "The geometry to be buffered" *(geometry, mandatory)*
     
       **distance**: "The distance (same unit of measure as the geometry)" *(double, mandatory)*

       **quadrant segments**: "Number of quadrant segments. Use > 0 for round joins, 0 for flat joins, < 0 for mitred joins" *(integer, optional)*

       **capstyle**: "The buffer cap style, round, flat, square" *(literal value, optional)*
   * - **Output formats**
     - One of GML 3.1.1, GML 2.1.2, or WKT
     

ExecuteProcess
--------------

The **ExecuteProcess** operation is a request to perform the process with specified input values and required output data items. The request may be made as either a GET URL, or a POST with an XML request document. Because the request has a complex structure, the POST form is more typically used.

The inputs and outputs required for the request depend on the process being executed. GeoServer provides a wide variety of processes to process geometry, features, and coverage data. For more information see the section on :ref:`wps_processes`.

Below is an example of a ``Execute`` POST request. The example process (``JTS:buffer``) takes as input:

* A geometry ``geom`` (``POINT(0 0)``)
* A ``distance`` (``10``)
* A quantization factor ``quadrantSegments`` (``1``)
* A ``capStyle`` (``flat``)

The ``<ResponseForm>`` element specifies the format for the single output ``result`` to be GML 3.1.1.

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <wps:Execute version="1.0.0" service="WPS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xmlns="http://www.opengis.net/wps/1.0.0" xmlns:wfs="http://www.opengis.net/wfs"
     xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" 
     xmlns:gml="http://www.opengis.net/gml" xmlns:ogc="http://www.opengis.net/ogc" 
     xmlns:wcs="http://www.opengis.net/wcs/1.1.1" xmlns:xlink="http://www.w3.org/1999/xlink" 
     xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsAll.xsd">
      <ows:Identifier>JTS:buffer</ows:Identifier>
      <wps:DataInputs>
        <wps:Input>
          <ows:Identifier>geom</ows:Identifier>
          <wps:Data>
            <wps:ComplexData mimeType="application/wkt"><![CDATA[POINT(0 0)]]></wps:ComplexData>
          </wps:Data>
        </wps:Input>
        <wps:Input>
          <ows:Identifier>distance</ows:Identifier>
          <wps:Data>
            <wps:LiteralData>10</wps:LiteralData>
          </wps:Data>
        </wps:Input>
        <wps:Input>
          <ows:Identifier>quadrantSegments</ows:Identifier>
          <wps:Data>
            <wps:LiteralData>1</wps:LiteralData>
          </wps:Data>
        </wps:Input>
        <wps:Input>
          <ows:Identifier>capStyle</ows:Identifier>
          <wps:Data>
            <wps:LiteralData>flat</wps:LiteralData>
          </wps:Data>
        </wps:Input>
      </wps:DataInputs>
      <wps:ResponseForm>
        <wps:RawDataOutput mimeType="application/gml-3.1.1">
          <ows:Identifier>result</ows:Identifier>
        </wps:RawDataOutput>
      </wps:ResponseForm>
    </wps:Execute>

The process performs a buffer operation using the supplied inputs and returns the outputs as specified. The response from the request is (with numbers rounded for clarity):

.. code-block:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <gml:Polygon xmlns:sch="http://www.ascc.net/xml/schematron"
     xmlns:gml="http://www.opengis.net/gml"
     xmlns:xlink="http://www.w3.org/1999/xlink">
      <gml:exterior>
        <gml:LinearRing>
          <gml:posList>
            10.0 0.0
            0.0 -10.0
            -10.0 0.0 
            0.0 10.0
            10.0 0.0
          </gml:posList>
        </gml:LinearRing>
      </gml:exterior>
    </gml:Polygon>

For help in generating WPS requests you can use the built-in interactive :ref:`wps_request_builder`.

.. _wps_dismiss:

Process dismissal
-----------------

According to the WPS specification, an asynchronous process execution returns a link to a status location that the client can query to get a progress report about the process, and eventually retrieve its final results.

In GeoServer this query is implemented as a pseudo-operation called **GetExecutionStatus**. It has the following structure::

    http://host:port/geoserver/ows?service=WPS&version=1.0.0&request=GetExecutionStatus&executionId=397e8cbd-7d51-48c5-ad72-b0fcbe7cfbdb

The ``executionId`` identifies the running request, and can be used in the **Dismiss** vendor operation in order to **cancel** the execution of the process:

   http://host:port/geoserver/ows?service=WPS&version=1.0.0&request=Dismiss&executionId=397e8cbd-7d51-48c5-ad72-b0fcbe7cfbdb

Upon receipt GeoServer will do its best to stop the running process. Subsequent calls to **Dismiss** or **GetExecutionStatus** will report that the ``executionId`` is not known anymore.

Internally, GeoServer will stop any process that attempt to report progress, retrieve inputs, or generate outputs, but this is not always possible. A process that already has inputs and is not reporting progress will continue until its natural end.

For example, consider the ``geo:Buffer`` process, possibly working against a very large input GML geometry, to be fetched from another host. The process itself does a single call to a JTS function, which cannot report progress. Here are three possible scenarios, depending on when the Dismiss operation is invoked:

* Dismiss is invoked while the GML is being retrieved. In this case the execution will stop immediately.
* Dismiss is invoked while the output GML is being encoded. In this case the execution will stop immediately.
* Dismiss is invoked while the process is doing the buffering. In this case, the execution will stop as soon as the buffering is completed.
