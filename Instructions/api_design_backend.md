# API Design (Backend)

Defines core API endpoints and contracts.

---

## 1. Auth & Workspaces

### POST `/auth/signup`
- Request: `{ email, password, name }`
- Response: `{ token, user, defaultWorkspace }`

### POST `/auth/login`
- Request: `{ email, password }`
- Response: `{ token, user, workspaces }`

### GET `/workspaces`
- Returns list of workspaces for the user.

### POST `/workspaces`
- Create new workspace.

---

## 2. Connectors

### GET `/workspaces/{id}/connectors`
- List connectors.

### POST `/workspaces/{id}/connectors`
- Create connector (type + initial config).

### POST `/connectors/{id}/oauth/callback`
- Handle OAuth callback.

---

## 3. Analysis

### POST `/workspaces/{id}/analyses`
- Body: `{ connectorIds, dateRange, caseDefinition, variantThreshold, options }`
- Action: create `AnalysisJob` and enqueue.

### GET `/analyses/{id}`
- Status and metadata.

### GET `/analyses/{id}/results/summary`
- High-level KPIs and status.

### GET `/analyses/{id}/results/process-map`
- Nodes, edges, metrics for visualisation.

### GET `/analyses/{id}/results/variants`
- Paginated variant list.

### GET `/analyses/{id}/results/bottlenecks`
- Top bottlenecks and metrics.

### GET `/analyses/{id}/results/explainability`
- Global driver importance.

### GET `/analyses/{id}/results/explainability/bottleneck/{bottleneckId}`
- Detailed “Why this bottleneck?” data.

### GET `/analyses/{id}/provenance`
- Full provenance report JSON.

---

## 4. Reports

### POST `/analyses/{id}/reports`
- Generate a report (PDF or CSV).
- Body: `{ type: "pdf" | "csv", options }`

### GET `/analyses/{id}/reports/{reportId}`
- Get report status or download.

---

## 5. Error Handling

- Errors respond with:
  ```json
  {
    "error": {
      "code": "ANALYSIS_FAILED",
      "message": "Human readable message",
      "details": { ... }
    }
  }
