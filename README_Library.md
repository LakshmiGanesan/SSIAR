# Timeless Compass: Research Library
## Developer Documentation & Integration Guide

**Version:** 1.0 Production Ready  
**Last Updated:** June 29, 2026  
**Built for:** Sri Sri Institute for Advanced Research (SSIAR)

---

## Table of Contents
1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Container-Content System](#container-content-system)
4. [File Structure](#file-structure)
5. [Responsive Design](#responsive-design)
6. [Development Guide](#development-guide)
7. [Updating Content](#updating-content)
8. [Integration Instructions](#integration-instructions)
9. [Customization](#customization)

---

## Overview

**Timeless Compass: Research Library** is an interactive digital library of global studies on Ancient Systems of Knowledge (ASK). It connects ancient wisdom with modern scientific inquiry through an intuitive, responsive web application.

### Key Features
- 🔍 Full-text search across 250+ research studies
- 📊 Interactive visualizations (charts, timelines, geographic maps)
- 🎯 Advanced filtering (9+ dimensions: topic, population, duration, etc.)
- 📱 Fully responsive design (desktop, tablet, mobile)
- 🔗 Direct access to published peer-reviewed papers
- ⚡ Zero-framework architecture (vanilla JavaScript, pure CSS)

---

## Architecture

### System Design

```
┌─────────────────────────────────────────────┐
│     Timeless Compass Research Library       │
├─────────────────────────────────────────────┤
│                                             │
│  ┌─ CONTAINER (HTML/CSS/JS)               │
│  │  Research-Library.html (single file)    │
│  │  • UI components                        │
│  │  • Application logic                    │
│  │  • Rendering engine                     │
│  │                                         │
│  └─ CONTENT (CSV Data)                    │
│     Library.csv (external file)            │
│     • 250+ research records                │
│     • Independently updatable              │
│     • No HTML modification needed          │
│                                             │
└─────────────────────────────────────────────┘
```

### Why This Architecture?

This **Container-Content separation** provides:

✓ **Content Independence:** Update research data without touching HTML  
✓ **Scalability:** Add 1000s of records without file bloat  
✓ **Maintainability:** Researchers can update CSV, developers maintain code  
✓ **Modularity:** Content can live in database, API, or file system  
✓ **Single Deployment:** One HTML file (no build process needed)  

---

## Container-Content System

### How It Works

```
1. USER VISITS WEBSITE
   ↓
2. Browser loads: Research-Library.html (single file, ~2100 lines)
   ↓
3. On DOMContentLoaded:
   - HTML structure rendered
   - CSS styles applied
   - JavaScript initializes
   ↓
4. loadLibraryData() function:
   - Fetches Library.csv from GitHub (or your server)
   - Parses CSV into JavaScript objects
   - Derives filter options from data
   ↓
5. initializeFilterOptions():
   - Extracts unique topics, populations, interventions, etc.
   - Builds filter UI dynamically
   - No hardcoded filter lists
   ↓
6. renderUI():
   - Tables, charts, and filters appear
   - All driven by CSV data
   ↓
7. User interacts:
   - Search, filter, sort, paginate
   - All operations on in-memory data
   - No server calls needed
```

### Key Advantage: Zero Code Changes

To update research:
1. Edit **Library.csv** (just a spreadsheet)
2. Upload to GitHub (or your server)
3. **Done.** Users see new data immediately (no HTML modification)

To add new research topic that appears in filters:
- Just add it to the CSV
- Filter options auto-generate
- No code changes required

---

## File Structure

### Single-File Architecture

```
Research-Library.html (2100+ lines)
│
├─ HEAD (lines 1-150)
│  ├─ Meta tags
│  ├─ Google Fonts (EB Garamond, DM Sans)
│  ├─ CDN scripts (Chart.js, D3.js, TopoJSON)
│  └─ CSS Variables & Design Tokens
│
├─ CSS STYLES (lines 150-620)
│  ├─ Base styles and resets
│  ├─ Design system (colors, typography, spacing)
│  ├─ Component styles
│  │  ├─ Header
│  │  ├─ Modals (About, Help)
│  │  ├─ Controls (search, filters)
│  │  ├─ Scorecard cards
│  │  ├─ Charts
│  │  └─ Database table
│  └─ Responsive breakpoints (600px, 960px, 1200px)
│
├─ HTML STRUCTURE (lines 620-920)
│  ├─ Header
│  │  ├─ Title
│  │  └─ Navigation buttons (About, Help)
│  ├─ Modals
│  │  ├─ About panel (mission, three views)
│  │  └─ Help panel (tutorial, examples)
│  ├─ Control bar
│  │  ├─ Search box
│  │  └─ Advanced search
│  ├─ Main content
│  │  ├─ Scorecard row (6 key metrics)
│  │  ├─ Charts row (4 visualizations)
│  │  └─ Database section (table view)
│  └─ Footer
│
└─ JAVASCRIPT APPLICATION (lines 920-2100+)
   ├─ CSV loading & parsing
   ├─ Data initialization
   ├─ State management (S object)
   ├─ Filter initialization
   ├─ Rendering functions
   │  ├─ renderTable() - database table
   │  ├─ renderCharts() - macro visualizations
   │  ├─ renderScores() - scorecard metrics
   │  └─ renderFilters() - filter UI
   ├─ Event handlers
   │  ├─ Search input
   │  ├─ Filter selection
   │  ├─ Chart clicks
   │  └─ Sort/pagination
   └─ Utility functions
      ├─ Data transformation
      ├─ Formatting
      ├─ Sorting
      └─ DOM helpers
```

### CSV Files (External Data)

```
Library.csv - Main research database
├─ Record Number (ID)
├─ Type (Research Article, Review, etc.)
├─ Author
├─ Year
├─ Title
├─ Journal / Source
├─ Link to Published Study
├─ Abstract
├─ Study Design
├─ Topic - Subtopic
├─ Interventions
├─ Partnership
├─ Study Population
├─ Author Affiliation
├─ City
├─ State
├─ Country
├─ Language
└─ [other metadata]
```

---

## Responsive Design

### Breakpoints & Behavior

#### Desktop (960px and above)
- ✓ Macro View (charts) visible on left
- ✓ Meso View (table) visible on right
- ✓ All controls accessible
- ✓ Full-width search bar
- ✓ 4-column chart grid
- ✓ 6-column scorecard grid

#### Tablet (640px - 960px)
- ✓ Single column layout
- ✓ Macro view charts full-width
- ✓ Meso view table below
- ✓ 2-column chart grid
- ✓ 3-column scorecard grid
- ✓ Responsive table with horizontal scroll
- ✓ Advanced search panel wraps filters to 2 columns

#### Mobile (<640px)
- ✓ Tab navigation (Macro/Meso tabs)
- ✓ Single column layout
- ✓ Full-width components
- ✓ Mobile optimized touches
- ✓ Single-column filters (Advanced Search)
- ✓ Horizontal scroll table
- ✓ Drawer-based filter panel
- ✓ Pagination simplified
- ✓ Font sizes scaled appropriately

### CSS Media Queries
```css
@media (max-width: 600px)     { /* Mobile */ }
@media (max-width: 960px)     { /* Tablet */ }
@media (max-width: 1200px)    { /* Large screens */ }
```

All breakpoints tested and verified for functionality.

---

## Development Guide

### Code Organization

The Research-Library.html file is organized into clear sections with comments:

```
/* =========================================================================
   CSS SECTION 1: DESIGN TOKENS & VARIABLES
   ========================================================================= */

/* =========================================================================
   CSS SECTION 2: RESPONSIVE DESIGN
   ========================================================================= */

<!-- =========================================================================
     HTML SECTION 3: PAGE STRUCTURE
     ========================================================================= -->

<!-- =========================================================================
     JAVASCRIPT SECTION 4: APPLICATION LOGIC
     ========================================================================= -->
```

### State Management

All application state is managed through a single object `S`:

```javascript
const S = {
  query: '',                    // Search text
  filters: {                    // Active filter selections
    topic: '',
    subtopic: '',
    population: '',
    intervention: '',
    partnership: '',
    language: '',
    year: '',
    country: ''
  },
  chartF: {},                   // Filters from chart clicks
  filtered: [...RAW],           // Current filtered dataset
  sort: { col: 'year', dir: 'desc' },  // Sort state
  page: 1,                      // Pagination page
  rpp: 10,                      // Results per page
  selId: null,                  // Selected study ID
  view: 'all',                  // View mode: 'all' or 'database'
  mobTab: 'macro'               // Active mobile tab
};
```

### Main Functions

#### Data Loading
```javascript
loadLibraryData()              // Fetch and parse CSV
initializeFilterOptions()       // Derive filters from data
```

#### Rendering
```javascript
renderTable()                  // Meso view (database table)
renderCharts()                 // Macro view (visualizations)
renderScores()                 // Scorecard metrics
renderFilters()                // Filter UI
update()                       // Refresh all views
```

#### Event Handlers
```javascript
handleSearch()                 // Full-text search
handleFilter()                 // Filter selection
handleSort()                   // Column sorting
handlePageChange()             // Pagination
selectStudy()                  // Open study modal
```

#### Utilities
```javascript
uniq()                         // Array deduplication
sortedData()                   // Apply sort
formatValue()                  // Value formatting
$()                           // DOM shorthand
```

---

## Updating Content

### Method 1: CSV Update (Recommended for Researchers)

**When to use:** Adding new research, updating study metadata

**Steps:**

1. **Edit Library.csv**
   - Open in Excel, Google Sheets, or text editor
   - Add new rows or modify existing ones
   - Ensure all required columns are present

2. **Upload to GitHub**
   - Replace the file in your GitHub repository
   - Commit with message: "Update: Add X new studies to Research Library"

3. **No code changes needed**
   - The application automatically fetches the latest CSV
   - Users see updated data on next page load
   - Filter options auto-generate from new data

**CSV Requirements:**
- First row must be headers
- Columns expected: Record Number, Title, Author, Year, Journal, Link, Topic - Subtopic, Interventions, Population, etc.
- Use commas as delimiters (standard CSV)

### Method 2: Update "Last Updated" Date

**Location in HTML:**

Line in footer: `<div class="footer-left">Last updated: June 28, 2026</div>`

**Manual Update:**

1. Open **Research-Library.html** in text editor
2. Find line with: `Last updated: June 28, 2026`
3. Change date to today: `Last updated: June 29, 2026`
4. Save file
5. Upload to your website

**Automated Update (Optional):**

To automatically update the date when CSV is refreshed, add this JavaScript:

```javascript
// At end of loadLibraryData() function:
const today = new Date();
const dateStr = today.toLocaleDateString('en-US', { 
  year: 'numeric', 
  month: 'long', 
  day: 'numeric' 
});
document.querySelector('.footer-left').textContent = 
  `Last updated: ${dateStr}`;
```

### Method 3: Hosting Considerations

**Option A: GitHub Pages (Recommended for small teams)**
- Store both HTML and CSV in GitHub
- Use raw.githubusercontent.com URLs
- Free hosting
- CSV: `CSV_URL = 'https://raw.githubusercontent.com/yourusername/repo/main/Library.csv'`

**Option B: Your Web Server**
- Host HTML and CSV on your server
- Full control over data
- CSV: `CSV_URL = 'https://yoursite.com/data/Library.csv'`

**Option C: Database Integration**
- Replace CSV loading with API call
- Modify `loadLibraryData()` to fetch from API
- Requires backend service

---

## Integration Instructions

### For Website Integration

1. **Download File**
   - Save `Research-Library.html` to your web server
   - Save `Library.csv` in same directory or GitHub

2. **Update CSV URL**
   - Find `CSV_URL` variable in JavaScript (line ~950)
   - Change to your CSV location:
   ```javascript
   const CSV_URL = 'https://yoursite.com/path/to/Library.csv';
   ```

3. **Test Locally**
   - Serve HTML via local server (not `file://`)
   - Verify all charts render
   - Check all filters work
   - Test on mobile device

4. **Deploy**
   - Upload HTML to your website
   - Ensure CSV URL is accessible
   - Clear browser cache
   - Verify data loads

5. **Monitor**
   - Check browser console for errors
   - Verify chart rendering
   - Test search/filter on sample data

### Embedding in Existing Page

To embed as an iframe:

```html
<iframe 
  src="https://yoursite.com/research-library.html"
  width="100%"
  height="800px"
  frameborder="0"
  style="border: 1px solid #ccc;"
></iframe>
```

### Linking from Main Site

```html
<a href="https://yoursite.com/research-library.html" 
   class="btn btn-primary">
  Explore Research Library
</a>
```

---

## Customization

### Color Scheme

Edit CSS variables in the `<style>` section (lines 13-40):

```css
:root {
  --ink: #1c1b18;           /* Primary text */
  --paper: #f8f6f1;         /* Background */
  --teal: #0a7865;          /* Primary color (buttons, links) */
  /* ... other colors */
}
```

**Colors Used:**
- **Ink** (#1c1b18): Body text, dark elements
- **Paper** (#f8f6f1): Backgrounds
- **Teal** (#0a7865): Buttons, active states, highlights
- **Border** (#cec8b8): Dividers, borders

### Typography

```css
--serif: "EB Garamond", Georgia, serif;     /* Headings */
--sans: "DM Sans", system-ui, sans-serif;   /* Body text */
```

Change font families in CSS variables (line 16-17).

### Text Content

#### About Modal
- File: Research-Library.html (line ~635)
- Section: "Welcome to Timeless Compass"
- Edit: HTML in `<div id="about-panel">`

#### Help Modal
- File: Research-Library.html (line ~690)
- Section: "Finding Your Way Around"
- Edit: HTML in `<div id="help-panel">`

#### Header & Footer
- Header title: Line ~628
- Header subtitle: Line ~629
- Footer copyright: Line ~900
- Footer links: Line ~900

### Filter Options

**No hardcoding needed!** Filters auto-generate from CSV data.

To exclude certain values from filters:

```javascript
// Line ~1127 - Interventions
F_INTS = uniq(...).filter(i => i !== 'SKY');

// Line ~1125 - Subtopics
F_SUBS = uniq(...).filter(s => !['At','Women','Youth'].includes(s));
```

### Chart Configuration

Edit Chart.js options in `renderCharts()` function (line ~1550):

```javascript
// Pie chart options
const pieOptions = {
  responsive: true,
  maintainAspectRatio: true,
  plugins: { legend: { position: 'right' } }
};
```

---

## Browser Compatibility

### Supported Browsers
- ✓ Chrome 90+
- ✓ Firefox 88+
- ✓ Safari 14+
- ✓ Edge 90+

### Features Used
- Fetch API (CSV loading)
- ES6+ (arrow functions, spread operator, template literals)
- CSS Grid & Flexbox
- CSS Custom Properties (variables)
- Chart.js (visualization)
- D3.js (mapping)

---

## Performance Notes

- **CSV Loading:** One-time fetch on page load
- **Filtering:** In-memory (instant, no server calls)
- **Pagination:** 10 items per page
- **Charts:** Rendered on-demand (lazy evaluation)
- **Mobile:** Optimized for slow networks
- **Bundle Size:** Single file, ~150KB minified

---

## Troubleshooting

### CSV Not Loading
- Check browser console (F12) for errors
- Verify CSV URL is correct and accessible
- Check CORS headers if cross-origin
- Ensure CSV has proper headers

### Charts Not Rendering
- Verify Chart.js loads from CDN
- Check CSV has required columns
- Ensure data has numeric values
- Check browser console for JavaScript errors

### Filters Empty
- Verify CSV has data
- Check filter initialization runs
- Ensure CSV columns match expected names
- Check for null/empty values in data

### Mobile Layout Issues
- Test viewport is set correctly
- Clear browser cache
- Check media query breakpoints
- Verify CSS loads without errors

---

## Support & Contribution

### Reporting Issues
- Document the issue clearly
- Include browser and OS version
- Provide steps to reproduce
- Include error messages from console

### Feature Requests
- Describe the feature
- Explain use case
- Provide mockup or example (if applicable)

### Code Updates
- Maintain container-content separation
- Keep single-file architecture
- Update documentation
- Test on mobile, tablet, desktop

---

## License & Attribution

**Copyright © 2026 Sri Sri Institute for Advanced Research (SSIAR)**

This Research Library is built with research data and community contributions.

**Credit:** Developed for the Sri Sri Institute for Advanced Research (SSIAR), the research wing of the Art of Living.

---

## Appendix: Quick Reference

### CSV URL Location
Line ~950 in JavaScript section

### Update "Last Updated" Date
Line ~900 in footer section

### Responsive Breakpoints
- Mobile: 600px
- Tablet: 960px  
- Large: 1200px

### Main State Object
Variable `S` - contains all app state

### Main Rendering Functions
- `renderTable()` - database view
- `renderCharts()` - visualizations
- `update()` - refresh all

### Filter Exclusions
- Line ~1125: Subtopic filters
- Line ~1127: Intervention filters

---

**Version:** 1.0  
**Last Updated:** June 29, 2026  
**Questions?** Contact development team or refer to inline comments in Research-Library.html
