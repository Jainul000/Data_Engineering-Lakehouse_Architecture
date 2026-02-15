# 🚕 Taxi Trips Data Engineering Pipeline

### Databricks • PySpark • Delta Lake • dbt • Lakehouse Architecture

---

## 📌 Project Overview

This project demonstrates an **end-to-end modern data engineering pipeline** built using the **Lakehouse architecture** on Databricks. It simulates an Uber-like operational data platform, ingesting raw transactional datasets and transforming them into **analytics-ready fact and dimension tables**.

The pipeline covers the complete lifecycle:

* Raw data ingestion using **PySpark Structured Streaming**
* Storage in **Delta Lake Bronze tables**
* Cleaning, deduplication, and upsert logic in **Silver layer**
* Dimensional modeling using **dbt**
* Creation of **Gold layer fact & dimension tables** for BI and analytics

This project is designed to showcase **real-world data engineering practices**, including incremental processing, schema management, and modular transformation workflows.

---

## 🏗️ Architecture Overview

```
Raw CSV Files
     ↓
Databricks Structured Streaming
     ↓
Bronze Layer (Raw Delta Tables)
     ↓
PySpark Cleaning & Merge Logic
     ↓
Silver Layer (Validated & Deduplicated Tables)
     ↓
dbt Transformations & Snapshots
     ↓
Gold Layer (Fact + Dimension Models)
     ↓
Analytics / Dashboards / BI Tools
```

---

## 📂 Repository Structure

```
Uber/
│
├── data/                         # Raw CSV datasets
│   ├── customers.csv
│   ├── drivers.csv
│   ├── trips.csv
│   ├── vehicles.csv
│   ├── locations.csv
│   └── payments.csv
│
├── databricks/
│   ├── bronze_ingestion.ipynb    # Streaming ingestion into Bronze Delta tables
│   ├── silver_transformation.ipynb # Cleaning, deduplication, merge/upsert logic
│   └── utils.ipynb               # Helper functions used across notebooks
│
├── dbt/
│   ├── dbt_project.yml
│   ├── models/
│   │   ├── sources/              # Source table definitions
│   │   ├── silver/               # Intermediate dbt transformations
│   │   └── gold/                 # Final dimensional models
│   │
│   ├── macros/                   # Custom dbt macros
│   └── snapshots/                # Snapshot configs for SCD handling
│
├── images/                       # Architecture / workflow screenshots
└── README.md
```

---

## 🥉 Bronze Layer — Raw Data Ingestion

**Notebook:** [bronze_ingestion.ipynb](databricks/bronze_ingestion.ipynb)

### Purpose

The Bronze layer stores **raw, unmodified source data** in Delta format for traceability and replayability.

### Key Features

* Uses **Spark Structured Streaming** to simulate continuous ingestion
* Reads raw CSV files from storage
* Schema inferred from initial batch read
* Writes to **Delta tables** in append mode
* **Checkpointing enabled** for fault tolerance and recovery
* Uses a **dynamic ingestion framework** instead of hardcoded/static table logic making the pipeline **modular, scalable, and concise**, while reducing code duplication and simplifying onboarding of new datasets.
* Ensures ingestion is:

  * scalable
  * restart-safe
  * idempotent

### Tables Created

* `bronze_customers`
* `bronze_drivers`
* `bronze_trips`
* `bronze_locations`
* `bronze_payments`
* `bronze_vehicles`

---

## 🥈 Silver Layer — Cleaned & Standardized Data

**Notebook:** [silver_transformation.ipynb](databricks/silver_transformation.ipynb)

### Purpose

The Silver layer contains **validated, cleaned, and deduplicated datasets** ready for downstream modeling.

### Transformations Performed

* Data type standardization
* Null handling and filtering invalid records
* Deduplication using business keys
* Timestamp normalization
* Incremental processing
* **Merge / Upsert logic** using Delta Lake

### Delta Lake Capabilities Used

* `MERGE INTO` for incremental updates
* ACID transactions
* Time travel support
* Schema evolution compatibility

### Utility Functions

Reusable logic is defined in:

**Notebook:** [utils.ipynb](databricks/utils.ipynb)

Includes:

* Deduplication helpers
* Merge/upsert functions
* Timestamp conversion utilities
* Reusable transformation patterns

---

## 🥇 Gold Layer — Analytics-Ready Models (dbt)

**Framework:** dbt
**Folder:** `dbt/models/`

### Purpose

The Gold layer provides **business-level dimensional models** optimized for reporting and analytics.

dbt is used to:

* Manage SQL-based transformations
* Build fact and dimension tables
* Track lineage and dependencies
* Enable modular, testable data modeling
* Support documentation generation

---

### 🔹 Sources

Defined in:

```
dbt/models/sources/sources.yml
```

Maps Silver Delta tables as dbt sources.

---

### 🔹 Silver Models (dbt)

Intermediate transformations:

* Standardized joins
* Derived fields
* Business logic refinement
* Preparation for dimensional modeling

Example:

```
dbt/models/silver/trips.sql
```

---

### 🔹 Gold Models

Final warehouse-style tables such as:

* **Fact tables**

  * Trips fact
  * Payments fact

* **Dimension tables**

  * Customer dimension
  * Driver dimension
  * Location dimension
  * Vehicle dimension

These tables support:

* Revenue analysis
* Trip trends
* Driver performance metrics
* Customer activity insights

---

### 🔹 Snapshots (Slowly Changing Dimensions)

Located in:

```
dbt/snapshots/
```

Used to implement **SCD Type-2 style tracking**, preserving historical changes in dimensional data such as:

* driver status updates
* vehicle assignments
* customer attribute changes

---

## ⚙️ Technologies Used

* **Databricks**
* **Apache Spark (PySpark)**
* **Structured Streaming**
* **Delta Lake**
* **dbt (Data Build Tool)**
* **SQL**
* **Lakehouse Architecture**

---

## 🚀 How to Run the Project

### 1️⃣ Upload Data

Place CSV files into the configured storage location accessible by Databricks.

---

### 2️⃣ Run Bronze Ingestion

Execute:

```
databricks/bronze_ingestion.ipynb
```

This will:

* create Bronze Delta tables
* start ingestion streams

---

### 3️⃣ Run Silver Transformations

Execute:

```
databricks/silver_transformation.ipynb
```

This will:

* clean and validate records
* deduplicate data
* merge updates into Silver tables

---

### 4️⃣ Run dbt Models

Navigate to the dbt project folder:

```
cd dbt
```

Run:

```
dbt run
dbt snapshot
dbt test
```

This will:

* build Gold models
* update snapshots
* validate data quality

---

## 📊 Example Use Cases

This pipeline enables analytics such as:

* Daily trip volume trends
* Revenue per location
* Driver utilization metrics
* Customer ride frequency
* Payment method distribution
* Operational performance dashboards

---

## 🎯 Learning Objectives Demonstrated

This project showcases:

* End-to-end Lakehouse pipeline design
* Streaming + batch hybrid ingestion
* Incremental ETL patterns
* Delta Lake merge strategies
* Modular transformation architecture
* dbt dimensional modeling workflow
* Production-style project structuring

---

## 📎 Future Improvements

Possible enhancements:

* Add data quality checks with dbt tests
* Implement orchestration (Airflow / Databricks Workflows)
* Add CI/CD for dbt deployment
* Integrate dashboard layer (Power BI / Tableau)
* Implement schema validation contracts
* Add unit tests for transformation logic

---

## 👤 Author

**Jainul Abideen**
Data Engineering Project – Lakehouse Pipeline with Databricks, PySpark & dbt

---

## 📜 License

This project is for **educational and portfolio purposes**.

<div align="center">

**⭐ If you find this project useful, please consider giving it a star! ⭐**

Made with ❤️

</div>
