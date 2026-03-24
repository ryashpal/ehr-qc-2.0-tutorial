Case Study
==========

This update introduces several key capabilities, including reliable automated concept mapping, an user-friendly interface for standardisation, and real-time synchronisation of Electronic Health Record (EHR) data with FHIR. To demonstrate these features, we conducted a case study using a subset of publicly available EHR data, following the steps outlined below.

The dataset comprises 193,541 hourly observation records collected from 989 patients across 1000 hospital episodes.

Concept Mapping
+++++++++++++++

Using ontomapllm, clinical concepts were mapped to the SNOMED vocabulary. These mappings were then used in the subsequent standardisation process.

Data Standardisation
++++++++++++++++++++

The dataset was standardised to the OMOP Common Data Model (OMOP-CDM) using the EHR-QC standardisation module, leveraging the mapped SNOMED concepts.


Data Extraction and Quality Assessment
++++++++++++++++++++++++++++++++++++++

Standardised entities were extracted from the OMOP-CDM. A missing data report was then generated through the EHR-QC-Web portal to assess data completeness.

Missing Data Handling
+++++++++++++++++++++

Missing values were handled using the imputation functionality available within the EHR-QC-Web platform.

Outlier Analysis
++++++++++++++++

Outliers in the dataset were identified and explored to ensure data quality and consistency.

FHIR Conversion and Ingestion
+++++++++++++++++++++++++++++

Finally, the processed dataset was converted to FHIR format and ingested into a locally deployed FHIR server using a FHIR–OMOP interconversion utility.
