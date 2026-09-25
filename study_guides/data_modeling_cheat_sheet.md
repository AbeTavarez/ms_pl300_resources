# Microsoft Power BI PL-300 Exam: Data Modeling Cheat Sheet

This cheat sheet covers the fundamental data modeling concepts, schema designs, relationship configurations, and optimization techniques frequently tested on the **Microsoft PL-300: Power BI Data Analyst** exam.

---

## 1. Schema Design: Star Schema vs. Snowflake Schema

Power BI engine (VertiPaq) is heavily optimized for **Star Schemas**.

| Aspect | Star Schema (Recommended) | Snowflake Schema (Avoid when possible) |
| :--- | :--- | :--- |
| **Structure** | Fact table in center surrounded by single-layer Dimension tables. | Dimensions are normalized into multiple related lookup tables (e.g., `Product` $\rightarrow$ `Subcategory` $\rightarrow$ `Category`). |
| **Performance** | **Faster** (fewer joins, better compression). | **Slower** (requires multi-hop relationship traversals). |
| **Usability** | **Simpler** for end-users to navigate fields. | **Complex** field lists with redundant hierarchies across tables. |
| **Exam Strategy** | Flatten/Denormalize snowflake dimensions into single dimension tables using Power Query (Merge Queries). |

---

## 2. Fact vs. Dimension Tables

| Attribute | Fact Tables | Dimension Tables |
| :--- | :--- | :--- |
| **Data Type** | Quantitative, numerical measurements, metrics (Sales, Qty, Cost). | Attributes, categories, descriptors (Customer Name, Region, Product Class). |
| **Granularity** | Detailed transaction level (high row count, narrow columns). | Unique entity level (low/moderate row count, wide columns). |
| **Keys** | Contains **Foreign Keys** pointing to dimensions. | Contains a **Primary Key** (surrogate key) uniquely identifying each row. |

---

## 3. Relationship Cardinality & Cross-Filter Direction

### Cardinality Options
* **`1-to-Many` (`1:*`) [Best Practice]:** One unique record in the Dimension table matches multiple records in the Fact table.
* **`1-to-1` (`1:1`):** Both columns contain unique values. Usually indicates tables should be merged into a single table.
* **`Many-to-Many` (`*:*`):** Neither column has unique values. Introduces ambiguity; resolve using a **Bridge Table** whenever possible.

### Cross-Filter Direction
* **Single Direction (Default):** Filtering flows from the "One" side (Dimension) to the "Many" side (Fact). Keeps performance optimal.
* **Both Directions (Bi-directional):** Filtering flows both ways. 
  * ⚠️ **Exam Warning:** Avoid using bi-directional filters on standard relationships as it can cause ambiguous paths, circular relationships, and major performance degradation.
  * **Valid Exception:** Required on dynamic RLS bridge tables when set to *"Apply security filter in both directions"*.

---

## 4. Role-Playing Dimensions & Active vs. Inactive Relationships

When a single dimension table connects to a fact table via multiple columns (e.g., `DateTable[Date]` connects to `Sales[OrderDate]`, `Sales[ShipDate]`, and `Sales[DueDate]`):

* **Rule:** Only **ONE** relationship can be **Active** (solid line) at a time. All others must be **Inactive** (dotted lines).
* **Option A (DAX):** Keep inactive relationships and activate them dynamically in measures using `USERELATIONSHIP()`:
  ```dax
  Shipped Sales = 
  CALCULATE(
      SUM(Sales[Amount]),
      USERELATIONSHIP(Sales[ShipDate], DateTable[Date])
  )
  ```
* **Option B (Power Query):** Duplicate/reference the dimension table (e.g., create a separate `Ship Date` table and `Order Date` table).

---

## 5. Calculated Columns vs. Measures vs. Calculated Tables

| Feature | Calculated Column | Explicit Measure | Calculated Table |
| :--- | :--- | :--- | :--- |
| **Evaluation Time** | During Data Refresh / Load. | At Query Time (on-the-fly when added to a visual). | During Data Refresh / Load. |
| **Context** | **Row Context** (evaluates row-by-row). | **Filter Context** (evaluates based on report filters/slicers). | Evaluates DAX table expression during refresh. |
| **RAM Impact** | Consumes RAM (stored in model memory). | Consumes CPU (uses minimal RAM). | Consumes RAM (stored in model memory). |
| **Best Used For** | Slicers, Rows/Columns in visuals, RLS grouping. | Dynamic aggregations (`SUM`, `AVERAGE`, `COUNT`). | Role-playing dates, bridge tables, summary tables. |

---

## 6. The Date Table Requirement

To use Time Intelligence functions (`TOTALYTD`, `SAMEPERIODLASTYEAR`, `DATEADD`), you **MUST** have a dedicated Date Table.

### Requirements for a Valid Date Table:
1. Must contain all contiguous dates for full calendar years represented in data.
2. Must contain at least one column configured with the **Date** data type.
3. Must contain unique values (no duplicates).
4. Must be explicitly marked as a **Date Table** (`Modeling` $\rightarrow$ `Mark as Date Table`).

---

## 7. Performance Optimization & VertiPaq Engine Rules

1. **High Cardinality Columns:** Columns with many unique values (e.g., GUIDs, transaction IDs, exact timestamps) destroy VertiPaq compression. 
   * **Action:** Remove GUIDs, split DateTime into separate `Date` and `Time` columns, or round timestamps.
2. **Auto Date/Time Feature:** Power BI creates hidden date tables for every date column by default.
   * **Action:** Disable *Auto Date/Time* in global settings to reduce memory footprint, and use a custom Date table instead.
3. **Star Schema Transformation:** Use Power Query to merge lookup tables (`Subcategory` into `Product`) rather than loading snowflake tables into the model.
4. **Reduce Unnecessary Columns:** Load only the columns needed for reporting; remove surplus columns in Power Query to reduce model size.

---

## High-Yield Data Modeling Exam Pitfalls

1. **"Bi-directional filtering causing unpredictable totals":** Switch relationship direction back to **Single** and solve filter propagation using DAX (`CROSSFILTER` or `CALCULATE`) or bridge tables.
2. **"Time Intelligence measures returning incorrect values":** Check if the Date table is **Marked as Date Table** and covers complete contiguous years without gaps.
3. **"Storage Engine Overhead / Large File Size":** Look for unmerged DateTime columns or high-cardinality ID keys that should be split or removed.
4. **"Context Transition in Calculated Columns":** Using a measure inside a calculated column triggers implicit `CALCULATE()`, converting row context to filter context, which can cause unexpected results or circular dependency errors.