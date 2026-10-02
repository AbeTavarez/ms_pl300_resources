# Power BI Desktop vs. Power BI Service: Feature & Capability Cheat Sheet

This cheat sheet outlines the key functional differences, core capabilities, and workflow division between **Power BI Desktop** (the authoring environment) and **Power BI Service** (the cloud delivery platform).

---

## High-Level Summary

* **Power BI Desktop:** The authoring tool (Windows application). Designed for data ETL/ingestion, data modeling, DAX measure creation, and initial visual report layout.
* **Power BI Service:** The cloud platform (SaaS via web browser). Designed for distribution, security, collaboration, governance, automated refreshes, and dashboard building.

---

## Detailed Comparison Matrix

| Feature / Action | Power BI Desktop | Power BI Service (SaaS) | Notes & Key Differences |
| :--- | :---: | :---: | :--- |
| **Primary Core Role** | Data Modeling & Report Design | Distribution, Governance & Sharing | "Desktop builds, Service delivers." |
| **Cost / Licensing** | Free | Free / Pro / PPU / Capacity | Desktop is free; Service requires Pro/PPU/Capacity to share. |
| **Operating System** | Windows Application | Browser / Any Device | Desktop runs locally; Service is accessible via Web, Teams, & Mobile. |
| **Data Connections** | 100+ Connectors | Restricted Direct Uploads | Desktop supports flat files, SQL, APIs, etc. Service natively imports only `.xlsx`, `.csv`, `.pbix`. |
| **Power Query Engine (ETL)** | Full Capabilities | Dataflows Only | Full transformation, M-code, and query merging are exclusive to Desktop (unless using Cloud Dataflows). |
| **Data Modeling & Relationships** | Full | View / Edit Model Layout | Desktop is where you build relationships, calculated columns, and star schemas. |
| **DAX Measures** | Full Creation & Editing | Web Editing Available | Full DAX functionality in Desktop; Web Editing allows adding basic measures to published models. |
| **Query Folding Diagnostics** | Yes (`View Native Query`) | No | Identifying query folding steps can only be done in Power Query inside Desktop. |
| **Dashboards (Tile Collections)** | ❌ No | ✅ Yes | Dashboards (pinned tiles from multiple reports onto a single canvas) exist *only* in Service. |
| **Paginated Reports (.rdl)** | Power BI Report Builder | ✅ Yes (Host & View) | Paginated report creation requires Report Builder, but they are hosted and scheduled in Service. |
| **Data Refreshing** | Manual Refresh | Scheduled / Automatic | Automatic scheduled refreshes (with Data Gateways for on-prem sources) are set up in Service. |
| **Row-Level Security (RLS)** | **Define** Roles | **Assign** Users to Roles | Roles are created using DAX in Desktop; users/groups are assigned to those roles in Service. |
| **Sharing & Workspace Apps** | ❌ No | ✅ Yes | Distribute content via Workspaces, Power BI Apps, and Microsoft Teams integration. |
| **Data Alerts & Goal Tracking** | ❌ No | ✅ Yes | Set threshold alerts on dashboard tiles and track Metrics/Goals in Service. |
| **Field Parameters** | ✅ Yes | ❌ Creation Not Supported | Field parameters must be created in Desktop (though end-users can interact with the slicer in Service). |
| **Submissions & Commenting** | ❌ No | ✅ Yes | In-context comments, team annotations, and chatter exist in Service. |

---

## Feature Deep-Dive

### 1. What You Can ONLY Do in Power BI Desktop

* **Full Power Query Transformations:** Applying M-code steps, pivoting columns, unpivoting data, custom functions, merging queries, and viewing step-by-step query folding.
* **Creating Complex DAX Relationships & Columns:** Defining explicit relationships, active/inactive cardinalities, directionality (single/both), and calculated tables.
* **Creating Field Parameters & Calculation Groups:** Advanced analytical switches (such as dynamic metric selection or time intelligence parameters).
* **Defining RLS Rules:** Writing DAX filter statements for security roles inside the *Manage Roles* window.
* **Creating Custom Themes & Formats:** Importing external JSON theme templates to set default corporate palettes.

---

### 2. What You Can ONLY Do in Power BI Service

* **Create Dashboards:** Pin visuals from multiple underlying reports into a consolidated, single-page executive view.
* **Schedule Automated Data Refreshes:** Configure scheduled updates (up to 8 times daily for Pro; 48 times daily for Premium/Fabric) using On-Premises Data Gateways.
* **Share Content & Manage Permissions:** Publish App Workspaces, build target-audience Apps, assign RLS security group memberships, and manage external guest access (B2B sharing).
* **Set Data Alerts:** Receive email or Teams notifications when a numerical KPI card or metric crosses a designated threshold.
* **Embed Reports:** Embed live interactive reports into SharePoint Online, Microsoft Teams, PowerPoint, web portals, or custom application code.
* **Export / Analyze in Excel:** Connect live Excel PivotTables back to the published Semantic Model hosted in the cloud.

---

## Workflow Best Practices

```text
  [ Raw Data Sources ]
           │
           ▼
┌─────────────────────────┐
│    Power BI Desktop     │  <-- 1. Connect & Clean (Power Query)
│   (Local Environment)   │  <-- 2. Model Data & Write DAX
└──────────┬──────────────┘  <-- 3. Design Visual Pages
           │
           │  Publish (.pbix)
           ▼
┌─────────────────────────┐
│    Power BI Service     │  <-- 4. Configure Refresh & RLS Users
│    (Cloud Platform)     │  <-- 5. Pin Dashboards & Create Apps
└─────────────────────────┘  <-- 6. Share with End Users & Executives
```