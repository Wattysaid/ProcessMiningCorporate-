# AI Explainability & Reports

Defines how explainability and AI summarisation work.

---

## 1. Explainability Goals

- Show **global** drivers of cycle time and bottlenecks.
- Show **local** explanations for:
  - A specific bottleneck (edge/activity)
  - A specific case
- Provide **narrative summaries** tailored to roles.

---

## 2. Feature Engineering

From `Cases` and `Events` build features:

- `total_steps`
- `total_handoffs`
- `time_in_activity_X`
- `time_between_activity_X_and_Y`
- `segment_region`, `segment_team`, etc.
- `variant_is_happy_path`

Store computed features in DuckDB for reuse.

---

## 3. Driver Importance (SHAP-like)

You may:

- Use a tree-based model (e.g. XGBoost / LightGBM) or linear model for cycle time prediction.
- Use SHAP or permutation importance to get feature contributions.

Outputs:
- Global feature importance: list of features with scores.
- Local contributions for a specific case/bottleneck.

Store in `Explainability.features`.

---

## 4. “Why This Bottleneck?” Panel Data

For a given bottleneck:

- Summary metrics:
  - Mean/median time for that edge
  - % of cases affected
  - Trend over time
- Segment comparison:
  - Boxplots / distributions by segment (team/region/product)
- Key drivers:
  - Top features for long duration at that edge
- Counterfactual:
  - Predicted cycle time if time at bottleneck is reduced

The API should return a structured payload for the frontend.

---

## 5. Narrative Summaries (LLM)

For each analysis:

- Executive Summary (CEO)
- Operational Summary (COO)
- Financial Summary (CFO)
- Technical Summary (Analyst)

Use prompt templates referencing:
- Top KPIs
- Top bottlenecks
- Key drivers & counterfactuals
- Confidence & coverage

LLMs must not hallucinate data – use only the provided metrics as facts.

---

## 6. Reports

PDF/PowerPoint-style structure:

- Title & context
- Summary KPIs
- Top 3 opportunities
- Process map screenshot (URL or pre-render)
- “Why this bottleneck?” examples
- Data & provenance appendix

The backend should generate a JSON representation of the report that can be rendered server-side or client-side.
