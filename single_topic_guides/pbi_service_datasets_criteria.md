# Dataset Creation Criteria: Power BI Service vs. Power BI Desktop

## Question Analysis

**Question:** You plan to create several datasets by using the Power BI service. You have the files configured as shown in the following table:

| File name | File type | Size | Location |
| :--- | :--- | :--- | :--- |
| **Data 1** | TSV | 50 MB | Microsoft OneDrive |
| **Data 2** | XLSX | 3 GB | Local |
| **Data 3** | XML | 100 MB | Microsoft OneDrive for Business |
| **Data 4** | CSV | 2 GB | Microsoft OneDrive |
| **Data 5** | JPG | 5 MB | Local |

**Correct Answer:** **Data 2 (XLSX)** and **Data 4 (CSV)**

---

## Core Criteria & Decision Logic

The selection criteria depend directly on the phrase **"by using the Power BI service"** (cloud interface) rather than **Power BI Desktop**.

### 1. File Type Limitations in Power BI Service

When creating a dataset **directly within the Power BI Service** (using the *Get Data / Upload* feature in the web browser without opening Power BI Desktop), the platform natively supports only specific file formats:

* **Excel Workbooks** (`.xlsx`, `.xlsm`)
* **Comma-Separated Values** (`.csv`)
* **Power BI Desktop Files** (`.pbix`)

---

### 2. Breakdown of File Options

* **Data 1 (TSV):** **Not Supported directly in Power BI Service.** Tab-Separated Values (TSV) require the Power Query transformation engine available in Power BI Desktop to parse delimiters correctly upon import.
* **Data 2 (XLSX):** **Supported.** Excel files are natively supported for direct dataset creation in Power BI Service regardless of location.
* **Data 3 (XML):** **Not Supported directly in Power BI Service.** Structured markup files like XML or JSON need Power Query in Power BI Desktop to expand nodes and build relational tables.
* **Data 4 (CSV):** **Supported.** Delimited CSV files are natively supported for direct import in Power BI Service.
* **Data 5 (JPG):** **Not Supported.** Image files cannot be converted into dataset schema.

---

## Power BI Desktop vs. Power BI Service Comparison

| Feature / Source | Power BI Desktop | Power BI Service (Direct Upload) |
| :--- | :--- | :--- |
| **Engine Used** | Full Power Query Engine | Simplified Service File Importer |
| **Supported File Types** | `.xlsx`, `.csv`, `.tsv`, `.xml`, `.json`, `.pdf`, `.parquet`, Folders, etc. | `.xlsx`, `.csv`, `.pbix` |
| **Database Connections** | SQL Server, Oracle, PostgreSQL, Snowflake, Web APIs, etc. | Connect via Data Gateways / Semantic Models |
| **Data Transformation** | Full M-query capabilities (merge, pivot, custom functions) | Minimal / No transformation upon direct upload |