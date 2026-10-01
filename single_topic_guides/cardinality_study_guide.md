# PL-300 Exam Study Guide: Cardinality in Power BI

This study guide focuses on **Cardinality**, a fundamental topic covered in the **PL-300: Microsoft Power BI Data Analyst** exam (specifically within Domain 1: Prepare the Data and Domain 2: Model the Data).

---

## 1. Core Concept: What is Cardinality?

In Power BI and data modeling, **cardinality** refers to **uniqueness**. Depending on whether you are analyzing a single column or setting up relationships between tables, cardinality has two distinct meanings on the PL-300 exam.

---

## 2. Column Cardinality (Data Profiling & Engine Performance)

**Column Cardinality** measures the number of **unique (distinct) values** present in a column relative to the total number of rows in the table.

### Factors That Influence Column Cardinality
When analyzing a column, two main factors dictate its cardinality profile:

1. **Number of Unique Values** *(Primary Driver)*: The count of distinct, non-duplicate entries in that column.
2. **Data Type of the Column**: The underlying data type determines the precision and potential range of unique values. For example, a `DateTime` column recorded down to the millisecond has significantly higher cardinality than a simple `Date` column or an `Integer` column.

> **Sample Exam Question:**
> *Which two factors influence the cardinality of a column? (Select two answers.)*
> - [x] **Number of unique values**
> - [x] **Data type of the column**
> - [ ] Sorting direction *(Incorrect: Display preference only)*
> - [ ] Slicer order *(Incorrect: UI configuration only)*

---

### Low vs. High Cardinality Comparison

| Attribute | Low Cardinality | High Cardinality |
| :--- | :--- | :--- |
| **Definition** | Few distinct values repeated across many rows. | Many distinct values (often almost equal to row count). |
| **Examples** | `Gender`, `State/Province`, `Order Year`, `IsActive` (True/False). | `Transaction ID`, `Timestamp (ms)`, `GUID`, `Customer Email`. |
| **VertiPaq Engine Compression** | **Extremely High** (Very small file size, fast performance). | **Extremely Low** (Consumes massive RAM, slows down reports). |
| **Best Practice** | Ideal for slicing, grouping, and dimension filtering. | Remove unused unique IDs; split `DateTime` into separate `Date` and `Time` columns. |

---

## 3. Relationship Cardinality (Data Modeling)

In the data model diagram, **Relationship Cardinality** defines how rows in one table map to rows in another table based on key columns.

```
┌─────────────────┐        1 : *        ┌─────────────────┐
│   DimCustomer   │ ───────────────────> │   FactSales     │
│  (CustomerKey)  │                      │  (CustomerKey)  │
└─────────────────┘                     └─────────────────┘
```

### The 4 Relationship Types

#### 1. One-to-Many ($1:*$) or Many-to-One (* : 1)
* **How it works:** The primary key in the Dimension table contains **100% unique values** (Primary Key), while the foreign key in the Fact table can contain **multiple occurrences** of that same value.
* **Exam Status:** **Best Practice.** This is the foundational building block of a Star Schema.

#### 2. One-to-One ($1:1$)
* **How it works:** Both columns in the relationship contain 100% unique values.
* **Exam Status:** Rare. Usually indicates that two tables should be merged into a single table in Power Query during the data preparation phase.

#### 3. Many-to-Many (* : *)
* **How it works:** Neither column in the relationship has unique values.
* **Exam Status:** **Use with caution.** Introduces ambiguity, non-deterministic aggregation, and performance overhead. Often better resolved by introducing a **Bridge / Junction table**.

---

## 4. Exam Tips & High-Yield Pitfalls

### How to Reduce High Column Cardinality
When the exam asks how to optimize model size or speed up refreshes, target high-cardinality columns:

1. **Split `DateTime` Columns:** A combined `DateTime` column (e.g., `2026-10-01 14:32:11`) has millions of unique combinations. Split it into a `Date` column and a `Time` column (or bin time into hours/quarters).
2. **Remove Unnecessary Surrogate / Primary Keys:** Fact table primary keys (like `FactSalesID`) are rarely needed for report visuals or DAX measures. Remove them in Power Query to save RAM.
3. **Change Float / Decimal to Fixed Decimal (Currency) or Integer:** High-precision decimals increase unique value counts. Rounding or changing data types reduces cardinality.

---

## 5. Practice Exam Questions

### Question 1
**Scenario:** You have a table named `WebLog` with 10,000,000 rows. The table includes a column named `UserIPAddress` and a column named `Country`. Which column has higher cardinality, and why?

* **A)** `Country`, because it contains text data types.
* **B)** `UserIPAddress`, because it contains significantly more unique values across the 10 million rows. *(Correct)*
* **C)** `Country`, because countries require complex spatial encoding.
* **D)** Both columns have identical cardinality because they belong to the same table.

**Explanation:** Cardinality measures distinct values. An IP address will have millions of distinct values across 10M rows, whereas `Country` will only have ~200 distinct values.

---

### Question 2
**Scenario:** A report developer notices that a Power BI model size is unexpectedly large. Performance Analyzer reveals that a column named `TransactionTimestamp` (formatted as `MM/DD/YYYY HH:MM:SS`) is taking up 60% of the dataset size. Which action best reduces the model size while preserving analysis capabilities?

* **A)** Change the relationship cardinality of the table from $1:*$ to $*:*$.
* **B)** Sort the `TransactionTimestamp` column in descending order.
* **C)** Split `TransactionTimestamp` into a `Date` column and a separate `Time` column (or Hour integer). *(Correct)*
* **D)** Add a slicer for `TransactionTimestamp` on the main report page.

**Explanation:** Splitting a high-cardinality `DateTime` column into separate `Date` and `Time` columns drastically reduces the distinct values per column, enabling the VertiPaq engine to compress the data efficiently.