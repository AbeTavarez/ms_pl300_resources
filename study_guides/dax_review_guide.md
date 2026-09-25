# PL-300 Exam Study Guide: DAX Fundamentals, Filter Context, & Time Intelligence

This guide covers the core DAX concepts tested in the **Microsoft Certified: Power BI Data Analyst Associate (PL-300)** exam.

---

## 1. Understanding Filter Context & Row Context

### Row Context
* **What it is:** Iteration over rows in a table. It exists automatically in **Calculated Columns** and **Iterating Functions** (e.g., `SUMX`, `AVERAGEX`, `FILTER`).
* **Key Concept:** Row context **does not** automatically convert into a filter context unless forced by **Context Transition**.

### Filter Context
* **What it is:** The environment or "filters" active when a measure is evaluated.
* **Sources:** Slicers, Visual filters, Page filters, Report filters, Rows/Columns in visuals, and DAX measures (`CALCULATE`).

### Context Transition
* Occurs when a measure is evaluated inside a row context or when using `CALCULATE()` within a row context.
* It transforms the current row context into an equivalent filter context.

---

## 2. The `CALCULATE` Function

`CALCULATE` is the most important function in DAX. It evaluates an expression in a modified filter context.

### Syntax
```dax
CALCULATE(<expression> [, <filter1>, <filter2>, ...])
```

### Context Modification Rules
1. **Evaluates filter arguments:** Filter arguments are evaluated independently outside `CALCULATE`'s context modifications.
2. **Applies context transition:** Converts active row contexts into filter contexts.
3. **Overwrites conflicting filters:** Filters supplied inside `CALCULATE` override existing filters on the same column unless overridden by explicit modifiers (like `KEEPFILTERS`).

---

## 3. Key Filter Modifiers

| Modifier | Description | PL-300 Exam Focus |
| :--- | :--- | :--- |
| `ALL()` | Removes all filters from a table or column(s). | Used to calculate percentages of total: `DIVIDE([Sales], CALCULATE([Sales], ALL(Sales)))` |
| `ALLEXCEPT()` | Removes filters from all columns *except* specified ones. | Useful for grouping calculations while preserving specific slicing dimensions. |
| `ALLSELECTED()` | Removes filters applied in the visual/rows, but keeps external slicers/page filters. | Used for dynamic visual totals and running totals. |
| `KEEPFILTERS()` | Modifies `CALCULATE` behavior so that filter arguments **intersect** with existing filters instead of overriding them. | Prevents overriding sliced values. |
| `USERELATIONSHIP()`| Activates an inactive relationship for the duration of the `CALCULATE` measure. | Used with role-playing dimensions (e.g., `OrderDate` vs `ShipDate`). |
| `CROSSFILTER()` | Dynamically changes relationship cross-filtering behavior (e.g., `Both`, `Single`, `None`). | Used to modify filter propagation direction without changing model schema. |

---

## 4. Time Intelligence Functions

Time Intelligence functions require a proper **Date Table** meeting these criteria:
* Contiguous date range (no missing dates).
* Covers full calendar years.
* Marked as an official **Date Table** in Power BI.

### Year-to-Date (YTD), Quarter-to-Date (QTD), Month-to-Date (MTD)
```dax
// Sales YTD
Sales YTD = TOTALYTD([Total Sales], 'Date'[Date])

// Equivalent using CALCULATE and DATESYTD
Sales YTD Alt = 
CALCULATE(
    [Total Sales],
    DATESYTD('Date'[Date])
)
```

### Shifting Date Ranges (Prior Period Comparisons)
```dax
// Sales Same Period Last Year
Sales SPLY = 
CALCULATE(
    [Total Sales],
    SAMEPERIODLASTYEAR('Date'[Date])
)

// Sales Previous Month
Sales Previous Month = 
CALCULATE(
    [Total Sales],
    DATEADD('Date'[Date], -1, MONTH)
)
```

### Flexible Date Windowing
* `DATEADD('Date'[Date], <number>, <interval>)` — Shifts dates by Days, Months, Quarters, or Years.
* `DATESBETWEEN('Date'[Date], <start_date>, <end_date>)` — Returns dates in a specified range. Useful for moving averages.
* `DATESINPERIOD('Date'[Date], <start_date>, <number_of_intervals>, <interval>)` — Useful for Rolling 12-Month totals.

```dax
// Rolling 12-Month Sales
Rolling 12M Sales = 
CALCULATE(
    [Total Sales],
    DATESINPERIOD(
        'Date'[Date],
        MAX('Date'[Date]),
        -12,
        MONTH
    )
)
```

---

## 5. Typical PL-300 Question Scenarios

1. **Role-Playing Dimensions:**
   * *Scenario:* Calculate Sales by `Ship Date` when active relationship is on `Order Date`.
   * *Solution:* `CALCULATE([Total Sales], USERELATIONSHIP(Sales[ShipDate], 'Date'[Date]))`

2. **Calculating % of Total:**
   * *Scenario:* Show category sales as a percentage of overall sales regardless of row filtering.
   * *Solution:* `DIVIDE([Total Sales], CALCULATE([Total Sales], ALL(Product[Category])))`

3. **Custom Fiscal Year YTD:**
   * *Scenario:* Fiscal year ends on June 30.
   * *Solution:* `TOTALYTD([Total Sales], 'Date'[Date], "06-30")`

4. **Iterating vs Aggregating:**
   * *Scenario:* Calculate line-level revenue (`Quantity * UnitPrice`) before summing.
   * *Solution:* Use `SUMX(Sales, Sales[Quantity] * Sales[UnitPrice])` instead of multiplying two separate `SUM()` measures.