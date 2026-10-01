Securely Sharing Reports Externally

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