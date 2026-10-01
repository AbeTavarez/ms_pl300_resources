# PL-300 Exam Reference Guide: Services & Ecosystem Components

This guide summarizes all key Power BI features, tools, and connected Microsoft Azure/Fabric ecosystem services tested on the **Microsoft Certified: Power BI Data Analyst Associate (PL-300)** exam.

---

## 1. Primary Power BI Client & Service Interfaces

| Service / Tool | Primary Purpose | Key PL-300 Exam Notes |
| :--- | :--- | :--- |
| **Power BI Desktop** | Local development environment for data ingestion, Power Query transformation, modeling, DAX, and report authoring. | **Where core design happens.** Free desktop application. Only environment where you can build data models, measures, and RLS roles. |
| **Power BI Service (SaaS)** | Cloud-based SaaS platform for publishing, sharing, collaborating, and managing reports and dashboards. | **Where distribution happens.** Supports workspace management, scheduled refresh, app publishing, metrics, and security assignment. |
| **Power BI Mobile App** | Mobile client for iOS, Android, and Windows for consuming reports and dashboards on mobile devices. | Supports responsive layouts, phone view report layouts, data alerts, and offline caching. |
| **Power BI Report Builder** | Dedicated authoring tool for **Paginated Reports** (`.rdl` files). | Used for pixel-perfect, highly formatted, printable reports (e.g., invoices, operational sheets). Connects to Power BI semantic models or SQL databases. |
| **Power BI Report Server (PBIRS)** | On-premises enterprise reporting server for hosting Power BI reports behind corporate firewalls. | Used when cloud deployment (Power BI Service) is prohibited due to strict regulatory compliance or air-gapped security policies. |

---

## 2. Data Ingestion & Transformation Components

| Feature / Service | Description | Exam Distinctions & Use Cases |
| :--- | :--- | :--- |
| **Power Query (M Engine)** | Data transformation and ETL engine built into Desktop, Dataflows, and Excel. | Evaluates steps, handles data type conversions, applies **Query Folding** to push transformations to SQL databases. |
| **Power BI Dataflows (Gen1 / Gen2)** | Cloud-based Power Query preparation stored independently in Power BI / Fabric workspaces. | Reusable ETL logic. Allows analysts to transform data once and reuse across multiple `.pbix` models across the organization. |
| **On-Premises Data Gateway** | Secure bridge/agent installed on local servers to connect Power BI Service to on-premises data sources. | **Standard Mode:** Shared, supports scheduled refresh & DirectQuery for multiple users. <br>**Personal Mode:** Single-user, scheduled refresh only (no DirectQuery). |
| **VNet / Private Link Gateways** | Azure network security components for connecting Power BI to secured Azure resources without public internet exposure. | Essential for enterprise security scenarios where sources reside inside private subnets/Virtual Networks (VNets). |

---

## 3. Storage Engine & Modeling Concepts

| Feature / Mechanism | Function | Key Exam Rules |
| :--- | :--- | :--- |
| **VertiPaq Engine** | In-memory columnar database engine powering Power BI Import storage mode. | Compresses and stores data by column rather than row. Delivers high query performance. |
| **Storage Modes (Import / DirectQuery / Dual)** | Determines how and where data is stored and queried. | **Import:** Fast, full DAX, cached. <br>**DirectQuery:** Real-time source queries, limited DAX. <br>**Dual:** Hybrid for dimensions linked to DirectQuery facts. |
| **Composite Models** | Allows mixing DirectQuery and Import modes or combining multiple Power BI semantic models into one. | Enables extending existing corporate semantic models with local data tables. |
| **XMLA Endpoints** | Open connection protocol (read/write) for managing Power BI semantic models via external tools. | Enables enterprise management via Tabular Editor, ALM Toolkit, DAX Studio, or PowerShell in Premium/Fabric capacities. |

---

## 4. Power BI Service Features & Collaboration Tools

| Feature / Service | Functionality | PL-300 Context |
| :--- | :--- | :--- |
| **Power BI Workspaces** | Collaborative containers for reports, dashboards, semantic models, dataflows, and workbooks. | **Roles to know:** Admin, Member, Contributor (can edit content; bypass RLS), and Viewer (read-only; subject to RLS). |
| **Power BI Apps** | Distributed, packaged collections of reports and dashboards for large audiences. | Best practice for broad content distribution to viewers without giving direct workspace editing access. |
| **Dashboards vs. Reports** | Core content types in the Power BI Service. | **Dashboard:** Single-page canvas pinned from multiple reports/semantic models. <br>**Report:** Multi-page interactive dataset visualizer. |
| **Deployment Pipelines** | ALM (Application Lifecycle Management) tool for moving content across **Dev $\rightarrow$ Test $\rightarrow$ Production** environments. | Allows testing changes with separate datasets before pushing updates to end-user Production workspaces. |
| **Usage Metrics Reports** | Built-in analytics tracking report views, unique users, and performance metrics. | Helps admins monitor report adoption and identify slow-loading reports in workspaces. |
| **Data Lineage & Impact Analysis** | Visual graph showing upstream data sources and downstream dependent reports/apps. | Used to determine which reports will break if a semantic model or dataflow column is changed or removed. |

---

## 5. Security & Governance Services

| Service / Security Mechanism | Function | Exam Details |
| :--- | :--- | :--- |
| **Row-Level Security (RLS)** | Restricts data row access for specific users based on DAX expressions. | Defined in Desktop (`USERPRINCIPALNAME()`), assigned to users/groups in Power BI Service. Applier only to **Viewers**. |
| **Object-Level Security (OLS)** | Restricts access to entire columns or tables containing sensitive data (e.g., salary, SSN). | Configured via Tabular Editor/XMLA endpoints. Completely hides unauthorized metadata and columns from visuals. |
| **Microsoft Purview Information Protection (Sensitivity Labels)** | Classifies and protects sensitive enterprise data (e.g., *Confidential*, *Highly Restricted*). | Applies encryption and export restrictions that persist when data is exported to Excel, PDF, or PowerPoint. |
| **Endorsement (Promoted / Certified)** | Content governance labeling for trusted datasets and dataflows. | **Promoted:** Added by workspace members. <br>**Certified:** Requires centralized admin tenant approval. |

---

## 6. External Microsoft Azure Data Sources

The PL-300 tests your ability to identify the correct Azure storage/database service when configuring connectivity, refresh, or DirectQuery.

| Azure Service | Type | Connectivity & Best Practices in Power BI |
| :--- | :--- | :--- |
| **Azure SQL Database** | Relational DB (PaaS) | Supports both Import and DirectQuery mode. Fully supports Query Folding. |
| **Azure Synapse Analytics** | Enterprise Data Warehouse | Connects via SQL endpoints. Ideal for massive datasets using DirectQuery or Composite models. |
| **Azure Data Lake Storage (ADLS Gen2)** | Unstructured / Semi-structured Object Storage | Stores Parquet, CSV, or Delta Lake files. Primary storage layer behind Power BI Dataflows. |
| **Azure Cosmos DB** | NoSQL Database | Connects using specialized Power Query connectors. Requires structuring JSON records into relational tables. |
| **Azure Analysis Services (AAS)** | Enterprise SSAS tabular models in the cloud | Connects via Live Connection mode; models are hosted centrally in Azure rather than Power BI. |

---

## 7. Ecosystem Integration & Advanced Features

| Feature / Service | Function | Exam Key Scenario |
| :--- | :--- | :--- |
| **Microsoft Teams Integration** | Embedded Power BI app within Teams tabs and chat channels. | Facilitates collaborative data discussions without leaving the Teams interface. |
| **Analyze in Excel** | Connects Microsoft Excel directly to a published Power BI semantic model via PivotTables. | Keeps data live and single-source-of-truth while catering to finance users who prefer Excel PivotTables. |
| **Power Apps / Power Automate Visuals** | Interactive visual controls embedded directly inside Power BI reports. | Enables write-back scenarios (updating databases from a report) or triggering automated workflows based on data alerts. |
| **Smart Narratives / Q&A Visual** | AI-driven visual capabilities. | **Q&A:** Allows users to ask natural language questions. <br>**Smart Narrative:** Automatically generates text summaries of visual findings. |

---

## 8. Summary Decision Matrix for Common Exam Scenarios

| Exam Requirement | Recommended Service / Feature |
| :--- | :--- |
| *Need to print pixel-perfect monthly invoices.* | **Power BI Report Builder** (Paginated Report) |
| *Need to securely connect Power BI Cloud to an on-premise SQL Server.* | **On-Premises Data Gateway (Standard)** |
| *Need to reuse a common customer dimension across 5 different teams.* | **Power BI Dataflow** |
| *Need to grant 500 business users read-only access to vetted reports.* | **Power BI App** |
| *Need to isolate Development, Testing, and Production content.* | **Deployment Pipelines** |
| *Need real-time streaming data with low latency.* | **DirectQuery** or **Streaming Dataset / Real-Time Dashboard** |
| *Need to restrict visibility of specific sensitive rows by employee email.* | **Dynamic Row-Level Security (RLS)** |