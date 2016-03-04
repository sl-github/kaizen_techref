.. Kaizen Technical Reference documentation master file, created by
   sphinx-quickstart on Mon Feb 15 16:10:59 2016.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

==========================
Kaizen Technical Reference
==========================




-------------------------------------
A Compendium of Vision & Requirements
-------------------------------------



:Edition: 1
:Date: 2016-02-10

Technical documentation answers the **WHY** questions about a product.  Unlike the code, 
which answers the **HOW** a product works.  Knowing the **HOW** allows you to understand 
(troubleshoot) the product's behaviour.  Knowing the **WHY** allows you to correct, 
maintain, and extend the product.

Technical documentation is geared for the developers of the product.  It captures the 
reasoning behind the design decisions that are embedded within the product.  As such, it is 
also very useful for anyone else wanting to know **why** the product does what it does.
 
Kaizen's technical documentation is a large document divided into major parts.
It should initially be read sequentially (like a guide book) to gain a complete understanding
of Kaizen, and thereafter in random order (like an encyclopedia) to focus on specific topics.
The ordering of the major parts, follows the flow of raw data from building, through analysis, and 
back out to the end-users as knowledge.

The major parts are:

====================  =============================
:ref:`Overview`       Topics of a general nature, important to basic understanding of Kaizen.
:ref:`Architecture`   System Design         
:ref:`acquire`        Acquisition of raw building data.
:ref:`analyze`        Turing raw data into Actionable information (Insights)
:ref:`advise`         Informing Users of the analysis results.
:ref:`Security`       Protection of the System and Data
:ref:`database`       Database Structure 
:ref:`gui`            Interacting with Kaizen via Mouse & Keyboard
:ref:`api`            Interacting with Kaizen via code
:ref:`integration`    Information exchange with other systems.
:ref:`international`  for non-English speakers.
====================  =============================


.. toctree::
    :maxdepth: 3

    outline
    intro
    

.. toctree::
    :maxdepth: 3
    :caption: Overview

    overview/terms


.. toctree::
    :maxdepth: 3
    :caption: Architecture



.. _acquire:

.. toctree::
    :maxdepth: 3
    :caption: Data Acquisition

    acquire/devices
    acquire/intake_queues
    acquire/intake_servers


.. _analyze:

.. toctree::
    :maxdepth: 3
    :caption: Data Analysis


.. _advise:

.. toctree::
    :maxdepth: 3
    :caption: User Advisement


.. toctree::
    :maxdepth: 3
    :caption: Security


.. _database:

.. toctree::
    :maxdepth: 3
    :caption: Data Storage

    database/ctTrends
	

.. _gui:

.. toctree::
    :maxdepth: 3
    :caption: User Interface


.. _api:

.. toctree::
    :maxdepth: 3
    :caption: Programing Interface


.. toctree::
    :maxdepth: 3
    :caption: Integration
    

.. toctree::
    :maxdepth: 3
    :caption: International


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
