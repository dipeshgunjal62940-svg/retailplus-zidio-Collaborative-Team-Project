 # RetailPulse — Collaborative Retail Analytics & ETL Platform

**A Four-Member Collaborative Project | ETL Development | Data Engineering | Business Analytics | Machine Learning**

RetailPulse is a collaborative retail analytics platform designed to transform raw retail transaction data into meaningful business insights.

The platform integrates data extraction, transformation, cleaning, enrichment, customer analytics, sales analysis, demand forecasting, inventory optimization, real-time monitoring, and MLOps capabilities.

This project was developed collaboratively by a team of four members, with each member contributing to a different technical area.

**My primary responsibility was ETL Development and Data Engineering**, where I worked on building the data processing pipeline responsible for preparing structured and analysis-ready data for the overall platform.

---

## My Individual Contribution

### Dipesh Gunjal — ETL Developer | Data Engineering

My primary contribution to RetailPulse was developing the ETL and data preparation layer.

I worked on the data processing workflow responsible for converting raw retail transaction data into a clean, structured, enriched, and analysis-ready dataset.

**Key Responsibilities**

- **Data Extraction:** Handling raw retail transaction data from Excel and CSV sources.
- **Data Cleaning:** Developing data cleaning and preprocessing workflows.
- **Data Transformation:** Standardizing and transforming raw data into structured formats.
- **Data Enrichment:** Creating additional analytical attributes and business-related features.
- **Data Quality:** Implementing data quality checks during processing.
- **Data Loading:** Generating processed and compressed datasets in CSV/GZIP format.
- **Pipeline Development:** Building a reusable Python-based ETL workflow.
- **Workflow Orchestration:** Contributing to Airflow-based pipeline integration.

### Files Associated With My Contribution

| File | Responsibility |
|---|---|
| `src/data.py` | Main ETL pipeline: extraction, cleaning, transformation, enrichment and data quality |
| `airflow/dags/retailpulse_dag.py` | Airflow DAG and workflow orchestration |
| `tests/test_pipeline.py` | ETL and data pipeline testing |
| `data/raw/` | Raw dataset input location |
| `data/processed/` | Processed dataset output location |
| `data/processed/source.txt` | Dataset source information |

**Main ETL Output**

`data/processed/retailpulse_enriched.csv.gz`

The ETL pipeline generates this compressed dataset for use by the platform's analytics and machine learning modules.

---

## Collaborative Team Structure

RetailPulse is a four-member team project. Each member contributed to a separate area of the overall platform.

| Member | Name | Primary Responsibility |
|---|---|---|
| Member 1 | **Dipesh Gunjal** | ETL Development, Data Engineering, Data Cleaning, Transformation and Pipeline Integration |
| Member 2 | **Raveena Shukla** | Sales Analytics and Business Intelligence |
| Member 3 | **Nasrin Begam** | Customer Analytics, Customer Segmentation and Churn Prediction |
| Member 4 | **Najia** | Demand Forecasting, Inventory Optimization and MLOps |

### How the Team Collaborated

The project was divided into individual technical responsibilities and integrated into one Streamlit-based analytics platform.

My ETL contribution forms the data preparation layer, while the other modules use the prepared data to deliver sales insights, customer analytics, forecasting, inventory management and operational monitoring.

The repository represents the collaborative platform, while the ETL responsibilities described above highlight my individual contribution.

---

## ETL Pipeline Architecture

The ETL pipeline follows three major stages: Extract, Transform and Load.

```text
             RAW RETAIL DATA
                    |
                    v
                 EXTRACT
            Read Excel / CSV
                    |
                    v
                TRANSFORM
             Data Cleaning
          Data Standardization
            Data Enrichment
            Feature Engineering
             Quality Checks
                    |
                    v
                  LOAD
           Processed Dataset
                    |
                    v
       retailpulse_enriched.csv.gz
                    |
           +--------+--------+
           |        |        |
           v        v        v
          Sales   Customer  Forecasting
        Analytics Analytics & Inventory
           |        |        |
           +--------+--------+
                    |
                    v
            RETAILPULSE APP
```

### ETL Workflow Details

**1. Extract**

- Load the source retail transaction dataset.
- Handle Excel and CSV input files.
- Prepare source data for processing.

**2. Transform**

- Clean and preprocess transaction data.
- Standardize data fields.
- Handle missing or inconsistent values where applicable.
- Generate engineered business attributes.
- Perform data quality checks.

**3. Load**

- Generate the enriched dataset.
- Store the output in compressed CSV/GZIP format.
- Make processed data available to the analytics application.

### Execute the ETL Pipeline

After downloading and placing the source dataset in the expected input directory, run:

```bash
python -m src.data --build
```

Expected output:

```text
data/processed/retailpulse_enriched.csv.gz
```

---

## Airflow Workflow Orchestration

The project includes an Apache Airflow DAG:

`airflow/dags/retailpulse_dag.py`

It is designed to orchestrate a weekly data and model maintenance workflow.

```text
       ingest_clean
             |
             v
        check_drift
             |
        +----+----+
        |         |
        v         v
   train_models  skip_training
```

The workflow is designed to:

- Rebuild the processed dataset.
- Check for changes in incoming data.
- Monitor data drift using PSI.
- Trigger model retraining when required.
- Record experiment information through the tracking module.

The DAG is a reference implementation that requires an Airflow environment for execution.

---

## Dataset Information

**Dataset:** Online Retail II

**Source:** Public UCI / Kaggle retail transaction dataset.

The original dataset is not included in this GitHub repository because of its large file size.

This repository contains the ETL source code, but users must download the source dataset separately to execute the full ETL workflow.

### Dataset Setup

Place the downloaded Excel or CSV dataset in:

```text
data/raw/
```

For example:

```text
data/raw/online_retail_II.xlsx
```

Then execute:

```bash
python -m src.data --build
```

The processed dataset will be generated locally at:

```text
data/processed/retailpulse_enriched.csv.gz
```

**Important:** Both raw and processed datasets are excluded from the GitHub repository where applicable. The processed output can be reproduced using the ETL pipeline and the source dataset.

### Data Enrichment Note

Some business attributes not available in the original public dataset — including sales channels, suppliers, lead times, promotions, customer demographics, shipping information and inventory-related fields — are simulated or engineered for demonstration purposes.

These fields must not be interpreted as actual historical business records.

---

## Platform Features

RetailPulse includes 15 dashboard modules covering different retail business areas.

### 1. Sales and Business Analytics

- Sales performance analysis.
- Product-level insights.
- Regional performance analysis.
- Profit and revenue analysis.
- Interactive data exploration.

### 2. Customer Analytics

- RFM-based customer segmentation.
- K-Means clustering.
- Churn prediction.
- Revenue-at-risk analysis.

### 3. Demand Forecasting

- 7–60 day forecasts.
- Forecast model comparison.
- What-if scenario analysis.
- Hybrid forecasting approach.

### 4. Inventory Optimization

- Safety stock calculations.
- Reorder point analysis.
- Economic Order Quantity (EOQ).
- ABC-XYZ classification.
- Suggested purchase orders.

### 5. Real-Time Analytics and MLOps

- Live order stream simulation.
- Revenue monitoring.
- Demand-spike detection.
- Inventory depletion monitoring.
- Data drift detection.
- Experiment tracking and model registry.
- Airflow-based retraining workflow.

---

## Technology Stack

| Area | Technologies |
|---|---|
| Programming | Python |
| ETL & Data Processing | Pandas, NumPy |
| Data Formats | Excel, CSV, CSV.GZ |
| Workflow Orchestration | Apache Airflow |
| Data Storage | SQLite |
| Machine Learning | Scikit-learn, SciPy, Statsmodels |
| Forecasting | Holt-Winters, Gradient Boosting, MLP, optional Prophet |
| Application | Streamlit |
| Visualization | Plotly, Matplotlib |
| MLOps | MLflow (optional), PSI, KS drift detection |
| Testing | Pytest |
| Version Control | Git, GitHub |
| Automation | GitHub Actions |

---

## Project Structure

```text
retailpulse-zidio/
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── .streamlit/
│   └── config.toml
│
├── airflow/
│   └── dags/
│       └── retailpulse_dag.py
│
├── artifacts/
│   └── .gitkeep
│
├── data/
│   ├── raw/
│   │   └── .gitkeep
│   │
│   └── processed/
│       ├── .gitkeep
│       └── source.txt
│
├── src/
│   ├── __init__.py
│   ├── data.py
│   ├── auth.py
│   ├── drift.py
│   ├── features.py
│   ├── forecast.py
│   ├── inventory.py
│   ├── realtime.py
│   ├── services.py
│   ├── tracking.py
│   └── ui.py
│
├── tests/
│   ├── test_app.py
│   ├── test_auth.py
│   ├── test_pipeline.py
│   └── test_realtime.py
│
├── views/
│   ├── login.py
│   ├── p01_overview.py
│   ├── p02_data_explorer.py
│   ├── p03_sales.py
│   ├── p04_products.py
│   ├── p05_segmentation.py
│   ├── p06_churn.py
│   ├── p07_forecast.py
│   ├── p08_models.py
│   ├── p09_inventory.py
│   ├── p10_drift.py
│   ├── p11_alerts.py
│   ├── p12_mlops.py
│   ├── p13_regional.py
│   ├── p14_profit_revenue.py
│   └── p15_realtime.py
│
├── app.py
├── requirements.txt
├── README.md
└── .gitignore
```

---

## Installation and Execution

### 1. Clone the Repository

```bash
git clone https://github.com/dipeshgunjal62940-svg/retailpulse-zidio.git
cd retailpulse-zidio
```

### 2. Create a Virtual Environment

```bash
python -m venv .venv
```

Activate on Windows:

```powershell
.venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Prepare the Dataset

Download the Online Retail II dataset and place it in:

```text
data/raw/
```

### 5. Run the ETL Pipeline

```bash
python -m src.data --build
```

### 6. Launch the Application

```bash
streamlit run app.py
```

### 7. Run Tests

```bash
pytest
```

---

## Key Project Highlights

- Four-member collaborative software development project.
- Individual contribution focused on ETL development and data engineering.
- Developed a Python-based ETL workflow.
- Worked with raw retail transaction data.
- Implemented data cleaning, transformation, enrichment and quality checks.
- Generated reusable, analysis-ready processed datasets.
- Integrated the data preparation workflow with Airflow orchestration.
- Supported downstream analytics and machine learning modules.
- Used Git and GitHub for collaborative development and version control.

---

## Learning Outcomes

Through this project, I gained practical experience in:

- Building Python-based ETL pipelines.
- Data extraction from Excel and CSV files.
- Data cleaning, transformation and enrichment.
- Data quality validation.
- Preparing structured datasets for analytics.
- Workflow orchestration using Apache Airflow.
- Integrating data engineering with analytics and machine learning.
- Team collaboration and GitHub-based project management.

---

## Author and Project Contributors

### Member 1 — Dipesh Gunjal
**Role:** ETL Developer | Data Engineering

### Member 2 — Raveena Shukla
**Role:** Sales Analytics | Business Intelligence

### Member 3 — Nasrin Begam
**Role:** Customer Analytics | Segmentation | Churn Prediction

### Member 4 — Najia
**Role:** Demand Forecasting | Inventory Optimization | MLOps

**Project Type:** Four-Member Collaborative Project

**GitHub Repository:**  
https://github.com/dipeshgunjal62940-svg/retailpulse-zidio

**GitHub Profile:**  
https://github.com/dipeshgunjal62940-svg

---

*RetailPulse represents a collaborative approach to retail analytics, with an individual contribution focused on building the ETL and data engineering foundation that supports the wider platform.*
