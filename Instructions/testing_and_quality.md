# Testing & Quality Strategy

---

## 1. Testing Levels

- **Unit Tests**:
  - Pure functions, services, feature engineering.
- **Integration Tests**:
  - DB interactions
  - Connectors (with mocked external APIs)
  - PM4Py pipeline integration
- **API Tests**:
  - Auth, analysis creation, result retrieval.
- **E2E Tests** (later):
  - Happy path from sign-up to report.

---

## 2. Tools

- Backend:
  - pytest
  - coverage
- Frontend:
  - Vitest / Jest
  - Testing Library (React)
- E2E:
  - Playwright or Cypress (optional but recommended)

---

## 3. Quality Gates

- CI must run:
  - Linting (backend + frontend)
  - Tests (backend + frontend)
  - Type checking (mypy/pyright + TS)
- Thresholds:
  - Coverage ≥ 80%
  - No critical/high lint violations

---

## 4. Test Data

- Use synthetic data in tests.
- Avoid real customer data.
- Provide fixtures for:
  - Simple event logs
  - Multi-variant processes
  - Bottlenecks and deviations

---

## 5. Regression Prevention

- When a bug is found:
  - Add a failing test reproducing it.
  - Fix the bug.
  - Mark the bug as resolved in `error_log.md`.
