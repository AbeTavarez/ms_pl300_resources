# MS PL-300 Resources

## PL-300: Microsoft Power BI Data Analyst Associate
## Comprehensive Study Glossary

> **Exam Overview**
> - Passing score: **700 / 1000**
> - Four skill domains (see weights below)
> - Proficiency in **Power Query** and **DAX** is essential

---

## Exam Domain Weights

| Domain | Weight |
|---|---|
| Prepare the data | 25–30% |
| Model the data | 25–30% |
| Visualize and analyze the data | 25–30% |
| Manage and secure Power BI | 15–20% |

---

## Domain 1: Prepare the Data (25–30%)

### 🔌 Get or Connect to Data

| Term | Definition |
|---|---|
| **Import Mode** | Data is copied into Power BI's in-memory engine (VertiPaq). Fastest query performance; requires scheduled refresh to stay current. |
| **DirectQuery Mode** | No data is stored locally; every visual sends a live query to the source. Always current, but slower; limited DAX support. |
| **Dual Mode** | Table can act as either Import or DirectQuery depending on query context. Used for dimension tables in mixed models. |
| **Live Connection** | Connects to an existing Power BI semantic model or Analysis Services model without importing data. No Power Query transformations available. |
| **Shared Semantic Model** | A published, reusable dataset in Power BI Service that multiple reports can connect to, promoting a single source of truth. |
| **Parameters** | Named, reusable values in Power Query used to dynamically control data source paths, filter values, or query logic. |
| **Privacy Levels** | Settings (None, Public, Organizational, Private) that control how data sources can be combined to prevent data leakage. |
| **Data Source Credentials** | Authentication settings (OAuth, username/password, key, etc.) stored per data source connection in Power BI. |
| **Dataflow** | A reusable Power Query ETL process stored in Power BI Service; produces tables consumable by multiple reports. |
| **Dataverse** | Microsoft's cloud data platform (used in Power Apps); accessible as a Power BI connector. |

---

### 🔍 Profile and Clean the Data

| Term | Definition |
|---|---|
| **Column Quality** | Power Query metric showing % of valid, error, and empty values in a column. |
| **Column Distribution** | Power Query metric showing distinct vs. unique value counts and a histogram of value frequency. |
| **Column Profile** | Detailed statistics (min, max, average, value distribution) for a selected column in Power Query. |
| **Data Profiling** | The process of examining data to understand its structure, quality, and completeness before transformation. |
| **Null Values** | Missing or unknown values. Can be replaced, removed, or handled with conditional logic in Power Query. |
| **Error Values** | Cells that failed to parse or convert; surfaced by Column Quality. Can be replaced or removed in Power Query. |
| **Data Type** | The classification of a column's values (Text, Whole Number, Decimal, Date, Boolean, etc.) that determines allowed operations. |

---

### 🔄 Transform and Load the Data

| Term | Definition |
|---|---|
| **Power Query (M)** | The ETL engine in Power BI Desktop; uses the M functional language to define transformation steps. |
| **Applied Steps** | The sequential list of transformations recorded in Power Query for each query; editable and reorderable. |
| **Merge Query** | Combines two queries by matching on one or more key columns (equivalent to a SQL JOIN). |
| **Append Query** | Stacks rows from two or more queries with matching column structures (equivalent to a SQL UNION). |
| **Pivot** | Turns distinct row values in one column into separate columns (wide format). |
| **Unpivot** | Converts multiple attribute columns into key-value row pairs (long/tall format). |
| **Transpose** | Flips rows and columns in a table. |
| **Group By** | Aggregates rows by one or more columns (e.g., SUM, COUNT, AVERAGE) in Power Query. |
| **Reference Query** | Creates a new query that starts from an existing query's output; changes to the source propagate forward. No data duplication. |
| **Duplicate Query** | Creates an independent copy of a query; changes to the source do NOT propagate. |
| **Fact Table** | A table containing measurable, numeric event data (sales, transactions). Typically has many rows and foreign keys. |
| **Dimension Table** | A table containing descriptive attributes used to filter or group facts (Products, Customers, Dates). |
| **Surrogate Key** | An artificial, system-generated integer key used in dimension tables instead of natural business keys. |
| **Semi-Structured Data** | Data with partial structure (JSON, XML) that must be expanded/parsed into tabular form in Power Query. |
| **Enable Load** | Power Query setting that controls whether a query's output is loaded into the data model. Disable to keep helper queries out of the model. |

---

## Domain 2: Model the Data (25–30%)

### 🗂️ Design and Implement a Data Model

| Term | Definition |
|---|---|
| **Star Schema** | A data model design with one central fact table surrounded by dimension tables. Optimal for Power BI performance. |
| **Snowflake Schema** | Extension of star schema where dimension tables are normalized into sub-dimensions. Less ideal for Power BI. |
| **Relationship** | A defined link between two tables based on a common key column. Enables cross-table filtering and calculations. |
| **Cardinality** | The uniqueness of values in the columns forming a relationship: One-to-Many (1:*), Many-to-Many (*:*), One-to-One (1:1). |
| **Cross-Filter Direction** | Controls how filters propagate across a relationship: Single (one way) or Both (bidirectional). |
| **Active Relationship** | The default relationship used in DAX calculations and visuals. Only one active relationship can exist between two tables. |
| **Inactive Relationship** | A secondary relationship between two tables, activated on demand using `USERELATIONSHIP()` in DAX. |
| **Role-Playing Dimension** | A single dimension table (e.g., Date) that relates to a fact table multiple times for different purposes (OrderDate, ShipDate). Requires inactive relationships or separate views. |
| **Bidirectional Filtering** | A relationship setting that allows filters to flow in both directions; use with caution as it can cause ambiguity and performance issues. |
| **Date Table** | A dedicated table with a continuous, complete date column used for time intelligence. Must be marked as a date table. |
| **Calculated Column** | A DAX column computed row-by-row and stored in the model. Evaluated at data refresh time; increases model size. |
| **Calculated Table** | A DAX table generated entirely by a DAX expression, stored in the model. |
| **Hidden Column** | A field marked as hidden in the model, still usable in DAX but not visible to report authors. |

---

### 📐 Create Model Calculations by Using DAX

| Term | Definition |
|---|---|
| **DAX (Data Analysis Expressions)** | A formula language used in Power BI, Analysis Services, and Power Pivot to define calculations and queries. |
| **Measure** | A DAX calculation evaluated dynamically based on filter context. Not stored in the model; computed on the fly. |
| **Implicit Measure** | An automatic aggregation Power BI creates when a numeric field is dragged to a visual (SUM by default). Less flexible than explicit measures. |
| **Explicit Measure** | A named DAX measure you write yourself. Preferred for reusability, clarity, and more complex logic. |
| **Filter Context** | The set of active filters applied to a DAX calculation from slicers, rows/columns, and report filters. |
| **Row Context** | The current row being evaluated in a calculated column or iterator function. |
| **Context Transition** | When `CALCULATE()` converts row context into an equivalent filter context. Occurs automatically inside iterators when using measures. |
| **CALCULATE()** | The most important DAX function. Evaluates an expression in a modified filter context. Syntax: `CALCULATE(expression, filter1, filter2, ...)` |
| **FILTER()** | Returns a subset of a table based on a condition. Often used inside `CALCULATE()`. |
| **ALL()** | Removes filters from a table or column; used to calculate grand totals or percentages of total. |
| **ALLSELECTED()** | Removes internal filters but respects external/slicer filters. Used for % of filtered total. |
| **RELATED()** | Retrieves a value from a related table in a many-to-one direction. Used in calculated columns. |
| **RELATEDTABLE()** | Returns all rows from a related table on the many side. Used in calculated columns or measures. |
| **SUMX() / AVERAGEX() / COUNTX()** | Iterator (X) functions that evaluate a DAX expression row-by-row over a table, then aggregate results. |
| **IF()** | Conditional function: `IF(condition, true_result, false_result)`. |
| **SWITCH()** | Evaluates a value against multiple conditions; cleaner alternative to nested `IF()`. |
| **DIVIDE()** | Safe division function that returns an alternate result instead of an error when dividing by zero. |
| **DATEADD()** | Time intelligence function that shifts a date column by a specified interval (days, months, quarters, years). |
| **TOTALYTD() / TOTALQTD() / TOTALMTD()** | Time intelligence functions returning year-to-date, quarter-to-date, and month-to-date aggregations. |
| **SAMEPERIODLASTYEAR()** | Returns dates shifted back one year; used for year-over-year comparisons. |
| **RANKX()** | Returns the rank of a value in a table based on a DAX expression. |
| **VAR / RETURN** | DAX syntax for defining a named variable within a measure, improving readability and performance. |
| **Quick Measures** | Pre-built DAX measure templates in Power BI Desktop for common patterns (YOY change, rolling average, etc.). |
| **Calculation Group** | A model object that applies reusable DAX logic (e.g., YTD, Prior Year) to multiple measures via calculation items. Requires Tabular Editor. |
| **Semi-Additive Measure** | A measure that aggregates correctly across some dimensions (e.g., time) but not others (e.g., balance at end of period). Uses `LASTDATE()` or `OPENINGBALANCE`. |

---

### ⚡ Optimize Model Performance

| Term | Definition |
|---|---|
| **VertiPaq** | Power BI's in-memory columnar storage engine. Compresses and stores Import mode data. |
| **Cardinality (Performance)** | High-cardinality columns (many unique values) compress poorly in VertiPaq and slow performance. |
| **Performance Analyzer** | A built-in Power BI Desktop tool that logs the time taken by each visual to execute its DAX query, render, and other operations. |
| **DAX Query View** | A dedicated view in Power BI Desktop for writing and testing DAX queries against the model. |
| **Aggregation Tables** | Pre-summarized tables that direct Power BI to use cached summaries instead of scanning the full fact table. |
| **Reduce Granularity** | Remove unnecessary detail columns or pre-aggregate data in Power Query to shrink model size. |
| **Unnecessary Columns/Rows** | Unused columns and pre-filtered rows should be removed in Power Query before loading to reduce model bloat. |

---

## Domain 3: Visualize and Analyze the Data (25–30%)

### 📊 Create Reports

| Term | Definition |
|---|---|
| **Report Canvas** | The design surface in Power BI Desktop where visuals, text boxes, images, and shapes are arranged. |
| **Visual** | A chart, table, map, or other graphical element placed on a report page to represent data. |
| **Bar / Column Chart** | Compares values across categories. Bar = horizontal; Column = vertical. |
| **Line Chart** | Shows trends over time or continuous data. |
| **Scatter Chart** | Plots relationships between two numerical measures; useful for identifying correlations and outliers. |
| **Matrix Visual** | A pivot-table–style visual supporting row and column groupings with drill-down capability. |
| **Card Visual** | Displays a single KPI number prominently. |
| **KPI Visual** | Compares a metric against a target with status indicators (on track / behind). |
| **Slicer** | An interactive filter visual placed on the report canvas that users click to filter other visuals. |
| **Paginated Report** | A pixel-perfect, print-ready report (built in Power BI Report Builder) optimized for tabular output and large data exports. |
| **Conditional Formatting** | Dynamically formats visual elements (color, icons, data bars) based on field values or rules. |
| **Themes** | A JSON-based configuration that standardizes colors, fonts, and formatting across an entire report. |
| **Narrative Visual** | An AI-powered visual that auto-generates natural language summaries of data in a report (Smart Narrative / Copilot Narrative). |
| **Visual Calculation** | A DAX calculation scoped to the visual's matrix structure; simpler than measures for running totals, ranks within a visual. |
| **Copilot (Report)** | An AI assistant in Power BI Service that can auto-generate report pages, suggest visuals, and summarize semantic models. |

---

### 🎨 Enhance Reports for Usability and Storytelling

| Term | Definition |
|---|---|
| **Bookmark** | A saved snapshot of a report's filter/slicer state and visual visibility. Used to create guided storytelling or navigation buttons. |
| **Tooltip Page** | A hidden report page displayed as a rich hover tooltip when a user hovers over a visual data point. |
| **Drill Through** | A navigation feature allowing users to right-click a data point and jump to a detail report page pre-filtered to that context. |
| **Drill Down** | Navigating to a lower hierarchy level within the same visual (e.g., Year → Quarter → Month). |
| **Cross-Filter** | When selecting a data point in one visual automatically filters other visuals on the same page. |
| **Cross-Highlight** | Similar to cross-filter, but non-selected values are dimmed rather than removed. |
| **Edit Interactions** | A mode in Power BI Desktop to control whether visuals cross-filter, cross-highlight, or are unaffected by each other. |
| **Sync Slicers** | A feature that synchronizes a slicer's selection across multiple report pages. |
| **Selection Pane** | A panel showing all visual layers on a page; used to manage visibility, z-order (layering), and grouping. |
| **Grouping (Visuals)** | Combining multiple visuals or shapes into a single moveable/resizable group. |
| **Automatic Page Refresh** | A setting that causes a report page to re-query data at a set interval; useful with DirectQuery for near-real-time monitoring. |
| **Mobile Layout** | A separate canvas view in Power BI Desktop optimized for phone-sized screens. |
| **Export Settings** | Controls on a report or visual governing whether users can export underlying data. |
| **Accessibility** | Designing reports with tab order, alt text, and color contrast to support screen readers and keyboard navigation. |
| **Personalized Visuals** | A feature allowing report consumers to swap visual types or fields themselves without Edit permission. |

---

### 🔎 Identify Patterns and Trends

| Term | Definition |
|---|---|
| **Analyze Feature** | A right-click option on a visual data point that uses AI to explain increases, decreases, or distribution differences. |
| **Grouping (Data)** | Manually combining values in a field into named buckets (e.g., grouping cities into regions). |
| **Binning** | Automatically splitting a continuous numeric column into equal-width or equal-frequency ranges. |
| **Clustering** | An AI feature in scatter charts that automatically detects natural groupings of data points using k-means. |
| **AI Visuals** | Built-in visuals: Key Influencers, Decomposition Tree, Q&A, Smart Narrative, and Anomaly Detection. |
| **Key Influencers Visual** | An AI visual that identifies which factors most significantly drive a selected metric. |
| **Decomposition Tree Visual** | An AI visual for interactive root-cause analysis; allows users to drill into dimensions to explain a metric. |
| **Q&A Visual** | Allows users to type natural language questions and receive visual answers driven by the semantic model. |
| **Reference Line** | A fixed constant, average, or percentile line overlaid on a chart to provide context. |
| **Error Bars** | Visual indicators on charts showing the range of uncertainty or variability around data points. |
| **Forecasting** | A line chart feature using exponential smoothing to project future trend values with a confidence interval. |
| **Anomaly Detection** | An AI feature on line charts that automatically flags data points deviating significantly from expected values. |
| **Outlier** | A data point that differs significantly from the overall pattern; identifiable via scatter charts, clustering, or anomaly detection. |

---

## Domain 4: Manage and Secure Power BI (15–20%)

### 🏢 Create and Manage Workspaces and Assets

| Term | Definition |
|---|---|
| **Workspace** | A collaborative container in Power BI Service for storing and sharing reports, dashboards, datasets, and dataflows. |
| **My Workspace** | A personal, private workspace available to every user. Not suitable for team collaboration or app publishing. |
| **Workspace Roles** | Access levels: Admin, Member, Contributor, Viewer. Control what users can do within the workspace. |
| **App (Power BI)** | A packaged, read-only collection of dashboards and reports published from a workspace for broad consumption. |
| **Dashboard** | A single-page canvas in Power BI Service composed of pinned tiles from reports or Q&A. Supports real-time data tiles. |
| **Pinned Tile** | A visual, image, or text box copied from a report onto a dashboard. |
| **Data Alert** | A notification triggered when a dashboard tile KPI crosses a user-defined threshold. |
| **Subscription** | An automated email snapshot of a report page or dashboard sent to recipients on a schedule. |
| **Promote (Content)** | A workspace-level endorsement indicating a dataset/report is ready for use. Requires workspace contributor rights. |
| **Certify (Content)** | An organizational endorsement (requires a specific admin-designated role) indicating a dataset meets data quality standards. |
| **Gateway (On-Premises Data Gateway)** | A bridge allowing Power BI Service to securely query on-premises data sources for scheduled refresh and DirectQuery. |
| **Scheduled Refresh** | A configuration in Power BI Service that automatically re-imports data into a semantic model on a timed schedule (up to 8×/day for Pro). |
| **Semantic Model (Dataset)** | The published data model in Power BI Service that reports connect to. Contains tables, relationships, measures, and security roles. |
| **Incremental Refresh** | A refresh policy that only updates new/changed rows rather than reloading the entire dataset. Requires Premium or PPU. |

---

### 🔒 Secure and Govern Power BI Items

| Term | Definition |
|---|---|
| **Row-Level Security (RLS)** | A feature that restricts which rows of data a user can see, based on DAX filter rules and security role membership. |
| **Static RLS** | RLS where filter rules are hard-coded (e.g., `[Region] = "East"`). Simple but requires manual maintenance. |
| **Dynamic RLS** | RLS using `USERPRINCIPALNAME()` or `USERNAME()` to filter data based on the logged-in user's identity. |
| **RLS Role** | A named set of DAX filter rules in the data model. Users are assigned to roles via Power BI Service. |
| **Object-Level Security (OLS)** | Restricts visibility of entire tables or columns to specific roles. Defined in Tabular Editor; hidden from unauthorized users. |
| **Sensitivity Label** | A Microsoft Purview classification tag (e.g., Confidential, Public) applied to Power BI items to control sharing and export behavior. |
| **Item-Level Permissions** | Direct permissions (Read, Build, Reshare) granted to a specific report or dataset without granting full workspace access. |
| **Build Permission** | Grants a user the ability to create new reports using an existing semantic model. Required for self-service analytics. |
| **Reshare Permission** | Allows a user who has been granted access to further share an item with others. |
| **Service Principal** | A non-human identity (app registration in Azure AD) used for automated/programmatic Power BI operations via API. |

---

## Quick Reference: Key DAX Functions

| Function | Purpose |
|---|---|
| `CALCULATE()` | Evaluate expression in modified filter context |
| `FILTER()` | Return subset of table matching a condition |
| `ALL()` | Remove all filters from a table/column |
| `ALLSELECTED()` | Remove internal filters, respect slicers |
| `RELATED()` | Fetch value from related table (many-to-one) |
| `RELATEDTABLE()` | Return related rows from one-to-many side |
| `SUMX()` | Iterate rows of a table, sum an expression |
| `DIVIDE()` | Safe division (no divide-by-zero errors) |
| `IF()` | Conditional branching |
| `SWITCH()` | Multi-condition evaluation |
| `DATEADD()` | Shift dates by interval for time intelligence |
| `TOTALYTD()` | Year-to-date aggregation |
| `SAMEPERIODLASTYEAR()` | Prior year equivalent date range |
| `USERELATIONSHIP()` | Activate an inactive relationship in a calculation |
| `USERPRINCIPALNAME()` | Return current user's UPN for dynamic RLS |
| `RANKX()` | Rank a value within a table |
| `COUNTROWS()` | Count rows in a table or filtered table |
| `DISTINCTCOUNT()` | Count unique values in a column |
| `FORMAT()` | Convert a value to a formatted text string |
| `CONCATENATE()` | Join two text strings |
| `BLANK()` | Return a blank value (Power BI's version of NULL) |

---

## Storage Mode Cheat Sheet

| Mode | Data Stored Locally? | Always Current? | Best For |
|---|---|---|---|
| Import | ✅ Yes | ❌ Requires refresh | Large models, fast performance |
| DirectQuery | ❌ No | ✅ Yes | Real-time, large/live sources |
| Dual | ✅ Depends on query | Partial | Dimension tables in composite models |
| Live Connection | ❌ No | ✅ Yes | Reusing existing AS/Power BI models |

---

## Workspace Roles Summary

| Role | Publish | Edit | View | Manage Access |
|---|---|---|---|---|
| Admin | ✅ | ✅ | ✅ | ✅ |
| Member | ✅ | ✅ | ✅ | ⚠️ Limited |
| Contributor | ✅ | ✅ | ✅ | ❌ |
| Viewer | ❌ | ❌ | ✅ | ❌ |

---

*Sources: Microsoft Learn PL-300 Study Guide (updated January 15, 2026) · Microsoft Power BI Documentation*