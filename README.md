# Capital Projects Portfolio Dashboard

A single-file BI dashboard demo built to mirror the kind of executive and project-level reporting a construction / capital-projects BI analyst role requires: standardized KPIs, planned-vs-actual tracking, data quality handling, and AI-assisted narrative summaries.

**[Open `index.html`](index.html)** in any browser — no build step, no dependencies, no server required.

## What it demonstrates

- **Executive + project-level dashboard views** — a portfolio summary (KPI tiles, SPI-by-project chart) and a per-project detail view (KPI cards, cost trend, task progress, risks), matching the "executive summary + drill-down" structure most PM tools use.
- **KPI design, not just display** — Schedule Performance Index and Cost Performance Index are computed from planned/actual/budget inputs using standard earned-value formulas (`SPI = actual% / planned%`, `CPI = earned value / actual cost`), so every number on the page reconciles back to the same underlying data rather than being hand-typed per view.
- **Planned vs. actual reporting** — an S-curve cost chart per project with a hover crosshair, plus task-level progress bars showing actual completion against a planned-completion tick mark.
- **Data quality handling** — a before/after example showing how messy field-report data (mixed time units, inconsistent date formats, duplicate entries, out-of-range values) gets standardized and flagged before it reaches a dashboard.
- **Practical AI/LLM use, actually wired up** — each project has an AI-generated executive narrative. By default it's a pre-written example (so the dashboard works for anyone browsing it with no setup), but clicking "Use live Claude API" and pasting an Anthropic API key makes a real call to `claude-opus-5` with that project's live KPI data and streams back a generated narrative. The key lives only in the browser's `localStorage` — it is never written to this repo, never committed, and is sent only to `api.anthropic.com`. The "How this works" disclosure shows the exact request.
- **Accessible chart design** — status colors (on track / at risk / behind) are never color-only (paired with text labels), every chart has a table-view fallback, and the palette is checked against colorblind-safe contrast targets rather than chosen by eye.

## Stack

Plain HTML/CSS/JS. Charts are hand-built inline SVG (no charting library) — this was a deliberate choice to show the underlying mechanics (scales, gridlines, hover/tooltip layers) rather than configuring a black-box chart component. Supports light/dark mode automatically.

## Data

All project, task, and risk data is synthetic — four representative capital projects (healthcare, industrial, commercial office, mission-critical/data center) spanning on-track, at-risk, behind-schedule, and completed states, so the dashboard shows the full range of status handling.

## What a production version would add

- A real ETL step (Python/pandas) ingesting source files (Excel/CSV exports from Procore, Primavera, etc.) instead of hardcoded data
- A server-side proxy holding the Claude API key (so it's never exposed client-side at all) instead of a user-supplied browser key, with narratives cached and refreshed on each reporting cycle
- Persistent storage (SQLite/Postgres) instead of data embedded in the page
