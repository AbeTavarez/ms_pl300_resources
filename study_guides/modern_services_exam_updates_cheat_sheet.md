# Microsoft Power BI PL-300 Exam: Modern Updates & Services Cheat Sheet

This cheat sheet details the newest services, storage modes, AI tools, and report features introduced to the **Microsoft PL-300: Power BI Data Analyst** exam, including Microsoft Fabric integration, Direct Lake, Visual Calculations, and Copilot.

---

## 1. Microsoft Fabric & OneLake Ecosystem Integration

Power BI serves as the primary visualization and semantic modeling layer within Microsoft Fabric.

| Concept | Primary Exam Distinction | 
 | ----- | ----- | 
| **Microsoft Fabric** | An end-to-end unified analytics platform. Power BI datasets are now referred to as **Semantic Models** within Fabric workspaces. | 
| **OneLake** | The unified "OneDrive for Data" lakehouse storage powering Fabric. Power BI can directly query data stored in Delta Parquet formats inside OneLake. | 
| **Power BI Semantic Models** | Replaces the traditional term "Dataset". Defines the tables, relationships, DAX measures, and security (RLS/OLS) built over data sources. | 
| **Fabric Capacity Workspaces** | Replaces old Premium Capacity naming for hosting high-scale Power BI semantic models and Fabric artifacts. | 

---

## 2. Direct Lake Mode (High-Yield Storage Mode Concept)

Fabric introduces **Direct Lake**, a storage mode that combines the query speed of **Import Mode** with the real-time capabilities of **DirectQuery**.

| Feature | Import Mode | DirectQuery | Direct Lake (New) | 
 | ----- | ----- | ----- | ----- | 
| **Data Location** | Duplicated into VertiPaq RAM. | Stored in target database. | Reads directly from Delta Parquet files in **OneLake**. | 
| **Performance** | Very Fast. | Can be slow / resource heavy. | **Near-Import Speed** (loads Delta column chunks into VertiPaq memory on-demand). | 
| **Data Freshness** | Delayed (Requires Scheduled Refresh). | Real-time. | **Real-Time** (No refresh required when lakehouse updates). | 
| **DAX Support** | Full DAX support. | Limited DAX / transforms. | **Full DAX support** (Fallback to DirectQuery if DAX query exceeds memory guardrails). | 

---

## 3. Visual Calculations (New DAX Capability)

Visual Calculations allow you to write DAX directly on a visual rather than building a measure in the model.

| Feature | Model Measures | Visual Calculations (New) | 
 | ----- | ----- | ----- | 
| **Storage & Context** | Stored in Data Model; evaluates dynamically across any visual context. | Stored **only on the specific visual**; operates directly on the visual matrix/table data structure. | 
| **Performance** | Requires engine to query model tables. | **Extremely Fast** (operates on already aggregated visual data). | 
| **Syntax Simplification** | Requires complex DAX (`CALCULATE`, `OFFSET`, `WINDOW`). | Uses intuitive visual functions: `EXPAND()`, `COLLAPSE()`, `PREVIOUS()`, `NEXT()`, `RUNNINGSUM()`, `MOVINGAVERAGE()`. | 
| **Exam Scenario** | Calculate running totals or year-over-year variances *without* writing complex evaluation context DAX in the model. |  | 

---

## 4. Generative AI & Copilot for Power BI

| Feature | Capabilities & Exam Application | 
 | ----- | ----- | 
| **Copilot for Power BI** | Integrated AI assistant across Power BI Desktop and Service. | 
| **DAX Generation** | Assists in writing and explaining DAX formulas, measures, and quick calculations. | 
| **Report Page Creation** | Generates entire report pages based on natural language prompts describing desired insights. | 
| **Executive Summaries** | Builds natural language narrative visual summaries highlighting trends and anomalies for leadership. | 

---

## 5. Report Usability & Accessibility (Updated Objectives)

Recent exam updates heavily focus on building inclusive, highly customizable reports.

### A. Personalized Visuals

* **Function:** Allows end-users in Power BI Service to change visual types (e.g., bar chart to column chart), swap dimensions, or change measure aggregation *without* edit permissions to the report.

* **Configuration:** Enabled in Power BI Desktop via `File` $\rightarrow$ `Options and Settings` $\rightarrow$ `Options` $\rightarrow$ `Current File` $\rightarrow$ `Report Settings` $\rightarrow$ Toggle **Personalize Visuals**.

* **User Persistence:** Users can save their customized views as **Personal Bookmarks**.

### B. Report Accessibility Best Practices

* **Tab Order:** Set logical keyboard navigation sequence using `View` $\rightarrow$ `Selection Pane` $\rightarrow$ `Tab Order`.

* **Alt Text:** Add descriptive Alt Text to every visual for screen reader support (explain chart type, axes, and main takeaway).

* **Color & Contrast:** Use high-contrast themes and avoid red-green color combinations for colorblind accessibility.

* **Keyboard Navigation:** Ensure slicers, buttons, and drillthrough components can be completely operated via `Tab`, `Enter`, and Arrow keys.

---

## High-Yield Exam Pitfalls for Modern Updates

1. **Direct Lake Fallback:** If a query in Direct Lake mode exceeds memory limits or uses unsupported operations, it falls back to **DirectQuery mode**, which reduces performance.

2. **Personalized Visuals vs. Bookmarks:** Use **Personalized Visuals** when end-users need flexible ad-hoc changes to visual dimensions/types. Use **Report Bookmarks** when providing fixed, predefined views created by the author.

3. **Visual Calculations vs. Model Measures:** Visual calculations cannot be reused across multiple report pages or visuals—they exist **only** within the context of the visual where created.