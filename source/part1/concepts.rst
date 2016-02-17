Concepts
========

Trend log:
    - fundamental data type
    - basis of all Kaizen analysis
    - time-series data – consisting on timestamp-value pairs

(Controller)(BACnet) Object
    - meta data describing the trend logs
    - heterogeneous key-value documents

Building:
    - basic unit for grouping trends and objects
    - represents the physical and logical ways that the BAS data is grouped.
    - Buildings are assigned unique id's.  These id's are used throughout Kaizen to 
      keep data separated as Kaizen is a mulch-tenant information system.

Client:
    - buildings are owned by companies or organizations.
    - Provides a higher-level means to group buildings.
    - Clients in turn may also be sub-clients of other clients.

Delegated Administration:
    A means to allow administration tasks to be off-loaded (delegated) to sub-clients, 
    in order to reduce (spread) the administration burden.
    Creates a hierarchy of administrators.

(HVAC) Systems:
    HVAC equipment – Air Handlers, VAV units, Boilers, Fan-coils – are called systems.
    Systems may also span (or connect to) multiple pieces of equipment.  
    Examples: the Hot-Water System, the Supply-Air System, the Exhaust System.
    Systems are a means to group sub-sets of data within a building.
