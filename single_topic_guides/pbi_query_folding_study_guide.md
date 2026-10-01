# PL-300 Exam Study Guide: Power Query Folding

Query Folding is one of the most heavily tested performance optimization topics on the **Microsoft Certified: Power BI Data Analyst Associate (PL-300)** exam.

---

## 1. What is Query Folding?

**Query Folding** is the ability of Power Query to convert data transformation steps written in M code into a **single native query statement** (such as SQL) and push it back to the source database server for execution.

### Key Benefits
* **Performance:** Processing happens on the database server, which is optimized for heavy data operations.
* **Network Efficiency:** Only the filtered/transformed subset of data is transferred over the network, rather than raw entire tables.
* **Prerequisite for Incremental Refresh:** Incremental Refresh **requires** Query Folding to dynamically filter data by `RangeStart` and `RangeEnd`.

---

## 2. Identifying Where Query Folding Breaks (Core Exam Scenario)

### The Exam Question Pattern

> **Question:** You need to identify which transformation step caused query folding to stop. What should you check?
>
> * A. View Native Query
> * B. Column Statistics
> * C. Lineage view
> * D. PQ Preview

---

### Breakdown of the Options

#### 1. **View Native Query (CORRECT ANSWER)**
* **How to use it:** Right-click a step in the **Applied Steps** pane in Power Query Editor.
* **Behavior:**
  * If **"View Native Query"** is **enabled (clickable)**, Query Folding is still active up to and including that step.
  * If **"View Native Query"** is **disabled (greyed out)**, Query Folding has broken at or before that step.
* **Diagnostic Strategy:** Click down through the **Applied Steps** pane from top to bottom. The first step where "View Native Query" becomes greyed out is the **exact step that stopped query folding**.

#### 2. **Column Statistics (Incorrect)**
* Displays data distribution metrics (nulls, distinct count, min/max values) at the top of a column in Power Query. It does not indicate query execution methods or query folding status.

#### 3. **Lineage View (Incorrect)**
* A feature in the **Power BI Service workspace** that shows visual connections between data sources, semantic models, reports, and dashboards. It operates at the workspace level, not inside Power Query Editor.

#### 4. **PQ Preview / Data Preview (Incorrect)**
* Refers to the sample rows shown in the central grid of Power Query Editor. It displays data samples but does not reveal the underlying SQL query generated for a step.

---

## 3. Query Folding Indicators in Power Query

Modern versions of Power Query Editor include **Step Folding Indicators** next to each step in the *Applied Steps* pane:

| Indicator Icon | Meaning | Explanation |
| :--- | :--- | :--- |
| **Folded** | Fully Folded | The step is converted to native source query language (e.g., SQL). |
| **Not Folded** | Not Folded | The step is processed locally in memory by the Power Query M engine. |
| **Might Fold** | Evaluation Pending | Status cannot be determined until the query is executed. |
| **Opaque** | Unknown / Special | Typically applies to custom or non-relational sources. |

---

## 4. Foldable vs. Non-Foldable Transformations

To pass the PL-300 exam, you must recognize which transformations preserve query folding and which break it.

### Foldable Transformations (Pushed to Source Server)
* Removing columns (`Table.SelectColumns`)
* Renaming columns (`Table.RenameColumns`)
* Basic row filtering (`Table.SelectRows` using `=`, `<>`, `>`, `<`)
* Merging / Joining tables from the **same** relational database source
* Group By / Summarizations (`Table.Group`)
* Adding simple custom columns using basic math operations

### Non-Foldable Transformations (Breaks Folding)
* **Changing Data Types:** Converting a string column to integer or date type often stops folding depending on the source driver.
* **Capitalization / Text Functions:** Applying custom M text functions like `Text.Upper` or custom RegEx scripts.
* **Adding Index Columns:** `Table.AddIndexColumn` requires row numbering that relational sources cannot guarantee natively.
* **Buffer Tables:** `Table.Buffer` explicitly loads data into local memory.
* **Merging Across Different Sources:** Joining a SQL Server table with an Excel sheet or CSV file breaks folding at the merge step.
* **Advanced Custom Code / Python / R Scripts:** Any custom external script step halts folding completely.

---

## 5. Best Practices & Optimization Rules for the Exam

1. **Reorder Transformation Steps:**
   * Always place **foldable steps** (filtering rows, removing columns) at the **top** of the *Applied Steps* list.
   * Place **non-foldable steps** (index columns, complex text formatting) at the **bottom** of the list.
   * *Exam Rule:* Filter data as early as possible so that when folding stops, the M engine only works on a small subset of data.

2. **Source Capabilities Matter:**
   * Relational databases (SQL Server, Azure SQL, Snowflake, Oracle) fully support Query Folding.
   * Flat files (CSV, Excel, JSON, Web API) do **NOT** support Query Folding because they lack a database engine to execute queries.

---

## 6. Sample PL-300 Practice Questions

### Practice Question 1
**Scenario:** You are loading data from an Azure SQL Database table containing 50 million rows into Power BI Desktop. You need to apply several transformations while ensuring maximum performance during data refresh.

Which two steps should you perform **first** in Power Query? (Choose two)

A. Add an Index Column starting from 1.  
B. Filter out inactive customer rows.  
C. Remove unnecessary historical text columns.  
D. Convert text columns to uppercase using custom M scripts.  

* **Correct Answer:** **B and C**
* **Explanation:** Filtering rows (`B`) and removing unused columns (`C`) are foldable transformations. Executing them first reduces the dataset size on the SQL server before any non-foldable steps are evaluated.

---

### Practice Question 2
**Scenario:** You configure Incremental Refresh for a Sales table connected to SQL Server. During testing, you receive an error stating that the dataset cannot be refreshed incrementally.

What is the most likely cause of this issue?

A. The table uses DirectQuery mode instead of Import mode.  
B. `RangeStart` and `RangeEnd` parameters use `Date` data type instead of `Date/Time`.  
C. A step applied prior to filtering by `RangeStart` and `RangeEnd` broke Query Folding.  
D. The table contains calculated columns created in DAX.  

* **Correct Answer:** **C**
* **Explanation:** Incremental Refresh relies entirely on Query Folding to pass `RangeStart` and `RangeEnd` filters to the SQL server. If an early step breaks query folding, Incremental Refresh fails.