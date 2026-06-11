# Data Engineering Assignment

## Overview

This project implements a simple medallion architecture (Bronze → Silver → Gold) in Databricks/Spark using the provided source files:

- item.csv
- event.csv

The goal is to build a small data lake and a final datamart (`top_item`) that provides yearly item popularity statistics.

---

## Architecture

### Bronze Layer

Raw ingestion layer.

Characteristics:
- Direct load from source files
- No transformations applied
- All columns stored as STRING
- One table per source file

Tables:
- bronze.item
- bronze.event

---

### Silver Layer

Cleansed and standardized layer.

Characteristics:
- Consistent naming conventions
- Flattened nested structures
- Proper data types applied

Tables:
- silver.item
- silver.events
- silver.event_items
- silver.event_referrers
- silver.event_tests
- silver.event_viewed_users

---

### Gold Layer

Business-oriented datamart.

Table:
- gold.top_item

For every item and year the datamart provides:
- Total number of item views
- Rank based on yearly views
- Most used platform

---

## Logic

- `view_item` events represent item views.
- Item ranking is calculated within each year.
- In case of a tie for most-used platform, Spark's `row_number()` determines the selected platform.

---

## Technologies

- Databricks
- Apache Spark (PySpark)
- Delta Lake
