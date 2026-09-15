# SQL Data Warehouse & ETL Pipeline

An end-to-end data warehousing project that ingests raw source data, cleans and transforms it through a layered architecture, and models it into a star schema ready for analytics and BI reporting.

## Overview

This project demonstrates a complete ETL workflow built on SQL Server, following industry-standard data warehousing practices. Raw data from multiple source systems is loaded into a staging layer, cleansed and standardised, then modelled into fact and dimension tables that power downstream reporting.

The goal was to build something close to a production setup — with proper layering, data quality checks, logging, and repeatable load logic — rather than a one-off script.

  Source Files (CSV)
        │
        ▼
   [ BRONZE ]  ──  Full load, raw ingestion
        │
        ▼
   [ SILVER ]  ──  Cleansing, standardisation, deduplication
        │
        ▼
   [  GOLD  ]  ──  Star schema (facts + dimensions)
        │
        ▼
  Power BI / SQL Reporting

## Tech Stack

1) Database: Microsoft SQL Server
2) ETL:SQL stored procedures
3) Orchestration: SQL Server Agent / scripted execution
4) Data Modelling: Star schema
5) Visualisation: Power BI
6) Version Control: Git / GitHub


# Data Model

The Gold layer is modelled as a star schema:

Fact Tables

fact_sales — transactional grain, one row per order line

Dimension Tables

dim_customer — customer attributes with surrogate keys
dim_product — product hierarchy and categorisation
dim_date — full calendar dimension for time-based analysis

Surrogate keys are generated in the Gold layer so the warehouse is decoupled from source system keys.

# ETL Process
1. Extract

Source CSV files are bulk-loaded into Bronze staging tables using BULK INSERT. Tables are truncated before each load to support repeatable full refreshes.

2. Transform

The Silver layer applies:

Removal of duplicate records using window functions (ROW_NUMBER())
Trimming and standardisation of text fields
Normalisation of coded values (e.g. gender, country codes) into readable labels
Handling of nulls and invalid values with defaults
Data type casting and date validation
Derivation of calculated fields
3. Load

The Gold layer builds dimension and fact views/tables, joining Silver tables and generating surrogate keys. Referential integrity between facts and dimensions is validated before publishing.

# Data Quality Checks

Validation scripts run against each layer to confirm:

1) No duplicate or null primary keys
2) No unwanted leading/trailing whitespace in string fields
3) Consistent and standardised values in coded columns
4) Valid date ranges and logical date ordering
5) No orphaned foreign keys between facts and dimensions

# Key Outcomes:

1) Built a repeatable, layered ETL pipeline with clear separation of raw, cleansed, and presentation data
2) Implemented reusable stored procedures with error handling and load duration logging
3) Designed a star schema optimised for analytical query performance
4) Applied automated data quality checks at each stage of the pipeline

## Author
Haider Ali Khan — Data Engineer Databricks Certified Associate Data Engineer | Microsoft DP-900

