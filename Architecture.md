🏗️ Architecture Note
This project implements the Azure Medallion Architecture for the University Chapters dataset using Azure Databricks and Azure Data Lake Storage (ADLS Gen2).

🔹 Layers
Bronze

Raw API payload from ArcGIS FeatureServer

Stored in /mnt/bronze/university_chapters/<run_id>/

Includes ingest metadata (run_id, timestamp)

Silver

Flattened, typed dataset

Data Quality applied:

DQ-Q1 → invalid coordinates → quarantined

DQ-W1 → missing/unknown city → warning

Stored in /mnt/silver/university_chapters/

Gold

Consumer-facing dataset

Contains only OK + WARNING rows

Stored in /mnt/gold/university_chapters/v1/

Quarantine

Invalid rows separated for debugging

Stored in /mnt/quarantine/university_chapters/<run_id>/

🔹 Chosen Tech
Compute: Azure Databricks (PySpark notebooks)

Storage: ADLS Gen2 (mounted /mnt/ paths)

Orchestration: Databricks Jobs workflow (Bronze → Silver → Gold)

Logging: Row counts per run (in, quarantined, warned, ok)

Contract: Markdown file defining schema, SLA, DQ rules, versioning

          +-------------------+
          | ArcGIS API Source |
          +-------------------+
                   |
                   v
        +-----------------------+
        | Bronze Layer (Raw)    |
        | /mnt/bronze/<run_id>/ |
        +-----------------------+
                   |
                   v
        +-----------------------+
        | Silver Layer (Clean)  |
        | /mnt/silver/          |
        | DQ-Q1 → Quarantine    |
        | DQ-W1 → Warning       |
        +-----------------------+
                   |
                   v
        +-----------------------+
        | Gold Layer (Product)  |
        | /mnt/gold/v1/         |
        | OK + WARNING only     |
        +-----------------------+
                   |
                   v
        +-----------------------+
        | Consumers / Analytics |
        +-----------------------+

Quarantine Path: /mnt/quarantine/<run_id>/
