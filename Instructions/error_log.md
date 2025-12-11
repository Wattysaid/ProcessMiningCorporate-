# Error Log & Incident Register

> This file tracks significant bugs, incidents, and their resolutions.
> Keep entries concise but complete. Newest entries at the top.

---

## Template

### [YYYY-MM-DD] ERROR_ID – Short title

**Component:** (e.g. backend/api, frontend/ui, connector/salesforce, mining/pm4py, ai/explainability)  
**Severity:** low / medium / high / critical  
**Status:** open / investigating / fixed / won't-fix  

**Summary:**  
Short description of the problem.

**Impact:**  
What’s broken? Who is affected (all users, specific workspaces)?

**Root Cause (when known):**  
Technical explanation (configuration, code bug, dependency issue, etc.).

**Resolution / Fix:**  
What was changed? Include commit reference(s) if possible.

**Tests Added:**  
List tests added to prevent regression (file + test name).

**Notes:**  
Any additional context, follow-ups, or related tickets/issues.

---

## Example

### [2025-01-10] ERR-0001 – Analysis job hangs on large CSV uploads

**Component:** backend/worker  
**Severity:** medium  
**Status:** fixed  

**Summary:**  
CSV uploads with >1M rows caused Celery workers to run out of memory and hang, leaving jobs in `running` state indefinitely.

**Impact:**  
Large customers could not complete analyses; workers needed manual restart.

**Root Cause:**  
Entire CSV was loaded into memory before chunked processing; DuckDB import path was not used.

**Resolution / Fix:**  
Reworked CSV ETL to stream data in chunks directly into DuckDB. Added job timeout and failure state when exceeded.

**Tests Added:**  
- `tests/integration/test_large_csv_etl.py::test_large_csv_import_streamed`

**Notes:**  
Consider adding job size estimates and warnings in the UI for very large uploads.
