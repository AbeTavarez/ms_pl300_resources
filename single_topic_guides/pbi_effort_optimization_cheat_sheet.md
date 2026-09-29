# Power BI Cheat Sheet: Minimizing Development & Administrative Effort

This reference guide maps scenario triggers, exam keywords, and common requirements directly to their optimal Power BI solution patterns to minimize manual work, setup time, and maintenance overhead.

---

## 1. Minimizing Development Effort

When scenarios require reducing development time, avoiding redundant work, or building solutions quickly, leverage solutions that **reuse existing assets** or **automate code generation**.

| Requirement / Keyword Trigger | Recommended Solution Pattern | Why It Minimizes Development Effort |
| :--- | :--- | :--- |
| **Build a new report using existing data & measures** | **Power BI Dataset** *(Live Connection / Semantic Model)* | Inherits the existing data model, relationships, hierarchies, and DAX measures instantly without rebuilding them. |
| **Reuse ETL / Power Query transformations across multiple reports** | **Power BI Dataflow** | Centralizes data preparation in the cloud so transformation logic (M code) is executed once and shared across models. |
| **Combine multiple structurally identical files (Excel/CSV)** | **Power Query Folder / SharePoint Connector** *(Combine Files)* | Automatically creates combining functions, sample parameters, and append queries without custom M scripting. |
| **Apply standardized branding and visual formatting** | **Import a Custom Report Theme (`.json`)** | Sets global colors, fonts, margins, and visual properties across all pages in a single step. |
| **Create visuals beyond out-of-the-box defaults** | **Import Visual from AppSource** | Uses pre-built, tested custom visuals instead of writing custom code (e.g., custom D3.js or Python visuals). |
| **Build standard time-intelligence calculations** | **Auto Date/Time** or **DAX Quick Measures** | Generates standard DAX formulas (YTD, QTD, YoY) automatically via a UI prompt. |

---

## 2. Minimizing Administrative & Maintenance Effort

When scenarios focus on reducing ongoing maintenance, simplifying access governance, or automating operational tasks, prioritize **centralized management** and **dynamic rules**.

| Requirement / Keyword Trigger | Recommended Solution Pattern | Why It Minimizes Administrative Effort |
| :--- | :--- | :--- |
| **Manage user permissions across multiple reports & dashboards** | **Publish a Power BI App** | Groups related content into a single package and manages access for entire teams via a single permission boundary. |
| **Manage user access permissions at scale** | **Microsoft Entra ID (Azure AD) Security Groups** | Assigns workspace/report access to security groups rather than manually adding or removing individual email addresses. |
| **Restrict row-level data access per user or region** | **Dynamic Row-Level Security (RLS)** | Uses a single DAX role with `USERPRINCIPALNAME()` mapped to a security table instead of creating and maintaining dozens of static roles. |
| **Maintain dataset freshness across multiple dependent reports** | **Scheduled Refresh on Shared Dataset/Dataflow** | Updates data once in Power BI Service; all dependent "thin reports" reflect the refreshed data automatically. |
| **Distribute reports securely to external users** | **B2B Guest Accounts with Security Groups** | Uses directory-managed external accounts rather than recreating or duplicating datasets for external parties. |
| **Standardize certified datasets across the organization** | **Dataset Endorsement** *(Promoted or Certified)* | Signals official, trustworthy datasets in Microsoft Fabric/Power BI Service so users don't build redundant models. |

---

## 3. Decision Matrix: Choosing the Right Shared Asset

Use this quick checklist to select the appropriate feature based on what asset already exists:

```
                  ┌──► Does a Data Model & DAX exist? ─────► Use a Power BI Dataset (Semantic Model)
                  │
What already     ─┼──► Do Power Query steps exist? ────────► Use a Power BI Dataflow
exists?          │
                  ├──► Do user security groups exist? ────► Use Microsoft Entra ID Groups
                  │
                  └──► Does standardized styling exist? ───► Use a JSON Report Theme
```

---

## 4. Key Takeaways for Scenario Questions

1. **Dataset vs. Dataflow:** If you need to reuse **DAX calculations and relationships**, choose a **Dataset**. If you only need to reuse **cleaned tables / M transformations**, choose a **Dataflow**.
2. **Static RLS vs. Dynamic RLS:** If a scenario mentions scaling access rules across dozens of departments or territories, **Dynamic RLS** is always the low-effort answer over creating static roles for every region.
3. **App vs. Workspace:** Workspaces are for *collaboration and development*; Apps are for *distribution to end consumers* with minimal ongoing access management.