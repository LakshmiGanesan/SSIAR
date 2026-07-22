# Timeless Compass: Evidence Finder

Interactive evidence dashboard for Ancient Systems of Knowledge (ASK) research, built for the **Sri Sri Institute for Advanced Research (SSIAR)**.

Live: `https://lakshmiganesan.github.io/SSIAR/Evidence-Explorer.html`

Part of the **Timeless Compass Suite**, alongside its sister app `Research-Library.html` (shares the same design tokens and tagging taxonomy; not included in this package).

---

## Files in this package

| File | Purpose |
|---|---|
| `Evidence-Explorer.html` | The dashboard itself — a single-file HTML5 app (vanilla JS, D3.js, TopoJSON via CDN). Fetches `Explorer.csv` at runtime. |
| `Explorer.csv` | The evidence database. Every row is one study. |
| `README.md` | This file. |

**Deployment:** drop both files in the same folder on GitHub Pages (or any static host). Must be served over `http://`/`https://` — opening via `file://` will block the CSV fetch in most browsers.

---

## What changed in this update (July 22, 2026)

1. **Renamed** the dashboard from *"Timeless Compass: Evidence Explorer"* to **"Timeless Compass: Evidence Finder"** — updated in the page title, the visible heading, the About panel copy, and internal code comments.
2. **Removed the "Acceptable"-only Journal Reputation filter.** The dashboard previously loaded only rows where `Journal Reputation == "Acceptable"` (128 of 162 rows). That filter has been deleted from the code (`loadData()`), so all 162 rows now load and display regardless of Journal Reputation (Acceptable / Mixed Reputation / Predatory). The `Journal Reputation` column itself is untouched and still visible in the data model.
3. **Interventions expanded, no code change needed.** The Intervention filter dropdown (and the "N Interventions Selected" chip logic) has always built its list dynamically from whatever values appear in the CSV's `Intervention` column — it's never hardcoded. Once the Acceptable-only filter was removed, the dropdown automatically surfaced all interventions now present in the data, including the newer additions: *Ayurveda, Advanced Meditation Programme (AMP), Sahaj Samadhi Meditation (SSM), Prajñā Yoga / Intuition Programme (IP), Youth Empowerment and Skills (YES!+), Youth Empowement Seminar (YES!), Youth Leadership Training Programme (YLTP), Utkarsha Yoga,* and *Gut Health Meditation*.
4. **Added a "Meditation" catch-all tag.** Several specific meditation techniques (AMP, SSM, Gut Health Meditation) are now *also* comma-tagged with the generic value `Meditation` in the CSV, so selecting "Meditation" in the filter surfaces all meditation-type studies together, in addition to each technique remaining separately selectable. (By design, *Prajñā Yoga / Intuition Programme (IP)* was **not** included in this catch-all.)
5. **Data correction:** the `Gut Health Meditation` tag had been mistakenly attached to Serial No. 139 (a high-altitude acclimatisation study). It has been moved to Serial Nos. 162 and 163 — the two Vaishvanara Agni Meditation studies that are actually about gut health.

### Current Intervention tag counts (Explorer.csv, 162 rows)

| Intervention | Rows |
|---|---|
| Sudarshan Kriya Yoga (SKY) | 130 |
| Meditation *(catch-all)* | 16 |
| Yoga | 9 |
| Youth Empowerment and Skills (YES!+) | 9 |
| Advanced Meditation Programme (AMP) | 8 |
| Ayurveda | 8 |
| Youth Empowement Seminar (YES!) | 8 |
| Sahaj Samadhi Meditation (SSM) | 7 |
| Prajñā Yoga / Intuition Programme (IP) | 4 |
| Gut Health Meditation | 2 |
| Utkarsha Yoga | 1 |
| Youth Leadership Training Programme (YLTP) | 1 |

*(Rows can carry more than one Intervention tag, so these counts don't sum to 162.)*

---

## Data model notes (for whoever maintains Explorer.csv next)

- **Three cross-cutting facets** are shared site-wide with the rest of SSIAR's content (Research-Library.html, Reflections essays, Resources): `Topic - Subtopic`, `Study Population`, and `Intervention`. Everything else in the CSV (Geography, Journal Reputation, Study Design, etc.) is local to this dashboard only.
- **Multi-value fields** (Topic - Subtopic, Study Population, Intervention) are comma-separated within a single CSV cell when a study spans more than one category.
- **Intervention** needs no code change to add a new value — just use the new string in the CSV and it will appear in the filter dropdown automatically. (The `Intervention ` CSV header has a trailing space; the code matches either form.)
- **Known, deliberate layout gaps** (not bugs): "Social Health" and "Mental Health - Intuition" are tagged in the data but intentionally excluded from the Body/Mind hexagon widgets, which use fixed 11-node/6-node layouts. Social-Health- and Intuition-tagged studies remain fully searchable and appear in the results table/cards regardless.
- **Default sort** is by the `Rating` column (starred rows first), though `Rating` itself is never rendered in the UI.

---

## Local development

Serve the folder with any static server, e.g.:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000/Evidence-Explorer.html`.
