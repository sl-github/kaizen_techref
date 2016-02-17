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

* :ref:`Overview` - Topics of a general nature, important to basic understanding of Kaizen
* :ref:`acquire` - Acquisition of raw building data
* :ref:`analyze` - 
* :ref:`advise` - 
* :ref:`Security` -
* :ref:`gui` -
* :ref:`api` - 


Introduction
============

.. toctree::
    :maxdepth: 3
    
    preface
    outline
    license
    contact


.. toctree::
    :maxdepth: 3
    :caption: Overview

    part1/concepts


.. _acquire:

.. toctree::
    :maxdepth: 3
    :caption: Data Acquisition

    part2/devices
    part2/intake_queues
    part2/intake_servers


.. _analyze:

.. toctree::
    :maxdepth: 3
    :caption: Data Analysis


.. _advise:

.. toctree::
    :maxdepth: 3
    :caption: User Advisement


.. _security:

.. toctree::
    :maxdepth: 3
    :caption: Security Issues


.. _gui:

.. toctree::
    :maxdepth: 3
    :caption: User Interface


.. _api:

.. toctree::
    :maxdepth: 3
    :caption: Programing Interface


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
