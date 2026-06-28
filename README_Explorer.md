# Timeless Compass: Evidence Explorer — Maintenance Guide

This app is two files that must always sit in the same folder:

```
Evidence-Explorer.html   ← app shell, styles, logic
Explorer.csv             ← the data (one row per study)
```

Published together at:
- https://lakshmiganesan.github.io/SSIAR/Evidence-Explorer.html
- https://lakshmiganesan.github.io/SSIAR/Explorer.csv

Open via a local server, not `file://` — CSV fetches are blocked under the
`file://` protocol by most browsers.

---

## Code structure (for anyone editing the HTML file)

`Evidence-Explorer.html` is organized top-to-bottom in the order the page
reads, with a `── SECTION ──` banner comment marking each part:

**`<style>` block:**
1. Reset + design tokens (CSS variables for color, type, radius)
2. Masthead (title, About/Help nav, panels)
3. Sticky search bar + intervention filter + info tooltips
4. Page layout shell
5. The four "Explorer" widgets (Body, Mind, Population, Duration, Map)
6. Evidence Database (table view, flashcard/card view, pagination)
7. Flashcard drawer (the slide-out detail panel)
8. Footer

**`<script>` block:** CSV parsing → data loading → taxonomy/filter logic →
each Explorer widget's render function → table/card rendering → drawer →
search/filter event handlers → tooltips → masthead panels → master
refresh/init. Every function and handler sits under its own banner comment.

A cleanup pass (June 2026) removed a handful of CSS rules that were defined
but never actually used by any element on the page (`.exp-eyebrow`,
`.exp-title`, `.exp-hint`, `.geo-note`, `.mnode-ico`, `.mnode-n`,
`.ms-line2`) — confirmed unused by checking the full file, not just
guessed. No visual or functional change resulted from removing them.

---

## Updating the "Last updated" date in the footer

The footer shows a **"Last updated: [date]"** note. Because this is a static
HTML file (no server, no database), it has no way to automatically detect
when a file changed — there's no file-system timestamp it can read. So this
date is a **manual flag** you update by hand, any time you:

- replace or edit `Explorer.csv` with new/revised study data, **or**
- edit `Evidence-Explorer.html` itself (new features, fixes, design changes)

### How to update it

1. Open `Evidence-Explorer.html` in a text editor.
2. Find this line near the top of the `<script>` block (search for
   `DATA_LAST_UPDATED`):

   ```js
   const DATA_LAST_UPDATED = 'June 28, 2026';
   ```

3. Change the date string to today's date, in the same `Month DD, YYYY`
   format, e.g.:

   ```js
   const DATA_LAST_UPDATED = 'July 3, 2026';
   ```

4. Save the file. The footer updates automatically from this one constant —
   no other edits needed.

**Rule of thumb:** if you touched `Explorer.csv` or `Evidence-Explorer.html`
today, update this line today. It's the only step required.

---

## Other things to remember when editing

- **Renaming files:** if you rename `Explorer.csv`, update the `CSV_FILE`
  constant right above `DATA_LAST_UPDATED` in the same script block.
- **Journal Reputation filter:** only rows where `Journal Reputation` equals
  the value in `FILTER_REPUTATION` (currently `"Acceptable"`) are shown in
  the app. Rows with other reputations are silently excluded — if a study
  isn't appearing, check this column first.
- **Study Design tooltips:** the flashcard drawer shows a plain-language
  description next to each study's "Study Design" value (e.g. what a
  "Quasi-Experimental Study" actually is). These descriptions live in the
  `DESIGN_GUIDE` object in the script block. If `Explorer.csv` ever
  introduces a new Study Design value that isn't in `DESIGN_GUIDE`, the
  value will still display correctly — it just won't have a tooltip until
  you add an entry for it.
