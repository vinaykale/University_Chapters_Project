📄 Data Product Contract – University Chapters
🔹 Product Overview
Name: University Chapters Data Product

Domain: Public university chapter locations (CA, OR, WA)

Purpose: Provide curated, consumer‑ready dataset of university chapters for analytics and reporting.

Owner: Data Engineering Team

🔹 Storage & Governance
Platform: Azure Databricks

Storage: Unity Catalog managed tables

Tables:

Bronze: catalog.bronze.university_chapters_<run_id>

Silver: catalog.silver.university_chapters

Gold: catalog.gold.university_chapters_v1

Quarantine: catalog.quarantine.university_chapters_<run_id>

Governance: Unity Catalog for access control, lineage, schema enforcement

🔹 Schema Definition (Gold Layer)
Column	Type	Description
chapter_id	STRING	Unique identifier for chapter
chapter_name	STRING	University chapter name
city	STRING	City name (may be missing/unknown)
state	STRING	US state (CA, OR, WA)
longitude	DOUBLE	Geographic longitude
latitude	DOUBLE	Geographic latitude
dq_status	STRING	Data quality status (OK / WARNING)
dq_warnings	ARRAY	List of warnings (if any)
ingest_run_id	STRING	Run identifier for lineage


🔹 Data Quality Rules
DQ‑Q1 (Quarantine): Invalid coordinates (null or out of range) → quarantined table.

DQ‑W1 (Warning): Missing or “UNKNOWN” city → flagged with dq_status = WARNING.

OK: All other records pass validation.

🔹 SLA & Refresh
Refresh Frequency: Daily by 06:00 UTC.

Availability: 99% uptime for Gold dataset.

Latency: < 1 hour from ingest to Gold publish.

🔹 Versioning
Gold dataset versioned by table name suffix (_v1, _v2, …).

Breaking schema changes trigger new version.

🔹 Classification
Data Sensitivity: Public data, no PII.

Compliance: No GDPR/CCPA impact.
