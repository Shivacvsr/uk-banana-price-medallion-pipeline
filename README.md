# uk-banana-price-medallion-pipeline
A Medallion Architecture data pipeline developed for Support Education Group(SEG) to automate the ingestion, cleaning, and analysis of UK government banana price data
SEG: UK Banana Price Data Pipeline
1. Overview
This pipeline was developed for Support Education Group (SEG) to automate the ingestion of UK government banana price data published by DEFRA. The system uses a Medallion Architecture to transform raw data into high-quality, business-ready insights.

2. System Design
The data flows through three distinct stages to ensure accuracy and auditability:

Bronze (Raw): Ingests original DEFRA CSVs and attaches lineage metadata (source URL, ingestion time) for full traceability.

Silver (Cleaned): Standardizes column names, parses dates, and removes duplicate records to create a "single version of truth".

Gold (Aggregated): Calculates annual average prices and country-of-origin metrics for SEG’s analytics and dashboards.

3. Technology Stack
Orchestration: Databricks Jobs, Azure Data Factory, or cron.

Compute: Azure Databricks Spark clusters or local Python.

Storage: Delta Lake (Cloud) or Parquet (Local) via ADLS Gen2/DBFS.

Libraries: pandas, requests, tenacity, pydantic-settings, and pyarrow.

4. Operational Flow
Trigger: The Orchestrator initiates a scheduled run.

Ingestion: The pipeline scrapes the gov.uk landing page to identify and download the latest CSV.

Validation: Data is checked for schema consistency and expected row counts.

Refinement: Data moves from Bronze to Silver for cleaning and Gold for final aggregation.

Monitoring: The run completes with logged metrics and error handling (retries for HTTP failures).

5. Deployment
Databricks: Run scripts/notebooks; configure using Databricks Secrets.

Local: Install via pip install -e . and run using python -m banana_pipeline --layers full.

6. Resilience and Failure HandlingThe pipeline is designed with a "fail-fast" philosophy to ensure data quality and system reliability:Potential FailureHandling Strategygov.uk unavailableUses HTTP retries with exponential backoff before alerting.CSV link missingFails immediately with a clear error log to prevent empty runs.Schema/Parse ErrorTriggers a validation failure to block corrupt data from the pipeline.Low Row CountFlags a data quality exception to ensure the dataset is complete.Storage Write FailureLogs the specific I/O error and raises an exception for troubleshooting
