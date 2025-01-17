API
=======================

The Dataherald Engine exposes RESTful APIs that can be used to:

* 🔌 Connect to and manage connections to databases
* 🔑 Add context to the engine through scanning the databases, adding database level instructions, adding descriptions to tables and columns and adding golden records
* 🙋‍♀️ Ask natural language questions from the relational data 

Our APIs have resource-oriented URL built around standard HTTP response codes and verbs. The core resources are described below.


Database Connections
------------------------------

The ``database-connections`` object allows you to define connections to your relational data stores. 

Related endpoints are:

* :doc:`Create database connection <api.create_database_connection>` -- ``POST api/v1/database-connections``
* :doc:`List database connections <api.list_database_connections>` -- ``GET api/v1/database-connections``
* :doc:`Update a database connection <api.update_database_connection>` -- ``PUT api/v1/database-connections/{db_connection_id}``

**Database connection resource example:**

.. code-block:: json

    {
        "alias": "string",
        "use_ssh": false,
        "connection_uri": "string",
        "path_to_credentials_file": "string",
        "llm_credentials": {
            "organization_id": "string",
            "api_key": "string"
          },
        "ssh_settings": {
            "db_name": "string",
            "host": "string",
            "username": "string",
            "password": "string",
            "remote_host": "string",
            "remote_db_name": "string",
            "remote_db_password": "string",
            "private_key_password": "string",
            "db_driver": "string"
        }
    }

Responses
------------------
The ``responses`` object is created from the answering natural language questions from the relational data.

The related endpoints are:

* :doc:`add_question <api.question>` -- ``POST api/v1/questions``
* :doc:`add_responses <api.add_responses>` -- ``POST api/v1/responses``
* :doc:`list_responses <api.list_responses>` -- ``GET api/v1/responses``
* :doc:`get_response <api.get_response>` -- ``GET api/v1/responses/{response_id}``

**Response resource example:**

.. code-block:: json

    {
      "question_id": "string",
      "response": "string",
      "intermediate_steps": [
        "string"
      ],
      "sql_query": "string",
      "sql_query_result": {
        "columns": [
          "string"
        ],
        "rows": [
          {}
        ]
      },
      "sql_generation_status": "INVALID",
      "error_message": "string",
      "exec_time": 0,
      "total_tokens": 0,
      "total_cost": 0,
      "confidence_score": 0,
      "created_at": "2023-10-12T16:26:40.951158"
    }

Table Descriptions
---------------------
The ``table-descriptions`` object is used to add context about the tables and columns in the relational database.
These are then used to help the LLM build valid SQL to answer natural language questions.

Related endpoints are:

* :doc:`Scan table description <api.scan_table_description>` -- ``POST api/v1/table-descriptions/sync-schemas``
* :doc:`Update a table description <api.update_table_descriptions>` -- ``PATCH api/v1/table-descriptions/{table_description_id}``
* :doc:`List table description <api.list_table_description>` -- ``GET api/v1/table-descriptions``
* :doc:`Get a description <api.get_table_description>` -- ``GET api/v1/table-descriptions/{table_description_id}``

**Table description resource example:**

.. code-block:: json

    {
        "columns": [{}],
        "db_connection_id": "string",
        "description": "string",
        "examples": [{}],
        "table_name": "string",
        "table_schema": "string"
    }

Database Instructions
---------------------
The ``database-instructions`` object is used to set constraints on the SQL that is generated by the LLM.
These are then used to help the LLM build valid SQL to answer natural language questions based on your business rules.

Related endpoints are:

* :doc:`Add database instructions <api.add_instructions>` -- ``POST api/v1/{db_connection_id}/instructions``
* :doc:`List database instructions <api.list_instructions>` -- ``GET api/v1/{db_connection_id}/instructions``
* :doc:`Update database instructions <api.update_instructions>` -- ``PUT api/v1/{db_connection_id}/instructions/{instruction_id}``
* :doc:`Delete database instructions <api.delete_instructions>` -- ``DELETE api/v1/{db_connection_id}/instructions/{instruction_id}``

**Instruction resource example:**

.. code-block:: json

    {
        "db_connection_id": "string",
        "instruction": "string",
    }


.. toctree::
    :hidden:

    api.create_database_connection
    api.list_database_connections
    api.update_database_connection

    api.scan_table_description
    api.list_table_description
    api.get_table_description
    api.update_table_descriptions

    api.add_instructions
    api.list_instructions
    api.update_instructions
    api.delete_instructions

    api.golden_record

    api.question
    api.list_questions
    api.get_question

    api.add_responses
    api.list_responses
    api.get_response
