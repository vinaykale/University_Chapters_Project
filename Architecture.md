🏗️ Architecture Note
This project implements the Azure Medallion Architecture for the University Chapters dataset using Azure Databricks with Unity Catalog managed tables.

🔹 Layers
Bronze (Raw Landing)

Ingests API payload from ArcGIS FeatureServer.

Stored as managed table: catalog.bronze.university_chapters_<run_id>.

Includes ingest metadata (run_id, timestamp).

Silver (Cleaned & Typed)

Flattened, typed dataset.

Data Quality applied:

DQ-Q1 → invalid coordinates → quarantined table.

DQ-W1 → missing/unknown city → warning flag.

Stored as managed table: catalog.silver.university_chapters.

Gold (Published Data Product)

Consumer-facing dataset.

Contains only OK + WARNING rows.

Stored as managed table: catalog.gold.university_chapters_v1.

Quarantine

Invalid rows separated for debugging.

Stored as managed table: catalog.quarantine.university_chapters_<run_id>.

🔹 Chosen Tech
Concern	          Technology
Compute	          Azure Databricks (PySpark notebooks)
Storage	          Unity Catalog managed tables (governed lakehouse)
Orchestration	Databricks Jobs workflow (Bronze → Silver → Gold)
Governance	Unity Catalog for access control, lineage, and schema enforcement
Logging	          Row counts per run (in, quarantined, warned, ok)
Contract	          Markdown file defining schema, SLA, DQ rules, versioning



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
        | /valume/silver/          |
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
