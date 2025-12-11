# Coding Plan

This document defines the implementation order and milestones for building the platform.

---

## 1. Milestone Overview

1. **M1 – Foundations & Skeleton**
2. **M2 – CSV-based MVP (no connectors yet)**
3. **M3 – Connectors & ETL Enhancements**
4. **M4 – Process Mining & Analytics v1**
5. **M5 – AI Explainability v1**
6. **M6 – Full Frontend Experience (UX flows)**
7. **M7 – Security, Governance, & Multi-Tenancy**
8. **M8 – Testing, Hardening, & CI/CD**

---

## 2. Milestone Details

### M1 – Foundations & Skeleton

- Setup monorepo or clear folder structure:
  - `/backend`, `/frontend`, `/infra`, `/docs`
- Backend:
  - FastAPI skeleton with health check
  - PostgreSQL + SQLAlchemy setup
  - Alembic migrations
- Frontend:
  - Astro + React scaffold
  - Tailwind configuration
- Docker + docker-compose for local dev

**Exit criteria**
- `docker-compose up` → API + Frontend running
- Health check endpoint returns 200
- Basic landing page loads

---

### M2 – CSV-Based MVP

- Implement CSV/Excel upload endpoint + UI
- Implement ETL to:
  - Map uploaded data into unified event schema
  - Validate case_id/activity/timestamp
- Integrate PM4Py:
  - Convert to event log
  - Run Inductive Miner
  - Compute basic metrics (cycle, count)
- Frontend:
  - Simple process map visual (DFG or similar)
  - Metrics cards (number of cases, avg duration)

**Exit criteria**
- User can upload CSV
- System shows basic process map & metrics

---

### M3 – Connectors & ETL Enhancements

- Add connectors for:
  - Salesforce (Opportunities)
  - Marketo (Leads/Programs)
  - At least one ticketing tool (e.g. Jira)
- Implement:
  - OAuth flows
  - Date-bounded extraction
  - Schema fingerprinting & drift detection
- Add provenance logging for all ETL steps

**Exit criteria**
- User can connect at least one SaaS tool
- Select date range
- Run analysis with provenance available

---

### M4 – Process Mining & Analytics v1

- Enhance process mining:
  - Support DFG + Inductive Miner
  - Compute:
    - Cycle time, wait time, throughput
    - Variant discovery (top variants)
    - Conformance against discovered model
- Persist:
  - Variants and case stats in DuckDB/PostgreSQL
- Expose:
  - API endpoints for metrics & variants

**Exit criteria**
- Full variant explorer view
- Key KPIs available via API & UI

---

### M5 – AI Explainability v1

- Build feature engineering pipeline:
  - Features per case: step counts, durations, handoffs, segments (region/team)
- Integrate SHAP-style or similar:
  - Global importance for cycle time
  - Local explanations for a case/bottleneck
- Integrate LLM(s) for:
  - Narrative summaries (per role)
  - “Why this bottleneck?” explanations

**Exit criteria**
- “Why this bottleneck?” panel functional
- Role-based textual summaries rendered in UI

---

### M6 – Frontend Experience

Implement flows per `process_mining_ux_wireframes.md`:

- Onboarding wizard (workspace → connect → configure)
- Data configuration screen with filters & cost estimate
- Process explorer:
  - Map, variants, metrics, explainability panel, provenance tab
- Reports screen:
  - Export PDF/CSV
  - Schedule reports (backend placeholder OK at first)

**Exit criteria**
- End-to-end UX from signup → connect → analyse → explore → export

---

### M7 – Security, Governance, & Multi-Tenancy

- Multi-tenant workspace isolation
- RBAC (roles: admin, analyst, viewer)
- Audit logging for key actions
- Regional data residency
- Basic rate limiting

**Exit criteria**
- Tenant isolation tested
- Workspace cannot see other workspace data
- Audit log contains key events

---

### M8 – Testing, Hardening, & CI/CD

- Comprehensive tests:
  - Unit, integration, end-to-end
- CI (GitHub Actions) pipeline:
  - Lint, test, build, containerize
- CD for:
  - Frontend → Cloudflare (or equivalent)
  - Backend → container environment
- Basic performance & load tests

**Exit criteria**
- CI passing
- Staging deployment available
- Smoke tests green

---

## 3. Ongoing Tasks

- Keep `error_log.md` up to date.
- Maintain alignment of implementation with architecture & data model docs.
- Review and refactor for simplicity and clarity where possible.
