---

## 8. `process_mining_engine.md`

```markdown
# Process Mining Engine

Defines how PM4Py is used and structured.

---

## 1. Event Log Construction

From `events` table (DuckDB) create PM4Py event logs:

- Case ID → `global_case_id`
- Activity → `activity`
- Timestamp → `timestamp`
- Resource → `resource_id`

Use `pm4py.objects.conversion.log.converter` with:
- `case_id_glue="global_case_id"`
- `activity_key="activity"`
- `timestamp_key="timestamp"`

---

## 2. Discovery Algorithms

Support:
- Directly-Follows Graph (DFG) for quick previews
- Inductive Miner for main analysis

Store which miner and parameters were used in provenance.

---

## 3. Metrics

For each analysis:
- Cycle times per case
- Wait vs work time (via timestamps between steps)
- Throughput (cases per period)
- Variant frequency and cycle times
- Conformance metrics (alignments):
  - Deviations per case
  - Skipped/added activities

Use PM4Py’s statistics modules where appropriate.

---

## 4. Variants

Define “variant” as ordered sequence of activities per case.

- Generate variant hash (e.g. stable hash of activity sequence).
- Identify “happy path” as:
  - Most frequent variant OR
  - Configurable (in future).

Persist variant metadata.

---

## 5. Performance Data for Explainability

Generate per-case features for AI layer:

- Number of steps.
- Number of handoffs (resource changes).
- Time spent in each activity.
- Time in queue before specific activities.
- Segmentation (region/team/product if available).

Store in `Cases.feature_vector`.

---

## 6. Configuration

Allow:
- Noise threshold parameter (default 0.2)
- Variant threshold for display
- Miner choice (DFG vs Inductive)

Store configuration in `Provenance.mining_config`.
