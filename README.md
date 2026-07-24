🎓 Azure Medallion Data Product – University Chapters
This project implements a Data Engineering take‑home assignment using Azure Databricks and Unity Catalog.
It builds a thin Medallion Architecture pipeline (Bronze → Silver → Gold) that ingests university chapter data from a public ArcGIS API and publishes it as a governed data product.


🔹 Chosen Technologies
Compute: Azure Databricks (PySpark notebooks)

Storage: Unity Catalog managed tables (governed lakehouse)

Orchestration: Databricks Jobs workflow (Bronze → Silver → Gold)

Governance: Unity Catalog for access control, lineage, and schema enforcement

Logging: Row counts per run (in, quarantined, warned, ok)

Contract: Markdown file defining schema, SLA, DQ rules, versioning

🚀 How to Run the Pipeline
🟤 Step 1: Bronze Ingest
Run the notebook:

Code
/Repos/azure-medallion-university-chapters/notebooks/bronze_ingest.py
This pulls data from the ArcGIS API and writes raw payload to the Bronze managed table.
It prints the run_id for this ingest.

⚪ Step 2: Silver Transform
Run:

Code
/Repos/azure-medallion-university-chapters/notebooks/silver_transform.py
Set the widget run_id to the value printed in Bronze.
This applies DQ rules and writes cleaned data to Silver and Quarantine tables.

🟡 Step 3: Gold Publish
Run:

Code
/Repos/azure-medallion-university-chapters/notebooks/gold_publish.py
Use the same run_id.
This publishes the consumer‑facing Gold dataset containing OK + WARNING rows only.

🖥️ Optional: Databricks Job Workflow
Automate the pipeline with a Databricks Job:

Bronze Ingest

Silver Transform (depends on Bronze)

Gold Publish (depends on Silver)

See docs/job_config.json for an example configuration.

📊 Logs & Metrics
Each notebook prints counts per run:

rows_in

rows_quarantined

rows_warned

rows_ok

📑 Deliverables
Bronze, Silver, Gold datasets + Quarantine table

Data Product Contract (data_product_contract/contract.md)

Architecture note + diagram (docs/architecture.md, docs/architecture_diagram_unity_catalog.png)

Tests (notebooks/tests.py)

README (this file)

🖼️ Architecture Diagram
See docs/architecture_diagram_unity_catalog.png for the visual pipeline flow.

✅ Why Unity Catalog
Using Unity Catalog instead of ADLS mounts:

Centralizes governance and access control.

Simplifies schema management and lineage tracking.

Makes the pipeline production‑ready and compliant with enterprise data standards.
