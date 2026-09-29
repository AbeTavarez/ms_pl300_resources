# Power BI Insert Ribbon & AI Visuals Guide

This guide details the layout and features available under the **Insert** ribbon tab in Power BI Desktop, with a specific focus on **AI Visuals** (Key Influencers, Decomposition Trees) and interactive report elements frequently tested on the PL-300 exam.

---

## 1. Overview of the Insert Ribbon

The **Insert** tab in Power BI Desktop is divided into four main functional sections:

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                                   INSERT RIBBON                                  │
├───────────────┬───────────────────┬──────────────────────┬───────────────────────┤
│    Visuals    │    AI Visuals     │    Power Platform    │       Elements        │
│               │                   │                      │                       │
│ • New Visual  │ • Key Influencers │ • Power Apps         │ • Text Box            │
│ • More Visuals│ • Decomp Tree     │ • Power Automate     │ • Buttons & Action    │
│               │ • Smart Narrative │                      │ • Shapes & Images     │
└───────────────┴───────────────────┴──────────────────────┴───────────────────────┘
```

---

## 2. AI Visuals Group (Insert Ribbon)

The **AI Visuals** group on the Insert ribbon houses Power BI's built-in Machine Learning algorithms.

### 1. Key Influencers Visual
* **Location:** `Insert Ribbon` ➔ `AI Visuals Group` ➔ `Key Influencers` *(Also accessible in the Visualizations Pane)*.
* **Core Purpose:** Analyzes data to rank factors that drive a specific metric or outcome.
* **Field Buckets:**
  * **Analyze:** The metric/KPI you want to evaluate (e.g., `Customer Churn = Yes` or `Rating`).
  * **Explain By:** Dimensions/factors you want to test as potential influencers (e.g., `Contract Type`, `Tenure`, `Region`).
  * **Expand By:** Used for unaggregated continuous fields when analyzing measures.
* **Key Tabs in Visual:**
  * **Key Influencers:** Shows individual factors ranked by impact (e.g., "When Contract Type is Month-to-Month, Churn is 2.3x more likely").
  * **Top Segments:** Shows clusters/groups of combined factors driving the result.

### 2. Decomposition Tree (Decision Tree Visual)
* **Location:** `Insert Ribbon` ➔ `AI Visuals Group` ➔ `Decomposition Tree` *(Also accessible in the Visualizations Pane)*.
* **Core Purpose:** Conducts root-cause analysis across multiple dimensions dynamically using decision tree logic.
* **Field Buckets:**
  * **Analyze:** The aggregated metric you want to break down (e.g., `Total Sales`, `Avg Delivery Delay`).
  * **Explain By:** Dimensions you want to drill into (e.g., `Category`, `Region`, `Ship Mode`).
* **AI Split Feature (The "+" Icon):**
  * **High Value:** Automatically selects the dimension path that yields the *highest* value for the metric.
  * **Low Value:** Automatically selects the dimension path that yields the *lowest* value for the metric.
* **Locking Levels:** Report authors can lock specific levels so end-users can only drill down into custom sub-levels.

### 3. Smart Narrative
* **Location:** `Insert Ribbon` ➔ `AI Visuals Group` ➔ `Smart Narrative`.
* **Core Purpose:** Automatically generates a textual summary of key findings and trends based on active report visuals.

---

## 3. Other Core Elements in the Insert Ribbon

| Group | Feature | PL-300 Use Case / Exam Relevance |
| :--- | :--- | :--- |
| **Visuals** | **New Visual** | Inserts a default card/bar visual onto the report canvas. |
| **Visuals** | **More Visuals** | Connects to **AppSource** to import custom visuals created by third-party developers. |
| **Power Platform** | **Power Apps / Automate** | Embeds custom low-code apps or triggers flow automation directly from Power BI visuals. |
| **Elements** | **Buttons & Navigator** | Adds **Page Navigators** or **Bookmark Navigators** to minimize navigation layout development. |
| **Elements** | **Shapes & Images** | Adds background banners, icons, and logos to report pages. |

---

## 4. PL-300 Quick Reference Sheet

| Scenario | Where to Find It | Correct Tool/Visual |
| :--- | :--- | :--- |
| Find top factors driving churn | `Insert` ➔ `Key Influencers` | Key Influencers |
| Break down sales by dimensions dynamically | `Insert` ➔ `Decomposition Tree` | Decomposition Tree |
| Automatically pick highest contributing category | Click `+` on Decomp Tree | AI Split ➔ High Value |
| Import custom visual not in standard palette | `Insert` ➔ `More Visuals` | AppSource |
| Create a dynamic page navigation toolbar | `Insert` ➔ `Buttons` | Page Navigator |