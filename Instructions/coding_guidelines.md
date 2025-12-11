# Coding Guidelines

These rules govern how all code in this project should be written.

---

## 1. General Principles

- Optimise for **readability, maintainability, and correctness** over cleverness.
- Use **explicit types** everywhere (Python type hints, TypeScript types).
- Prefer **pure functions** and small modules with clear responsibilities.
- Any non-trivial logic must be **unit tested**.

---

## 2. Backend (Python / FastAPI)

- Use **Python 3.11+**.
- Enforce typing:
  - `from __future__ import annotations`
  - Mypy (or pyright) for static checks
- Style:
  - Follow **PEP 8** and use `black` + `isort` for formatting.
- Structure:
  - `/app/api` – routers
  - `/app/models` – ORM models
  - `/app/schemas` – Pydantic models
  - `/app/services` – business logic
  - `/app/workers` – Celery tasks
  - `/app/core` – config, security, utilities
- Error handling:
  - Use custom exception classes for domain errors.
  - Map known errors to appropriate HTTP status codes.
- Logging:
  - Use `structlog` or `logging` with JSON logs in production.
  - No sensitive data (PII) in logs.

---

## 3. Frontend (Astro + React + TypeScript)

- Use **TypeScript strict mode**.
- Use **functional components** and React hooks.
- Keep components small:
  - “Smart” containers (data fetching, routing)
  - “Dumb” presentational components (UI only)
- Styling:
  - Tailwind CSS utility classes
  - Extract common UI into reusable components
- State management:
  - Prefer React Query (or similar) for server state
  - Keep local state minimal
- Accessibility:
  - Use semantic HTML
  - Provide ARIA labels and alt text
  - Ensure keyboard navigation

---

## 4. Data & Models

- Data models must be **documented** in `data_model_and_storage.md`.
- Any schema change must:
  - Have a migration
  - Update associated Pydantic schemas and TS types
- Field naming:
  - Snake_case in DB and Python
  - camelCase in JSON APIs and frontend

---

## 5. API Design

- Follow RESTful conventions.
- Use nouns for resources: `/analyses`, `/connectors`, `/workspaces`.
- Use plural resource names.
- Responses:
  - Always wrap in a JSON object (no bare arrays at top level).
  - Include pagination metadata where relevant.

---

## 6. Testing

- Minimum coverage: **80% line coverage** for backend, **core modules** for frontend.
- Test types:
  - Unit tests for pure logic
  - Integration tests for DB / external APIs
  - API tests for endpoints
- Use factories/builders for test data.
- Tests must be deterministic (no time or randomness without seeding).

---

## 7. Documentation

- All major modules must have:
  - High-level description in docstring
  - Usage example where appropriate
- Any public function or class must have:
  - Docstring describing parameters, returns, and behaviour

---

## 8. Performance

- Avoid N+1 queries.
- Use indexes for common filters (workspace_id, created_at).
- Use DuckDB for heavy analytics workloads, not Postgres.
- Use streaming where appropriate for large results.

---

## 9. Security

- Never log secrets or full tokens.
- Validate and sanitise all external input.
- Enforce auth for all non-public endpoints.
- Respect workspace boundaries for authorisation.

Refer to `security_and_compliance_dev.md` for additional rules.
