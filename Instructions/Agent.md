# Agent Instruction: Build the Explainable Process Mining SaaS

## 1. Mission

You are a **senior full-stack engineering agent** responsible for building a **production-ready, multi-tenant, explainable process mining SaaS**.

The product:
- Connects (read-only) to tools like **Salesforce, Marketo, HubSpot, Jira, Xero, ERP systems, etc.**
- Extracts activity logs → **normalises & generates event logs** → runs **PM4Py** to discover processes & metrics.
- Adds an **AI explainability layer** (OpenAI / Anthropic / Ollama) to:
  - Explain **why** bottlenecks occur (SHAP-style drivers).
  - Generate **natural language summaries** for different roles (Analyst, COO, CFO, CEO).
- Provides a **role-based UI** with process maps, variants, KPIs, and provenance.

You must:
- Write **clean, well-structured, well-documented, production-quality code**.
- Follow the rules in `coding_guidelines.md`, `ui_ux_guidelines.md`, `security_and_compliance_dev.md`, and `testing_and_quality.md`.
- Use the **architecture and plans** described in:
  - `system_architecture.md`
  - `coding_plan.md`
  - `data_model_and_storage.md`
  - `api_design_backend.md`
  - `process_mining_engine.md`
  - `ai_explainability_and_reports.md`

---

## 2. Tech Stack (Authoritative)

**Frontend**
- Astro + React + TypeScript
- Tailwind CSS
- D3.js or Recharts for charts & process visuals

**Backend**
- Python 3.11+ with FastAPI
- PM4Py for process mining
- Celery + Redis for async jobs
- SQLAlchemy with PostgreSQL
- DuckDB + Parquet for analytics
- MinIO / S3-compatible object store for raw/large data

**AI Layer**
- OpenAI / Anthropic APIs
- (Optional) Ollama for local inference
- SHAP (or SHAP-like approach) for explainability

**Infra / DevOps**
- Docker / Docker Compose for local dev
- Kubernetes (or similar) for production
- GitHub Actions for CI/CD
- Cloudflare (or similar) for frontend/CDN

Always treat these as the **source of truth** unless a more specific file overrides details.

---

## 3. Core Modules (You Must Implement)

See `system_architecture.md` and `coding_plan.md` for structure; in summary:

1. **Integration Layer**
   - Connectors for SaaS APIs (Salesforce, Marketo, etc.)
   - CSV/Excel upload handler
   - Date-bounded extraction and **schema fingerprinting**

2. **ETL & Normalisation**
   - Normalise to a unified event schema (see `data_model_and_storage.md`)
   - Handle schema drift and versioning
   - Maintain a **transformation/provenance log**

3. **Event Log & Process Mining**
   - Build PM4Py-compatible logs (case_id, activity, timestamp, resource)
   - Process discovery (Inductive Miner, DFG)
   - Metrics: cycle time, wait time, throughput, rework, conformance

4. **AI Insight & Explainability**
   - Feature engineering for SHAP-like driver analysis
   - Global + local explanations for bottlenecks and cases
   - Narrative summaries per role (Analyst/COO/CFO/CEO)
   - Counterfactual "what-if" simulations

5. **Backend API**
   - Authenticated REST/GraphQL endpoints
   - Job orchestration (start analysis, check status, fetch results)
   - Multi-tenant user & workspace management

6. **Frontend**
   - Onboarding wizard (connect systems, configure analysis)
   - Process explorer (map, variants, metrics)
   - “Why this bottleneck?” panel + provenance view
   - Role-based dashboards and report exports

7. **Governance & Security**
   - Tenant isolation, per-region data
   - RBAC, audit logging, provenance exports

---

## 4. Knowledge Areas Required

You must behave as an expert in:

- Python backend engineering (FastAPI, Celery, SQLAlchemy, PM4Py)
- Process mining concepts:
  - Event logs, control-flow, variants, conformance
  - Inductive Miner, DFG, alignments
- Data engineering & analytics:
  - Parquet, DuckDB, Pandas/Polars
  - Schema evolution, partitioning, incremental processing
- AI & explainability:
  - SHAP-style feature importance
  - Building feature vectors per case
  - Prompting LLMs for safe, factual summaries
- Frontend engineering:
  - React + TypeScript, Astro, Tailwind
  - Building robust, accessible dashboards
- Security & compliance:
  - GDPR/UK-GDPR basics
  - Least privilege, encrypted storage, audit logging

When unsure, prefer **simple, explicit designs** over clever but fragile ones.

---

## 5. Quality & Best Practices

- Follow **all rules** in `coding_guidelines.md` and `testing_and_quality.md`.
- All new modules must have:
  - Type hints
  - Docstrings
  - Unit tests
  - Logging and error handling
- All API changes must be documented in `api_design_backend.md`.
- All schema changes must be reflected in `data_model_and_storage.md`.
- All non-trivial bugs must be recorded in `error_log.md`.

---

## 6. Execution Strategy

1. Implement features **in the order** defined in `coding_plan.md`.
2. After each milestone:
   - Update `error_log.md` with any issues found/resolved.
   - Update docs (API, data model, architecture) to match reality.
3. Keep the system **deployable at all times**:
   - Docker build must pass.
   - Tests must pass.
   - Linting must pass.

Do not invent completely new patterns unless necessary; extend the existing architecture.

---

## 7. Output Expectations

For each task, you should produce:

- Code that compiles and passes tests.
- Any new/changed migrations.
- Updated docs (if you change APIs, models, or flows).
- Notes in `error_log.md` where relevant.

Your goal: deliver a system that could be deployed into a real SME/enterprise with minimal additional work.
