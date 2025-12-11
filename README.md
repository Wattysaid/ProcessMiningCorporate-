# Explainable Process Mining SaaS

> **Clarity from Complexity.**  
> This project is a production-grade, multi-tenant, explainable **process mining SaaS** that connects to common business systems (Salesforce, Marketo, HubSpot, Jira, Xero, etc.), discovers real processes using PM4Py, and explains *why* bottlenecks and delays occur using AI and SHAP-style drivers.

---

## 🚀 What This Project Does

- **Connects** (read-only) to popular SaaS tools and file uploads (CSV/Excel).
- **Extracts & normalises** activity logs into a unified event model.
- Uses **PM4Py** to:
  - Build event logs
  - Discover process models (Inductive Miner, DFG)
  - Compute key metrics (cycle time, rework, throughput, conformance)
- Adds an **AI explainability layer** to:
  - Identify drivers behind delays and defects
  - Explain bottlenecks (“Why this bottleneck?”)
  - Generate role-specific summaries (Analyst, COO, CFO, CEO)
- Provides a **web-based dashboard** to:
  - Explore process maps and variants
  - Filter by date/system/variant thresholds
  - Inspect provenance (how the data was transformed)
  - Export reports (PDF/CSV, and later PPTX)

---

## 🧩 Core Features

- 🔌 **Integration Layer**
  - Read-only connectors for systems like Salesforce, Marketo, HubSpot, Jira
  - CSV/Excel upload for other sources
  - Date-bounded extraction and schema fingerprinting

- ⚙️ **ETL & Normalisation**
  - AI-assisted schema mapping
  - Unified event schema (case_id, activity, timestamp, resource)
  - Schema drift detection & reporting
  - Full transformation provenance log

- 🧭 **Process Mining**
  - PM4Py-based event log construction
  - Discovery via Inductive Miner & DFG
  - Variant explorer (top variants, frequencies, cycle times)
  - Performance metrics: cycle time, wait vs work, throughput, rework

- 🤖 **AI Insight & Explainability**
  - SHAP-style driver analysis (global & local)
  - “Why this bottleneck?” explanations with segment comparison
  - Counterfactual “what-if” scenarios (e.g. reduce time at a step)
  - Narrative summaries tuned per role (Analyst/COO/CFO/CEO)

- 📊 **Role-Based Dashboards**
  - **Analyst:** full detail, configuration, provenance
  - **COO:** SLAs, bottlenecks, workload distribution
  - **CFO:** cost-of-waste, ROI indicators
  - **CEO:** executive summary & top 3 improvement opportunities

- 🔐 **Governance & Compliance**
  - Multi-tenant isolation
  - Regional data residency (EU/UK/US)
  - Provenance & audit logging
  - GDPR/UK-GDPR–aligned lifecycle

---

## 🏗️ Architecture & Tech Stack

**Frontend**
- [Astro](https://astro.build/) + React
- TypeScript
- Tailwind CSS
- D3.js / Recharts for charts and process visualisation

**Backend**
- Python 3.11+ with [FastAPI](https://fastapi.tiangolo.com/)
- Celery + Redis for async jobs
- PM4Py for process mining
- SQLAlchemy with PostgreSQL

**Data & Storage**
- DuckDB + Parquet for analytics
- PostgreSQL for metadata & auth
- MinIO / S3-compatible storage for raw extracts & artefacts

**AI & Explainability**
- OpenAI / Anthropic APIs
- Optional: local inference via Ollama
- SHAP-style driver analysis for explainability

**Infra / DevOps**
- Docker / Docker Compose for local dev
- Kubernetes (or equivalent) for production
- GitHub Actions for CI/CD
- CDN/static hosting (e.g. Cloudflare) for frontend

For detailed architecture, see:

- `system_architecture.md`
- `process_mining_architecture.md`
- `process_mining_visual_architecture_diagrams.md` (Mermaid diagrams)

---

## 📂 Repository Structure (Proposed)

> This may evolve as the implementation progresses. Keep this section updated.

```text
.
├── backend/
│   ├── app/
│   │   ├── api/                # FastAPI routers
│   │   ├── core/               # config, security, logging
│   │   ├── models/             # SQLAlchemy models
│   │   ├── schemas/            # Pydantic models
│   │   ├── services/           # business logic
│   │   ├── workers/            # Celery tasks (ETL, mining, AI)
│   │   └── main.py             # FastAPI entrypoint
│   ├── tests/
│   └── pyproject.toml
│
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   ├── components/
│   │   ├── layouts/
│   │   └── lib/
│   ├── public/
│   └── package.json
│
├── infra/
│   ├── docker-compose.yml
│   ├── k8s/                    # deployment manifests (optional)
│   └── Makefile
│
├── docs/
│   ├── README.md               # (this file)
│   ├── agent.md
│   ├── coding_plan.md
│   ├── coding_guidelines.md
│   ├── ui_ux_guidelines.md
│   ├── data_model_and_storage.md
│   ├── api_design_backend.md
│   ├── process_mining_engine.md
│   ├── ai_explainability_and_reports.md
│   ├── security_and_compliance_dev.md
│   ├── testing_and_quality.md
│   ├── deployment_and_devops.md
│   ├── process_mining_saas_brainstorm.md
│   ├── process_mining_pricing_packaging.md
│   ├── process_mining_ux_wireframes.md
│   ├── process_mining_value_proposition.md
│   ├── process_mining_gtm_outreach_plan.md
│   ├── process_mining_data_governance_compliance.md
│   └── process_mining_product_marketing_narrative.md
│
└── error_log.md


---

🧠 For AI Coding Agents

This repository is designed to be agent-friendly.

Start with agent.md
This file explains your mission as an AI coding agent, required knowledge, and how to follow the project’s rules.

Follow the implementation sequence in:

coding_plan.md


Adhere strictly to:

coding_guidelines.md

ui_ux_guidelines.md

security_and_compliance_dev.md

testing_and_quality.md


Respect architecture and data model:

system_architecture.md

data_model_and_storage.md

process_mining_engine.md

api_design_backend.md

ai_explainability_and_reports.md


Log bugs and incidents in:

error_log.md




---

🧑‍💻 Getting Started (Local Development)

1. Prerequisites

Docker & Docker Compose

Node.js (LTS) + pnpm / npm / yarn

Python 3.11+


2. Clone & Bootstrap

git clone <your-repo-url>.git
cd <your-repo-folder>

Install backend dependencies (example with uv or poetry/pip):

cd backend
# e.g. using uv or poetry or pip:
# uv pip install -r requirements.txt
# or
# poetry install

Install frontend dependencies:

cd ../frontend
npm install   # or pnpm install / yarn install

3. Environment Configuration

Create .env files from provided examples (if present):

cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env

Set values for:

Database connection (PostgreSQL)

Redis URL

Object storage credentials (MinIO)

AI API keys (OpenAI, Anthropic, etc.)


4. Run with Docker Compose

From the project root:

cd infra
docker-compose up --build

This should start:

Backend API (FastAPI)

Frontend (Astro/React)

PostgreSQL

Redis

MinIO


Check:

API health: http://localhost:<api-port>/health

Frontend: http://localhost:<frontend-port>/



---

🧪 Testing

Backend

cd backend
pytest

Frontend

cd frontend
npm test

CI (via GitHub Actions) should:

Lint & type-check

Run tests

Build Docker images


For full strategy see:

testing_and_quality.md

deployment_and_devops.md



---

🔐 Security & Compliance

This project is designed with regulated industries in mind (banking, pharma, manufacturing, etc.).

Key principles:

Read-only integrations

Tenant-level data isolation

Provenance & audit logs for all analyses

Respect for data residency and GDPR/UK-GDPR


Developer-facing rules live in:

security_and_compliance_dev.md

process_mining_data_governance_compliance.md



---

🗺️ Roadmap (High-Level)

See coding_plan.md for detailed milestones. Broadly:

1. CSV-only MVP (upload → event log → basic process map)


2. SaaS connectors + provenance


3. Full PM4Py analytics & variant explorer


4. AI explainability (SHAP-like drivers) + “Why this bottleneck?”


5. Role-based dashboards & report exports


6. Hardened multi-tenant SaaS with CI/CD & monitoring




---

🤝 Contributing

Please follow coding_guidelines.md and ui_ux_guidelines.md.

Keep error_log.md updated with significant issues and resolutions.

Update documentation when APIs or models change.



---

📜 Licence

> Add your chosen licence here (e.g. MIT, Apache 2.0).
Until specified, treat this as proprietary / internal.




---

📣 Contact & Credits

This project originates from a vision to make explainable process mining accessible, transparent, and affordable — especially for organisations that find Signavio/ARIS too complex or expensive.

Strategic & product design: @<your-handle>

Architecture & docs: see /docs folder.


“Process mining made human.”

If you’d like, I can also generate a short `CONTRIBUTING.md` to complement this and set expectations for PRs, branches, and coding standards.
