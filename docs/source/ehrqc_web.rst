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


Data Standardisation Process is shown in Figure 1;

The data standardisation step begins by importing and configuring CSV files. Users are prompted to:

1. **Import Configurations** – Upload predefined configuration files or manually set up the schema.

2. **Select Required Tables** – Choose relevant datasets such as Patient, Labevents, Admissions, Chart Events, and Diagnosis.

3. **Column Mapping** – Map the columns from the imported CSVs to standardised fields expected by the EHR-QC pipeline.

4. **Prepopulate Option** – Use the Prepopulate button to automatically fill in default mappings for commonly used datasets.

5. **Navigation** – Move between steps (e.g., Back and Next) to progress through the pipeline until the data is ready for ETL (Extract, Transform, Load).

This process ensures raw clinical data is harmonised into a standardised OMOP-CSM format, enabling reliable downstream quality checks, and preprocessing.

.. figure:: ./images/standardise.gif

   Figure 1: Standardisation Process


Preprocessing
-------------


Extract Data
~~~~~~~~~~~~

Data Extraction Process is shown in Figure 2;

Once the data has been standardised, the extraction step enables users to retrieve specific subsets of data for further analysis. The process includes:

1. **Upload SQL Files** – Users can upload their own SQL scripts to customise the data extraction process.

2. **Predefined Queries** – Alternatively, a dropdown menu provides access to a set of pre-built SQL queries which can be made use of.

3. **Run Extraction** – After selecting or uploading a query, the Run Extract button executes the process, extracting the requested dataset from the standardised database.

This step provides flexibility by supporting both custom SQL and pre-built queries, making it easier for users to tailor data extraction to their analytical or research needs.

.. figure:: ./images/extract.gif

   Figure 2: Data Extraction Process


Data Coverage Report Generation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Data Coverage Report Generation Process is shown in Figure 3;

The Data Coverage Analysis step evaluates the completeness of imported datasets by identifying missing values across columns. The process works as follows:

1. **Upload File** – Users provide the standardised CSV file that needs to be analysed.

2. **Chunk Size Configuration** – Define the number of rows processed per batch (default: 10,000) to efficiently handle large datasets.

3. **ID Columns Selection** – Specify identifier columns (e.g., patient IDs, admission IDs) used to group data when calculating missing percentages.

4. **Threshold Setting** – Enter a percentage threshold (e.g., 30%) to filter columns based on their level of missingness.

5. **Run Analysis** – By clicking Run Data Coverage Analysis, the system processes the file and generates a summary, automatically dropping columns with missing values above the defined threshold.

The resulting report highlights the coverage of each variable, helping users assess data quality and decide which features are suitable for downstream analysis.

.. figure:: ./images/data_coverage.gif

   Figure 3: Data Coverage Report Generation


Impute Missing Values
~~~~~~~~~~~~~~~~~~~~~

Missing Value Imputation Process is shown in Figure 4;

The Impute step addresses missing data by filling in the gaps. The workflow is as follows:

1. **Upload File** – Users begin by uploading the standardised dataset that contains missing values.

2. **Select Action** – From the Actions dropdown, users choose the desired imputation strategy. Options may include single imputation methods (e.g., mean, median, mode), advanced machine learning–based techniques, or algorithm comparison.

3. **Specify ID Columns** – Identify the columns that serve as unique identifiers (such as patient IDs or encounter IDs) to ensure imputations are applied correctly across grouped records.

4. **Run Imputation** – By clicking Run Impute, the system processes the dataset, applies the selected method, and fills in missing values.

5. **Model Evaluation (Optional)** – If multiple algorithms are compared, their performance is assessed using R² scores, allowing users to select the most effective method for their dataset.

This imputation step ensures complete datasets for machine learning purposes.

.. figure:: ./images/impute.gif

   Figure 4: Impute Missing Values


Outlier Handling
~~~~~~~~~~~~~~~~

Outlier Detection and Correction Process is shown in Figure 5;

The Outliers step focuses on identifying and handling extreme or anomalous values that may distort analysis results. The process includes:

1. **Upload File** – Users begin by uploading the dataset that potentially contain outliers.

2. **Select Action** – From the Actions dropdown, users can choose an action - Clean or Visualise.

3. **Specify ID Columns** – Define which columns to group by while detecting outliers. This ensures that calculations are applied correctly across grouped entities such as patients, visits, or lab events.

4. **Run the Process** – Clicking Run Outliers initiates the process. The system detects unusual values in an un-supervised manner.

5. **Visualisation (Optional)** – Users can generate plots to visually inspect distributions before and after outlier correction.

By detecting and correcting anomalies, this step improves dataset quality and ensures more reliable modeling of the data.

.. figure:: ./images/outlier.gif

   Figure 5: Detect and Remove Outliers


Interactive Visualisation
-------------------------

Data Visualisation Process is shown in Figure 6;

The Graphing step enables users to create visual representations of their processed data, helping to explore trends, patterns, and anomalies. The process works as follows:

1. **Upload File** – Users provide a standardised and cleaned dataset to be visualised.

2. **Select Graph Type** – From the dropdown menu, users can choose different visualisation options depending on the data and their analysis needs.

3. **Define ID Columns** – Specify identifier columns (such as patient IDs or admission IDs) to group the data by.

4. **Generate Visualisation** – Clicking Run Graphing produces the selected plot.

5. **Iterative Exploration** – Users can repeat the process with different graph types or columns.

This step provides an intuitive way to explore data and communicate results effectively through clear, interpretable graphics.

.. figure:: ./images/graph.gif

   Figure 6: Interactive Visualisation
