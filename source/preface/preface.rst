Preface
-------

All products embody the visions of their designers, the requirements of the problem they 
attempt to solve, and the constraints of the environment in which they operate.  
Software is a long-lived, continuously evolving product, and over time the vision and 
requirements of its many features become scattered or lost.  All that remains is the 
software itself, and stories of how it came to be what it is now.    This is a problem 
for those (developers, QA) now working on the product, as soon there is no single 
authoritative source that contains all these requirements and explains the both the 
vision for each software component and how “the pieces all fit together”.

This document aims to be this authoritative source for Kaizen.  It compiles the product's 
vision & requirements from a number of sources (wiki, Jira, Aha, human memories), 
in an attempt to explain how “the pieces all fit together”.  It acts as both a guide book 
and a reference manual.  It allows readers to compare “what is” against “what should be”.  
As such it is equally useful for QA as for Developers.

Kaizen is built upon the model of “Acquire – Analyze – Advise”.  
[In future, “Action” will be added, taking its place in completing the loop.]  
This document follows the model to structure its content.  Features are ordered in the 
sequence that data flows through the system – from BAS (Building Automation System) to 
CopperCube and Kaizen Intake & Database [Acquire], through the analytic engines [Analyze], 
and out to the end users via reports [Advise].

Shared components (like the database, and messaging system) are not detailed as monolithic 
entities, but as a collection of individual items (tables, collections, queues) that can 
described separately.  These items are detailed in the feature section where data first 
enters them, and referenced again from the feature section(s) that consume the data.  
Such items are the glue that join different features.  The fact that they reside together 
(in one database server or message broker) is an implementation detail, not a fundamental 
attribute of the item or of Kaizen's system design.
