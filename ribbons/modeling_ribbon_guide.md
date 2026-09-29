# Power BI Modeling Ribbon Guide

The **Modeling Ribbon** is dedicated to defining business logic, data structures, security rules, and performance optimizations within your data model. It plays a critical role in the "Model the Data" domain of the PL-300 exam.

```
┌─────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                                              MODELING RIBBON                                                │
├───────────────────┬──────────────────────┬───────────────────────┬───────────────────┬──────────────────────┤
│    Relationships  │     Calculations     │        What-If        │     Security      │        Culture       │
│                   │                      │                       │                   │                      │
│ • Manage Rel.     │ • New Measure        │ • New Parameter       │ • Manage Roles    │ • Linguistic Schema  │
│ • Edit Rel.       │ • Quick Measure      │   (Numeric / Fields)  │ • View as         │                      │
│                   │ • New Table          │                       │                   │                      │
│                   │ • New Column         │                       │                   │                      │
└───────────────────┴──────────────────────┴───────────────────────┴───────────────────┴──────────────────────┘
```

## 1. Functional Groups & Tool Breakdown

### A. Relationships Group
* **Manage Relationships:** Opens the central manager to view, create, edit, or autodetect table relationships, cardinalities (1:1, 1:Many, Many:Many), and cross-filter directions (Single or Both).

### B. Calculations Group
* **New Measure:** Creates a dynamic, context-aware DAX measure (calculated at visual rendering time).
* **Quick Measure:** Uses a guided UI wizard to write DAX calculations without coding.
* **New Column:** Adds a calculated column to a table stored in memory, calculated row-by-row during data refresh.
* **New Table:** Creates a model table using a DAX expression (frequently used for standard Date/Calendar tables using `CALENDAR()` or `CALENDARAUTO()`).

### C. Parameters Group (What-If Analysis)
* **Numeric Range Parameter:** Creates a dynamic numeric slider (and corresponding table/measure) to run scenario analysis (e.g., "What if discount increases by 5%?").
* **Field Parameter:** Allows users to dynamically change the dimensions or measures displayed in a visual at runtime without needing multiple charts or complex bookmarks.

### D. Security Group (Row-Level Security - RLS)
* **Manage Roles:** Creates security roles and defines DAX filter expressions to restrict data access at the row level (e.g., `[Region] = USERPRINCIPALNAME()`).
* **View as:** Simulates report views under specific roles or user accounts to test and validate RLS rules directly within Power BI Desktop.

### E. Q&A / Culture Group
* **Linguistic Schema:** Configures synonyms and terms for data columns.

---

## 2. PL-300 Exam Scenarios & Triggers

| Exam Scenario / Question Requirement | Correct Modeling Ribbon Action |
| :--- | :--- |
| **Create a standard Date table using DAX** | `Modeling` ➔ `New Table` (write `CALENDARAUTO()`) |
| **Allow report users to dynamically toggle axis fields between Region, Category, and Segment** | `Modeling` ➔ `New Parameter` ➔ `Fields` |
| **Configure security filters so managers only see their own country's data** | `Modeling` ➔ `Manage Roles` |
| **Test Row-Level Security rules before publishing to Power BI Service** | `Modeling` ➔ `View as` |
| **Create a scenario slider to evaluate 5%, 10%, and 15% tax hikes** | `Modeling` ➔ `New Parameter` ➔ `Numeric Range` |

---

## 3. Key Takeaways for PL-300

1. **Calculated Column vs. Measure:** 
   * **Calculated Columns** consume RAM, are calculated row-by-row at refresh, and are primarily used as slicers/axis dimensions.
   * **Measures** use minimal memory, calculate on-the-fly based on report filter context, and are used for quantitative values.
2. **Field Parameters vs. Bookmarks:** Field Parameters minimize development effort when enabling dynamic measure/dimension switching, reducing the need for dozens of complex bookmarks.
3. **RLS Testing:** Always use `View as` on the Modeling ribbon to verify that DAX filter expressions work correctly before uploading to Power BI Service.