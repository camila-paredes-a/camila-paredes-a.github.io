# Copilot instructions — camila-paredes-a.github.io ✅

Purpose: Short, actionable guidance so an AI coding agent can be immediately productive in this static portfolio that embeds Vega/Vega-Lite charts.

## Big picture 🔧
- This is a **static GitHub Pages** site (repo name follows the pattern `username.github.io`). Pages are plain HTML/CSS with embedded Vega/Vega-Lite chart specs in JSON.
- Charts are embedded client-side using `vega-embed` and Vega/Vega-Lite CDN scripts (see `index.html` and `portfolio.html`). Many specs live in the repo root (e.g., `W2-chart1.json`, `wk1_chart1.json`) and some reference remote CSV/JSON.

## Where to look (quick references) 📁
- Page shells and embeds: `index.html`, `portfolio.html` (uses `vegaEmbed('#id', spec)` pattern).
- Chart specs: `wk1_chart1.json`, `wk1_chart2.json`, `W2-chart1.json`, `W2-chart2.json`, `w2-visualization.vl.json`.
- Example data: `series-091025.csv`, `CPIAUCSL_PC1.csv` and external API usage in specs (see `W2-chart1.json`).
- Data-processing examples: `8-Example-Goods-Inflation.ipynb` (Python notebook demonstrating scraping/processing and plotting steps).

## Common conventions & patterns (use these exactly) 📌
- All Vega specs include `$schema` (mostly v5; one file uses v6). Check the page's CDN versions before rendering.
- Typical encoding fields: **`date`** (temporal) and **`value`** (quantitative). Many specs assume this naming convention.
- Autosizing pattern used across specs:
  - `"width": "container"` + `"autosize": {"type": "fit", "contains": "padding"}`
- Data sources appear in two forms:
  - Remote API/CSV using `data.url` (e.g., `https://api.economicsobservatory.com/...?...vega`)
  - Inline `data.values` for small examples (see `wk1_chart1.json`).

## How to add/update a chart (step-by-step) ➕
1. Create/modify a Vega-Lite JSON spec (v5 or v6) and commit it near the page that will embed it (or in a `weekX/` subfolder).
2. If using local CSV/JSON, reference it via a relative `data.url` so it's served over HTTP.
3. Add a `<figure id="myChart"></figure>` in the HTML page and call `vegaEmbed('#myChart', 'path/to/spec.json')` in a `<script>` block.
4. Test locally over HTTP (see debugging tips below) and push to `main` to publish (GitHub Pages auto-publishes changes for this repo type).

## Important debugging notes & gotchas ⚠️
- Do not open files via `file://` in the browser — Vega/vega-embed fetches resources via XHR and will fail; use an HTTP server (see Quick dev commands).
- **Schema/CDN mismatch**: one spec (`w2-visualization.vl.json`) uses Vega-Lite v6 but HTML pages load `vega-lite@5`. Either:
  - update the page to use `vega-lite@6`, or
  - change the spec to v5-compatible syntax.
- Remote spec URLs in the repo sometimes point to `raw.githubusercontent.com/.../refs/heads/main/...`. If you rename the branch, update those raw links.
- APIs used in specs (e.g., `api.economicsobservatory.com`) accept `?vega` to return Vega-friendly JSON.

## Local dev & common commands ▶️
- Quick HTTP server (recommended):
  - `cd /path/to/repo && python3 -m http.server 8000`
  - Open: `http://localhost:8000/index.html`
- VS Code: use the **Live Server** extension for faster preview.
- Jupyter Notebook: run cells in `8-Example-Goods-Inflation.ipynb` to reproduce data wrangling steps.

## Tests, CI, build → none present
- There is no build pipeline, test suite, or bundler. Changes are static and deployed by pushing to `main` (GitHub Pages behavior for this repo name).

## When you modify files, check these files for related updates 🔍
- `index.html`, `portfolio.html` — embedding & CDN versions
- Any chart JSON you changed — ensure `$schema` matches available vega-lite version
- External `data.url` values — verify health and CORS

## Example snippets (copy/paste) ✂️
- Embed a spec in a page:

  <figure id="myChart"></figure>
  <script> vegaEmbed('#myChart', 'week3/my-chart.vl.json').catch(console.error); </script>

- Start a local server:

  python3 -m http.server 8000

## Notes for an AI code agent 🤖
- Make small, testable changes and verify the chart loads locally over HTTP.
- If you see `CORS` or `Failed to load` errors, check whether the resource is available via HTTP and whether the `$schema` and CDN versions align.
- If asked to add tests or CI, request explicit guidance — the project currently doesn't include any testing patterns to follow.

---

If anything here is unclear or you want me to include extra examples (e.g., a checklist for adding a new dataset), tell me what to add and I will iterate. ✅