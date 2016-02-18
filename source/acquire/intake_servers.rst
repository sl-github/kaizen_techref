Intake Servers
==============

The Intake Servers receive the BAS data from the collection devices.  
Composed of four data specific consumer tasks, they transfer the incoming data from the 
Intake Queues into Kaizen's primary database collections.  Each task attaches to a 
specific intake queue.  Each pulls the incoming data off the head of the queue;  
performs some basic validation; then determines to which building the data belongs; and 
stores the data in the appropriate database collection.  

The tasks map - these queues to these storage collections:

+-----------------+--------------------+--------------------------+--------------------+
| Intake Task     | Data Stream        |           Queue          | Storage            | 
+-----------------+--------------------+--------------------------+--------------------+
| intake_trends   || Trend logs        | | cta.trendlogs          | | ctTrends::Latest |
|                 |                    | | cta.arrivals.trendlogs | | ---              |
+-----------------+--------------------+--------------------------+--------------------+
| intake_objects  | Controller Objects | cta.objects              | ctObjects::Latest  |
+-----------------+--------------------+--------------------------+--------------------+
| intake_backups  | Controller Backups | cta.Backups              | ctBackups::Latest  |
+-----------------+--------------------+--------------------------+--------------------+
| intake_events   | Controller Events  | cta.events               | ctEvents::Latest   |
+-----------------+--------------------+--------------------------+--------------------+

.. note:: The Trend logs Intake Server also produces trend log arrival notifications for the 
    Triggered Events engine so actions can be taken immediately.

.. caution:: The Intake Server is a critical element of Kaizen.  It is the main entry point 
    where building data enters the system.  It is also the collection devices' primary point-of-contact.

Key Benefits
------------

* Maps BAS building data to Kaizen buildings and stores it into the primary database.
* Allows 1:1, 1:M, M:1, and M:N mappings of collection device data to buildings. [see Device Settings]
* Handles all incoming building data.
* Data may arrive out-of-order;
* Data may be repeated – allowing updates to prior data.
* Ignores unlicensed data - i.e. unknown data sources, unknown locations, & unknown TL ranges.
* Shields the collection devices from Kaizen's storage internals.
* Shields Kaizen storage internals (database & engines) from the collection devices and bad data.

Constraints
-----------

* Expect up to 1000 different collection devices to send data (to a Kaizen cluster)
* Expect inflow rates of 1000's of msg/sec.
* Incoming TL & Objects are formatted in BSON

+------------------+-----------------------+------------------+
|                  | | Inflow              | Processing       |
|                  | | (2016-02-11)        | (measured max.)  |
+------------------+-----------------------+------------------+
| Trends           | 150/s, peak 300/s     | 2000/s           |
+------------------+-----------------------+------------------+
| Objects          | 60/s, peak 75/s       | 2400/s           |
+------------------+-----------------------+------------------+
| Backups          | < 1/s, peak 50/s      | --               |
+------------------+-----------------------+------------------+
| Events           | 2.5/s, peak 7/s       | --               |
+------------------+-----------------------+------------------+

intake_trends
^^^^^^^^^^^^^

* receives incoming trend log sample sets (one or more time-value pairs belonging to the same date). 
* data is required to have been divided-by-date into individual BJON docs by the collection device.
* docs containing more than 1455 fields are discarded; since this exceeds the #of fields that could
  result from a full day of 1-minute samples plus the required meta-data overhead.  This protects 
  Kaizen from overly large day buckets and the excessive load such docs place on the system.
* determines the data's destination building (see: device to building mapping) 
* trend logs are stored in ctTrends::Latest collection
  * *upsert* is used to merge the incoming partial day bucket with the (likely) existing day bucket.
* current data (today or yesterday) causes a TL notification to be posted to the 
  cta.arrival.trendlogs queue - for the benefit of the Triggered Events engine.
  Older data does not generate notifications, to lessen overall system loads, since such data is 
  considered to be *history*, usually a result of a device emptying its backlog, or a user re-sending 
  data to backfill gaps in a trend log.  
* ctTrends::Summary - fields 'first date' and 'last date' are updated for the benefit of the 
  Vault (list of trend logs), and TLViewer.
  
.. caution:: Deeper validation of the trend samples (samples in ascending order, repeated samples, 
  bad timestamps, bad data types, bad data ranges) is not performed because it is expensive.  The 
  proirity is to empty the queue.  These validations are left to future background tasks.
  

intake_objects
^^^^^^^^^^^^^^

* receives incoming controller (BACnet) Objects
* determines the data's destination building (see: device to building mapping) 
* controller Objects are stored in ctObjects::Latest
  * *save* - is used to overwrite the (likely) existing Object
* ctTrends::Summary - 'name' field is updated if the object is a trend log (TL) or a multi-trend (MT), 
  for the benefit of the Vault (list of trend logs), and TLViewer 

.. note:: Objects are assumed to be key-value pairs of BACnet format (see BACnet spec), but data 
  from non-BACnet BAS controllers is acceptable provided it is also in key-value format.
  
  
intake_backups
^^^^^^^^^^^^^^

* receive incoming controller Device Backups
* determines the data's destination building (see: device to building mapping) 
* controller Backups are stored in ctBackups::Latest


intake_events
^^^^^^^^^^^^^

* receive incoming BACnet Events
* determines the data's destination building (see: device to building mapping) 
* Events are stored in ctEvents::Latest



Requirements
------------

The Intake system is the data entry point for all data entering Kaizen. As such its 
message processing rate must exceed the incoming data rate. In order to determine the required 
processing rate we must first determine a number of other items. From these answers we can then 
state the requirements for the Intake Server.

By how much should the processing rate exceed the incoming data rate?
  Queuing theory states 2x the incoming rate is the minimum to ensure unbounded backlogs do not form. 
  Our processing rate goal should be *4x* the incoming data rate.

.. sidebar:: Clusters

  Kaizen is a cluster of servers.  As CopperTree grows, additional Kaizen clusters will be created; 
  each responsible for its own set of cubes. Reasons for multiple clusters are  
  including scaling, geography, legal jurisdication, & administrative convenience.

What is the incoming data rate?
  The incoming data rate is a function of the amount of data generated by each CopperCube *multiplied* 
  by the number of cubes associated with the Kaizen server cluster. Assume:
   
  * 700 cubes per Intake Server (~1000 buildings)
  * Typical transmission rates [see Collection Devices] of:
    * Trends (typical, 5000 TL over 4 hours): 0.34/s
    * Objects (typical): 1.5/s
  * Events - not significant
  * Backups - not significant

  Inflow= 700 * (0.34/s + 1.5/s) = 1288/s
  
Requirements:

   * handle all data (TL, Objects, Events, & Backups) for 700 cubes
   * have a processing capacity of 2000-2400/s
   * re-map cube data (MAC_Address+Sitename) to Kaizen building (LocationID).

   * data arrives via four RabbitMQ queues (one per data type)
   * data is stored in MongoDB (one database per data type).

   * Incoming TL & Objects are formatted in BSON
   * Incoming Object data overwrites existing Object data
   * Incoming TL data merges with existing data (overwriting duplicated timestamps)
   * validation of data is off-loaded to batch tasks (to improve performance)

   * data packets may arrive out-of-order (within a packet samples at in ascending timestamp order)
   * data may received more than once (either unintentionally, or intentionally to update/correct previously sent data)



Design
------

The primary concern for the Intake Server is performance. As in:
* How many inserts per second can it support?  (Which dictates the #of CopperCubes a Kaizen cluster can support, and
* How does the performance change with growth in data volume & #of CopperCubes sending data?

Goals
^^^^^

Database (MongoDB) Architecture Goals:
* Asynchronous Writes
* Upserts - avoid unnecessary reads & read/write round-trips
* Writes to Replica set Masters. Reads spread over Replica set Slaves.
* Pre-aggregration of data
* Use Natural keys (ObjRef vs. Mongo ObjectID)
* Writes buffered in RAM and bulk written to disk
* Multiple smaller collections - faster than single large collection
* Multiple smaller databases - faster than single large database
* Pre-allocation vs. allowing documents to grow.

Design Choices
^^^^^^^^^^^^^^

Mapping data to building
""""""""""""""""""""""""

Incoming data must be re-mapped from the CopperCube (BACnet-site based) baddressing scheme to 
Kaizen's (Building based) addressing scheme. BACnet identifies TL's by BACnet-device & TL-instance#.
BACnet was never meant to span logical networks (not to be confused with spanning multiple physical 
networks - ethernet, MS/TP - which are joined into a single logical network).
 
CopperCubes may connect to multiple BACnet logical network - called *sites* by convention.
Doing so, results in naming conflicts, as each *site* may overlap the device id range used in 
another *site*.  To distinguish between multiple *sites* (with overlapping device id's) a 
*Sitename* is prepended to all object references.  e.g.
* Device100, TL#5 on network "Site A" becomes "SiteA".100.TL.5
* Device100, TL#5 on network "Site B" becomes "SiteB".100.TL.5

When these site named object references are sent from the CopperCube, they must be re-mapped again, 
as multiple Kaizen clients may be using 'SiteA' & 'SiteB' to identify *sites* within their CopperCubes, 
resulting in conflicts at the Kaizen-level.  For this Kaizen uses unique building (location) ids to 
resolve the conflict.  The **device settings** mapping table is used to re-map data from CopperCube 
to Kaizen buildings.  [see Device Settings t]

.. note::
  Location IDs do not change. New IDs are added only as new buildings are created.

.. note::
  The mapping table is cached for performance.  It is reloaded hourly to balance, time to learn of 
  new buildings vs. time to reload cache.
