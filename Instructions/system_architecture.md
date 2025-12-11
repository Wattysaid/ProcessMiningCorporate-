# System Architecture

This document guides the implementation of the overall system design. It turns the conceptual architecture into concrete components and boundaries.

---

## 1. High-Level Layout

The system is composed of the following layers:

1. **Frontend (Astro + React)**
2. **API Gateway / Backend (FastAPI)**
3. **Integration & ETL Layer**
4. **Process Mining & Analytics (PM4Py)**
5. **AI Insights & Explainability**
6. **Data Storage (PostgreSQL, DuckDB, MinIO)**
7. **Asynchronous Processing (Celery + Redis)**
8. **Auth & Security**
9. **Monitoring & Logging**

See `process_mining_visual_architecture_diagrams.md` (already created) for mermaid diagrams.

---

## 2. Services & Responsibilities

### 2.1 Frontend Service

- Serves the main SPA (Astro + React) for:
  - Onboarding & connectors
  - Analysis configuration
  - Dashboards & explainability views
- Communicates with backend via JSON APIs over HTTPS.

### 2.2 API Gateway (FastAPI)

- Provides:
  - Auth endpoints (login, signup, workspace switch)
  - Connector management
  - Analysis job creation & status endpoints
  - Data retrieval for dashboards and reports
- Validates JWTs and enforces RBAC.

### 2.3 Integration & ETL Workers

- Perform:
  - Data extraction from SaaS APIs (Salesforce, Marketo, etc.)
  - CSV/Excel ingestion
  - Schema fingerprinting & drift detection
  - Transformation to unified event schema

### 2.4 Process Mining Engine

- Converts normalised tables into PM4Py event logs.
- Runs:
  - Discovery (Inductive Miner / DFG)
  - Performance analysis (cycle/ wait/ throughput)
  - Conformance (alignments) for deviations
- Stores:
  - Process models
  - Metrics
  - Variants

### 2.5 AI Insight & Explainability

- Receives:
  - Case-level features
  - Process metrics
  - Segment metadata (region, team, product, etc.)
- Produces:
  - Global driver rankings for cycle time & defects
  - Local explanations per bottleneck & variant
  - Narrative summaries per role
  - Counterfactual “what-if” evaluations

### 2.6 Storage Layer

- **PostgreSQL**
  - Users, workspaces, connectors
  - Provenance & audit logs
  - Metadata for jobs, schemas, models

- **DuckDB + Parquet**
  - Normalised event tables
  - Case/variant tables
  - Feature matrices

- **MinIO / S3-compatible**
  - Raw extracts
  - Snapshots & exports (reports, logs)

---

## 3. Data Flow (Implementation Perspective)

1. User configures **analysis** in UI → POST `/analyses/`
2. API creates job record + enqueues Celery task.
3. ETL Worker:
   - Pulls data from connectors or file uploads
   - Writes raw → normalised → event-level data
   - Logs all transformations in provenance table
4. Mining Worker:
   - Builds PM4Py event log
   - Discovers model & metrics
   - Persists results in DuckDB/PostgreSQL
5. AI Worker:
   - Builds feature matrix
   - Computes SHAP-style attributions (or analogous)
   - Calls LLM(s) to generate narratives
6. Frontend polls or subscribes for completion & loads results.

---

## 4. Multi-Tenancy & Isolation

- All tables keyed by `workspace_id`.
- Object store prefixes per tenant.
- Analysis jobs must **never** mix data between workspaces.
- Data residency (EU/UK/US) is set at workspace creation; ETL & storage must respect region.

---

## 5. Configuration & Secrets

- Use environment variables for:
  - DB credentials
  - Object storage endpoints
  - AI API keys
  - Redis/Celery settings
- Never hard-code secrets.
- Provide `.env.example` but not `.env`.

---

## 6. Extensibility

- New connectors should:
  - Implement a standard `BaseConnector` interface.
  - Output a standard schema `events_raw` which is then normalised.
- New explainability methods should:
  - Implement a standard `BaseExplainabilityStrategy`.

Any new component must be documented in this file when added.
