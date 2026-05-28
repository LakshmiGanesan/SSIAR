# Timeless Compass Explorer 2.0
### README — System Architecture, Data Management & Deployment Guide

> **SSIAR · Sri Sri Institute for Advanced Research**  
> An interactive digital research library for published evidence on Ancient Systems of Knowledge.

---

## Table of Contents

1. [Overview](#1-overview)
2. [File Structure](#2-file-structure)
3. [How the System Works](#3-how-the-system-works)
4. [Deploying on GitHub Pages](#4-deploying-on-github-pages)
5. [Updating the Data](#5-updating-the-data)
6. [CSV Data Reference](#6-csv-data-reference)
   - [Column Definitions](#column-definitions)
   - [Controlled Vocabulary](#controlled-vocabulary)
   - [Topic & Subtopic Codes](#topic--subtopic-codes)
7. [Adding New Records](#7-adding-new-records)
8. [Local Development](#8-local-development)
9. [Dashboard Features](#9-dashboard-features)
10. [Troubleshooting](#10-troubleshooting)
11. [Technical Dependencies](#11-technical-dependencies)

---

## 1. Overview

Timeless Compass Explorer 2.0 is a self-contained, browser-based research dashboard. It presents a curated database of published studies on Ancient Systems of Knowledge — including Sudarshan Kriya Yoga, meditation, yoga, Ayurveda, and related practices — through an interactive three-level exploration model:

| Level | Name | Purpose |
|---|---|---|
| Bird's-Eye View | Overview | Charts, scorecards, global map — the full evidence landscape |
| Fish-Eye View | Database | Filtered, paginated table of studies |
| Worm's-Eye View | Selected Study | Deep-dive detail panel with link to source publication |

The system is designed for **zero-code data updates**. The HTML container never needs to be touched. Only the CSV file is replaced when new studies are added.

---

## 2. File Structure

Your GitHub repository should contain exactly two files:

```
your-repo/
├── TimelessCompassExplorer.html   ← the dashboard (never edit this)
└── TCE_data.csv                   ← the data (replace to update)
```

Both files must sit in the **same folder**. The HTML fetches the CSV at runtime using its relative path (`./TCE_data.csv`).

> **Naming matters.** The HTML is hardcoded to look for `TCE_data.csv`. If you rename the CSV, the dashboard will not load.

---

## 3. How the System Works

```
Browser loads TimelessCompassExplorer.html
        │
        ▼
HTML fetches ./TCE_data.csv  (via browser fetch API)
        │
        ▼
PapaParse parses CSV → each row transformed into a study object
        │
        ▼
Dashboard renders: charts, map, database table, filter system
        │
        ▼
User interacts: search, filter, select study, open publication link
```

**Why this architecture?**

Previously, all study data was embedded directly inside the HTML file as a large JSON block (~620 KB). This made every data update require editing the HTML, re-embedding data, and redeploying the whole file.

With the container–content split:
- The HTML file is ~80 KB and never changes
- The CSV file holds all the data and is the only file that ever needs updating
- Updating data on GitHub is as simple as replacing one file

---

## 4. Deploying on GitHub Pages

### Initial Setup

1. Create a new GitHub repository (can be public or private; GitHub Pages requires public for the free plan)
2. Upload both files to the repository root:
   - `TimelessCompassExplorer.html`
   - `TCE_data.csv`
3. Go to **Settings → Pages**
4. Under **Source**, select **Deploy from a branch**
5. Select branch: `main` (or `master`), folder: `/ (root)`
6. Click **Save**
7. GitHub will provide a URL such as `https://yourusername.github.io/your-repo-name/`

### Accessing the Dashboard

The dashboard will be available at:
```
https://yourusername.github.io/your-repo-name/TimelessCompassExplorer.html
```

> If you rename the HTML file to `index.html`, it will load automatically at the root URL without specifying a filename.

### Deployment Time

GitHub Pages typically takes 1–3 minutes to publish after a file change. Changes to the CSV are usually live within this window.

---

## 5. Updating the Data

### When new studies are added to the database

1. Open `TCE_data.csv` in Excel, Google Sheets, or any spreadsheet tool
2. Add new rows following the column format described in [Section 6](#6-csv-data-reference)
3. Save as **CSV (UTF-8)** — this is important; do not save as `.xlsx`
4. Go to your GitHub repository
5. Click on `TCE_data.csv`
6. Click the **pencil icon** (Edit) or drag-and-drop the new file using **Add file → Upload files**
7. If uploading: check **"Replace existing file"** or simply drop the new CSV — GitHub will overwrite it
8. Commit the change with a message such as `Add 5 new studies — June 2026`
9. The live dashboard will reflect the new data within 1–3 minutes

**No changes to the HTML are ever needed.**

### Checklist before uploading

- [ ] File is named exactly `TCE_data.csv`
- [ ] File is saved as CSV with UTF-8 encoding
- [ ] All required columns are present (see Section 6)
- [ ] No blank rows at the top of the file
- [ ] The header row is the first row (column names unchanged)
- [ ] `Record Number` values are unique integers
- [ ] `Topic - Subtopic` values use the correct code format (e.g., `MH - Anxiety & Depression`)

---

## 6. CSV Data Reference

### Column Definitions

| Column | Type | Required | Description |
|---|---|---|---|
| `Record Number` | Integer | ✅ Yes | Unique ID for each study. Must be unique across all rows. |
| `Type` | Text | ✅ Yes | Publication type. See controlled vocabulary below. |
| `Author` | Text | ✅ Yes | Author(s) in short citation format, e.g., `Sharma et al.` or `Brown and Lee` |
| `Year` | Integer | ✅ Yes | Year of publication, e.g., `2023` |
| `Title` | Text | ✅ Yes | Full title of the publication |
| `Journal / Source` | Text | ✅ Yes | Journal name or publication source |
| `Link to Published Study` | URL | Recommended | Full URL to the published paper (DOI link or direct) |
| `Abstract` | Text | Recommended | Full abstract text. Shown in the Selected Study panel. |
| `Study Design` | Text | Recommended | Methodology used, e.g., `Randomised Controlled Trial (RCT)` |
| `Topic - Subtopic` | Text | ✅ Yes | One or more topic-subtopic codes. See format below. |
| `Interventions` | Text | Recommended | Comma-separated list of interventions used |
| `Partnership` | Text | ✅ Yes | Research partnership type. One of: `Independent`, `Collaborative`, `Internal` |
| `Study Population` | Text | Recommended | Comma-separated list of population groups |
| `Author Affiliation` | Text | Optional | Institutional affiliation(s) of the authors |
| `City` | Text | Optional | City of primary author affiliation |
| `State` | Text | Optional | State/province of primary author affiliation |
| `Country` | Text | ✅ Yes | Country of primary author affiliation. Must match known country names for the map to plot correctly. |
| `Included in TC` | Boolean | Optional | Whether the study is included in the Timeless Compass (TRUE/FALSE). Currently not used to filter the dashboard — all rows are shown. |
| `Date Added` | Date/Number | Optional | Date the record was added to the database |
| `Language` | Text | Recommended | Language of the publication, e.g., `English`, `Spanish`, `Tamil` |

### Controlled Vocabulary

These fields use a fixed set of values. Using values outside these lists will not break the dashboard, but filters and charts may not behave as expected.

**Type**
```
Research Article
Review Article
Conference Proceedings
Study Protocol
Report
Book Section
Letter
Journal Article
Editorial
```

**Partnership**
```
Independent     — research conducted without SSIAR affiliation
Collaborative   — joint research with external institutions
Internal        — research conducted within SSIAR
```

**Study Population** *(comma-separate multiple values)*
```
General Population
Meditators / Practitioners
Patients
Professionals
Youth / Students
Women
At-Risk Communities
Transgender Population
Others
```

**Language**
```
English
Spanish
Tamil
```
*(New languages can be added freely — they will appear as filter options automatically)*

### Topic & Subtopic Codes

The `Topic - Subtopic` column uses a structured code format: `CODE - Subtopic Name`

Multiple topic-subtopics for a single study are separated by commas.

**Format:**
```
CODE - Subtopic Name
CODE - Subtopic Name, CODE - Subtopic Name
```

**Example:**
```
MH - Anxiety & Depression, PH - Biomarkers
```

**Topic Codes:**

| Code | Full Name | Colour in Dashboard |
|---|---|---|
| `MH` | Mental Health | Blue |
| `PH` | Physical Health | Green |
| `SH` | Social Health | Amber |

**Subtopics by Topic:**

| Code | Available Subtopics |
|---|---|
| `MH` | Anxiety & Depression · Cognition · Happiness · Sleep · Stress · Advanced Meditation Programme · Sahaj Samadhi Meditation Programme |
| `PH` | Biomarkers · Brain · Breath / Lungs · Cancer · Diabetes · Genes · Gut Health · Heart · Immunity · Vagus Nerve |
| `SH` | At-Risk Communities · Environment · Women · Youth |

> New subtopics can be introduced by simply using a new name in the CSV. They will appear automatically in the Topics chart (outer donut) and Advanced Search filter.

---

## 7. Adding New Records

### Step-by-step guide

1. Open `TCE_data.csv` in your spreadsheet tool of choice
2. Scroll to the last row and add a new row below it
3. Fill in each column according to the definitions above
4. For `Record Number`: use the next available unique integer (check what the current highest number is)
5. For `Topic - Subtopic`: use the format `MH - Anxiety & Depression` — check existing entries for exact spelling
6. Save the file as CSV (UTF-8 encoded)
7. Upload to GitHub (see [Section 5](#5-updating-the-data))

### Example new row

| Record Number | Type | Author | Year | Title | Journal / Source | Link to Published Study | Topic - Subtopic | Partnership | Country | Language |
|---|---|---|---|---|---|---|---|---|---|---|
| 500 | Research Article | Patel et al. | 2026 | Effect of SKY on cortisol in nurses | Journal of Integrative Medicine | https://doi.org/... | MH - Stress, PH - Biomarkers | Independent | India | English |

---

## 8. Local Development

The dashboard cannot be opened directly as a local file (`file://`) because browsers block `fetch()` requests for security reasons when using the `file://` protocol.

### Option A — Python (simplest, no install needed)

```bash
# Navigate to the folder containing both files
cd /path/to/your/folder

# Python 3
python -m http.server 8000

# Then open in browser:
# http://localhost:8000/TimelessCompassExplorer.html
```

### Option B — Node.js

```bash
npx serve .
# Then open the URL shown in the terminal
```

### Option C — VS Code Live Server extension

Install the **Live Server** extension in VS Code, right-click `TimelessCompassExplorer.html`, and select **Open with Live Server**.

---

## 9. Dashboard Features

### Navigation

| Feature | How to use |
|---|---|
| **Search** | Type any keyword — searches titles, authors, journals, and abstracts in real time |
| **Advanced Search** | Click to open a dropdown with filters for Topic, Subtopic, Population, Intervention, Partnership, Language, and Year Range |
| **Reset Filters** | Clears all active filters and returns to the full dataset |
| **View All** | Shows the full dashboard including charts and database |
| **View Database** | Hides the charts section and shows only the database and selected study |

### Charts (Bird's-Eye View)

| Chart | Click interaction |
|---|---|
| Explore by Year | Click a bar to filter by that publication year |
| Explore by Topics | Click an inner segment (topic) or outer segment (subtopic) to filter |
| Geo-distribution of Authors | Click a bubble to filter by that country |
| Explore by Partnership | Click a bar to filter by Independent / Collaborative / Internal |

All charts update to reflect the currently filtered dataset. Clicking an already-selected element deselects it.

### Active Filters Bar

Displays all currently applied filters as removable chips. Click **×** on any chip to remove that filter individually. Click **Clear all** to reset everything.

### Database Panel (Fish-Eye View)

Click any row to open its full details in the Selected Study panel. The first study is pre-selected on load. Use the **Rows per page** dropdown and **Prev / Next** buttons to navigate pages.

### Selected Study Panel (Worm's-Eye View)

Shows the full metadata for the selected study: title, authors, year, journal, study design, country, intervention, topics, subtopics, population, and abstract. Click **Link to Published Study** to open the original publication in a new tab.

> On mobile, the Selected Study panel opens as a full-screen overlay when a row is tapped.

---

## 10. Troubleshooting

### Dashboard shows "Unable to load data"

| Cause | Fix |
|---|---|
| CSV file is missing from the repo | Upload `TCE_data.csv` to the same folder as the HTML |
| CSV file is named differently | Rename it exactly to `TCE_data.csv` |
| Opened as a local file (`file://`) | Use a local HTTP server (see [Section 8](#8-local-development)) |
| GitHub Pages not enabled | Enable Pages in repo Settings → Pages |

### Data is not updating after CSV upload

- Wait 1–3 minutes for GitHub Pages to republish
- Hard-refresh the browser: `Ctrl + Shift + R` (Windows/Linux) or `Cmd + Shift + R` (Mac)
- Check the GitHub Actions tab in your repo to confirm the Pages build succeeded

### A new country doesn't appear on the map

The bubble map uses a built-in country → coordinates lookup. If a newly added country doesn't appear, it may not yet be in the map's coordinate table. Contact the system maintainer to add it, or raise an issue in the repository.

### Topic / Subtopic filter not working for a new entry

Check the spelling of the `Topic - Subtopic` value in the CSV. It must exactly match the format `CODE - Subtopic Name` with a space–dash–space separator (`MH - Stress`, not `MH-Stress` or `MH- Stress`). Compare against existing entries in the CSV.

### Charts appear empty after filtering

This is expected when the applied filters match zero studies. The database will show "No studies match the current filters." Click **Reset Filters** to return to the full dataset.

---

## 11. Technical Dependencies

All dependencies are loaded from public CDNs. No installation is required. An internet connection is needed for the dashboard to function (for CDN scripts and the GitHub Pages host).

| Library | Version | Purpose |
|---|---|---|
| [D3.js](https://d3js.org) | 7.9.0 | All charts (year histogram, topic donut, geo map, partnership bars) |
| [Chart.js](https://www.chartjs.org) | 4.4.1 | Supplementary charting |
| [TopoJSON](https://github.com/topojson/topojson) | 3.0.2 | World map geometry |
| [PapaParse](https://www.papaparse.com) | 5.4.1 | CSV parsing |
| [World Atlas](https://github.com/topojson/world-atlas) | 2 | Country boundary data for the map |
| [Google Fonts](https://fonts.google.com) | — | Spectral (display) + Plus Jakarta Sans (body) |

---

## Document Information

| | |
|---|---|
| System | Timeless Compass Explorer 2.0 |
| Maintained by | SSIAR — Sri Sri Institute for Advanced Research |
| Data coverage | 1997 – present · 249 records · 23 countries |
| Last updated | May 2026 |
| File | `README_TCE.md` |
