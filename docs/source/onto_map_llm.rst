Ontology Mapping using LLM: ONTOMAPPLLM
========================================


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

   user@hostname:~/workspace$ git clone https://gitlab.com/tyagilab/ontomapllm.git


Open the directory
------------------

Navigate to the utility path after cloning the repository from GitHub.

.. code-block:: console

   user@hostname:~/workspace$ cd ontomapllm


Create Python virtual environment
---------------------------------

The Python virtual environment encaptulates all the libraries required for the utility. All the necessary libraries listed in a requirements.txt file that can be found at the root of the repository. Below are the instructions to create and install dependancies in the Python virtual environment.

.. note::
   The utility requires Python version 3.9 or higher. For installing Python, please refer the below link: https://www.python.org/downloads/


Create virtual environment
~~~~~~~~~~~~~~~~~~~~~~~~~~

Inside the opened directory, create a new Python virtual enviroment to conveniently manage all the required dependencies.

.. code-block:: console

   user@hostname:~/workspace/ontomapllm$ python3 -m venv .venv


Activate virtual environment
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After creating the Python virtual enviroment, activate the virtual enviroment to start using it for subsequent commands. The prompt will change with ``(.venv)`` appearing in front of it as shown below;

.. code-block:: console

   user@hostname:~/workspace/ontomapllm$ source .venv/bin/activate
   (.venv) user@hostname:~/workspace/ontomapllm$


Install dependencies
~~~~~~~~~~~~~~~~~~~~

Install all the required dependencies listed in the requirements.txt file in the newly created Python virtual environment.

.. code-block:: console

   (.venv) user@hostname:~/workspace/ontomapllm$ pip install -r requirements.txt

Project Structure
+++++++++++++++++

Apps directory currently contains 5 files:

- A common.py file which holds objects used between app files (i.e Embedding Model)
- An arg parser for CLI use. It is currently easier to integrate into a common file between bash scrips. This may change as the programs require so.
- build.py is used for creating a new or overwrite an old vector database
- lookup.py allows the user to convert a whole ontology using a vector look up or the functionality to perform a single item search
- llm_query.py provides prompts to filter egregious matches and filter out potential non-matches 

General Args
++++++++++++

- model_name accepts and argument or path containing a huggingface encoding model
- overwrite_db if present overrwites the previous DB at the same location
- use_gpu enables gpu use if avaialable
- hf_model is used to elect which LLM model is used. Default = m42-v2 LLama 3.1 8B
- top_k - number of items to query, default is 5. LLM model works best at 5 with max_prompts at 5.
- dtype should be left at float16
A sample end to end conversion is found in ./docs.sh

Add your Ontology
+++++++++++++++++

To build a new ontology use the build.py script.

Specify a new file location to host the new DB. Specifying an existing location will overwrite the previous DB.

The current file supports TSV and CSV files.

The user must also specify the following:

- Column in CSV file that contains the unique concept identifiers (CUI/Codes)
- Column in CSV file that contains the concept label

For example:\
CUI	Label	Domain	Entity_Type\
987	XYZ	Condition	Disease\

- code_field "CUI" \
- label_field "Label" \


Sample Build
------------


.. code-block:: console

    python apps/build_db.py \
    --comparator_ontology_files "$ONTO_A" \
    --comparator_file_format tsv \
    --code_field "$CODE_COL" \
    --label_field "$LABEL_COL" \
    --model_name "$EMB_MODEL" \
    --db_location "$MAIN_DB" \
    --overwrite_db \
    --use_gpu

Semantic Concept Matching
+++++++++++++++++++++++++

The current file supports TSV and CSV files.

The user must also specify the following:

- Column in CSV file that contains the unique concept identifiers (CUI/Codes)
- Column in CSV file that contains the concept label

For example:\
CUI	Label	Domain	Entity_Type\
123	ABC	Condition	Disease\

- code_field "CUI" \
- label_field "Label" \

Sample Search
-------------


.. code-block:: console

    python apps/lookup.py \
    --db_location "$MAIN_DB" \
    --model_name "$EMB_MODEL" \
    --input_ontology_files "$ONTO_B" \
    --input_file_format tsv \
    --code_field "$CODE_COL" \
    --label_field "$LABEL_COL" \
    --top_k 5 \
    --output_file results_top5.csv \
    --use_gpu


LLM Filtering
+++++++++++++


Sample LLM Query
----------------

.. code-block:: console

    python apps/llm_query.py \
    --csv_file results_top5.csv \
    --output_csv combined_results.csv \
    --hf_model "$LLM_MODEL" \
    --threshold 0.90 \
    --max_prompts 1 \
    --dtype bfloat16

.. note::

    The vector DB process currently works the smoothest when the vector DB is controlled. The same encoding model should be used for both storing and retrieving from the vector database. Currently BERT models from huggingface and the encoding arm for T5 models are supported with the best performance in "Self-Aligned" clinically trained BERT models.

.. note::

    The current pipeline is equipped to use m42-llama 3.1 or llama 3.1 from huggingface.
