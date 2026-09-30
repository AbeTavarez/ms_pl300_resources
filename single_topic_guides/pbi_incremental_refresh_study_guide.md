# PL-300 Exam Study Guide: Incremental Refresh

Incremental Refresh optimizes dataset scheduled refreshes by importing only new or modified data rather than reloading entire historical datasets. It is a critical topic on the **PL-300 (Microsoft Power BI Data Analyst)** exam.

---

## 1. Core Concepts & Benefits

### Why Use Incremental Refresh?

* **Faster Refreshes:** Only data that has changed needs to be queried and processed.
* **Reliability:** Shorter refreshes reduce the risk of network timeouts or source system locks.
* **Resource Consumption:** Reduces memory, CPU, and network usage on both Power BI capacity and data sources.
* **Large Dataset Support:** Enables working with datasets that exceed standard import size limits.

---

## 2. Deep Dive: `RangeStart` and `RangeEnd` Parameters

`RangeStart` and `RangeEnd` are reserved parameters in Power Query used by Power BI to partition data by date ranges.

### Purpose and Functionality

1. **In Power BI Desktop (Development Phase):**
   * Act as **developer placeholders/filters**.
   * By setting `RangeStart` and `RangeEnd` to a small date range (e.g., 1 month), you load only a small sample into Power BI Desktop.
   * Keeps the `.pbix` file small, lightweight, and fast during development.

2. **In Power BI Service (Production Phase):**
   * Upon publishing, the Power BI Service **overrides** the hardcoded dates typed in Desktop.
   * Power BI dynamically generates date-based partitions according to the configured refresh policy.
   * On scheduled runs, Power BI queries **only** the active refresh partition (e.g., last 10 days), leaving historical partitions untouched.

---

## 3. Selecting & Managing Parameters Across Environments

### How to Select `RangeStart` and `RangeEnd` in Desktop

* **Choose a Small Sample Period:** Set a short recent interval (e.g., 1 week or 1 month) containing enough sample data to build visuals and measures.
  * Example:
    * `RangeStart` = `2024-01-01 00:00:00`
    * `RangeEnd`   = `2024-02-01 00:00:00`
* **Purpose:** This controls Desktop file size. You do **not** need to load years of historical data on your local machine while designing reports.

### What Happens When Published to Power BI Service?

* **Automatic Override:** You **do not manually change** parameter dates after publishing.
* Power BI reads the **Incremental Refresh Policy** (e.g., *Store 5 Years, Refresh 10 Days*) and current date to manage query boundaries automatically:
  * **First Refresh:** Power BI creates initial historical partitions covering the full archive window (e.g., 5 years).
  * **Subsequent Refreshes:** Power BI automatically recalculates `RangeStart` and `RangeEnd` every day to pull only data within the rolling refresh window (e.g., `Today - 10 Days` to `Today`).

---

## 4. What Happens Outside the Archive Window?

When configuring a policy (e.g., **Store 5 Years / Refresh 10 Days**), Power BI maintains data within a rolling historical window.

### Automated Partition Dropping (Purging)

1. **Rolling Progression:** As time moves forward into new calendar periods, Power BI evaluates partition age against the policy archive boundary.
2. **Permanent Purging:** Partitions containing data that fall completely **outside** the archive window (e.g., older than 5 years) are automatically **dropped and deleted** from the Power BI dataset/semantic model.
3. **Source System Safety:** Deleting partitions inside Power BI **does not delete data from the underlying source database** (e.g., SQL Server remains untouched).
4. **Query Impact:** Once a partition is dropped, historical data prior to that threshold is no longer accessible in report visuals or measures.

---

## 5. Technical Requirements & Rules for the Exam

To configure Incremental Refresh successfully, you must adhere to strict requirements:

1. **Exact Case-Sensitive Naming:**
   * Parameters must be named exactly `RangeStart` and `RangeEnd` (capital `R`, `S`, and `E`).
   * Incorrect casing (e.g., `rangestart` or `Range_Start`) will cause the policy setup to fail.

2. **Data Type Must Be Date/Time (`DateTime`):**
   * Parameters **must** use the `Date/Time` data type.
   * If the source date column is stored as `Date` or `Integer` (e.g., `20240101`), convert types in Power Query while maintaining Query Folding.

3. **Strict Operator Logic (`>=` and `<`):**
   * Filter rule: `[Date Column] >= RangeStart AND [Date Column] < RangeEnd`
   * **Why:** Using `>=` on start and `<` on end ensures midnight boundary values belong to exactly **one** partition, preventing duplicate or missing records across adjacent partitions.

4. **Query Folding Dependency:**
   * The source must support **Query Folding** (e.g., SQL Server, Azure SQL, Snowflake).
   * If query folding breaks before the filter step, Power BI Service will fail or pull full historical datasets during refresh.

---

## 6. Step-by-Step Setup Flow

```
[ Power Query Editor ]
  1. Create RangeStart & RangeEnd parameters (Type: DateTime).
  2. Filter Fact Table: [Date] >= RangeStart AND [Date] < RangeEnd.
  3. Ensure step folds to native SQL query.

[ Power BI Desktop ]
  4. Right-click table in Data View -> Incremental Refresh.
  5. Enable policy:
     - Set Archive Window (e.g., Store 5 Years)
     - Set Refresh Window (e.g., Refresh 7 Days)
  6. Publish .pbix to Power BI Service.

[ Power BI Service ]
  7. Configure scheduled refresh / gateway settings.
  8. Run initial refresh (populates historical + active partitions).
```

---

## 7. Key Exam Concepts Summary

| Concept / Feature | Desktop Behavior | Service Behavior / Rule |
| :--- | :--- | :--- |
| **`RangeStart` / `RangeEnd`** | Static sample filter set manually by developer. | Dynamically overwritten by service partition engine. |
| **Archive Window** | Controls policy definition (e.g., 5 Years). | Purges/drops partitions older than configured limit. |
| **Refresh Window** | Defines rolling update interval (e.g., 10 Days). | Query runs strictly against data within this moving window. |
| **Filtering Operator** | Requires `>= RangeStart` and `< RangeEnd`. | Guarantees zero duplicate rows across partition boundaries. |
| **Query Folding** | Required step in Power Query transformation. | Essential for source-side query filtering during refreshes. |

---

## 8. Sample PL-300 Practice Questions

### Question 1
**Scenario:** An analyst creates parameters named `rangeStart` and `rangeEnd` with `Date/Time` data types and applies them to filter a sales table. When configuring Incremental Refresh in Power BI Desktop, the option to enable the policy is unavailable. What is the cause?

A. Incremental refresh requires a Power BI Premium capacity workspace.  
B. Parameter names are case-sensitive and must be `RangeStart` and `RangeEnd`.  
C. The parameters must be set to `Date` data type instead of `Date/Time`.  
D. Query folding must be disabled before setting an incremental policy.  

* **Correct Answer:** **B**  
* **Explanation:** Power BI looks for exact, case-sensitive parameter names (`RangeStart` and `RangeEnd`). Lowercase variations will not be recognized.

### Question 2
**Scenario:** A dataset is configured with an incremental refresh policy to **Store 3 Years** and **Refresh Last 7 Days**. Today is October 1, 2026. What happens to sales records dated May 2023 during tonight's scheduled refresh?

A. Records are re-imported from the database source.  
B. Records are deleted from the underlying source database.  
C. Records remain preserved inside historical partitions without re-querying the source database.  
D. Records are permanently dropped from the Power BI semantic model because they exceed 7 days.  

* **Correct Answer:** **C**  
* **Explanation:** May 2023 is within the 3-year archive window (October 2023–2026). It stays cached inside historical partitions and is not re-queried during the 7-day refresh run.