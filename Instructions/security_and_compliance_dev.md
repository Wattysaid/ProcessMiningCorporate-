# Security & Compliance (Developer Guidelines)

---

## 1. Authentication & Authorisation

- Use JWTs (access tokens with expiry).
- All non-public endpoints require auth.
- Authorisation must check:
  - User is member of workspace
  - User has correct role for action

---

## 2. Data Protection

- Encrypt data at rest (DB-level or disk).
- Always use HTTPS/TLS in transit.
- Do not log PII or raw payloads from external systems.

---

## 3. Multi-Tenancy

- All queries must be scoped by `workspace_id`.
- Never process or return data for a workspace the user is not part of.
- Tests must assert isolation.

---

## 4. External Integrations

- Use least-privilege scopes (read-only).
- Safely store tokens (encrypted at rest).
- Refresh tokens securely; handle revocation gracefully.

---

## 5. Governance & Provenance

- Every analysis must create a `Provenance` record.
- Never silently ignore transformation errors; log and link to analysis error.

---

## 6. Compliance Notes

- Support GDPR/UK-GDPR:
  - Data deletion endpoint for workspaces.
  - Export functionality for user/analysis data.
- Respect regional residency.
