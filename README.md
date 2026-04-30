SFMC Email Tracking Report
A reusable Salesforce Marketing Cloud (SFMC) CloudPage solution that gives you a complete, searchable, and paginated view of email send performance — including delivery, engagement, bounce breakdowns, List Detective counts, and a drill-down bounce detail page per Job ID.

Pages
Page 1 - Email Tracking Report (Main Page)
The primary dashboard. Displays all email send records from the tracking DE with filters, summary stats, a data table, and a CSV export.
Page 2 - Bounce Detail Report (Drill-down Page)
Opened when a user clicks any Job ID on the main page. Shows a full breakdown of bounces for that specific send — by subcategory, count, percentage, and individual email addresses with their domains.

Features:-
Main Tracking Report:
Search by Email Name (partial match supported)
Filter by Date Range (Start Date and End Date)
Configurable pagination (10 / 25 / 50 / 100 rows per page)
Summary stat cards: Actual Send, Avg Delivery Rate, Avg Open Rate, Avg Click Rate, Total Bounces, Hard Bounces, Total Block Bounces, Total Tech Bounces
Frozen first 3 columns (Job ID, Email Name, Event Date) during horizontal scroll
Clickable Job ID links that open the Bounce Detail page
Download CSV button — exports all matching records (not just current page), including filters and summary in the CSV header
Instructional hint label below record count
Bounce Detail Report:
Triggered via Job ID link from the main page
Meta bar showing Job ID, Event Date, Total Bounces, Unique Bounce Types, Unique Domains
Summary stat cards per bounce subcategory
Aggregated bounce table with visual bar indicators and percentage breakdown
Full email address list grouped by bounce subcategory, with domain tags
Collapsible/expandable sections per subcategory
Download CSV button — exports subcategory summary and full email list
-


Columns Displayed
Main Tracking Report Table -
Column	Description
Job ID	SFMC Job ID (clickable link to bounce detail)
Email Name	Name of the email send
Event Date	Date of the send event
Planned Send	Planned send count
Actual Send	Actual number of subscribers sent to
Delivered	Actual Send minus Total Bounce
Unique Opens	Unique subscribers who opened
Unique Clicks	Unique subscribers who clicked
Delivery Rate	Delivered / Actual Send percentage
Open Rate	Unique Opens / Actual Send percentage
Click Rate	Unique Clicks / Actual Send percentage
Unsubscribes	Subscribers who unsubscribed
Total Bounce	Total bounced subscribers
Hard Bounce	Hard bounce count
Soft Bounce	Soft bounce count
Block Bounce	Block bounce count
Tech Bounce	Technical bounce count
Unknown Bounce	Bounces with no category
Held	Subscribers in held status
List Detective	Count excluded by List Detective
Exclusion Date	Date of List Detective exclusion
Comments	Auto-generated bounce subcategory summary


Bounce Detail Report
Column	Description
Bounce Subcategory	Type of bounce
Count	Number of bounces in that subcategory
Percentage	Share of total bounces
Email Address	Individual bounced email
Domain	Domain extracted from email address



Tech Stack
Layer	Technology
Server-side logic	SSJS (Server-Side JavaScript)
Data retrieval	DataExtension.Init() with filter chaining
URL parameters	AMPscript RequestParameter + SSJS GetQueryStringParameter
Data injection	SSJS string concatenation into window object (CSP-safe, no JSON.stringify)
Frontend	HTML / CSS / Vanilla JavaScript
CSV export	Client-side Blob with UTF-8 BOM (Excel-compatible)
Hosting	SFMC CloudPages
Data population	SQL Query Activity via Automation Studio
-

How It Works
Data Layer (Automation)
An automation named Test_Email_Tracking_Report_Refresh runs on a schedule and contains two SQL query activities:
Query 1 - Test_Populate_Email_Tracking_Report_DE
Pulls data from SFMC system data views and joins with Test_Not_Sent_Tracking_DE to get List Detective exclusion counts.
Query 2 - Test_Populate_Email_Bounce_Detail_DE
Pulls individual bounce records. Used by the Bounce Detail page.

Data Extensions Required
DE NamePurposeTest_Email_Tracking_Report_DEStores aggregated send-level metrics per Job IDTest_Email_Bounce_Detail_DEStores individual bounce records with email addressesTest_Not_Sent_Tracking_DEStores not-sent records including List Detective exclusions

How to Use :-
Create the three Data Extensions — Test_Email_Tracking_Report_DE, Test_Email_Bounce_Detail_DE, and Test_Not_Sent_Tracking_DE — in your SFMC BU
Set up the Test_Email_Tracking_Report_Refresh automation with the two SQL query activities (Test_Populate_Email_Tracking_Report_DE and Test_Populate_Email_Bounce_Detail_DE) and schedule it daily
For List Detective data: add a Data Extract and File Import activity to populate Test_Not_Sent_Tracking_DE
Create two CloudPages and paste the respective page code into each
Update the BOUNCE_PAGE_URL variable in the main tracking page
Publish both CloudPages and access via the main tracking page URL



File Structure
```
sfmc-email-tracking-report/
│
├── Email_Tracking_Report.html     # Main tracking report CloudPage (Version 3)
└── Bounce_Detail_Report.html      # Bounce drill-down CloudPage
```


Final Version
This solution consists of two linked CloudPages working together:
Page 1 - Email Tracking Report
The main dashboard showing all email send metrics. Includes filters, summary stat cards (Actual Send, Avg Delivery Rate, Avg Open Rate, Avg Click Rate, Total Bounces, Hard Bounces, Block Bounces, Tech Bounces), a fully paginated data table with 22 columns, frozen first 3 columns during horizontal scroll, and a Download CSV button in the filter bar that exports all matching records across all pages.
Page 2 - Bounce Detail Report
A drill-down page linked directly from Page 1. Every Job ID in the main table is a clickable link — clicking it passes the Job ID and Event Date as URL parameters to the Bounce Detail page. The page then queries the Email_Bounce_Detail_DE for that specific Job ID and renders a full breakdown: bounce subcategory summary with counts and percentages, visual bar indicators, and a complete list of bounced email addresses grouped by subcategory with domain tags. Both pages include a Download CSV button that exports all data including metadata and summary sections.


Notes
Data covers the last 180 days by default (controlled by the WHERE clause in the SQL queries)
The CSV export includes all matching records across all pages, not just the current page view
The Bounce Detail page is accessed only via the Job ID link on the main page — it requires a jobid URL parameter to function
Secure the CloudPages appropriately for internal use — these pages expose send-level performance data


Author
Built by an SFMC Developer as a reusable internal reporting tool.
If this helped you, leave a star on the repo and connect on LinkedIn!
-

License
MIT License — free to use, modify, and distribute.
