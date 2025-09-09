EHR-QC Web
==========

This project provides a web-based interface for the EHR-QC utility, making all standardisation and preprocessing steps accessible through a user-friendly web UI.

Deploy your own Web Server
++++++++++++++++++++++++++

To get a copy of the EHR-QC Web Server up and running in a Linux manchine, follow these simple example steps;

Login
------

Upon login to a system and opening a terminal, the following prompt should appear, where the ``user`` is the user name and the ``hostname`` is the hostname of the system.

.. code-block:: console

   user@hostname:~$


Change directory
----------------

From the home directory which will be open by default, change to a suitable directory on your computer where the utility needs to be installed. For example, in this tutorial we have changed to ``workspace`` directory.

.. code-block:: console

   user@hostname:~$ cd workspace


Clone
-----

In a destination folder, clone current version of the repository from GitHub.

.. code-block:: console

   user@hostname:~/workspace$ git clone https://gitlab.com/tyagilab/ehr-qc-2.0.git


Open the directory
------------------

Navigate to the utility path after cloning the repository from GitHub.

.. code-block:: console

   user@hostname:~/workspace$ cd ehr-qc-2.0/ehrqc-web

Update the submodules
---------------------

.. code-block:: console

   user@hostname:~/workspace/ehrqc-web$ git submodule init && git submodule update

Build the docker container
--------------------------

.. code-block:: console

   user@hostname:~/workspace/ehrqc-web$ docker compose build


Start the docker container
--------------------------

.. code-block:: console

   user@hostname:~/workspace/ehrqc-web$ docker compose up -d


Stop the docker container
-------------------------

.. code-block:: console

   user@hostname:~/workspace/ehrqc-web$ docker compose down


Usage
+++++


Standardise
-----------

Data Standardisation Process is shown in the image below;

The data standardisation step begins by importing and configuring CSV files. Users are prompted to:

1. **Import Configurations** – Upload predefined configuration files or manually set up the schema.

2. **Select Required Tables** – Choose relevant datasets such as Patient, Labevents, Admissions, Chart Events, and Diagnosis.

3. **Column Mapping** – Map the columns from the imported CSVs to standardised fields expected by the EHR-QC pipeline.

4. **Prepopulate Option** – Use the Prepopulate button to automatically fill in default mappings for commonly used datasets.

5. **Navigation** – Move between steps (e.g., Back and Next) to progress through the pipeline until the data is ready for ETL (Extract, Transform, Load).

This process ensures raw clinical data is harmonised into a standardised OMOP-CSM format, enabling reliable downstream quality checks, and preprocessing.

.. image:: ./images/standardise.gif
   Figure 1: Standardisation Process


Preprocessing
-------------

Data Extraction Process is shown in the image below;

Once the data has been standardised, the extraction step enables users to retrieve specific subsets of data for further analysis. The process includes:

Upload SQL Files – Users can upload their own SQL scripts to define the exact data they wish to extract.

Predefined Queries – Alternatively, a dropdown menu provides access to ready-made SQL queries, such as example lab extractions.

Run Extraction – After selecting or uploading a query, the Run Extract button executes the process, extracting the requested dataset from the standardised database.

This step provides flexibility by supporting both custom SQL and preconfigured queries, making it easier for users to tailor data extraction to their research or quality control needs.


Extract Data
~~~~~~~~~~~~

.. image:: ./images/extract.gif


Data Coverage
~~~~~~~~~~~~~

.. image:: ./images/data_coverage.gif


Impute
~~~~~~

.. image:: ./images/impute.gif


Outlier
~~~~~~~

.. image:: ./images/outlier.gif


Visualisations
--------------

.. image:: ./images/graph.gif

