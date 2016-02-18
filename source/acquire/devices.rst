Collection Devices
==================

CopperCube
----------

- Manufactured by Delta Controls Inc.  Carries the CopperTree branding.
- The primary data collection device.
- Sends Heartbeats every 15 minutes.
- Sends Trend log samples every four hours.
- Sends BAS (BACnet) Objects daily.
- Sends BAS Controller Backups – weekly, and each day that they change
- Supports 5000 TL's with 5 yrs of data.
- User may request it to re-send any and all trend log data at any time.
- Transmission Rates:
    - Maximum: 20/s
    - Trends (typical, 5000 TL over 4 hours): 0.34/s
    - Objects (typical): 1.5/s

Trendr
------

- Manufactured by Building Robotics.
- Non-BACnet device sending non-BACnet data, but mimicking sufficient CopperCube behaviour so that Kaizen will accept its Trend and Object data.
- Implemented by one partner as a cheaper CopperCube alternative.
- Transmission Rates:
    - Maximum: 20/s
    - Trends: unknown
    - Objects: N/A


Advantech
---------

- 1st generation CopperCube [a.k.a. the Silverbox].  [obsolete]
- Off-the-shelf embedded PC.  Manufactured by Advantech
- Discontinued due to high rate of Hard Disk failures.
- Transmission Rates:
    - Maximum: 20/s
    - Trends (typical, 5000 TL over 4 hours): 0.34/s
    - Objects (typical): 1.5/s

Weather Fetcher
---------------

- Virtual device (cron tasks running on Kaizen server).
- Two models:
    - Canada Wx - Retrieves weather data (current, historic, forecast) from Environment Canada.
    - USA Wx – retrieves weather data (current, historic) from NOAA
- Non-BACnet device sending non-BACnet data, but mimicking sufficient CopperCube behaviour so that Kaizen will accept its Trend and Object data.
- Runs hourly.
- Database:
    - Configuration [mongoDB] – ctWeather::Stations

IESO (Ontario Energy prices)
----------------------------

- IESO – Independent Electricity System Operator.  Ontario Canada's electricity marketplace.
- Virtual device (cron tasks running on Kaizen server).
- Runs hourly.
- Retrieves – Hourly Ontario Electricity Price (HOEP), and monthly Global Adjustment (GA)

