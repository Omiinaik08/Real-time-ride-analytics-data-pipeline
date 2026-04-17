# Real-Time Medallion Data Pipeline for Ride Booking Analytics 🚖

An enterprise-grade, end-to-end real-time streaming data pipeline simulating a high-velocity ride-booking application. This project ingests, transforms, and serves streaming and batch data using a strict Medallion Architecture (Bronze, Silver, Gold) deployed entirely on the Microsoft Azure Cloud.

## 📌 Project Overview
Traditional ETL pipelines suffer from latency, rigid schemas, and brittle SQL codebases. This capstone project addresses these industry challenges by engineering a highly scalable, fault-tolerant system. It leverages **Spark Declarative Pipelines (SDP)** for automated DAG orchestration and idempotent streaming, alongside a **Metadata-Driven Jinja Architecture** to dynamically generate complex SQL transformations on the fly.

## 🏗️ Architecture & Data Flow


1. **Producer (FastAPI):** A custom web application that acts as the data generator, securely publishing live ride-booking JSON payloads to Azure Event Hubs.
2. **Batch Ingestion (Azure Data Factory):** Automates the ingestion of static mapping data and historical bulk files from a GitHub HTTP API into ADLS Gen2 using parameterized configurations.
3. **Data Processing (Azure Databricks):**
   * **🥉 Bronze Layer (Raw):** Ingests raw binary Kafka offsets from Event Hubs and raw JSON batch files from ADLS Gen2. Acts as an immutable historical archive.
   * **🥈 Silver Layer (Staging/OBT):** Intelligently merges high-velocity streaming data with historical batch data using SDP `@dp.append_flow` to achieve exactly-once processing. Utilizes a metadata-driven Jinja SQL template to dynamically construct a massive One Big Table (OBT) via automated `LEFT JOIN`s.
   * **🥇 Gold Layer (Star Schema):** Decomposes the Silver OBT into a refined Dimensional Data Model. Implements **Slowly Changing Dimensions (SCD Type 2)** via Spark's Auto-CDC API to track historical geographic changes without data loss.

## 🛠️ Tech Stack
* **Cloud Platform:** Microsoft Azure (ADLS Gen2, Azure Databricks)
* **Streaming & Messaging:** Azure Event Hubs (Kafka protocol)
* **Orchestration:** Azure Data Factory (ADF), Spark Declarative Pipelines (SDP)
* **Data Processing:** Apache Spark (PySpark), Delta Lake
* **Dynamic Templating:** Jinja2
* **Backend Application:** Python, FastAPI, Uvicorn

## 🚀 Key Engineering Achievements
* **Idempotent Streaming:** Achieved exactly-once processing and robust state management without recalculating historical datasets during micro-batch triggers.
* **Schema Evolution Resilience:** Eliminated hardcoded SQL. Adding a new mapping table requires simply updating a JSON configuration object, completely neutralizing pipeline brittleness.
* **Auto-CDC Implementation:** Successfully tracked temporal business territory shifts (e.g., city name changes) by dynamically maintaining `start_at` and `end_at` timestamps for strict referential integrity.

## 💻 Local Setup (Producer App)
To run the FastAPI producer locally and trigger events into the pipeline:

1. Clone the repository:
   ```bash
   git clone [https://github.com/Omiinaik08/Real-time-ride-analytics-data-pipeline.git](https://github.com/Omiinaik08/Real-time-ride-analytics-data-pipeline.git)
   cd Real-time-ride-analytics-data-pipeline



