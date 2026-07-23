🚀 How to Run the University Chapters Medallion Pipeline
This project implements a Bronze → Silver → Gold medallion architecture in Azure Databricks for the University Chapters dataset.

📂 Prerequisites
Access to an Azure Databricks workspace

A mounted ADLS Gen2 container (or local /mnt/ paths for simulation)

Python 3.9+ runtime with PySpark

Repo cloned into Databricks (/Repos/azure-medallion-university-chapters/)

🟤 Step 1: Bronze Ingest
Run the notebook:

Code
/Repos/azure-medallion-university-chapters/notebooks/bronze_ingest.py
This will:

Pull data from the ArcGIS FeatureServer API

Save raw JSON payload to /mnt/bronze/university_chapters/<run_id>/

Print the run_id for this ingest

⚪ Step 2: Silver Transform
Run the notebook:

Code
/Repos/azure-medallion-university-chapters/notebooks/silver_transform.py
Before running, set the widget run_id to the value printed in Bronze.
This will:

Read Bronze data for that run

Apply DQ rules:

DQ-Q1 → invalid coordinates → /mnt/quarantine/university_chapters/<run_id>/

DQ-W1 → missing/unknown city → flagged with dq_status = WARNING

Write cleaned Silver dataset to /mnt/silver/university_chapters/

🟡 Step 3: Gold Publish
Run the notebook:

Code
/Repos/azure-medallion-university-chapters/notebooks/gold_publish.py
Set the same run_id widget.
This will:

Read Silver data

Publish consumer-facing Gold dataset to /mnt/gold/university_chapters/v1/

Exclude quarantined rows, include OK + WARNING rows

📊 Logs & Metrics
Each notebook prints counts per run:

rows_in

rows_quarantined

rows_warned

rows_ok

🖥️ Orchestration (Optional)
You can automate the pipeline with a Databricks Job:

Task 1: Bronze Ingest

Task 2: Silver Transform (depends on Bronze, passes run_id)

Task 3: Gold Publish (depends on Silver, same run_id)

See docs/job_config.json for an example configuration.

✅ Deliverables
Bronze, Silver, Gold datasets + Quarantine path

Data Product Contract (data_product_contract/contract.md)

Architecture diagram (docs/architecture_diagram.png)

Tests (notebooks/tests.py)
