# Power BI Concepts & Solutions

## Question 8: Dynamic Metric Switching

A report must allow users to switch dynamically between metrics such as Sales, Margin, and Profit. What feature enables this?

### 1. Primary Feature: Field Parameters

**Field Parameters** allow report authors to dynamically change the fields or measures being displayed inside visuals using a slicer.

#### How to Implement:

1. **Create the Parameter:**
   Navigate to the **Modeling** tab in Power BI Desktop $\rightarrow$ Click **New Parameter** $\rightarrow$ Select **Fields**.

2. **Select Metrics:**
   Add your existing measures (`[Total Sales]`, `[Margin]`, `[Profit]`) into the parameter definition box and name the parameter (e.g., `Select Metric`).

3. **Add Slicer & Visuals:**

   * Check the option to **Add slicer to this page**.

   * Drag the newly created Parameter field into the **Value** or **Axis** section of your charts/tables.

4. **User Experience:**
   When users pick a metric from the slicer, the visual updates its values, tooltips, axis labels, and formatting dynamically.

### 2. Alternative Method: DAX `SWITCH()` Pattern

Before Field Parameters were added to Power BI, developers used a custom DAX pattern using a disconnected table.

#### Implementation Steps:

1. **Create a Disconnected Table:**
   Create a standalone table with a single column containing the metric names:

   ```
   Metric Table = DATATABLE(
       "Metric Name", STRING,
       {
           {"Sales"},
           {"Margin"},
           {"Profit"}
       }
   )
   ```

2. **Create a Dynamic DAX Measure:**
   Use the `SWITCH` and `SELECTEDVALUE` functions to conditionally evaluate the correct calculation:

   ```
   Dynamic Metric = 
   VAR SelectedMetric = SELECTEDVALUE('Metric Table'[Metric Name], "Sales")
   RETURN
       SWITCH(
           SelectedMetric,
           "Sales", [Total Sales],
           "Margin", [Margin],
           "Profit", [Profit],
           [Total Sales] -- Default fallback
       )
   ```

3. **Configure Visuals:**
   Put `Metric Table[Metric Name]` in a slicer and use `[Dynamic Metric]` inside your visual's value field.

---

## Question 9: Securely Sharing Reports Externally

You want to securely share a report with people outside your organization so they can interact with it. What must be enabled first?

To securely share a Power BI report with external users (guest users) so they can interact with it, the following settings and configurations must be enabled:

### 1. Power BI Admin Portal Settings (Tenant Settings)

In the **Power BI Admin Portal** under **Tenant settings** $\rightarrow$ **Export and sharing settings**:

* **"Invite external users to your organization"** must be set to **Enabled** for the entire organization or specified security groups.

* **"Allow external guest users to edit and manage content in the organization"** (Optional) can be enabled if guest users require edit permissions instead of read-only interactive access.

### 2. Microsoft Entra ID (Azure AD) Settings

In Microsoft Entra ID under **External Collaboration Settings**:

* External user access and guest invite settings must allow internal users to invite external guest users (Microsoft Entra B2B sharing).

### 3. Licensing Requirements

External users must be properly licensed to access the content. One of the following must apply:

* **User-based licensing:** The external user has a **Power BI Pro** or **Premium Per User (PPU)** license assigned (either in their home organization or by your organization).

* **Capacity-based licensing:** The content is hosted in a workspace backed by a **Power BI Premium capacity (P SKU)** or **Fabric capacity (F64 or higher)**, which allows guest users with free Power BI accounts to view and interact with the report.

---

## Question 10: Query Folding Diagnostics

You need to identify which transformation step caused query folding to stop. What should you check?

* [x] **View Native Query**
* [ ] Column Statistics
* [ ] Lineage view
* [ ] Q preview

### Explanation:

* **View Native Query:** In Power Query (Power BI), right-clicking a step in the **Applied Steps** pane displays the **View Native Query** option. If this option is enabled, the step is folded back to the database. If it is grayed out for a specific step, it indicates that Power Query could not translate that transformation into the source's native database language (such as SQL), meaning **query folding stopped at that step**.

### Why Other Options Are Incorrect:

* **Column Statistics:** Displays data distribution, error rates, and empty value counts for column data profiling.
* **Lineage View:** Displays dependencies between workspaces, semantic models, reports, and data sources across Power BI service.
* **Q Preview (Data Preview):** Shows a preview of the transformed data table at a specific step without giving information about backend query translation.

---

## Question 11: Measure Evaluation in Granular Context

A measure returns unexpectedly low values when placed in a table containing multiple dimension fields. What is the most likely cause?

* [ ] The field formatting is incorrect
* [ ] Auto date/time is disabled
* [ ] The matrix visual is too large
* [x] **The measure is evaluated at a more granular filter context**

### Explanation:

* **Filter Context Granularity:** Placing a measure into a visual alongside multiple dimension fields introduces additional filtering conditions per row (a higher degree of granularity). Since the measure calculates over a smaller, highly filtered subset of rows for each row combination, the output yields smaller numeric values than expected.

### Why Other Options Are Incorrect:

* **The field formatting is incorrect:** Formatting changes how numbers appear visually (e.g., currency symbols or decimal places), not their underlying numerical values.
* **Auto date/time is disabled:** Disabling auto date/time disables automatic date hierarchy tables, but does not decrease calculated measure values across regular dimensions.
* **The matrix visual is too large:** Canvas and visual size impact performance and display layout, but do not alter DAX measure evaluation logic.

# Question 12: Dynamic Calculation Logic & Time Intelligence in Power BI

**Question:** You need to control which calculation logic is applied to a measure based on a user’s slicer selection for “LY”, “MTD”, or “QTD”. What should you use?

* [ ] Field parameters
* [x] **Calculation groups**
* [ ] Role-playing date tables
* [ ] Synonyms

---

## Detailed Explanation

### Primary Solution: Calculation Groups

**Calculation groups** (configured via Tabular Editor in Power BI Desktop) allow you to apply reusable DAX calculation modifiers across all existing measures in your semantic model using the `SELECTEDMEASURE()` function.

#### How It Works:
1. Instead of creating explicit measures for every combination (e.g., *Sales MTD*, *Sales QTD*, *Sales LY*, *Profit MTD*, *Profit QTD*, *Profit LY*), you create a single **Calculation Group**.
2. Inside the calculation group, you define **Calculation Items** using `SELECTEDMEASURE()`:
   * **MTD Item:** `CALCULATE(SELECTEDMEASURE(), DATESMTD('Date'[Date]))`
   * **QTD Item:** `CALCULATE(SELECTEDMEASURE(), DATESQTD('Date'[Date]))`
   * **LY Item:** `CALCULATE(SELECTEDMEASURE(), SAMEPERIODLASTYEAR('Date'[Date]))`
3. Placing the Calculation Group column into a report slicer allows end users to select "LY", "MTD", or "QTD" to instantly transform any measure displayed in the report's visuals.

---

## Comparison: Why Other Options Are Incorrect

* **Field Parameters:** Used to dynamically pick which *columns or measures* to display in a visual (e.g., switching between `[Sales]` and `[Profit]`). They do not alter the underlying DAX aggregation pattern of existing measures across multiple metrics.
* **Role-Playing Date Tables:** Used when a fact table has multiple relationship pathways to a single date table (e.g., `Order Date` vs. `Ship Date`), typically activated via `USERELATIONSHIP()`.
* **Synonyms:** Used in Power BI Q&A feature to map natural language words and phrasing to dataset columns and tables.

---

## Time Intelligence Acronyms Reference

In data analytics and business reporting, these acronyms represent standard cumulative and period-over-period time horizons:

| Acronym | Full Form | Definition | Example (If Today is Oct 15) |
| :--- | :--- | :--- | :--- |
| **LY** | **Last Year** | Compares metrics against the exact matching period from the previous year. | Same date range in the prior calendar year |
| **MTD** | **Month-to-Date** | Aggregates data from the 1st day of the current month up to the current date. | Oct 1 to Oct 15 |
| **QTD** | **Quarter-to-Date** | Aggregates data from the 1st day of the current quarter up to the current date. | Oct 1 to Oct 15 (Start of Q4) |
| **YTD** | **Year-to-Date** | Aggregates data from the 1st day of the current calendar/fiscal year up to the current date. | Jan 1 to Oct 15 |
