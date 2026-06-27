# Timeless Compass: Interactive Research Library
## Container-Content Architecture

---

## 📋 Overview

This is a **modular, data-driven research library application** that separates the User Interface (Container) from the Research Data (Content).

**Architecture Benefits:**
- ✅ Update research data without modifying HTML
- ✅ Easy to maintain and scale
- ✅ Non-technical users can update CSV files
- ✅ Version control friendly for data and code separation
- ✅ Reusable container for different datasets

---

## 📁 File Structure

```
Timeless Compass Project
├── Research-Library.html       (UI Container - Do not modify data here)
├── Library.csv                 (Research articles data - UPDATE THIS)
├── Institutions.csv            (Top contributing institutes - Optional update)
└── README-Library.md           (This file)
```

### **Files Explained**

#### 1. **Research-Library.html** ✓ Container
- The **interactive UI application**
- Dynamically loads data from CSV files
- Contains all visualization, filtering, and search logic
- **Do not store data in this file** - only update if fixing bugs or changing UI design

**File Size:** ~700KB  
**Maintenance:** Minimal - only update for UI/feature changes

#### 2. **Library.csv** 📊 Content (Research Data)
- **Main research article database**
- One row per published study
- Source of truth for all research data
- **This is what you update regularly**

**Required Columns:**
```csv
Record Number, Type, Author, Year, Title, Journal / Source, 
Link to Published Study, Abstract, Study Design, Topic - Subtopic, 
Interventions, Partnership, Study Population, Author Affiliation, 
City, State, Country, Included in TC, Date Added, Language
```

**Update Frequency:** As new research is added  
**How to Update:** Edit directly in Excel/Google Sheets or text editor

#### 3. **Institutions.csv** 🏛️ Configuration (Optional)
- List of top 24 contributing institutions
- Used for the "Top Contributing Institutes" filter
- Pre-populated but can be modified

**Columns:**
```csv
Column 1, Institutions (Value), Display Name (Label), COUNT
```

**Update Frequency:** When adding new institutions or updating counts  
**How to Update:** Edit directly or regenerate from Library.csv data

---

## 🚀 Getting Started

### **Local Setup**

1. **Download or clone all files** in the same directory:
   ```
   Research-Library.html
   Library.csv
   Institutions.csv
   ```

2. **Open Research-Library.html** in a web browser
   - Click the file or drag it into your browser
   - No web server required!

3. **View the research library**
   - Filters automatically populate from Library.csv
   - All data is loaded on page load

### **Troubleshooting**

**Problem:** "Error loading Library.csv"
- **Solution:** Make sure Library.csv is in the **same folder** as Research-Library.html
- **Note:** Due to browser security, this won't work with `file://` URLs if hosted locally - you need a local web server

**Quick Local Server Setup:**

Python 3:
```bash
cd /path/to/files
python -m http.server 8000
# Open: http://localhost:8000/Research-Library.html
```

Python 2:
```bash
python -m SimpleHTTPServer 8000
```

Node.js:
```bash
npx http-server
```

---

## ✏️ Updating Research Data

### **Add a New Study**

1. **Open Library.csv** in Excel, Google Sheets, or a text editor
2. **Add a new row** with the study information:
   ```csv
   500,Research Article,Author Name,2024,Study Title,Journal Name,...
   ```
3. **Save the file**
4. **Reload Research-Library.html** in your browser
   - Filters automatically update
   - New study appears in search results

### **CSV Format Guidelines**

**String fields with commas or quotes:**
```csv
"Niche Topic - Sub-Topic, Another Subtopic"
"Smith, John"
"Quote: ""Beautiful minds"""
```

**Multiple values separated by commas:**
```csv
Yoga, Pranayama, Meditation
```

**Key Mappings in Application:**
- `Record Number` → Study ID
- `Type` → Publication type (Research Article, Review, etc.)
- `Author` → Citation author
- `Year` → Study year
- `Topic - Subtopic` → Filters (use format: `Topic - Subtopic`)
- `Interventions` → Multiple, comma-separated
- `Study Population` → Multiple, comma-separated
- `Author Affiliation` → Used for institute filtering
- `Country` → Determines continent in app
- `Language` → Language filter

---

## 🔍 Filter System

### **Dynamic Filters** (Auto-populated from CSV)
- **Topic** - From "Topic - Subtopic" column
- **Subtopic** - From "Topic - Subtopic" column
- **Population** - From "Study Population" column
- **Intervention** - From "Interventions" column
- **Partnership** - From "Partnership" column
- **Language** - From "Language" column
- **Year** - From "Year" column
- **Country** - From "Country" column

### **Static Filters**
- **Top Contributing Institutes** - Hardcoded list (can be updated in Institutions.csv)

### **Search**
- Full-text search across: Title, Abstract, Author, Journal, Country, Topics

---

## 🌍 Supported Countries & Continents

The application automatically maps countries to continents. Supported countries:

**Asia:**
- India, Japan, Nepal, United Arab Emirates

**North America:**
- United States, Canada

**Europe:**
- United Kingdom, Sweden, Norway, Italy, Germany, Finland, Poland, Denmark

**Other:**
- Any country not in the list will be marked as "Other"

To add more countries, edit the `getContinent()` function in Research-Library.html

---

## 📈 Data Statistics

The application automatically calculates and displays:

**Scorecards:**
- Number of selected studies
- Percentage of total studies
- Year range covered
- Number of countries
- Number of continents
- Percentage of independent research
- Number of languages

**Charts:**
- Studies by year (bar chart) - clickable to filter
- Studies by topic (horizontal bar)
- Studies by partnership type (pie chart)
- Geographic distribution (world map)

---

## 🔧 Advanced: Customization

### **Change Institute List**

Edit the `F_INSTITUTES` array in Research-Library.html:

```javascript
const F_INSTITUTES = [
  'Your Institute 1',
  'Your Institute 2',
  // ... add more
];
```

### **Add New Filter**

1. **Add a column to Library.csv** (e.g., "Author Email")
2. **Add to filter definitions** in Research-Library.html:
   ```javascript
   function buildFilterDefs() {
     FILTER_DEFS = [
       // ... existing filters
       {key:'email',  label:'Author Email', opts:F_EMAILS, all:'All Emails'},
     ];
   }
   ```
3. **Generate filter values** in `initializeFilterOptions()`:
   ```javascript
   let F_EMAILS = uniq(RAW.map(d=>d.email));
   ```

### **Change Chart Visuals**

Edit the CSS variables in the `<style>` section:
```css
--gold: #E8A020;     /* Primary color */
--jade: #0D9E7A;     /* Secondary color */
--rose: #C7325A;     /* Accent color */
```

---

## 📊 Data Import from Excel

### **Export from Excel to CSV:**
1. Open your Excel file
2. File → Save As
3. Choose format: **CSV (Comma delimited) (.csv)**
4. Click Save

### **Ensure proper formatting:**
- Quotes around text with commas: `"Topic 1, Topic 2"`
- Multiple values separated by: `, ` (comma-space)
- No blank rows in the middle
- Date format: YYYY (just the year)

---

## 🔄 Version Control Recommendations

### **Git Best Practices**

```bash
# Track both files
git add Research-Library.html Library.csv Institutions.csv README-Library.md

# Separate commits for different changes
git commit -m "Update: Added 5 new research articles"
git commit -m "Fix: Filter button styling on mobile"

# Branch for major data updates
git checkout -b update/2024-research-batch
# ... make updates ...
git merge main
```

### **What to Commit**
✅ `Library.csv` - Research data
✅ `Institutions.csv` - Institute list  
✅ `Research-Library.html` - UI code
✅ `README-Library.md` - Documentation

---

## 🎯 Workflow Examples

### **Example 1: Quarterly Update**
```
1. Collect new research papers (5-10 new studies)
2. Add rows to Library.csv with study information
3. Save Library.csv
4. Test by opening Research-Library.html
5. Commit changes: "Q4 2024: Added quarterly research batch"
6. Deploy to GitHub Pages / web server
```

### **Example 2: Fix Research Entry**
```
1. Open Library.csv in Excel
2. Find the study (use Ctrl+F)
3. Correct the title/year/authors
4. Save
5. Refresh Research-Library.html to verify
6. Commit: "Fix: Corrected study title for ID #342"
```

### **Example 3: Add New Institution**
```
1. Edit Institutions.csv
2. Add new row with institution name
3. Optionally update F_INSTITUTES in HTML
4. Test the filter dropdown
5. Commit: "Add: Harvard University to institutes filter"
```

---

## 📱 Features

### **Macro View**
- Overview statistics (scorecards)
- Year distribution chart
- Topic distribution chart
- Partnership types pie chart
- World map with study locations

### **Meso View (Research Database)**
- Sortable table of all studies
- Click to select and preview
- Pagination (20 studies per page)
- Responsive design for mobile

### **Micro View (Study Details)**
- Full study information
- Abstract and key details
- Link to published paper
- All metadata visible

### **Search & Filter**
- Full-text keyword search
- 9+ filterable dimensions
- Multi-filter combinations
- Active filter display ("chips")

---

## 🐛 Debugging

### **Check Browser Console**
Press `F12` to open Developer Tools → Console tab

Common messages:
- `Error loading Library.csv` - File not found or CORS issue
- `Error loading Institutions.csv` - Not required, app will continue

### **Verify Data Loading**
In Console, type: `RAW.length`
Should show the number of studies loaded

### **Check Filter Population**
In Console, type: `F_TOPICS`
Should show array of all topics from your data

---

## 📞 Support & Troubleshooting

| Problem | Solution |
|---------|----------|
| Files not loading | Make sure all files are in the same directory |
| Filters empty | Check that Library.csv has the correct columns |
| Map not showing | Ensure "Country" column has values |
| Mobile layout broken | Clear browser cache (Ctrl+Shift+Del) and reload |
| Old data showing | Hard refresh browser (Ctrl+Shift+R or Cmd+Shift+R) |

---

## 📝 License & Attribution

**Timeless Compass: Interactive Research Library**

Curated by the Sri Sri Institute for Advanced Research (SSIAR)

Attribution: Exploring published research on Ancient Systems of Knowledge

---

## 🔄 Version History

**v2.0** - Container-Content Architecture
- Separated HTML (UI) from CSV (Data)
- Dynamic data loading
- Independent content updates
- This version

**v1.0** - Original Explorer 2.0
- Hardcoded data in HTML
- Static visualization

---

## 📚 Resources

- [GitHub Pages Deployment](https://pages.github.com/)
- [CSV Best Practices](https://www.ietf.org/rfc/rfc4180.txt)
- [Excel to CSV Guide](https://support.microsoft.com/en-us/office/save-a-worksheet-as-csv)

---

**Last Updated:** 2024  
**Maintained By:** [Your Organization]
