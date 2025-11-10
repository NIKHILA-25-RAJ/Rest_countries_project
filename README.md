ETL Pipeline with AWS Lambda, Kafka, AWS Glue, S3, Snowflake, and Power BI
--------------------------------------------------------------------------
Overview
--------
This project implements an ETL (Extract, Transform, Load) pipeline utilizing AWS Lambda, Apache Kafka (on EC2), Apache Spark on AWS Glue, AWS S3, Snowflake, and Power BI.
The pipeline ingests data from the REST Countries API, processes and transforms it, stores it in the cloud, and enables interactive reporting via Power BI.

Architecture
-------------
1. Extraction (AWS Lambda + REST Countries API)

- AWS Lambda fetches data from the REST Countries API.

- The ingested data is published to a Kafka topic.

2. Streaming (Kafka on AWS EC2)

- Kafka acts as the message broker.

- Ensures reliable, real-time delivery of data to the processing layer.

3. Transformation (AWS Glue with Apache Spark)

- AWS Glue consumes Kafka streams.

- Spark ETL jobs handle cleaning, transformation, and enrichment of data.

4. Storage (AWS S3 + Snowflake)

- Transformed data is written to AWS S3 for scalable storage.

- Data is then ingested into Snowflake for analytics.

5. Visualization (Power BI)

- Power BI connects with Snowflake to build dashboards and reports.

Tech Stack
----------
- Language: Python

- Data Source: REST Countries API

- Ingestion: AWS Lambda

- Messaging: Apache Kafka on EC2

- Processing: Apache Spark (AWS Glue)

- Storage: AWS S3, Snowflake

- Visualization: Power BI

Features
--------
- Automated extraction from REST Countries API

- Real-time streaming with Kafka

- Scalable data transformations with Spark on Glue

- Storage integration with AWS S3 and Snowflake

- End-to-end BI workflow with Power BI

Folder Structure
-----------------
- lambda_functions/        # AWS Lambda scripts
- flask_kafka_producer/    # Kafka producer and consumer code
- Glue_spark_script/       # Spark ETL scripts
- snowflake_integration/   # Snowflake setup and queries
- README.md /              # Project documentation

Airflow automation 
------------------
Airflow Automation: REST Countries ETL

Lightweight Airflow pipeline to fetch country data from the REST Countries API (via a Lambda), publish records to Kafka, transform with Spark, store results in S3, and load into Snowflake.

 Repository layout
 
- Airflow_dag/ — main DAG that orchestrates the workflow and defines tasks and operators.
  - Key symbols: `send_to_kafka`, `invoke_lambda`, `spark_transform`, `load_to_snowflake`, `KAFKA_BROKER`, `KAFKA_TOPIC`
- dataprocessing_lambda/ — Lambda function that fetches and normalizes country data from the REST Countries API.
  - Key symbol: `lambda_handler`
- spark_job/ — PySpark job that reads from Kafka, parses JSON, deduplicates and writes CSV to S3.
  - Key symbol: `clean_df`

How it works (high level)

1. Airflow invokes the Lambda (`lambda_handler`) to fetch country data.
2. The DAG publishes each record to Kafka (`KAFKA_BROKER` / `KAFKA_TOPIC`).
3. A Spark job (`spark_job`) subscribes to the Kafka topic, parses JSON, trims and deduplicates fields, then writes CSV output to S3.
4. Snowflake copies the CSV files from the S3 stage into a target table (configured in the DAG).

Quick start / run notes

- Install Airflow providers: amazon, spark, snowflake.
- Provide AWS credentials to Airflow (connection `aws_default`) so the DAG can invoke Lambda and Spark can access S3.
- Start Kafka and create the configured topic.
- Update the SparkSubmitOperator `application` path in the DAG to point to `spark_job` or your Spark script.
- Configure the Snowflake connection (`snowflake_default`) and S3 stage referenced in the DAG SQL.

Configuration highlights

- AWS credentials: `aws_default` Airflow connection or environment variables for Spark.
- Kafka broker & topic: configured by `KAFKA_BROKER` and `KAFKA_TOPIC`.
- REST API base URL: configurable via `REST_COUNTRIES_API` env var in the Lambda.
- Spark uses `s3a` settings to write CSV to S3.

