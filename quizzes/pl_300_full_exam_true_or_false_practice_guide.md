# PL-300 Exam Comprehensive 50-Question True/False Study Guide

This study guide covers all four domains of the **Microsoft Certified: Power BI Data Analyst Associate (PL-300)** exam.

---

## Domain 1: Prepare the Data (25%–30%)

### Question 1
**In Power Query, unpivoting attribute-value pairs transforms wide multi-column data into clean tall data rows.**
* **Answer:** True
* **Explanation:** Unpivoting reshapes wide attribute columns into dynamic attribute-value pairs essential for tabular star-schema modeling.

### Question 2
**Query Folding forces the Power BI Desktop engine to process all transformations locally on your client CPU.**
* **Answer:** False
* **Explanation:** Query Folding pushes Power Query transformation steps back to the source database engine (e.g., SQL Server) as a native SQL query.

### Question 3
**In Power Query, "Merging" queries acts like a SQL JOIN, while "Appending" queries acts like a SQL UNION.**
* **Answer:** True
* **Explanation:** Merging combines columns from two tables based on matching keys; Appending stacks rows from tables with matching schema structures.

### Question 4
**DirectQuery mode copies all underlying data into the VertiPaq in-memory storage engine.**
* **Answer:** False
* **Explanation:** DirectQuery queries the underlying data source live at report run time. Import mode loads data into VertiPaq RAM.

### Question 5
**The Power Query M formula language is case-insensitive.**
* **Answer:** False
* **Explanation:** Power Query M is strictly case-sensitive (`Table.SelectRows` works, while `table.selectrows` throws a syntax error).

### Question 6
**"Column Distribution" profiling in Power Query evaluates data quality across the entire dataset by default, even if it has millions of rows.**
* **Answer:** False
* **Explanation:** By default, data profiling operates on the top 1,000 rows. You can manually toggle it in the status bar to profile the full dataset.

### Question 7
**Dual storage mode allows a table to act as Import mode for local visual queries and DirectQuery mode when joining with DirectQuery sources.**
* **Answer:** True
* **Explanation:** Dual storage mode caches data in RAM while allowing the engine to combine queries seamlessly with DirectQuery source tables without cross-source performance penalties.

### Question 8
**Referencing a Power Query table creates an independent duplicate copy of all previous steps in memory.**
* **Answer:** False
* **Explanation:** "Reference" creates a downstream query that uses the output of the base query as its starting point. "Duplicate" copies all steps independently.

### Question 9
**Replacing errors in a column using Power Query modifies the source database table physically.**
* **Answer:** False
* **Explanation:** Power Query transformations are applied virtually in the ETL pipeline during refresh and never alter source database storage.

### Question 10
**Adding custom SQL statements directly inside the Native Query box in Power Query disables Query Folding for subsequent UI steps.**
* **Answer:** True
* **Explanation:** Using custom native SQL queries breaks automatic Query Folding for steps added further down in the Power Query UI sequence.

### Question 11
**The "Fill Down" transformation in Power Query replaces `BLANK` or null values with the non-null value immediately above them.**
* **Answer:** True
* **Explanation:** Fill Down scans top-to-bottom and propagates valid values into succeeding null cells.

### Question 12
**Combining two `DateTime` columns into a single string column reduces dataset size in VertiPaq memory.**
* **Answer:** False
* **Explanation:** Combining `DateTime` values into strings dramatically increases column cardinality, ruining Hash Encoding and increasing RAM consumption.

### Question 13
**Parameters in Power Query can be used to dynamically alter connection strings, server names, or database targets.**
* **Answer:** True
* **Explanation:** Parameters allow users to swap target environments (e.g., Dev vs. Prod) without editing underlying M code.

---

## Domain 2: Model the Data (25%–30%)

### Question 14
**In a Star Schema, Fact tables contain numeric quantitative measurements, while Dimension tables contain attributes used for filtering and slicing.**
* **Answer:** True
* **Explanation:** Fact tables store transactional metrics (e.g., Sales, Units); Dimension tables hold context entities (e.g., Customer, Product, Date).

### Question 15
**A calculated column in DAX evaluates dynamically whenever a user interacts with a report slicer.**
* **Answer:** False
* **Explanation:** Calculated columns evaluate during data refresh and consume disk/RAM. Measures evaluate dynamically at report run time based on visual filter context.

### Question 16
**The `CALCULATE()` function in DAX can modify or override the existing filter context of an expression.**
* **Answer:** True
* **Explanation:** `CALCULATE()` evaluates an expression in a modified filter context and is the core function for DAX context manipulation.

### Question 17
**Bi-directional cross-filtering ($1:*$ both directions) on relationships is recommended for all table relationships to simplify DAX writing.**
* **Answer:** False
* **Explanation:** Bi-directional relationships introduce filter ambiguity, circular paths, and performance degradation. Single-direction filtering is best practice.

### Question 18
**Context Transition converts an existing Row Context into an equivalent Filter Context when `CALCULATE()` is executed.**
* **Answer:** True
* **Explanation:** `CALCULATE()` implicitly triggers context transition, taking current row values and applying them as active filters.

### Question 19
**`USERELATIONSHIP()` can activate an inactive relationship between two tables inside a DAX measure calculation.**
* **Answer:** True
* **Explanation:** `USERELATIONSHIP()` allows a measure to temporarily activate an inactive model relationship (e.g., `ShipDate` vs `OrderDate`).

### Question 20
**DAX Time Intelligence functions (e.g., `TOTALYTD`) work correctly even if the Date table has missing dates or duplicate values.**
* **Answer:** False
* **Explanation:** Time intelligence functions strictly require a marked Date table with contiguous dates and no missing or repeated days.

### Question 21
**`SUMX` is an iterator function that evaluates a custom DAX expression row-by-row over a specified table before aggregating.**
* **Answer:** True
* **Explanation:** Iterator functions (ending in **X**) execute line-by-line in a row context before calculating the final aggregate value.

### Question 22
**High column cardinality improves VertiPaq data compression efficiency.**
* **Answer:** False
* **Explanation:** High cardinality (many unique values) increases dictionary sizes and reduces VertiPaq compression efficiency.

### Question 23
**`RELATED()` can retrieve a value from another table provided a valid relationship exists from the "Many" side to the "One" side.**
* **Answer:** True
* **Explanation:** `RELATED()` follows $N:1$ model relationships upward to fetch values from dimension tables.

### Question 24
**Using `DIVIDE(A, B)` in DAX handles division-by-zero errors safely without throwing runtime exceptions.**
* **Answer:** True
* **Explanation:** `DIVIDE()` returns `BLANK` (or an alternate specified output) when dividing by zero, avoiding runtime errors.

### Question 25
**`ALL(Table)` removes all active filters from the specified table within the filter context.**
* **Answer:** True
* **Explanation:** `ALL()` acts as a filter remover inside `CALCULATE()`, ignoring user visual slicers on the targeted table.

### Question 26
**Role-Playing dimensions (like multiple date fields) are best resolved in Power BI by creating explicit Many-to-Many relationships.**
* **Answer:** False
* **Explanation:** Role-playing dimensions are resolved using distinct dimension copies (e.g., `Ship Date` and `Order Date`) or inactive relationships with `USERELATIONSHIP()`.

### Question 27
**`KEEPFILTERS()` replaces existing column filters inside a `CALCULATE` expression.**
* **Answer:** False
* **Explanation:** `KEEPFILTERS()` intersects new filter predicates with existing visual filters rather than overwriting them.

### Question 28
**Parent-Child organizational hierarchies can be flattened into standard columns using DAX functions like `PATH()` and `PATHITEM()`.**
* **Answer:** True
* **Explanation:** `PATH()` parses parent-child employee keys into delimited strings, allowing `PATHITEM()` to split levels into dimensional columns.

---

## Domain 3: Visualize and Analyze Data (25%–30%)

### Question 29
**The Decomposition Tree visual enables users to drill down dynamically into multiple dimensions in any order to analyze root causes.**
* **Answer:** True
* **Explanation:** Decomposition Trees allow ad-hoc exploratory analysis across dimensions based on AI high/low split logic.

### Question 30
**Page-level tooltips allow report authors to show a custom formatted report page when a user hovers over a data point in a visual.**
* **Answer:** True
* **Explanation:** Report page tooltips bind target visuals to dedicated hidden report pages designed to show contextual pop-up details.

### Question 31
**Bookmarks can save visual visibility state, filter context, and page navigation settings.**
* **Answer:** True
* **Explanation:** Bookmarks capture the exact state of a report page (filters, visual visibility, selection states) for interactive storytelling.

### Question 32
**High-contrast themes and custom tab order configuration improve report accessibility for screen readers.**
* **Answer:** True
* **Explanation:** Tab order dictates keyboard navigation sequence, while high contrast aids visually impaired users.

### Question 33
**The Key Influencers visual requires a complex custom DAX script to compute regression drivers.**
* **Answer:** False
* **Explanation:** Key Influencers is a built-in AI visual that automatically runs machine learning regressions behind the scenes.

### Question 34
**Cross-highlighting between visuals on a report page can be turned off or customized using the "Edit Interactions" feature.**
* **Answer:** True
* **Explanation:** Edit Interactions allows authors to set visual relationships to Filter, Highlight, or None.

### Question 35
**Drill-through filters pass the filter context of a selected data point from a summary page to a detailed target page.**
* **Answer:** True
* **Explanation:** Drill-through carries selected category contexts across report pages automatically.

### Question 36
**Small Multiples split a single visual into grid variations based on a chosen categorical dimension.**
* **Answer:** True
* **Explanation:** Small multiples create side-by-side comparative sub-charts partitioned by category.

### Question 37
**Sync Slicers allow a single slicer selection on Page 1 to filter visuals on Page 2 and Page 3 simultaneously.**
* **Answer:** True
* **Explanation:** Sync Slicers synchronize active selections across chosen pages in a Power BI report.

### Question 38
**Conditional formatting in matrix visuals can evaluate color scales based on DAX measure values.**
* **Answer:** True
* **Explanation:** Matrix cells can be dynamically colored using DAX measure outputs, rule sets, or color hex codes.

### Question 39
**The "Analyze in Excel" feature requires converting Power BI datasets into static CSV files first.**
* **Answer:** False
* **Explanation:** Analyze in Excel establishes a live connection to the hosted Power BI dataset via Excel PivotTables.

### Question 40
**Exporting a Power BI report page to PDF preserves interactive slicers and hover tooltips in the exported file.**
* **Answer:** False
* **Explanation:** Exporting to PDF generates a static document; all hover tooltips and interactive elements are lost.

---

## Domain 4: Manage and Maintain Deliverables (15%–20%)

### Question 41
**Row-Level Security (RLS) defined in Power BI Desktop restricts data access for workspace users assigned the "Admin" or "Member" roles.**
* **Answer:** False
* **Explanation:** Admins, Members, and Contributors bypass RLS with edit permissions. RLS applies only to workspace **Viewer** roles or App consumers.

### Question 42
**`USERPRINCIPALNAME()` in dynamic RLS DAX filters returns the logged-in user's email address (e.g., `user@company.com`).**
* **Answer:** True
* **Explanation:** `USERPRINCIPALNAME()` identifies the current viewer's Azure Active Directory UPN/email for dynamic row filtering.

### Question 43
**Workspace "Viewer" role members can edit underlying datasets and modify report visuals.**
* **Answer:** False
* **Explanation:** Viewers have read-only access to published content and cannot modify dataset definitions or report layouts.

### Question 44
**An On-Premises Data Gateway (Standard Mode) allows multiple users to refresh datasets connected to local organizational data sources.**
* **Answer:** True
* **Explanation:** Standard Mode Gateways serve centralized enterprise teams across multiple cloud datasets and users.

### Question 45
**Power BI Apps allow report creators to bundle and distribute published reports and dashboards to large consumer audiences securely.**
* **Answer:** True
* **Explanation:** Apps are the primary enterprise distribution method for presenting polished content packages to end users.

### Question 46
**Scheduled Refresh for DirectQuery storage mode datasets requires uploading raw data files every hour.**
* **Answer:** False
* **Explanation:** DirectQuery datasets do not import data, so standard scheduled data refreshes are unnecessary (visual queries hit the source live).

### Question 47
**Object-Level Security (OLS) can hide sensitive columns or entire tables completely from unauthorized user roles.**
* **Answer:** True
* **Explanation:** OLS secures specific columns or tables from appearing in model metadata for restricted security roles.

### Question 48
**Performance Analyzer in Power BI Desktop breaks down visual load times into DAX query, Visual display, and Other process timings.**
* **Answer:** True
* **Explanation:** Performance Analyzer measures exact duration metrics for query execution, visual rendering, and background waiting.

### Question 49
**Sensitivity labels (Microsoft Purview) applied to Power BI reports persist when data is exported to Excel or PowerPoint files.**
* **Answer:** True
* **Explanation:** Information protection sensitivity labels follow exported data into Microsoft Office file formats.

### Question 50
**Data Lineage view in Power BI Service visualizes the flow of data from data sources to datasets, reports, and dashboards.**
* **Answer:** True
* **Explanation:** Lineage view displays upstream and downstream data dependencies across workspace artifacts.