# Unified Tagging Taxonomy — README

This file documents the tagging system shared across the SSIAR site's Research,
Reflections, and Resources (R-R-R) sections, and the two live data dashboards
(`Evidence-Explorer.html`, `Research-Library.html`). Read this before adding a
new tag value or wiring up a new content type to the taxonomy.

## What this is for

Every study, essay, report, or video across SSIAR's site can be tagged against
the same small vocabulary. Because the vocabulary is shared, a "Related
Content" module can pull relevant items from *any* section by matching on
shared tags — a Reflections essay tagged `Mental Health - Stress` can surface
Evidence Explorer studies with that same tag, and vice versa, with no manual
linking required.

## The three facets

Only three tag facets are meant to run across the whole site:

| Facet | Example | Lives in |
|---|---|---|
| **Topic - Subtopic** | `Mental Health - Stress` | `Explorer.csv` → `Topic - Subtopic`; `Library.csv` → `Topic - Subtopic` |
| **Study Population** | `Meditators / Practitioners` | `Explorer.csv` / `Library.csv` → `Study Population` |
| **Intervention** | `Sudarshan Kriya Yoga (SKY)` | `Explorer.csv` → `Intervention `; `Library.csv` → `Interventions` |

Nothing else is shared site-wide. Geography, Partnership, Contributing
Institution, Journal Reputation, Study Design, and Language stay local to
the two dashboards — they describe a study's provenance, not its subject
matter, and have no equivalent for a Reflections essay or a Resources video.

**Content Format is deliberately not a tag.** The section a piece of content
lives in (Research / Reflections / Resources / Multimedia) already tells any
"Related Content" module what kind of card to render — adding a redundant
format tag would just be one more thing to keep in sync.

## `taxonomy.csv` is the source of truth

`taxonomy.csv` (in this repo, alongside `Explorer.csv` and `Library.csv`)
lists every currently-allowed value for all three facets, with a `Status`
column (`Active` / `Seed`) and a `Notes` column explaining anything
non-obvious.

**To add a new tag value:** add it to `taxonomy.csv` first, then use the
identical string in the CSV data. Both dashboards build their filter
dropdowns dynamically from whatever appears in the data (not from a
hardcoded list), so most new tag values need no code change — just a
`taxonomy.csv` entry plus a data update. The one exception is Evidence
Explorer's Body/Mind visual widgets, which use fixed layouts (see "Known,
deliberate gaps" below) — a new subtopic there needs a small code change to
actually appear in the visual, even though it'll still be filterable via
search either way.

## Format rules

- **`Topic - Subtopic` is one hyphenated string, not two columns.** Always
  `"Mental Health - Stress"`, never `"MH: Stress"`, `"MH - Stress"`, or a
  bare `"Stress"` with no topic. Keeping it atomic is what makes exact-match
  cross-linking reliable, and it's what fixed the earlier "SKY" appearing as
  a stray, meaningless entry in a dropdown (that bug came from a topic value
  and an intervention value getting cross-contaminated during parsing when
  the format wasn't consistent).
- **Multi-value cells are comma-separated** within a single CSV cell (e.g. a
  study tagged both Stress and Sleep: `"Mental Health - Stress, Mental
  Health - Sleep"`).
- **The four Topic domains are always spelled out in full:** `Mental
  Health`, `Physical Health`, `Social Health`, `Planetary Health`. No
  abbreviations.
- **Population and Intervention values must match `taxonomy.csv` exactly**
  — same spelling, casing, and punctuation. If Explorer and Library ever
  disagree on how to spell the same population or intervention, `Library.csv`
  is the older resource and its spelling wins as canonical (this is why, for
  example, the canonical form is `Youth / Students` and `Meditators /
  Practitioners`, not Explorer's earlier reversed/differently-punctuated
  versions).

## Known, deliberate gaps

These are intentional design decisions, not bugs — please check with the
content owner before "fixing" any of them:

- **Social Health is tagged in the data (49 rows combined across both
  files as of the last audit) but is not shown in Evidence Explorer's
  Body/Mind widgets.** Those two widgets use fixed hexagonal layouts (11
  body nodes, 6 mind nodes) built for Physical and Mental Health only;
  adding Social Health is a layout redesign, not a data fix. Social-Health–
  tagged studies are still fully searchable in Evidence Explorer and appear
  automatically in Research Library's topic chart, which builds its topic
  list dynamically from the data rather than a hardcoded list.
- **`Mental Health - Intuition` is tagged in the data but excluded from
  Evidence Explorer's Mind widget** for the same fixed-layout reason (the
  widget has exactly 6 node slots). Same caveat: still searchable, still
  shown in Research Library.
- **Planetary Health (`Planetary Health - Environment`) exists in the
  taxonomy and has tagged rows in Library.csv, but none yet in Explorer.csv.**
  Nothing to fix here — it'll appear in Explorer automatically once a study
  is tagged with it, since Explorer's filter/search logic isn't hardcoded to
  a fixed topic list (only the two visual widgets are).

## A note on the retired `Flashcard ID` column

Earlier audits flagged that `Explorer.csv`'s `Flashcard ID` column had 8 pairs
of rows sharing duplicate values across distinct studies. On review, this
column turned out to be unused for anything user-facing or cross-file — no
external linking, no citation display, no join with `Library.csv`.
`Evidence-Explorer.html` only ever used it as an internal DOM handle (to look
up which study a clicked row/card corresponded to), and that's now done with
`Serial No` instead, which has no duplicates. **`Flashcard ID` can be safely
deleted from `Explorer.csv`** — nothing in either dashboard reads it anymore.

## Files in this repo related to the taxonomy

- `taxonomy.csv` — canonical tag list (this is what to check before adding
  any new tag)
- `Explorer.csv` — Evidence Explorer's data, using this taxonomy
- `Library.csv` — Research Library's data, using this taxonomy
- `Evidence-Explorer.html` — see the developer header comment at the top of
  the file for the app-specific version of this taxonomy summary, plus the
  full architecture map
- `Research-Library.html` — same, in its own developer header comment
