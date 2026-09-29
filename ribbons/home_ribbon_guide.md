# Power BI Home Ribbon Guide

The **Home Ribbon** is the main control center in Power BI Desktop. It houses the most frequently used tools for data ingestion, query transformation, basic data modeling, visual creation, and report publishing.

```
┌───────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                                   HOME RIBBON                                                     │
├─────────────────┬──────────────┬──────────────────┬─────────────────┬─────────────┬─────────────┬─────────────────┤
│    Clipboard    │     Data     │  Queries / ETL   │     Insert      │ Daily DAX   │ Operational │   Publishing    │
│                 │              │                  │                 │             │             │                 │
│ • Paste / Copy  │ • Get Data   │ • Transform Data │ • New Visual    │ • New Measure│ • Sensitivity│ • Publish      │
│ • Format Painter│ • Excel      │ • Refresh        │ • Text Box      │ • Quick     │   Labels    │   (to Service)  │
│                 │ • Dataflows  │                  │ • More Visuals  │   Measure   │             │                 │
│                 │ • Enter Data │                  │                 │             │             │                 │
└─────────────────┴──────────────┴──────────────────┴─────────────────┴─────────────┴─────────────┴─────────────────┘
```

---

## 1. Functional Groups & Tool Breakdown

### A. Data Group
* **Get Data:** Connects to over 100+ data sources (SQL Server, Web, SharePoint, Azure, OData, etc.).
* **Excel Workbench / Power BI Datasets / SQL Server:** Quick-access shortcuts to the most common enterprise data sources.
* **Enter Data:** Manually creates a small static table directly within Power BI Desktop (useful for small mapping tables or static disconnect slicers).
* **Recent Sources:** Displays recently used connection strings for quick reconnection.

### B. Queries Group
* **Transform Data:** 
  * **Clicking top half:** Opens the **Power Query Editor** in a separate window to perform data transformations (M code).
  * **Data Source Settings:** Allows changing connection paths, credentials, or switching from file paths (e.g., local C:\ drive to SharePoint URL).
* **Refresh:** Re-executes the data load queries to pull the latest updated data from connected sources into the local desktop model.

### C. Insert Group
* **New Visual:** Adds a standard chart/visual container to the active report canvas.
* **Text Box:** Inserts formatted static text.
* **More Visuals:** Accesses Microsoft AppSource or imports a custom visual `.pbiviz` file from a local folder.

### D. Calculations Group
* **New Measure:** Opens the DAX formula bar to write a dynamic measure.
* **Quick Measure:** Provides a UI wizard to generate standard DAX formulas (e.g., running total, year-over-year change, star rating) without typing code manually.

### E. Share Group
* **Sensitivity:** Applies Microsoft Purview Information Protection labels (e.g., *Confidential*, *Public*, *Restricted*) to govern data security.
* **Publish:** Uploads the `.pbix` file, its semantic model, and report pages directly to a specified workspace in the **Power BI Service**.

---

## 2. PL-300 Exam Scenarios & Triggers

| Exam Scenario / Question Requirement | Correct Home Ribbon Action |
| :--- | :--- |
| **Change source credentials or update file path from local to SharePoint** | `Home` ➔ `Transform Data` ➔ `Data Source Settings` |
| **Create a small, static 3-row reference lookup table without external files** | `Home` ➔ `Enter Data` |
| **Publish report to Power BI Service workspace** | `Home` ➔ `Publish` |
| **Generate complex DAX math without writing manual code** | `Home` ➔ `Quick Measure` |
| **Import a custom `.pbiviz` visual file provided by a developer** | `Home` ➔ `More Visuals` ➔ `From My Files` |

---

## 3. Key Takeaways for PL-300
1. **Enter Data Limit:** `Enter Data` is meant for small reference tables (hard cap around 10MB or a few hundred rows). Do not use it for large transactional datasets.
2. **Transform Data vs. Refresh:** `Transform Data` modifies the structural query steps in Power Query; `Refresh` executes those steps to load updated data rows into the model.
3. **Publish Requirement:** You must sign in with an organizational account and save the `.pbix` file locally before the `Publish` button becomes active.