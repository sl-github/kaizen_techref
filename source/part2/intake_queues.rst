Intake Queues
=============

The *Collection Devices* send data to Kaizen via Kaizen's *Intake Queues*.  
There are four intake queues, one for each type of data: trends (cta.trendlogs), 
objects (cta.objects), backups (cta.backups), and events (cta.events).  The queues are::

* Password protected - open to any collection device knowing the userid/password [cucube/public  
* SSL enabled – to protect data in transit
* Persisted to server disk – to protect against broker (rabbitmq) crashes
