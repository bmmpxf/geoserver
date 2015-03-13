.. _wps_webadmin:

WPS configuration
=================

The Web Processing Service (WPS) page supports the basic metadata for the service, as well as service specific settings. 

This page can be accessed in the :ref:`web_admin` by clicking :guilabel:`WPS` under :guilabel:`Services`.

Service metadata
----------------

The service metadata section is common among all services. See the section on :ref:`service_metadata` for more details.

.. figure:: images/metadata.png

   WPS metadata
   
Execution and resource management options
-----------------------------------------

.. figure:: images/execution.png

   WPS execution setting
   
Execution settings:

* **Connection timeout**: Number of seconds WPS will wait before giving up on a remote HTTP connection used to retrieve complex inputs.
* **Maximum synchronous executions run parallel**: Maximum number of synchronous processes that will run in parallel at a given time. The others will be queued.
* **Maximum execution time for synchronous requests**: Processes running in synchronous mode will have to complete within the set time limit, or they will be dismissed automatically. These requests have the client waiting for a response on a HTTP connection, so choose a relatively short time (such as 60 seconds) 
* **Maximum asynchronous executions run parallel**: Maximum number of asynchronous processes that will run in parallel at a given time. The others will be queued.
* **Maximum execution time for asynchronous requests**: Processes running in asynchronous mode will have to complete within the set time limit, or they will be dismissed automatically.  

Resource settings:

* **Resource expiration timeout**: number of seconds the result of a asynchronous execution will be kept available on disk for user to retrieve. Once this time is expired these resources will be eligible for clearing (which happens at regular intervals).
* **Resource storage directory**: where on disk the input, temporary and output resources associated to a certain process will be kept. By default it will be the ``temp/wps`` directory inside the GeoServer data directory
  
Process status page
-------------------

The process status page, available in the :guilabel:`About & Status` section, reports about running and recently
completed processes:

.. figure:: images/statuspage.png

   Process status page
   
The table contains several information bits:

* **S/A**: Synchronous or asynchronous execution
* **Node**: Name of the machine running the process (important in a clustered environment)
* **User**: User that started the execution
* **Process name**: Main process being run (chained processes will not appear here)
* **Created**: When the process got created
* **Phase**: Current phase in the process lifecycle
* **Progress**: Current progress
* **Task**: What the process is doing at the moment

In GeoServer there are the following execution phases:

* **QUEUED**: Process is waiting to be executed
* **RUNNING**: Process is either retrieving/parsing the inputs, computing the results, or writing them out
* **FAILED**: Process execution terminated with a failure
* **SUCCESS**: Process execution terminated with a success
* **DISMISSING**: Process execution is being dismissed. Depending on the process nature this might take some time

All executions listed in the table can be dismissed by selecting them and them clicking :guilabel:`Dismiss selected processes` at the top of the table. This operation allows you to dismiss synchronous processes (something the "Dismiss" vendor operation cannot do). Once the process dismissal is complete, the process execution will disappear from the table.

Completed processes can also be dismissed. This will cause all disk resources associated to the process to be removed immediately instead of waiting for the regular time-based expiration.
