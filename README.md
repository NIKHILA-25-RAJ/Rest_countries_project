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
