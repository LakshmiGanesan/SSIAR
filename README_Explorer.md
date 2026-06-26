# Timeless Compass: Interactive Evidence Explorer

A self-contained, client-side research explorer for studies on Sudarshan Kriya
Yoga (SKY), Sahaj Samadhi Meditation, Advanced Meditation Programmes, Yoga,
Ayurveda, and related interventions — built as a single static HTML page that
reads its data from a companion CSV file at load time.

## Files in this ecosystem

| File | Role |
|---|---|
| `Evidence-Explorer.html` | The application — all markup, styles, and logic in one file. |
| `Explorer.csv` | The data — one row per study, 161 records. |
| `README_Explorer.md` | This file. |

**These two files are a paired unit.** The HTML file fetches the CSV by
filename at runtime (`fetch('Explorer.csv')`), so they must always be kept
**in the same directory**, with **these exact filenames and casing**. If you
rename either file, update the `CSV_FILE` constant near the top of the
`<script>` block in `Evidence-Explorer.html` to match.

## Live location

Published together at the root of this repository's GitHub Pages site:

- App: `https://lakshmiganesan.github.io/SSIAR/Evidence-Explorer.html`
- Data: `https://lakshmiganesan.github.io/SSIAR/Explorer.csv`

Both files should sit at the repo root (not in a subfolder) unless the
`CSV_FILE` path is updated to match a new relative location.

## How it works

1. On load, the page `fetch()`es `Explorer.csv` and parses it with a small
   built-in CSV parser (handles quoted fields, embedded commas, and embedded
   line breaks inside quoted values — e.g. multi-line author affiliations).
2. Rows are filtered to **Journal Reputation = "Acceptable"** only
   (`FILTER_REPUTATION` constant) — of the 161 total rows, 126 currently meet
   this bar. This is a deliberate editorial filter, not a UI control.
3. Each remaining row is mapped into a `STUDIES` object used throughout the
   app (search, filters, sorting, card/list views, and the study detail
   drawer).
4. The **Intervention filter** (multi-select dropdown next to the search box)
   is populated dynamically from whatever distinct values appear in the
   `Intervention` column — it is never hardcoded, so adding a new
   intervention to the CSV automatically adds it to the filter.

## Updating the data

To add, edit, or remove studies, edit `Explorer.csv` directly and keep these
columns intact (column order does not matter, but names must match exactly):

- `Serial No`, `Flashcard ID`, `Year`, `Intervention ` *(note: header has a
  trailing space — the app already trims/handles this, but be aware of it if
  you script edits to the CSV)*
- `Subtopic`, `Study Population`, `Highlight`, `Participant Type`,
  `Participants (n)`, `Intervention Duration`, `Follow-up Timepoints`,
  `Study Design`, `Proven Benefits Statement`
- `Journal Reputation` — must be exactly `Acceptable`, `Mixed Reputation`, or
  `Predatory`. Only `Acceptable` rows are shown.
- `Citation Short`, `Journal`, `Corresponding Author Affiliation`, `Country`,
  `Link to Published Study`
- `Rating` — retained in the data model and used only as a tie-breaker for
  default sort order; **intentionally not displayed anywhere in the UI.**

A few formatting conventions the app relies on:

- **Multiple interventions** in one row: comma-separate them
  (e.g. `Sudarshan Kriya Yoga (SKY), Ayurveda`). Each is split out and
  appears as its own option in the Intervention filter.
- **Multiple subtopics**: comma-separated (e.g. `PH: Heart, MH: Stress`).
- **Multiple benefit statements**: pipe-separated (e.g.
  `↑ 6% increased X|↓ 12% reduced Y`). Leading `↑`/`↓` characters are used to
  colour-code the benefit as positive/negative in the study drawer.

No build step is required — just replace the CSV and refresh the page.

## Running locally

Because the page loads `Explorer.csv` via `fetch()`, opening the HTML file
directly from disk (`file://...`) will fail in most browsers due to CORS
restrictions on local file access. Serve the folder instead, e.g.:

```bash
# from the folder containing both files
python3 -m http.server 8000
# then open http://localhost:8000/Evidence-Explorer.html
```

Any static file server (Python, `npx serve`, VS Code's Live Server, GitHub
Pages, etc.) works equally well.

## Notes on the codebase

- Everything lives in one HTML file by design (CSS in `<style>`, JS in
  `<script>`) — no build tooling, no dependencies beyond the D3 CDN script
  used for the world-map visualization and Google Fonts for typography.
- The file is organized into clearly commented sections: CONFIG, TAXONOMY
  CONFIG, STATE, CSV PARSER, LOAD DATA, FILTER ENGINE, BUILD (per-explorer
  widgets), TABLE/CARD rendering, INTERVENTION FILTER, FILTER STRIP, SORT,
  RESET, DRAWER, and MASTER REFRESH + INIT.
- Responsive behavior is handled via CSS media queries at the bottom of the
  relevant rule blocks (search bar/filter layout, table column hiding on
  narrow screens, etc.) — no JS-based breakpoint logic.
