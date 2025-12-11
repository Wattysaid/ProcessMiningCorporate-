# Deployment & DevOps

---

## 1. Local Development

- Use `docker-compose` with services:
  - `backend`
  - `frontend`
  - `postgres`
  - `redis`
  - `minio`
- Provide a `Makefile` or scripts for:
  - `make dev`
  - `make test`
  - `make lint`

---

## 2. Environments

- **Local**
- **Staging**
- **Production**

Configuration differs via environment variables.

---

## 3. CI/CD

- GitHub Actions:
  - On pull request:
    - Lint, test, type-check
  - On main branch push:
    - Build images
    - Run smoke tests
- Deployment:
  - Frontend → CDN/Static host
  - Backend → container platform (Kubernetes, ECS, etc.)

---

## 4. Monitoring

- Use Prometheus-compatible metrics.
- Dashboards:
  - Request latency & errors
  - Job queue depth & duration
  - Connector error rates

---

## 5. Logging

- Structured logs (JSON).
- Centralised log aggregation.
- Include correlation IDs for analysis jobs.
