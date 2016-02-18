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
-------------

* receives incoming trend log sample sets (one or more time-value pairs belonging to the same date). 
* data is required to have been divided-by-date into individual BJON docs by the collection device.
* docs containing more than 1455 fields are discarded; since this exceeds the #of fields that could
  result from a full day of 1-minute samples plus the required meta-data overhead.  This protects 
  Kaizen from overly large day buckets and the excessive load such docs place on the system.
* determines the data's destination building (see: device to building mapping) 
* trend logs are stored in ctTrends::Latest collection
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
--------------

* receives incoming controller (BACnet) Objects
* determines the data's destination building (see: device to building mapping) 
* controller Objects are stored in ctObjects::Latest
* ctTrends::Summary - 'name' field is updated if the object is a trend log (TL) or a multi-trend (MT), 
  for the benefit of the Vault (list of trend logs), and TLViewer 

.. note:: Objects are assumed to be key-value pairs of BACnet format (see BACnet spec), but data 
  from non-BACnet BAS controllers is acceptable provided it is also in key-value format.
  
  
intake_backups
--------------

* receive incoming controller Device Backups
* determines the data's destination building (see: device to building mapping) 
* controller Backups are stored in ctBackups::Latest


intake_events
-------------

* receive incoming BACnet Events
* determines the data's destination building (see: device to building mapping) 
* Events are stored in ctEvents::Latest
