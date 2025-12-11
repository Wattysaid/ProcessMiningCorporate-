# Data Model & Storage

Defines the core schema and storage layout.

---

## 1. Core Entities

### 1.1 User

- `id (uuid)`
- `email`
- `name`
- `role` (global)
- `created_at`

### 1.2 Workspace

- `id (uuid)`
- `name`
- `owner_user_id`
- `region` (EU/UK/US)
- `plan` (free/pro/business/enterprise)
- `created_at`

### 1.3 WorkspaceMember

- `user_id`
- `workspace_id`
- `role` (admin/analyst/viewer)

### 1.4 Connector

- `id (uuid)`
- `workspace_id`
- `type` (salesforce, marketo, csv_upload, etc.)
- `status` (connected, error, disabled)
- `config` (JSONB – redacted where needed)
- `last_sync_at`

### 1.5 AnalysisJob

- `id (uuid)`
- `workspace_id`
- `status` (queued, running, succeeded, failed)
- `created_at`
- `started_at`
- `finished_at`
- `parameters` (JSONB – filters, miners, thresholds)
- `error_message` (nullable)
- `provenance_id`

---

## 2. Process Data

### 2.1 Events Table (DuckDB + optional Postgres pointer)

- `workspace_id`
- `analysis_id`
- `global_case_id` (uuid)
- `system` (e.g. salesforce)
- `native_case_id`
- `activity`
- `timestamp`
- `resource_id`
- `role`
- `org_unit`
- `schema_version`
- `partition_id`
- `attributes` (JSON)

### 2.2 Cases Table

- `workspace_id`
- `analysis_id`
- `global_case_id`
- `start_ts`
- `end_ts`
- `cycle_time_seconds`
- `variant_id`
- `variant_hash`
- `feature_vector` (JSON/array)

### 2.3 Variants Table

- `workspace_id`
- `analysis_id`
- `variant_id`
- `variant_hash`
- `frequency`
- `avg_cycle_time_seconds`
- `is_happy_path` (bool)
- `metadata` (JSON)

---

## 3. Provenance & Explainability

### 3.1 Provenance

- `id (uuid)`
- `analysis_id`
- `workspace_id`
- `etl_steps` (JSON – ordered list of steps)
- `schemas` (JSON – fingerprint per source)
- `mining_config` (JSON)
- `explainability_config` (JSON)
- `created_at`

### 3.2 Explainability

- `id (uuid)`
- `analysis_id`
- `workspace_id`
- `scope` (global, bottleneck, case)
- `target` (metric e.g. cycle_time)
- `features` (JSON – feature importance)
- `summary` (text – narrative)

---

## 4. Storage Partitioning

- Partition by:
  - `workspace_id`
  - `analysis_id`
  - date (e.g. month of event)

- Raw data:
  - `s3://bucket/workspace_id/raw/connector_type/yyyy-mm/*.parquet`
- Normalised events:
  - `s3://bucket/workspace_id/analyses/{analysis_id}/events/*.parquet`

This model may evolve; update this document if new entities are created.
