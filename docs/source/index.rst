EHR-QC-2.0
==========

**EHR-QC 2.0** is an enhanced version of the EHR-QC pipeline for preparing 
Electronic Health Record (EHR) and linked genomic data for machine learning 
and clinical outcome modeling. It introduces three major improvements:

- **LLM-driven terminology mapping** – Leverages large language models (LLM) to perform 
  accurate, semantic standardisation of biomedical concepts.
- **Expanded interoperability** – Supports FHIR (HL7) standard for seamless data exchange.
- **Web-based interface** – Provides a web-based UI reducing reliance on command-line operations.

Together, these updates enable fully automated workflows, robust interoperability, and improved accessibility making EHR-QC 2.0 a more powerful and user-friendly tool for healthcare data analytics.

The tool is available at: `http://ehrqc.tsonika-lab.cloud.edu.au <http://ehrqc.tsonika-lab.cloud.edu.au>`_


.. note::

   In this documentation, we provide detailed usage information about the enhancements that are part of EHR-QC-2.0 release. The usage instructions for the original version of EHR-QC can be found here `https://ehr-qc-tutorials.readthedocs.io/ <https://ehr-qc-tutorials.readthedocs.io/>`_


.. note::

   This project is under active development.

.. topic:: Acknowledgements

   .. image:: images/monash.png
      :width: 20 %

   .. image:: images/alfred.png
      :width: 20 %

   .. image:: images/superbugai.png
      :width: 20 %

   .. image:: images/rmit.png
      :width: 20 %

Contents
--------

.. toctree::
   :maxdepth: 1

   onto_map_llm
   omop_fhir
   ehrqc_web
