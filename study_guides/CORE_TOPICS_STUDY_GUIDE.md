# PL-300 Exam Core Topics Study Guide

This guide covers the most frequently tested topics on the **Microsoft Certified: Power BI Data Analyst Associate (PL-300)** exam.

---

## 1. `CALCULATE` & Filter Context

`CALCULATE` is the single most tested DAX function on the exam. It evaluates an expression under a modified filter context.

### Key Rules
1. **Context Transition:** Translates an existing **Row Context** into an equivalent **Filter Context**.
2. **Filter Modification:** Overrides existing filters on specified columns unless wrapped in `KEEPFILTERS()`.
3. **Filter Propagation:** Filters flow across active relationships from the **One** side to the **Many** side by default.

### Exam Pitfalls & Decision Matrix
* **Override Default Filters:** `CALCULATE([Sales], Product[Category] = "Audio")` replaces any existing slicer or visual filter on `Product[Category]`.
* **Preserve Visual Context:** `CALCULATE([Sales], KEEPFILTERS(Product[Category] = "Audio"))` calculates the intersection between existing filters and `"Audio"`.
* **Clear Filters:** Use `ALL(Table)` or `ALL(Table[Column])` inside `CALCULATE` to clear filters and compute overall totals or percentages.

---

## 2. Storage Modes: Import vs. DirectQuery vs. Dual

Choosing the correct storage mode depends on constraints regarding **data latency, model performance, dataset size, and DAX capabilities**.

| Feature / Scenario | Import | DirectQuery | Dual (Hybrid) |
| :--- | :--- | :--- | :--- |
| **Data Location** | Loaded into VertiPaq engine in memory. | Stored at source (SQL, Snowflake, etc.). | Cached in memory **and** queried live as needed. |
| **Data Freshness** | Scheduled Refresh / On-Demand. | Real-time (Live query per interaction). | Hybrid (Depends on visual context). |
| **Query Performance** | Blazing fast (In-memory). | Dependent on underlying source speed. | Fast for cached dimensions; flexible. |
| **DAX Capabilities** | 100% full DAX support. | Limited DAX support (no complex time intel). | Full DAX on imported side. |
| **Best Used For...** | High-performance reporting, offline data. | Near real-time requirements, massive datasets. | Slicer dimensions connected to DirectQuery fact tables. |

---

## 3. Relationship Cardinality & Cross-Filter Direction

Data modeling questions focus heavily on maintaining a clean **Star Schema** and avoiding performance issues.

### Cardinality Rules
* **1:Many (One-to-Many):** Standard, recommended relationship type in star schemas (Dimension $\rightarrow$ Fact).
* **1:1 (One-to-One):** Rarely used; usually indicates two tables that should be merged in Power Query.
* **Many-to-Many ($* : *$):** Used only when unique keys do not exist on either side (e.g., Target vs. Actuals at aggregated levels). Introduces ambiguity.

### Cross-Filter Direction
* **Single Direction (Default):** Filters flow from the dimension table (1) to the fact table ($*$).
* **Both (Bidirectional):** Filters flow in both directions.
  * **Exam Alert:** Avoid setting persistent bidirectional filters in the model schema! Use `CROSSFILTER()` inside a DAX measure instead to enable bidirectional filtering temporarily for a specific calculation.

---

## 4. Calculated Columns vs. Measures

Knowing *where* and *when* to calculate data is a primary focus of the PL-300 exam.

| Dimension | Calculated Column | Measure |
| :--- | :--- | :--- |
| **Evaluation Time** | During data refresh. | On-the-fly at query time (when visual loads). |
| **Context** | Evaluated per row (Row Context). | Evaluated per visual/cell filter (Filter Context). |
| **RAM / File Size** | Consumes RAM and increases file size (`.pbix`). | Minimal memory footprint; computed dynamically. |
| **Primary Use Cases** | Slicers, Rows, Columns, Axis labels, RLS rules. | Matrix values, Cards, KPI metrics, dynamic aggregation. |

---

## 5. Row-Level Security (RLS)

RLS restricts data access for specific users based on roles defined by DAX filter expressions.

### Static vs. Dynamic RLS
* **Static RLS:** Hardcoded values inside DAX table filters.
  * Example: `[Region] = "North America"`
* **Dynamic RLS:** Uses built-in security functions matched against an user access table.
  * Example: `[Email] = USERPRINCIPALNAME()` or `[User] = USERNAME()`

### Key Exam Facts
* **Creation vs. Testing:** Roles are defined and tested in **Power BI Desktop** ("View as Roles"), but users are assigned to roles in the **Power BI Service**.
* **Permissions:** RLS does **not** apply to workspace **Admin, Member, or Contributor** roles (they have edit access). It only restricts users with **Viewer** access.

---

## 6. Power Query: Merge vs. Append

Combining data in Power Query requires choosing between adding rows or adding columns.

### Append (SQL UNION equivalent)
* **What it does:** Stacks tables on top of each other.
* **Requirement:** Tables should have identical or similar column names and structure.
* **Use Case:** Combining monthly sales files (e.g., `Sales_Jan.csv`, `Sales_Feb.csv`).

### Merge (SQL JOIN equivalent)
* **What it does:** Combines columns from two tables based on a matching key.
* **Join Types to Know for the Exam:**
  * **Left Outer (Default):** All rows from the first table, matching rows from the second.
  * **Inner:** Only matching rows from both tables.
  * **Full Outer:** All rows from both tables.
  * **Anti Joins (Left/Right):** Only rows that **do not match** (great for finding missing keys/data auditing).