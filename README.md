100+ Reasons to Do SKY — Evidence Atlas
An interactive, data-driven evidence explorer for peer-reviewed research on Sudarshan Kriya Yoga (SKY).

File Structure
/
├── sky-evidence-explorer.html   ← Dashboard (presentation layer — do not edit for data updates)
├── sky_data.csv                 ← Dataset (content layer — update this file to refresh the dashboard)
└── README.md

How It Works
The dashboard reads sky_data.csv at runtime using the Fetch API.
To update the evidence base: replace sky_data.csv with an updated version using the same filename.
No changes to the HTML file are needed.
The pipeline automatically:

Filters to rows where Journal Reputation = Acceptable
Sorts featured studies (Rating = ⭐⭐⭐⭐⭐) to the top by default
Derives all explorer taxonomy (subtopics, populations, countries) from the live dataset
Scales gracefully as the dataset grows beyond 127 records


CSV Schema
The dashboard reads the following columns. Column names must match exactly (case-sensitive):
ColumnPurposeFlashcard IDUnique study identifierSerial NoDisplay numberYearPublication yearSubtopicComma-separated tags e.g. PH: Brain, MH: StressStudy PopulationPopulation category for filteringHighlightOne-line research highlight (primary display text)Participant TypeSpecific participant descriptionParticipants (n)Sample sizeIntervention DurationDuration stringFollow-up TimepointsFollow-up durationStudy DesignRCT, Observational, etc.Proven Benefits StatementPipe-separated `Journal ReputationAcceptable / Mixed Reputation / PredatoryCitation ShortShort citation e.g. Smith et al., 2023JournalFull journal nameCorresponding Author AffiliationInstitutionCountryGeography (comma-separated for multi-country)Link to Published StudyURLRating⭐⭐⭐⭐⭐ for featured studies, blank otherwise
Adding new columns: Future schema extensions are safe — the dashboard only reads the columns it needs and ignores extras.

Filtering Logic (Built-in)

Journal Reputation filter: Only rows marked Acceptable are shown. Change FILTER_REPUTATION at the top of the <script> block to adjust.
Default sort: Featured studies (⭐) appear first, then all others in original order.
All explorer interactions act as live filters on the database.


Hosting on GitHub Pages

Push both files (sky-evidence-explorer.html + sky_data.csv) to a GitHub repository
Go to Settings → Pages → Source → Deploy from branch → main / root
GitHub Pages will serve the site at https://yourusername.github.io/your-repo/sky-evidence-explorer.html


Important: GitHub Pages serves files over HTTPS, so the fetch() call to sky_data.csv works correctly. Opening the HTML file directly via file:// in a browser will block the fetch due to CORS. Use a local server for local development (see below).


Local Development
bash# Python 3
python3 -m http.server 8080

# Then open:
# http://localhost:8080/sky-evidence-explorer.html

Updating the Dataset

Open sky_data.csv in Excel, Google Sheets, or any CSV editor
Add or edit rows — new studies are automatically picked up on the next page load
Save as CSV (UTF-8 encoding recommended)
Replace the existing sky_data.csv in the repository
Commit and push — the dashboard updates automatically

No changes to the HTML file are required.

Configuration (Advanced)
At the top of the <script> block in sky-evidence-explorer.html:
javascriptconst CSV_FILE = 'sky_data.csv';          // change filename here if needed
const FILTER_REPUTATION = 'Acceptable';   // reputation filter value
const PER_PAGE = 10;                      // studies per page in database view

Explorer Taxonomy
All explorer nodes are generated dynamically from the dataset:

Physical Health nodes — derived from PH: prefixed subtopics
Mental Health nodes — derived from MH: prefixed subtopics
Population bars — derived from Study Population field
Geography bubbles — derived from Country field
Duration buckets — matched against Intervention Duration field

Adding a new subtopic tag to the CSV (e.g. PH: Metabolism) will automatically create a new node in the explorer on next load.
