# SKY Evidence Atlas
### *100+ Reasons to Do SKY — An Interactive Explorer of Research-Backed Evidence on Sudarshan Kriya Yoga*

---

## Overview

The SKY Evidence Atlas is a self-contained, single-file interactive web application that makes the growing body of peer-reviewed research on Sudarshan Kriya Yoga (SKY) explorable, filterable, and legible for a broad audience — from clinicians and researchers to curious individuals and communications teams.

The interface is designed to feel less like a dashboard and more like a **contemplative research library** — calm, editorial, and scientifically credible.

---

## What It Is

A single HTML file (`sky-evidence-explorer.html`) that loads one external CSV (`sky_data.csv`) and renders a fully interactive evidence explorer with no backend, no database, and no server-side logic required.

---

## File Structure

```
sky-evidence-explorer.html   ← The entire application (335 KB, self-contained)
sky_data.csv                 ← Research data file (must be served alongside the HTML)
README.md                    ← This file
```

> **Note:** The two PNG illustrations (physical health body figure and mental health wellness figure) are base64-encoded directly inside the HTML file. No separate image assets are required.

---

## How to Run

**Locally:**
```bash
# Any static server works. Examples:
python3 -m http.server 8080
npx serve .
```
Then open `http://localhost:8080/sky-evidence-explorer.html` in a browser.

**Hosted:**
Drop both files (`sky-evidence-explorer.html` and `sky_data.csv`) into any static hosting environment — GitHub Pages, Netlify, Vercel, AWS S3, or a standard web server. No build step required.

**Important:** The HTML file fetches `sky_data.csv` via a relative path. Both files must live in the same directory and be served over HTTP/HTTPS (not opened as `file://` directly in a browser, which will block the fetch).

---

## External Dependencies

The application loads three libraries from CDN at runtime. An internet connection is required for first load; after that, browsers typically cache these.

| Library | Version | Purpose |
|---|---|---|
| D3.js | 7.8.5 | Map projection, data binding, geo rendering |
| TopoJSON | 3.0.2 | World map topology decoding |
| world-atlas | 2 | Country boundary data (110m resolution) |

**Typography** (Google Fonts, loaded at runtime):
- **EB Garamond** — serif headings and prominent statements
- **DM Sans** — interface labels and body text

---

## The Data — `sky_data.csv`

### Source
The CSV contains peer-reviewed research studies on SKY drawn from the Art of Living's internal research library. Each row is one study (flashcard).

### Key Fields Used by the Application

| CSV Column | Used As | Notes |
|---|---|---|
| `Proven Benefits Statement` | Main highlight / headline | Displayed bold as primary takeaway |
| `Participant Type` | Population filter | Maps to 7 population groups |
| `Intervention Duration` | Duration filter | Parsed into 3 time buckets |
| `Topic` / `Subtopic` | Physical & Mental Health filters | Maps to 10 PH + 6 MH categories |
| `Journal` | Card metadata | Shown on each evidence card |
| `Year` | Card metadata | Publication year |
| `Citation` | Card metadata | Author/citation string |
| `Geography (Country)` | Global Research Spread map | Parsed from author affiliation country |
| `Journal Reputation` | Quality filter | Only peer-reviewed / reputable journals shown |
| `Rating` | Default sort order | Higher-rated studies surface first |
| `URL` | Card link | Links to source publication |

---

## The Five Explorer Panels

### 1. Physical Health Explorer
An interactive anatomical body illustration (embedded PNG) with **10 clickable organ-system filter pills** arranged in two vertical columns flanking the central figure.

| Left column | Right column |
|---|---|
| Brain | Heart |
| Breath / Lungs | Biomarkers |
| Vagus Nerve | Immunity |
| Gut Health | Genes |
| — | Diabetes |
| — | Cancer |

Clicking any pill filters the evidence database to studies mapped to that body system. Multiple selections are supported (OR logic within the same panel).

### 2. Mental Health Explorer
An interactive holistic wellness illustration (embedded PNG) with **6 clickable domain filter pills** in two vertical columns.

| Left column | Right column |
|---|---|
| Stress | Anxiety |
| Happiness | Depression |
| Cognition | Sleep |

Clicking filters to studies in that psychological/emotional domain.

### 3. Study Population Explorer
A filterable list of **7 population groups** with study counts and proportional bars.

| Population | Icon |
|---|---|
| General Population | 🧑 |
| Patients | 🏥 |
| Youth & Students | 🎓 |
| At-risk Communities | 🛡️ |
| Professionals | 💼 |
| Practitioners / Meditators | 🧘 |
| Women | ♀️ |

### 4. Intervention Duration
A **3-point clickable timeline** sorting studies by how long the SKY intervention lasted.

| Bucket | Range |
|---|---|
| Short-term | ≤ 8 weeks |
| Medium-term | > 8 – 24 weeks |
| Long-term | > 24 weeks |

Duration is parsed from free-text entries in the CSV using regex matching across weeks, days, and months.

### 5. Global Research Spread
A D3-rendered **world map** with bubble clusters sized by number of studies per country, based on corresponding author affiliation. Clicking a country bubble filters the evidence database to that geography.

---

## The Evidence Database

Below the five explorer panels sits the full, filterable, paginated evidence database.

### Views
- **Card View** (default) — each study as a standalone evidence card with bold headline statement, journal, year, population type, duration tag, and a link to the source
- **Line View** — compact tabular format for scanning and sorting

### Search
Full-text search across: headline statement, population, participant type, journal, author affiliation, country, citation, year, subtopics, and benefits.

Placeholder: *"Search findings, populations, outcomes, geography, institutions…"*

### Sorting (Line View)
| Column | Sort |
|---|---|
| S.No / Rating | Default descending (highest quality first) |
| Research Highlight | Alphabetical |
| Participant Type | Alphabetical |
| Duration | Alphabetical |

### Pagination
10 studies per page. Page state resets when any filter changes.

### Filter Combinations
All five explorer panels filter simultaneously with AND logic across panels (e.g. Brain AND Stress AND Short-term AND Youth AND India). Clicking an active filter deselects it.

---

## Visual Design

### Design Language
The interface aims for the aesthetic of a **contemplative research library** or **scientific atlas** rather than a health-tech dashboard.

| Token | Value | Role |
|---|---|---|
| `--paper` | `#f8f6f1` | Page background (warm parchment) |
| `--ink` | `#1c1b18` | Primary text |
| `--teal` | `#0a7865` | Physical health / primary accent |
| `--rose` | `#7e1e3c` | Mental health accent |
| `--blue` | `#1a4880` | Population accent |
| `--amber` | `#7b5006` | Duration accent |
| `--serif` | EB Garamond | Display type, evidence statements |
| `--sans` | DM Sans | Interface, labels, metadata |

### Central Illustrations
Both explorer illustrations are anatomical/wellness illustrations embedded as base64 PNG inside the HTML. They sit in the central column of a symmetric 3-column grid alongside their filter pills. Black backgrounds are removed via luminance-based alpha masking in Python (Pillow) before embedding; white backgrounds use CSS `mix-blend-mode: multiply` for seamless panel integration.

---

## Illustration Swap Guide

To update either central illustration without touching the rest of the build:

1. Prepare your PNG at **800 × 1200 px**, transparent background, 2:3 aspect ratio
2. Run the following snippet to base64-encode it:

```python
from PIL import Image
import base64, io, numpy as np

img = Image.open('your-image.png').convert('RGBA')
# For black backgrounds — remove via luminance:
data = np.array(img, dtype=np.float32)
lum = np.maximum(np.maximum(data[:,:,0], data[:,:,1]), data[:,:,2])
alpha = np.clip((lum - 8) / 28 * 255, 0, 255).astype(np.uint8)
rgba = np.zeros((*data.shape[:2], 4), dtype=np.uint8)
rgba[:,:,:3] = img.convert('RGB')
rgba[:,:,3] = alpha
result = Image.fromarray(rgba, 'RGBA').resize((296, 444), Image.LANCZOS)

buf = io.BytesIO()
result.save(buf, format='PNG', optimize=True)
b64 = base64.b64encode(buf.getvalue()).decode()
print(b64[:80], '...')
```

3. In `sky-evidence-explorer.html`, find the `<img>` tag inside either `body-svg-center` (physical health) or `mind-center` (mental health) and replace the `src="data:image/png;base64,..."` value with your new string.

---

## Pending / In Progress

- [ ] Mental Health Explorer central illustration — final wellness figure PNG to be supplied and embedded (replacing current placeholder)
- [ ] Mental Health Explorer layout — to be restructured to 3-column grid (matching Physical Health) once final illustration is confirmed
- [ ] Potential: export / share filtered view as URL hash state

---

## Technology Stack

| Layer | Technology |
|---|---|
| Structure | HTML5 |
| Styling | Vanilla CSS (custom properties, CSS Grid, Flexbox) |
| Logic | Vanilla JavaScript (ES2020+) |
| Visualisation | D3.js v7 |
| Map data | TopoJSON + world-atlas |
| Data | CSV (fetched, parsed client-side) |
| Images | PNG (base64-encoded, embedded) |
| Fonts | Google Fonts (EB Garamond, DM Sans) |
| Build tool | None — zero build step |

---

## Browser Support

Tested and designed for modern evergreen browsers: Chrome 100+, Firefox 100+, Safari 15+, Edge 100+. Requires JavaScript enabled. Not optimised for mobile (desktop-first layout).

---

## Credits

Research data curated by the Art of Living Foundation.
Interface design and development: iterative collaborative build, May 2026.
Illustration assets: custom anatomical and wellness illustrations prepared to specification.

---

*This tool is intended for educational and informational purposes. All studies link to their original published sources.*
