# Capital Projects Portfolio Dashboard

A single-file BI dashboard demo built to mirror the kind of executive and project-level reporting a construction / capital-projects BI analyst role requires: standardized KPIs, planned-vs-actual tracking, data quality handling, and AI-assisted narrative summaries.

**[Open `index.html`](index.html)** in any browser — no build step, no dependencies, no server required.

## What it demonstrates

- **Executive + project-level dashboard views** — a portfolio summary (KPI tiles, SPI-by-project chart) and a per-project detail view (KPI cards, cost trend, task progress, risks), matching the "executive summary + drill-down" structure most PM tools use.
- **KPI design, not just display** — Schedule Performance Index and Cost Performance Index are computed from planned/actual/budget inputs using standard earned-value formulas (`SPI = actual% / planned%`, `CPI = earned value / actual cost`), so every number on the page reconciles back to the same underlying data rather than being hand-typed per view.
- **Planned vs. actual reporting** — an S-curve cost chart per project with a hover crosshair, plus task-level progress bars showing actual completion against a planned-completion tick mark.
- **An explanation attached to every number** — the ⓘ marker beside each KPI tile, chart and panel opens a popover with the metric's definition, its formula, how to read the thresholds, and what *this* instance is telling you right now. The reading is computed, not written: the portfolio SPI tooltip works out that the weighted index (0.98) differs from the plain average (0.95) and says which projects cause the gap, rather than asserting it in prose that could go stale.
- **Full traceability from metric to source** — "Trace to source" in any popover opens a lineage drawer showing the calculation with today's numbers substituted, a table of the raw inputs with the source system, staging column and refresh timestamp behind each one, the transformation path from field entry to displayed figure, and an explicit note on what the metric *doesn't* tell you (e.g. portfolio SPI is 0.98 including a completed project worth 41% of portfolio value — active-only it's 0.96). A **Data Sources & Metric Lineage** section documents the four source files and catalogs all 14 metrics in one table.
- **Traceable AI output** — the narrative panel shows a provenance strip listing the exact figures handed to the model, each a shortcut into that metric's lineage, plus the resolved prompt text. Any claim in the generated summary can be checked against the numbers that produced it.
- **Data quality handling** — a before/after example showing how messy progress-report data (mixed time units, inconsistent date formats, duplicate entries, out-of-range values) gets standardized and flagged before it reaches a dashboard.
- **Practical AI/LLM use, actually wired up** — each project has an AI-generated executive narrative. By default it's a pre-written example (so the dashboard works for anyone browsing it with no setup), but clicking "Use live Claude API" and pasting an Anthropic API key makes a real call to `claude-opus-5` with that project's live KPI data and streams back a generated narrative. The key lives only in the browser's `localStorage` — it is never written to this repo, never committed, and is sent only to `api.anthropic.com`. The "How this works" disclosure shows the exact request.
- **Accessible chart design** — status colors (on track / at risk / behind) are never color-only (paired with text labels), every chart has a table-view fallback, and the palette is checked against colorblind-safe contrast targets rather than chosen by eye. Tooltips open on hover *and* keyboard focus, pin on click, and dismiss on Escape; the lineage drawer is `inert` when closed and traps focus when open.

## How the definitions stay honest

Metric definitions, formulas, source mappings and thresholds live in one registry (`SOURCES`, `FIELDS`, `METRICS`) near the top of the script. The tiles, the SVG charts, the status colors, the tooltips, the lineage drawer and the metric catalog table all render *from that registry* — so a metric's documentation cannot drift from the number it describes, because they are the same object. Portfolio figures likewise come from a single `PORTFOLIO` roll-up rather than being recomputed per view.

## Stack

Plain HTML/CSS/JS. Charts are hand-built inline SVG (no charting library) — this was a deliberate choice to show the underlying mechanics (scales, gridlines, hover/tooltip layers) rather than configuring a black-box chart component. Supports light/dark mode automatically.

## Data

All project, task, and risk data is synthetic — four representative capital projects (healthcare, industrial, commercial office, mission-critical/data center) spanning on-track, at-risk, behind-schedule, and completed states, so the dashboard shows the full range of status handling.

## What a production version would add

- A real ETL step (Python/pandas) ingesting the source workbooks and CSV extracts instead of hardcoded data, with the lineage metadata (refresh timestamps, row counts, flag rates) emitted by the pipeline rather than declared in the page
- A server-side proxy holding the Claude API key (so it's never exposed client-side at all) instead of a user-supplied browser key, with narratives cached and refreshed on each reporting cycle
- Persistent storage (SQLite/Postgres) instead of data embedded in the page
