# Microsoft Power BI PL-300 Exam: Security & Governance Cheat Sheet

This cheat sheet covers the key security concepts, Row-Level Security (RLS) patterns, Object-Level Security (OLS), workspace permission levels, and sharing mechanisms tested on the **Microsoft PL-300: Power BI Data Analyst** exam.

---

## 1. Row-Level Security (RLS) vs. Object-Level Security (OLS)

| Feature | Row-Level Security (RLS) | Object-Level Security (OLS) |
| :--- | :--- | :--- |
| **What it hides** | Specific **rows** of data (e.g., region-specific sales). | Entire **columns** or **tables** (e.g., SSN, Salary). |
| **Visual Behavior** | Visuals display filtered data (empty rows omitted). | Visuals referencing hidden objects throw an **error** or fail to render for restricted users. |
| **Where Configured** | Power BI Desktop (`Manage Roles` -> DAX filter). | Tabular Editor or ALM Toolkit (not directly editable in native Desktop UI). |
| **Member Assignment** | Power BI Service (Dataset -> Security). | Power BI Service (Dataset -> Security). |

---

## 2. Row-Level Security (RLS) Implementation Types

### A. Static RLS
Hardcoded DAX expressions assigned to specific security roles.
* **Example DAX Filter:** `[Region] = "North America"`
* **Use Case:** Small, fixed number of roles (e.g., US Managers vs. EU Managers).

### B. Dynamic RLS
Uses DAX user functions matched against a user table in the data model so one role scales across thousands of users.
* **Key DAX Functions:**
  * **`USERPRINCIPALNAME()`**: Returns `user@domain.com` in Power BI Service. **Primary function for dynamic RLS.**
  * **`USERNAME()`**: Returns `Domain\User` in Desktop / On-Premises, or UPN/GUID in Service.
* **Typical Data Pattern:**
  1. A `Users` table containing `UserEmail` and `AccessID`/`Region`.
  2. A 1-to-Many relationship from `Users` to `Data` (or bridge table).
  3. DAX Filter on `Users` table: `[UserEmail] = USERPRINCIPALNAME()`

### C. Dynamic RLS with Many-to-Many / Security Bridge Tables
* **Scenario:** A user belongs to multiple regions or departments.
* **Setup:**
  * `User` table (1) $\rightarrow$ `UserRegion` bridge table (*) $\rightarrow$ `Region` dimension table (1) $\rightarrow$ `Sales` table (*).
* **Crucial Setting:** Cross-filter direction on the relationship **MUST** be set to **Single (UserRegion filters Region)** or **Both** with **"Apply security filter in both directions"** checked.

---

## 3. Testing RLS

| Location | Method | Key Exam Scenario |
| :--- | :--- | :--- |
| **Power BI Desktop** | `Modeling` tab $\rightarrow$ `View as` | Select a role or test dynamic RLS by choosing "Other user" and typing a test UPN (`user@company.com`). |
| **Power BI Service** | Dataset Settings $\rightarrow$ `...` $\rightarrow$ `Security` $\rightarrow$ `Test as role` | Validates security behavior as it will appear in the web service. |

---

## 4. Workspace Roles & RLS Bypass Rules (High-Yield Exam Topic)

> **CRITICAL EXAM RULE:** RLS is **ONLY** enforced for users with **Viewer** access. Members, Contributors, and Admin roles **bypass RLS** if they have edit permissions on the workspace datasets.

| Workspace Role | Can Edit Content? | RLS Enforced? | Build Permission Granted? |
| :--- | :--- | :--- | :--- |
| **Admin** | Yes | **No** (Bypasses RLS) | Yes |
| **Member** | Yes | **No** (Bypasses RLS) | Yes |
| **Contributor** | Yes | **No** (Bypasses RLS)* | Yes |
| **Viewer** | No (Read-only) | **YES** (Enforced) | No (Unless granted via dataset sharing) |

*\*Note: If Contributors are restricted from editing workspace content via workspace settings, RLS will apply to them.*

---

## 5. Dataset Permissions & Sharing

| Permission Level | Description | Key Exam Distinctions |
| :--- | :--- | :--- |
| **Read** | Allows viewing reports built on the dataset. | Default permission when sharing a report. |
| **Build** | Allows creating new reports on top of the dataset (Analyze in Excel, Live Connection). | Required for users building their own reports from your published dataset. |
| **Write** | Allows modifying the dataset itself. | Granted automatically to Admins/Members/Contributors. |
| **Reshare** | Allows users to share the report/dataset with others. | Can be unselected when sharing. |

---

## 6. End-User Sharing vs. Apps vs. Workspaces

* **Power BI Apps:** Best practice for distributing content to large organizational audiences. Allows granting permission to Security Groups or Azure AD groups.
* **Direct Sharing:** Good for ad-hoc access to single reports/dashboards.
* **Workspace Access:** Best for team collaboration among content creators.

---

## High-Yield Security Exam Pitfalls

1. **"Users see all data despite RLS setup":** Check their Workspace Role! If they are an **Admin, Member, or Contributor**, RLS is bypassed. Change their role to **Viewer**.
2. **"Single Direction Cross-Filtering in Security Tables":** Dynamic RLS on a bridge table will not filter fact tables unless **"Apply security filter in both directions"** is enabled on the relationship settings.
3. **`USERPRINCIPALNAME()` vs `USERNAME()`:** For Power BI Service dynamic RLS, always prefer `USERPRINCIPALNAME()` as it predictably returns the tenant UPN (`username@domain.com`).
4. **Scheduled Refresh with Service Principals:** To schedule refresh on datasets using Row-Level Security, Service Principals or explicit admin credentials must be configured correctly in the gateway/dataset settings.