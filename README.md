# Airflow Automation: REST Countries ETL

Lightweight Airflow pipeline to fetch country data from the REST Countries API (via a Lambda), publish records to Kafka, transform with Spark, store results in S3, and load into Snowflake.

## Repository layout

- Airflow_dag/ — main DAG that orchestrates the workflow and defines tasks and operators.
  - Key symbols: `send_to_kafka`, `invoke_lambda`, `spark_transform`, `load_to_snowflake`, `KAFKA_BROKER`, `KAFKA_TOPIC`
- dataprocessing_lambda/ — Lambda function that fetches and normalizes country data from the REST Countries API.
  - Key symbol: `lambda_handler`
- spark_job/ — PySpark job that reads from Kafka, parses JSON, deduplicates and writes CSV to S3.
  - Key symbol: `clean_df`

## How it works (high level)

1. Airflow invokes the Lambda (`lambda_handler`) to fetch country data.
2. The DAG publishes each record to Kafka (`KAFKA_BROKER` / `KAFKA_TOPIC`).
3. A Spark job (`spark_job`) subscribes to the Kafka topic, parses JSON, trims and deduplicates fields, then writes CSV output to S3.
4. Snowflake copies the CSV files from the S3 stage into a target table (configured in the DAG).

## Quick start / run notes

- Install Airflow providers: amazon, spark, snowflake.
- Provide AWS credentials to Airflow (connection `aws_default`) so the DAG can invoke Lambda and Spark can access S3.
- Start Kafka and create the configured topic.
- Update the SparkSubmitOperator `application` path in the DAG to point to `spark_job` or your Spark script.
- Configure the Snowflake connection (`snowflake_default`) and S3 stage referenced in the DAG SQL.

## Configuration highlights

- AWS credentials: `aws_default` Airflow connection or environment variables for Spark.
- Kafka broker & topic: configured by `KAFKA_BROKER` and `KAFKA_TOPIC`.
- REST API base URL: configurable via `REST_COUNTRIES_API` env var in the Lambda.
- Spark uses `s3a` settings to write CSV to S3.

## Troubleshooting

- Verify broker host/port and topic names if Kafka messages are missing.
- Confirm S3 credentials and `s3a` config if Spark cannot write to S3.
- Check Airflow task logs for Lambda, Kafka publish, and Spark submit output.

License: MIT