# UI & UX Guidelines

This document guides how the UI should look and behave.

---

## 1. Design Principles

- **Clarity over density**: prioritise legibility and meaning.
- **Progressive disclosure**: hide advanced options behind “Advanced” toggles.
- **Role-based simplicity**: Analysts see more knobs; executives see summarised views.
- **Consistency**: same components for filters, tables, and charts across screens.

---

## 2. Layout Patterns

- **Global layout**:
  - Top bar: workspace selector, date range, user menu.
  - Left navigation: pages (Connect, Configure, Explore, Reports).
  - Main content: cards, charts, and maps.
- **Process Explorer**:
  - Header: filters (date range, systems, variant threshold).
  - Left: tab navigation (Map, Variants, Metrics, Explainability, Provenance).
  - Centre: main visual (graph or charts).
  - Right: contextual details (“Why this bottleneck?” panel).

Follow the ASCII sketches from `process_mining_ux_wireframes.md`.

---

## 3. Visual Styling

- Use Tailwind for all styling.
- Colour palette:
  - Neutral greys for base
  - 1–2 accent colours (e.g. blue for primary, amber for warnings)
- Charts:
  - Avoid excessive colours; use saturation and thickness instead.
  - Use tooltips with clear labels.
- Icons:
  - Prefer a consistent icon set (Lucide or similar).
  - Use icons sparingly to reinforce meaning.

---

## 4. Interaction Patterns

- **Filter changes**:
  - Update compute estimate and result preview.
  - Avoid blocking UI; show loading states.
- **Clickable process nodes/edges**:
  - On click, open right-hand “Why?” panel.
- **Modals**:
  - For destructive actions (delete, disconnect).
  - For connector setup (OAuth).

---

## 5. Accessibility

- Minimum AA contrast.
- Tab order must follow visual order.
- Keyboard shortcuts where appropriate (arrow keys for navigating variants).
- Provide text alternatives for graphs:
  - Summary sentences below charts.

---

## 6. Role Views

- **Analyst**:
  - Full set of filters & provenance tab.
- **COO**:
  - Focus on SLAs, bottlenecks, and backlogs.
- **CFO**:
  - Cost equivalents, waste, ROI.
- **CEO**:
  - High-level KPIs and top 3 opportunities.

Implement role views via feature flags or layout variants, not entirely separate pages.
