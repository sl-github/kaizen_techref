Outline
=======

Part 1 – Overview

- Concepts
    - Trend log, 
    - Object (controller)
    - Building
    - Client-
    - Delegated Admin
    - System (Building, HVAC)

- Goals
- Architecture


Part 2 - Acquire

- Collection Devices
    - CopperCube
    - Trendr
    - Weather Fetcher
        - ctWeather
    - IESO – Ontario Energy $
	
- Intake Queues
    - cta.trends
    - cta.objects
    - cta.backups
    - cta.events

- Intake Servers
    - Device Settings
    - Device Inventory
    - intake_trends
    - intake_objects
    - intake_backups
    - intake_events

- Storage - General
    - ctTrends (Latest, Summary)
    - ctObjects (Latest, Trend Summary[TL names])
    - ctTrash
    - ctBackups
        - Backup Pruning
    - ctEvents

- Device Monitoring
    - Heartbeats
    - ctMonitoring

- Statistics (Summaries)
    - ctStats

Part 3 - Analyze

- Golden Standard
- Integrity
- Logic Flows
- Faulting
- Rules
    - Built-in (...integrity)
    - Provided (CT BAE....)
    - User created
        - Community Library
- Systems
- Scheduled Tasks
- Triggered Tasks
- Calculated Trend Log (CTL)
- Queues
    - cta.bus
    - cta.faulting
    - cta.advising.insight
    - celery
    
- Storage
    - ctAdvising
    - ctCharts
    - ctGoldenStandards
    - ctInsights


Part 4 - Advise

- Reporting
    - Subscriptions
    - Email
        - ctNotifications


Part 5 - Action

- Closing the Loop


Part 6 - Security

Part 7 - User Interface

- Navigation


Part 8 - API (Program) Interface

Part 9 - Integration

- enteliWEB
