**REAL-TIME WEATHER DATA PIPELINE USING API**

**Project Overview**
This project is a real-time weather data engineering pipeline built using Databricks, PySpark, Delta Lake, and OpenWeather API. The pipeline ingests live weather data for multiple cities, processes and validates the data through Bronze, Silver, and Gold layers, performs pipeline monitoring and testing, and prepares analytics-ready datasets for dashboards and operational monitoring.

The project demonstrates a modern Lakehouse-based Medallion Architecture using Structured Streaming and Delta Lake.

**Architecture**
OpenWeather API
        ↓
Bronze Layer (Raw Ingestion)
        ↓
Silver Layer (Streaming + Validation + Cleansing)
        ↓
Gold Layer (Business Aggregations)
        ↓
Pipeline Testing + Monitoring
        ↓
Dashboards / Analytics

**Technologies Used**

Technology	Purpose
Python	Core programming
PySpark	Distributed data processing
Databricks	Lakehouse platform
Delta Lake	Storage layer
Structured Streaming	Incremental processing
OpenWeather API	Real-time weather source
Spark SQL	Analytics queries
Databricks Workflows	Orchestration
GitHub	Version control

**Project Structure**
Real-Time-Weather-Data-Pipeline-Using-API/
│
├── README.md
│
├── configs/
│   └── config.json
│
├── bronze/
│   └── 00_bronze_ingestion.py
│
├── silver/
│   └── 01_silver_pipeline.py
│
├── gold/
│   └── 02_gold_pipeline.py
│
├── utils/
│   └── weather_utils.py
│
├── unit_tests/
│   └── test_weather_utils.py
│
├── monitoring/
│   └── 99_pipeline_tests.py
│
├── screenshots/
│
└── docs/
**Bronze Layer**
Purpose

The Bronze layer stores raw API responses exactly as received from the OpenWeather API.

Features

API ingestion using Python requests
Stores raw JSON payloads
Captures API metadata:
HTTP status
response latency
ingestion timestamp
Delta table storage
Append-only ingestion
Bronze Table
realtime_weather.bronze.bronze_weather_data

**Silver Layer**
Purpose

The Silver layer parses, cleans, validates, and enriches raw weather data.

Features

Structured Streaming processing
JSON parsing using StructType schema
Data quality validations
Quarantine handling for invalid rows
Derived business columns
Delta Change Data Feed (CDF)
Checkpointing for incremental processing
Validations Implemented
Validation	Rule
API errors	http_status != 200
Invalid temperatures	> 60 or < -50
Invalid humidity	> 100 or < 0
Negative wind speed	< 0
Future timestamps	event_timestamp > current_timestamp
Missing coordinates	latitude/longitude null
Empty city names	null or blank
Silver Tables
realtime_weather.silver.silver_weather_clean
realtime_weather.silver.silver_weather_quarantine
realtime_weather.silver.validation_log

**Gold Layer**
Purpose

The Gold layer creates analytics-ready business datasets.

Gold Tables
1. gold_city_current

Latest weather reading for each city.

2. gold_hourly_summary

Hourly weather aggregations for the last 24 hours.

3. gold_daily_extremes

Daily maximum and minimum weather conditions.

4. gold_severe_weather_alerts

Active severe weather alerts.

5. gold_api_health

Operational API monitoring and pipeline health metrics.

Structured Streaming

The Silver pipeline uses Structured Streaming with:

readStream
foreachBatch
checkpointing
availableNow=True

This ensures:

incremental processing
fault tolerance
no duplicate reprocessing

**Testing Framework**
1. Unit Testing

Notebook:

unit_tests/test_weather_utils.py

Tests:

temperature band logic
severity score logic
alert classification logic
2. Pipeline Validation Testing

Notebook:

monitoring/99_pipeline_tests.py

Validates:

Bronze ingestion success
Silver data quality
Gold aggregation correctness
freshness checks
duplicate detection
API success rate validation
Workflow Orchestration

Databricks Workflow executes:

Bronze → Silver → Gold → Unit Tests → Pipeline Tests
Schedule

Runs every 30 minutes.

**Monitoring**

The project includes operational monitoring tables for:

API health
latency tracking
pipeline freshness
quarantine tracking
test execution history
Dashboard & Visualization

The Gold layer tables are designed for:

Databricks dashboards
Power BI
Tableau
operational monitoring
trend analysis
Key Engineering Concepts Demonstrated
Medallion Architecture
Delta Lake
Structured Streaming
Data Quality Validation
Quarantine Pipelines
Change Data Feed (CDF)
Workflow Orchestration
Real-Time ETL
Pipeline Observability
Dynamic Pipeline Testing
Future Improvements
CI/CD integration using GitHub Actions
Dockerized deployment
Terraform infrastructure setup
Kafka-based ingestion
Great Expectations integration
Real-time dashboarding
ML-based anomaly detection
Setup Instructions
1. Clone Repository
git clone <repository-url>
2. Configure API Key

Update:

configs/config.json

Example:

{
  "api_key": "YOUR_API_KEY",
  "api_base_url": "https://api.openweathermap.org/data/2.5/weather",
  "cities": [
    "Chennai",
    "Mumbai",
    "Delhi"
  ]
}
3. Execute Pipeline

Run notebooks in order:

1. 00_bronze_ingestion.py
2. 01_silver_pipeline.py
3. 02_gold_pipeline.py
4. test_weather_utils.py
5. 99_pipeline_tests.py
Screenshots

Add screenshots for:
<img width="548" height="940" alt="image" src="https://github.com/user-attachments/assets/60f173c0-1538-4c4a-99d7-c0a30cec755a" />
<img width="2226" height="787" alt="image" src="https://github.com/user-attachments/assets/cd4abf23-4a68-4b9d-b909-af8c8a7f20ac" />
<img width="1133" height="634" alt="image" src="https://github.com/user-attachments/assets/e56b6910-7d53-40ab-b458-fc7e047515e1" />
<img width="1101" height="621" alt="image" src="https://github.com/user-attachments/assets/1bfa4983-456b-42a6-8083-0b1f93a10f0c" />
<img width="1104" height="633" alt="image" src="https://github.com/user-attachments/assets/e1ff3965-843b-46bc-87da-a98eacf5c913" />
<img width="2783" height="1200" alt="image" src="https://github.com/user-attachments/assets/ad7c78a0-9314-439d-9680-9d5f69acc1fe" />
<img width="2816" height="1520" alt="image" src="https://github.com/user-attachments/assets/cd77cc78-1af7-4eac-91a7-e5288e28f864" />



Author

HariHaran S
