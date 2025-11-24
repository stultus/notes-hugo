---
title: "ELT (Extract - Load - Transform)"
date: 2025-11-24
draft: false
tags: ["data", "data-engineering", "architecture"]
status: "seeding"
summary: "Data processing approach where raw data is loaded before transformation, preserving original data."
---

**ELT (Extract - Load - Transform)** is a data processing approach where data is extracted from source systems, loaded into a destination (typically a data warehouse or lake), and then transformed in place.

## Core Philosophy

A core tenet of ELT philosophy is that data should be **untouched** as it moves through the Extract and Load stages so that the raw data is always accessible. If an unmodified version of the data exists in the destination, it can be transformed without needing to sync data again.

## Key Benefits

- **Preserves raw data**: Original data remains available for reprocessing
- **Flexibility**: Transformations can be changed without re-extracting
- **Efficiency**: Avoids redundant data movement
- **Auditability**: Raw data serves as a source of truth
- **Iterative development**: Transformations can be refined without re-syncing

## Comparison to ETL

Unlike traditional **ETL (Extract - Transform - Load)**, where transformation happens before loading:
- ELT loads raw data first, then transforms
- ELT leverages the processing power of modern data warehouses
- ELT allows multiple transformation pipelines on the same raw data

## Use Cases

- Modern data warehouses (Snowflake, BigQuery, Redshift)
- Data lakes and lakehouses
- Real-time analytics pipelines
- Multi-consumer data architectures

## Tools

### Extract-Load (EL)
- **Airbyte**: Open-source data integration platform
- **Meltano**: Open-source ELT platform

### Transform (T)
- **dbt**: Data transformation tool that runs in your warehouse

## Related Concepts

- [Operational Data](operational-data) — source data for extraction
- [Analytical Data](analytical-data) — transformed data for analysis
- [Analytically Operational Data](analytically-operational-data) — data that bridges both worlds
- [Architecture Decision (AD)](architecture-decision)

---
*Definition based on data engineering practice.*

