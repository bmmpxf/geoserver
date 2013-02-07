.. _wms_dimensions:

WMS Dimensions
==============

.. warning:: Document Status: **Draft**

The WMS specification describes the ability to reference specialized "custom dimensions" common to spatial datasets.  Examples include time, elevation, and temperature. These values, when included in a given dataset, can provide an orthogonal axis by which to view and interpret data.

GeoServer, while primarily a two-dimensional renderer, nevertheless has the ability to handle multi-dimensional attribute data. In particular, GeoServer has native support for both the **Time (temporal)** and **Elevation (vertical)** dimensions. 

This section will discuss both the Time and Elevation profiles for a given layer in GeoServer, how this information gets published, and how it is requested. For more information on setting dimensions on a given layer in the :ref:`web_admin`, please see the section on :ref:`Setting WMS Dimensions <webadmin_data_layer_edit_dimensions>`.

.. _wms_dimensions_time:

Time
----

.. note:: This section is not to be confused with :ref:`creating time-based animations in Google Earth <tutorials_time>`.

GeoServer supports the notion of a temporal dataset, where one attribute in a given layer is determined to be the time dimension. For example, a set of points containing details of weather events might include timestamps as an attribute. As such it could be used to show only weather events over a specific period of time, or even to create an animation of weather events over that period of time. As there are many different ways of representing and storing time strings, hooking a specific attribute up to the WMS Time dimensions can greatly simplify usage.

.. todo:: Image showing theory.

For Time to be available on a given layer, one of the attributes must be of type **Date**.

.. note::

    All time values are referenced in `ISO 8601 date format <http://www.w3.org/TR/NOTE-datetime>`_, which in its most expanded form, looks like this::

      YYYY-MM-DDThh:mm:ss.sTZD

    where TZD means "time zone designator" (Z or +hh:mm or -hh:mm).

    For example::

      2010-01-01T12:00:00.000Z

Capabilities structure
~~~~~~~~~~~~~~~~~~~~~~

When Time is enabled for a given layer, a ``<Dimension>`` tag will be added to the layer entry in the WMS capabilities document. The structure of the tag is as follows:

.. code-block:: xml

   <Dimension name="time" default="current" units="ISO8601">
     {time_presentation}
   </Dimension>

The content of the tag (``{time_presentation}``) is dependent on the settings used when enabling Time on the particular layer, which in turn is dependent on the type of data in the layer.

There are three methods of presentation for time intervals:  **list**, **interval and resolution**, and **continuous interval**.

When using **list**, the capabilities document will advertise every single time value in the entire dataset. This is most useful in datasets when there is a small number of discrete values. Do not use this if the data has a continuous distribution, as the tag content will become very large.

.. code-block:: xml

   <Dimension name="time" default="current" units="ISO8601">
     time1,time2,time3,...,timeN
   </Dimension>

For example, the code below would imply that the layer would contain only three time values throughout all the features:

.. code-block:: xml

   <Dimension name="time" default="current" units="ISO8601">
     2010-01-01T12:00:00.000Z,2010-01-01T13:00:00.000Z,2010-01-01T14:00:00.000Z
   </Dimension>

When using **interval and resolution**, the capabilities document will advertise the earliest value, the latest value, and the unit of time resolution. This presentation format is designed to be used with a layer that has time values that are continuously distributed but with a minimum resolution.

.. code-block:: xml

   <Dimension name="time" default="current" units="ISO8601">
     time_minimum/time_maximum/time_resolution
   </Dimension>

For example, the code below would imply that the layer would have dates throughout the year 2010, with a resolution of one day:

.. code-block:: xml

   <Dimension name="time" default="current" units="ISO8601">
     2010-01-01T12:00:00.000Z/2010-12-31T08:00:00.000Z/P1D
   </Dimension>

Time resolution is given in ISO 8601 Period format, with the form::

  PnYnMnDTnHnMnS

where:

* ``P`` is the duration designator. It is always used.
* ``Y`` is the year designator
* ``M`` is the month designator
* ``W`` is the week designator
* ``D`` is the day designator
* ``T`` is the time designator, used if any time value follows.
* ``H`` is the hour designator
* ``M`` is the minute designator
* ``S`` is the second designator

The time values ``n`` always precede the designator, so ``P1Y2M`` means "one year and two months". If a designator is not necessary (with a value equal to zero) it may be omitted.

When using **continuous interval**, the format is similar to interval and resolution, except that the time resolution is not specified by the user, and in its place is the difference between the earliest and latest time values.

.. code-block:: xml

   <Dimension name="time" default="current" units="ISO8601">
     time_minimum/time_maximum/time_difference
   </Dimension>

For example, the code below would imply that the layer would have dates throughout the year 2010:

.. code-block:: xml

   <Dimension name="time" default="current" units="ISO8601">
     2010-01-01T12:00:00.000Z/2011-01-01T12:00:00.000Z/P1Y
   </Dimension>

.. todo:: Guessing on this behavior.

.. todo:: With the "difference", which isn't included, earliest or latest? Or are they both included?

Request structure
~~~~~~~~~~~~~~~~~

To make a WMS request that includes a time parameter, append the following to the GET request::

  ...&TIME={timestamp}

.. todo:: For a POST request, do what?

The ``{timestamp}`` can consist of a number of different collections of values.

* **Single value**—Only one value, such as ``2010-01-01T12:00:00.000Z``
* **Multiple values**—Comma-separated list, such as ``2010-01-01T12:00:00.000Z,2010-01-01T13:00:00.000Z,2010-01-01T14:00:00.000Z``
* **Range**—Start and end values separated by a slash, such as ``2010-01-01T12:00:00.000Z/2010-12-31T08:00:00.000Z``.  This is a closed interval (start and end values are included).
* **Combination**—Both values and ranges in a single request, such as ``2010-01-01T13:00:00.000Z,2010-01-02T12:00:00.000Z/2010-12-31T08:00:00.000Z``.

.. todo:: Not sure if start and end values are included. Need to verify.

When using a single value or multiple values, they must exactly match attribute values or else no data will be retrieved.

If Time is enabled on a layer and no parameter is included in the request, the attributes with the latest value will be displayed.


.. _wms_dimensions_elevation:

Elevation
---------

Enabling Elevation on a given layer allows for an additional vertical dimension to be used to interpret data. For example, temperature readings may be available at the same position relative to the ground, but may (will) differ based on elevation. Having a dataset that includes the elevation level in the attributes allows for multi-dimensional analysis to occur.

For Elevation to be available, one of the attributes must be of type **Number**.

.. note:: As elevation requires only a numerical value, it is possible to use the Elevation dimension for other applications that don't explicitly refer to the z-axis of the data.

.. todo:: Image showing theory.

Capabilities structure
~~~~~~~~~~~~~~~~~~~~~~

When Elevation is enabled for a given layer, a ``<Dimension>`` tag will be added to the layer entry in the WMS capabilities document. The structure of the tag is as follows:

.. code-block:: xml

   <Dimension name="elevation" default="1.0" units="EPSG:5030" unitSymbol="{units}">
     {elevation_presentation}
   </Dimension>

.. todo:: Explain why default=1.0 and units=EPSG:5030. Also how to change the default (from 1.0)?

where:

* ``{units}``—Symbol denoting the units of the elevation value. Usually the same as the default units for the layer CRS.

The content of the tag (``{elevation_presentation}``) is dependent on the settings used when enabling Elevation on the particular layer, which in turn is dependent on the type of data in the layer.

There are three methods of presentation for elevation intervals:  **list**, **interval and resolution**, and **continuous interval**.

When using **list**, the capabilities document will advertise every single elevation value in the entire dataset. This is most useful in datasets when there is a small number of discrete values. Do not use this if the data has a continuous distribution, as the tag content will become very large.

.. code-block:: xml

   <Dimension name="elevation" default="{default}" units="{crs}" unitSymbol="{units}">
     elevation1,elevation2,elevation3,...,elevationN
   </Dimension>

For example, the code below would imply that the features in the layer would contain only five elevation values:

.. code-block:: xml

   <Dimension name="elevation" default="1.0" units="EPSG:5030" unitSymbol="m">
     500,600,700,800,900,1000
   </Dimension>

When using **interval and resolution**, the capabilities document will advertise the lowest elevation value, the highest elevation value, and the unit of resolution. This presentation format is designed to be used with a layer that has elevation values that are continuously distrbuted.

.. code-block:: xml

   <Dimension name="elevation" default="{default}" units="{crs}" unitSymbol="{units}">
     elevation_minimum/elevation_maximum/elevation_resolution
   </Dimension>

For example, the code below would imply that the layer would have integer elevation values from 0 to 1000 meters:

.. code-block:: xml

   <Dimension name="elevation" default="1.0" units="EPSG:5030" unitSymbol="m">
     0/1000/1
   </Dimension>

When using **continuous interval**, the format is similar to interval and resolution, except that the resolution is not specified by the user, and in its place is the difference between the maximum and minimum elevation values.

.. code-block:: xml

   <Dimension name="elevation" default="{default}" units="{crs}" unitSymbol="{units}">
     elevation_minimum/elevation_maximum/elevation_difference
   </Dimension>

For example, the code below would imply that the layer would have elevation values from 0 to 1000 meters:

.. code-block:: xml

   <Dimension name="elevation" default="1.0" units="EPSG:5030" unitSymbol="m">
     0/1000/999
   </Dimension>

.. todo:: Guessing on this behavior.

.. todo:: With the "difference", which isn't included, highest or lowest? Or are they both included?

Request structure
~~~~~~~~~~~~~~~~~

To make a WMS request that includes an elevation parameter, append the following to the GET request::

  ...&ELEVATION={elevation}

.. todo:: For a POST request, do WHAT?

The ``{elevation}`` can consist of a number of different collections of values.

* **Single value**—Only one numerical value, such as ``25``
* **Multiple values**—Comma-separated list, such as ``25,30,35,40,45,50``
* **Range**—Start and end values separated by a slash, such as ``25/50``.  This is a closed interval (start and end values are included).
* **Combination**—Both values and ranges in a single request, such as ``25,50/100,125,150``.

.. todo:: Not sure if start and end values are included. Need to verify.

When using a single value or multiple values, they must exactly match attribute values or else no data will be retrieved.

If Elevation is enabled on a layer and no parameter is included in the request, the attributes with the default value will be displayed.
