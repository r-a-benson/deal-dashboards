# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is the RRM (Risk Resource Management) deal-dashboards project — a collection of standalone, self-contained HTML dashboard files for insurance risk analysis. Each file is a complete single-page application requiring no build step, server, or package manager.

## Running Dashboards

Open any `.html` file directly in a browser — no server needed:

```bash
open Envision_GL_Compass_Dashboard_v8branded.html
open Finneys_WC_Dashboard.html
```

There are no build, lint, or test commands. All dependencies (Chart.js) load from CDN at runtime.

## Architecture

**Single-file pattern:** Each dashboard is one HTML file with all CSS (`<style>`) and JavaScript (`<script>`) inline. There are no external `.css` or `.js` source files.

**Data model:** All claim, financial, and risk data is hardcoded as JavaScript arrays/objects in the `<script>` block near the bottom of each file. There is no API or `fetch()` — updating dashboard data means editing these inline constants directly (e.g., `const CLAIMS = [...]`).

**Charts:** All visualizations use Chart.js 4.4.x loaded from CDN. Charts are initialized against `<canvas>` elements by ID using `new Chart($('canvasId'), config)`. The `$` helper is defined as `const $ = id => document.getElementById(id)`.

**Navigation:** Tab-based navigation uses a `switchTab(name, btn)` function that shows/hides `.panel` divs and manages `.on` active states. Some charts are lazy-initialized on first tab activation (e.g., `closedChartsReady` flag pattern).

**Theming:** Newer files use CSS custom properties (`:root { --sage: ...; --coral: ... }`) for the color system. The RRM brand palette centers on navy (`#1B3A6B` / `#0A1B4A`) with coral for alerts and gold/amber for warnings.

**Drill-downs:** Interactive location/entity cards call `drillLoc(name, idx)` to filter the inline `CLAIMS` array and re-render a detail panel and Chart.js donut.

## File Inventory

| File | Dashboard | Status |
|------|-----------|--------|
| `Envision_GL_Compass_Dashboard_v8branded.html` | Envision Foods GL Compass | Current (v8) |
| `Finneys_WC_Dashboard.html` | Finney's Workers' Comp & Loss | Current |
| `Envision_Owner_Dashboard.html` | Envision Owner Summary | Current |
| `Envision_GL_Loss_Dashboard_Interactive.html` | Envision GL Loss (interactive) | Current |
| `Envision_GL_Loss_Dashboard.html` | Envision GL Loss (static) | Earlier version |
| `Envision_GL_Compass_Dashboard.html` | Envision GL Compass | Earlier version of v8 |
| `RRMCompass_Envision_Risk_Dashboard_Branded*.html` | RRM Compass Risk (v1–v3) | Earlier versions |

File versions are tracked via suffixes (`_v2`, `_v3`, `_v8branded`). The highest-versioned or `*branded` file is the current one.

## Assets

`RRM_logo.svg`, `RRM_logo.png`, and `RRM_Parent_Brand_Logo.png` are referenced inline via `<img src="...">` tags and must stay co-located with the HTML files.
