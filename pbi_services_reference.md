# PL-300 Exam Reference List: Connected Services & Ecosystem Components

This reference list covers the connected Microsoft, Azure, and Fabric services that frequently appear in questions on the **Microsoft Certified: Power BI Data Analyst Associate (PL-300)** exam.

---

## 1. Identity, Access & Governance Services

| Service | Exam Relevance & Use Cases |
| :--- | :--- |
| **Microsoft Entra ID** *(formerly Azure Active Directory)* | Provides user identity management, single sign-on (SSO), Microsoft Entra Security Groups for workspace permissions, dynamic Row-Level Security (RLS) user authentication (`USERPRINCIPALNAME()`), and Service Principals for automated API operations. |
| **Microsoft Purview** | Used for enterprise data governance, data lineage tracking, cross-tenant data cataloging, and applying **Information Protection Sensitivity Labels** to protect confidential data exported from Power BI. |
| **Microsoft 365 Groups** | Underlies Power BI Workspaces; used for assigning permissions, sending automated email subscriptions, and governing workspace access. |

---

## 2. Relational & Operational Data Sources

| Service | Exam Relevance & Use Cases |
| :--- | :--- |
| **Microsoft Dataverse** | Native data platform for Microsoft Power Platform (Power Apps / Power Automate). Supports DirectQuery and Import storage modes, automatic relationship detection, and inherits Dataverse security roles. |
| **Azure SQL Database** | Fully managed cloud SQL relational database. Fully supports **Query Folding**, Import mode, and DirectQuery mode. |
| **Azure SQL Managed Instance** | Enterprise PaaS SQL instance with near-100% compatibility with on-premises SQL Server. Frequently tested with On-Premises or VNet Data Gateways. |
| **SQL Server (On-Premises)** | Relational database requiring an **On-Premises Data Gateway** for scheduled refreshes or DirectQuery connections from the Power BI Service. |
| **Salesforce / Dynamics 365** | SaaS operational sources. Tested in scenarios involving native Power Query connectors, API throttling, and DirectQuery vs. Import trade-offs. |

---

## 3. Analytics, Warehouse & Data Lake Services

| Service | Exam Relevance & Use Cases |
| :--- | :--- |
| **Azure Synapse Analytics** | Enterprise data warehousing platform. Connects via SQL endpoints using DirectQuery, Import, or Composite Models for massive datasets. |
| **Azure Data Lake Storage Gen2 (ADLS Gen2)** | High-capacity object storage for big data analytics. Used as the underlying storage engine for **Power BI Dataflows** when configured as "Bring Your Own Storage" (BYOS). |
| **Microsoft Fabric** | Unified analytics platform. Features tested include **OneLake**, **Direct Lake** storage mode (reading Delta tables directly without Import or DirectQuery penalties), and **Fabric Dataflows Gen2**. |
| **Azure Databricks** | Spark-based analytics platform. Connects via Delta Lake / Spark connectors in Power Query. Supports Query Folding for Spark SQL operations. |
| **Azure Stream Analytics** | Real-time event processing engine. Pushes real-time streaming data directly into Power BI streaming datasets and push dashboards. |

---

## 4. Unstructured & Productivity Data Sources

| Service | Exam Relevance & Use Cases |
| :--- | :--- |
| **Microsoft SharePoint Online** | Used for loading files (Excel, CSV, PDFs) hosted in SharePoint Document Libraries via the `SharePoint Folder` or `SharePoint Implementation 2.0` connectors. |
| **OneDrive for Business** | Cloud storage used for personal report authoring and auto-refresh scenarios (Power BI automatically updates datasets hosted on OneDrive without needing a Gateway). |
| **Microsoft Excel** | Primary source for manual imports, as well as the target for **Analyze in Excel** (connecting Excel PivotTables directly to a published Power BI semantic model). |

---

## 5. Integration, Automation & Visual Services

| Service | Exam Relevance & Use Cases |
| :--- | :--- |
| **Power Automate** | Used for automated workflows (e.g., triggering data export emails when data alerts fire) and via the **Power Automate for Power BI visual** to perform write-back actions directly from report pages. |
| **Power Apps** | Embedded via the **Power Apps visual** to allow end-users to update underlying database records (write-back) directly within a Power BI report interface. |
| **Microsoft Teams** | Used for collaborative analytics by embedding Power BI reports directly into Teams tabs, sharing visual links, and receiving workspace notifications. |
| **Azure Cognitive Services / AI Insights** | Integrated directly into Power Query for AI transformations such as Sentiment Analysis, Key Phrase Extraction, and Image Tagging. |