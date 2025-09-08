FHIR to OMOP interconversion
============================

Installation
++++++++++++

Follow the below steps to install this utility in your computer.


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

   user@hostname:~/workspace$ cd ehr-qc-2.0/ehr-genomics


Create Python virtual environment
---------------------------------

The Python virtual environment encaptulates all the libraries required for the utility. All the necessary libraries listed in a requirements.txt file that can be found at the root of the repository. Below are the instructions to create and install dependancies in the Python virtual environment.

.. note::
   The utility requires Python version 3.9 or higher. For installing Python, please refer the below link: https://www.python.org/downloads/


Create virtual environment
~~~~~~~~~~~~~~~~~~~~~~~~~~

Inside the opened directory, create a new Python virtual enviroment to conveniently manage all the required dependencies.

.. code-block:: console

   user@hostname:~/workspace/ehr-qc-2.0/ehr-genomics$ python3 -m venv .venv


Activate virtual environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After creating the Python virtual enviroment, activate the virtual enviroment to start using it for subsequent commands. The prompt will change with ``(.venv)`` appearing in front of it as shown below;

.. code-block:: console

   user@hostname:~/workspace/ehr-qc-2.0/ehr-genomics$ source .venv/bin/activate
   (.venv) user@hostname:~/workspace/ehr-qc-2.0/ehr-genomics$


Install dependencies
~~~~~~~~~~~~~~~~~~~~

Install all the required dependencies listed in the requirements.txt file in the newly created Python virtual environment.

.. code-block:: console

   (.venv) user@hostname:~/workspace/ehr-qc-2.0/ehr-genomics$ pip install -r requirements.txt


Verify
------

Verify the installation by running the following command. The expected output should contain ``EHR-Genomics <version number>``.

.. code-block:: console

   (.venv) user@hostname:~/workspace/ehr-qc-2.0/ehr-genomics$ python -m EHR-Genomics -v
   EHR-Genomics 1.0

Run
+++

Use the below command to run the utility. This will run it based on the workflow setup in the configuration file.

For instructions on seting up configuration files, please refer to this `link https://ehr-int-tutorial.readthedocs.io/en/latest/pathogene_on_fhir.html#run-config`_

.. code-block:: console

   (.venv) user@hostname:~/workspace/ehr-qc-2.0/ehr-genomics$ python src/Run.py


Config
++++++

This utility uses two configuration files

* App Config
* Run Config

App Config
----------

The App Config file contains app level customisations including FHIR server and database server details;

FHIR Server Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~

The following FHIR server connection details needs to be updated in the configuration file to perfrm CRUD operations on the FHIR resources. An outline of the configuration for specifying the FHIR server is given below;

.. code-block:: json

    fhir_server_base_url: '<FHIR Server Base URL>'

.. note::

   There are multiple test servers avaialble freely online to get started with. These servers are listed in this link: https://confluence.hl7.org/display/FHIR/Public+Test+Servers. You can use one of these servers if you do not have access for your own FHIR server.

An example configuration using one of the public FHIR server is given below;

.. code-block:: json

    fhir_server_base_url: 'https://hapi.fhir.org/baseR5/'

Here in this case, I have selected a public HAPI-FHIR R5 server.

Database Server Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following database server connection details needs to be updated in the configuration file to perform CRUD operations on the OMOP-CDM standard schema;

.. code-block:: json

    # database connection details
    db_details = {
        "sql_host_name": '<Host Name>',
        "sql_port_number": '<Port Number>',
        "sql_user_name": '<User Name>',
        "sql_password": '<Password>',
        "sql_db_name": '<DB Name>',
    }

Run Config
----------

The Run Config file contains step-by-step instructions on how to run this application.

It can perform 2 different functionalities;

* OMOP-to-FHIR
* FHIR-to-OMOP

OMOP-to-FHIR
~~~~~~~~~~~~

Migrate Bolock
##############

In this functionality, the data is read from the OMOP-CDM standard schema and ingested in to the FHIR server.

The root tag for this functionality is ``run_config_omop_to_fhir`` in the configuration file.

There can be multiple configuration blocks under the parent tag. An outline of a configuration block for migrating patient entity is given below;

.. code-block:: json

    # Migrating ``patient`` entity
    {
        'entity': 'Patient',
        'type': 'migrate',
        'sqlFilePath': '<Extraction SQL File Path>',
        'jsonTemplatePath': '<FHIR Resource JSON Template File Path>',
        'json_sql_mapping': {
            '<SQL outout column name 1>': '<FHIR JSON tag name 1>',
            '<SQL outout column name 2>': '<FHIR JSON tag name 2>',
        },
        'readFromDb': <True/False>,
        'loadToFHIR': <True/False>,
        'save': <True/False>,
        'savePath': '<Save Path>',
    },

Please refer the table below for information on the configuration fields.

+----------------------+--------------------------------------------------------------------------------------------------------------+
| Configuration Field  | Field Details                                                                                                |
+======================+==============================================================================================================+
| entity               | Identifier string for the migrating entity                                                                   |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| type                 | Type of operation performed (``migrate`` for migration configuration)                                        |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| sqlFilePath          | Path to the file containing entity extraction SQL query                                                      |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| jsonTemplatePath     | Path to the file containing FHIR resource JSON template                                                      |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| json_sql_mapping     | Mapping SQL output columns to FHIR JSON tags                                                                 |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| readFromDb           | A Boolean value indicating if the data needs to be read from DB (<``True/False``>)                           |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| loadToFHIR           | A Boolean value indicating if the data needs to be persisted to FHIR (<``True/False``>)                      |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| save                 | A Boolean value indicating if the intermediate files data needs to be saved to a directory (<``True/False``>)|
+----------------------+--------------------------------------------------------------------------------------------------------------+
| savePath             | Path to the directory where the intermediate files are saved                                                 |
+----------------------+--------------------------------------------------------------------------------------------------------------+

.. note::
   To map sub-tags, you need to provide the complete path from the root tag down to the sub-tag you're interested in. This path is created by concatenating all the tags within the hierarchy, separated by a double pipe symbol (``||``).
   
   For example, imagine you have a hierarchy like this:
   
   Root Tag

      Level 1 Tag 1

         Sub-tag A

         Sub-tag B

      Level 1 Tag 2

   To map ``Sub-tag A``, you would provide the path: ``Root Tag||Level 1 Tag 1||Sub-tag A``

An example configuration block for ``migrate`` functionality is given below;

.. code-block:: json

    # Migrating "patient" entity
    {
        'entity': 'Patient',
        'type': 'migrate',
        'sqlFilePath': os.environ['GENOMIC_DATA_LOCATION'] + '/templates/sql/Person.sql',
        'jsonTemplatePath': os.environ['GENOMIC_DATA_LOCATION'] + '/templates/fhir/Patient.json',
        'json_sql_mapping': {
            'id': 'id',
            'sex': 'gender',
            'patient_name': 'name||text',
        },
        'readFromDb': True,
        'loadToFHIR': True,
        'save': True,
        'savePath': os.environ['INTERMEDIATE_DATA_LOCATION'] + '/data/omop_to_fhir/patient',
    },

.. note::

   As part of the codebase, we provided the FHIR resource templates for some commonly used resources including Organization, Patient, Encounter, Observation, and RiskAssessment resources. Additionally, we also provided the extraction logic (SQL query files) for these resources from the standard OMOP-CDM schema. Futher, the pipeline provides the flexibility for the user to extract data from any schema and persist to any FHIR resource as they desire.

Execute Bolock
##############

In addition to ``migrate`` blocks within configuration sections, you can also define ``execute`` blocks. These ``execute`` blocks allow users to run a specified code snippet when encountered. The primary purpose is to facilitate pre-processing code execution before migrating entities.

For instance, consider ingesting data into the ``RiskAssessment`` resource. If risk scores are fetched from a machine learning API endpoint, an ``execute`` block can be used to collect these scores for all patients in the migrating cohort. This code would be executed before running the ``migrate`` block responsible for loading the resource.

The execute block contains the following tags;

.. code-block:: json

    # Execute block structure
    {
        'type': 'execute',
        'function': <Name of a fucntion to execute>,
        'args': {
            '<function param 1>': '<value 1>',
            '<function param 2>': '<value 2>'
            }
    },

Please refer the table below for information on the configuration fields.

+----------------------+----------------------------------------------------------------------+
| Configuration Field  | Field Details                                                        |
+----------------------+----------------------------------------------------------------------+
| type                 | Type of operation performed (``execute`` for execute configuration)  |
+----------------------+----------------------------------------------------------------------+
| function             | Name of the fucntion to execute                                      |
+----------------------+----------------------------------------------------------------------+
| args                 | a list of arguments and their values to be passed to the function    |
+----------------------+----------------------------------------------------------------------+

An example configuration block for ``execute`` functionality is given below;

.. code-block:: json

   # Execute block structure
    {
        'type': 'execute',
        'function': importRiskScores,
        'args': {
            'risk_scores_file': os.environ['EHR_DATA_LOCATION'] + '/wb_30_wa_3.csv',
            'description': {
                "Model": "XGBoost Ensemble",
                "Window Before": "30 days",
                "Window After": "3 day",
                "Target": "Mortality",
                "Target Time": "7 day",
                }
            }
    },


.. note::

   The function has to be either defined in the configuration file or imported from elsewhere such that it is visible for execution.


FHIR-to-OMOP
~~~~~~~~~~~~

In this functionality, the data is read from the FHIR server and ingested in to the OMOP-CDM standard schema.

The root tag for this functionality is ``run_config_fhir_to_omop`` in the configuration file.

There can be multiple configuration blocks under the parent tag. An outline of a configuration block for migrating FHIR data to OMOP-CDM standard schema is given below;

.. code-block:: json

    # Migrating ``Patient`` entity
    {
        'entity':'Patient',
        'type': 'migrate',
        'urlQueryStringPath':'<Extraction URL Query String File Path>',
        'sqlFilePath':'<Insert SQL query file Path>',
        'sql_json_mapping': {
            '<FHIR JSON tag name 1>': '<SQL outout column name 1>',
            '<FHIR JSON tag name 2>': '<SQL outout column name 2>',
        }
    },

Please refer the table below for information on the configuration fields.

+----------------------+--------------------------------------------------------------------------------------------------------------+
| Configuration Field  | Field Details                                                                                                |
+======================+==============================================================================================================+
| entity               | Identifier string for the migrating entity                                                                   |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| type                 | Type of operation performed (``migrate`` for migration configuration)                                        |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| urlQueryStringPath   | Path to the file containing FHIR resource extraction Query String URL                                        |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| sqlFilePath          | Path to the file containing entity insertion query                                                           |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| sql_json_mapping     | Mapping FHIR JSON tags to SQL output columns                                                                 |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| readFromFHIR         | A Boolean value indicating if the data needs to be read from FHIR (<``True/False``>)                         |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| loadToDB             | A Boolean value indicating if the data needs to be persisted to DB (<``True/False``>)                        |
+----------------------+--------------------------------------------------------------------------------------------------------------+
| save                 | A Boolean value indicating if the intermediate files data needs to be saved to a directory (<``True/False``>)|
+----------------------+--------------------------------------------------------------------------------------------------------------+
| savePath             | Path to the directory where the intermediate files are saved                                                 |
+----------------------+--------------------------------------------------------------------------------------------------------------+

An example configuration block for ``migrate`` functionality is given below;

.. code-block:: json

    # Migrating "patient" entity
    {
        'type': 'migrate',
        'index_file': os.environ['GENOMIC_DATA_LOCATION'] + '/index.csv',
        'jsonTemplatePath': os.environ['GENOMIC_DATA_LOCATION'] + '/templates/MolecularSequence.json',
        'save': True,
        'savePath': os.environ['INTERMEDIATE_DATA_LOCATION'] + '/data/gtf_to_fhir/molecular_sequence',
    },

.. note::

   To map sub-tags, you need to provide the complete path from the root tag down to the sub-tag you're interested in. This path is created by concatenating all the tags within the hierarchy, separated by a double pipe symbol (``||``).
   
   For example, imagine you have a hierarchy like this:
   
   Root Tag

      Level 1 Tag 1

         Sub-tag A

         Sub-tag B

      Level 1 Tag 2

   To map ``Sub-tag A``, you would provide the path: ``Root Tag||Level 1 Tag 1||Sub-tag A``

An example configuration block for ``migrate`` functionality is given below;

.. code-block:: json

    # Migrating "patient" entity
    {
        'entity':'Observation',
        'type': 'migrate',
        'urlQueryStringPath': os.environ['GENOMIC_DATA_LOCATION'] + '/templates/fhir/Patient.json',
        'sqlFilePath': os.environ['GENOMIC_DATA_LOCATION'] + '/templates/sql/insert/Person.sql',
        'sql_json_mapping': {
            'id': 'id',
            'value_as_number': 'valueQuantity||value',
            'measurement_concept_id': 'code||coding||0||code',
            'unit_concept_id': 'valueQuantity||unit',
            'person_id': 'subject||reference',
            'visit_occurrence_id': 'encounter||reference',
            'measurement_datetime': 'effectiveDateTime',
        }
    },

.. note::

   As part of the codebase, we provided the FHIR resource templates for some commonly used resources including Organization, Patient, Encounter, Observation, and RiskAssessment resources. Additionally, we also provided the insertion logic (SQL query files) to persist these resources in the standard OMOP-CDM schema. Futher, the pipeline provides the flexibility for the user to extract data from FHIR resource and persist to any OMOP-CDM entity as they desire.
