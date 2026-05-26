# 100+ Reasons to Do SKY — Evidence Atlas

An interactive, data-driven evidence explorer for peer-reviewed research on Sudarshan Kriya Yoga (SKY).

Live site: https://lakshmiganesan.github.io/SKY-Evidence-Altas/

---

## Repository Structure

```
/
├── index.html          ← Dashboard (presentation layer — never edit for data updates)
├── sky_atlas_data.csv  ← Dataset    (content layer — replace this to update content)
└── README.md
```

**Two-layer architecture:** The dashboard fetches `sky_atlas_data.csv` at runtime.
To update research content, replace the CSV only. No HTML changes needed.

---

## Updating the Dataset

1. Open `sky_atlas_data.csv` in Excel, Google Sheets, or any CSV editor
2. Add or edit rows — existing columns must keep their names exactly
3. Save as **CSV UTF-8**
4. Replace the file in the repository (same filename: `sky_atlas_data.csv`)
5. Commit and push — the live dashboard updates automatically

**The dashboard automatically:**
- Filters to rows where `Journal Reputation = Acceptable` (currently 101 studies)
- Surfaces starred studies (⭐⭐⭐⭐⭐ in the `Rating` column) at the top
- Derives all explorer nodes and counts from the live data
- Scales as the dataset grows beyond the current record count

---

## CSV Column Schema

Column names are case-sensitive. The dashboard reads these fields:

| Column | Purpose |
|--------|---------|
| `Flashcard ID` | Unique study identifier |
| `Serial No` | Display serial number |
| `Year` | Publication year |
| `Subtopic` | Comma-separated topic tags — e.g. `PH: Brain, MH: Stress` |
| `Study Population` | Population category used for filtering |
| `Highlight` | One-line research headline (primary display text) |
| `Participant Type` | Specific participant description shown in flashcard |
| `Participants (n)` | Sample size |
| `Intervention Duration` | Duration string — e.g. `6 weeks`, `Single SKY Session` |
| `Follow-up Timepoints` | Follow-up period if applicable |
| `Study Design` | RCT, Observational, Cross-Sectional, etc. |
| `Proven Benefits Statement` | Pipe-separated `\|` list of outcome bullets |
| `Journal Reputation` | `Acceptable` / `Mixed Reputation` / `Predatory` |
| `Citation Short` | Short citation — e.g. `Smith et al., 2023` |
| `Journal` | Full journal name |
| `Corresponding Author Affiliation` | Institution name |
| `Country` | Geography; comma-separate for multi-country studies |
| `Link to Published Study` | DOI or full URL |
| `Rating` | `⭐⭐⭐⭐⭐` for featured studies, blank otherwise |

**Adding new columns** is safe — the dashboard only reads the columns it uses and ignores extras.

**Adding new subtopic tags** (e.g. `PH: Metabolism`) will automatically create a new filter node
in the Physical Health Explorer on the next page load — no code changes needed.

---

## Subtopic Tag Format

Tags in the `Subtopic` column must use the prefix format below for explorer categorisation:

- **Physical Health:** `PH: Brain`, `PH: Heart`, `PH: Breath / Lungs`, `PH: Vagus Nerve`,
  `PH: Biomarkers`, `PH: Immunity`, `PH: Genes`, `PH: Diabetes`, `PH: Cancer`, `PH: Gut Health`
- **Mental Health:** `MH: Stress`, `MH: Anxiety`, `MH: Depression`, `MH: Sleep`,
  `MH: Cognition`, `MH: Happiness`
- **Social / Community:** `SH: Women`, `SH: Youth & Students`, `SH: At-risk Communities`

Multiple tags per study: separate with commas — `PH: Brain, MH: Stress, SH: Youth & Students`

---

## Duration Categorisation

The Intervention Duration panel groups studies into three segments:

| Segment | Matches |
|---------|---------|
| **Short-Term** | Single session, up to 4 weeks, up to 30 days, up to 1 month |
| **Medium-Term** | 5–12 weeks, 2–6 months, 90 days |
| **Long-Term** | > 6 months, 1+ years, long-term practice |

---

## Configuration

At the top of the `<script>` block in `index.html`:

```javascript
const CSV_FILE          = 'sky_atlas_data.csv'; // rename here if you rename the file
const FILTER_REPUTATION = 'Acceptable';          // which rows to include
```

---

## GitHub Pages Deployment

1. Push `index.html` + `sky_atlas_data.csv` + `README.md` to your repository root
2. Go to **Settings → Pages → Source → Deploy from branch → main / root**
3. Your live URL will be: `https://yourusername.github.io/your-repo/`

> **Note:** GitHub Pages serves files over HTTPS, so `fetch('sky_atlas_data.csv')` works correctly.
> Opening `index.html` via `file://` locally will fail due to browser CORS policy.

---

## Local Development

```bash
# Serve with Python (from the project folder):
python3 -m http.server 8080

# Then visit:
http://localhost:8080/
```

---

## Explorer Features

| Explorer | Behaviour |
|----------|-----------|
| **Physical Health** | Illustrated body figure with organ-system nodes on left/right; each is a live filter |
| **Mental Health** | Stylised brain illustration with floating emotional-domain nodes; each is a live filter |
| **Study Population** | Horizontal bar chart per population group; click to filter |
| **Intervention Duration** | Three segment cards (Short / Medium / Long-Term) with study counts; click to filter |
| **Global Research Spread** | D3 world map with proportional bubbles; hover for country detail, click to filter |
| **Evidence Database** | Searchable, sortable table with Line View / Card View toggle; starred studies ranked first |
| **Study Flashcard** | Slide-in drawer with full metadata, proven benefits, citation, and link to published study |
