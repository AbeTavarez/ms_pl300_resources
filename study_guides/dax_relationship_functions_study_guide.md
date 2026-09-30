# PL-300 DAX Study Guide: `TREATAS` & Relationship Control Functions

This study guide focuses on DAX functions that control relationship behavior, propagate filters virtually, and manipulate data lineage—core skills tested in advanced PL-300 model optimization and DAX scenarios.

---

## 1. `TREATAS` (Virtual Relationships)

`TREATAS` applies the result of a table expression as a filter to columns from an unrelated table. It creates a **virtual relationship** for the duration of the calculation without requiring a physical model relationship.

### Syntax
```dax
TREATAS(<table_expression>, <column1> [, <column2>, ...])
```

### Key Characteristics
* **Performance:** Highly optimized compared to traditional `INTERSECT` or `CONTAINS` patterns.
* **Lineage:** Overwrites the data lineage of the input table expression to match the target columns.
* **No Physical Model Modification:** Useful when creating a physical relationship is impossible, causes ambiguity, or degrades performance.

### Typical Exam Use Cases

#### Case A: Budget vs. Actuals at Different Granularities
When `Actuals` are recorded daily, but `Budget` is set monthly, a physical relationship to the daily `Date` table might cause grain mismatch. You can use `TREATAS` to filter `Date` based on a monthly summary.

```dax
Budget Target = 
VAR MonthlyBudget = 
    SELECTCOLUMNS(
        'Monthly Targets',
        "YearMonth", 'Monthly Targets'[YearMonthKey],
        "Amount", 'Monthly Targets'[TargetAmount]
    )
RETURN
    CALCULATE(
        SUM('Monthly Targets'[TargetAmount]),
        TREATAS(
            VALUES('Date'[YearMonthKey]),
            'Monthly Targets'[YearMonthKey]
        )
    )
```

#### Case B: Dynamic Disconnected Slicers
When users need to select values from a slicer table that is deliberately disconnected from the data model.

```dax
Filtered Sales = 
CALCULATE(
    [Total Sales],
    TREATAS(
        VALUES(DisconnectedSlicer[SelectedCategory]),
        Product[Category]
    )
)
```

---

## 2. `USERELATIONSHIP` (Role-Playing Dimensions)

`USERELATIONSHIP` activates an existing **inactive** physical relationship for the duration of a `CALCULATE` expression.

### Syntax
```dax
CALCULATE(<expression>, USERELATIONSHIP(<column1>, <column2>))
```

### Exam Rules & Behavior
* **Requires Physical Relationship:** An inactive relationship **must already exist** in the model schema between `<column1>` and `<column2>`.
* **Overrides Active Relationship:** If an active relationship exists between the same two tables, `USERELATIONSHIP` temporarily disables the active relationship and activates the target inactive one.
* **Cannot Override RLS:** If Row-Level Security is active on a table, `USERELATIONSHIP` cannot bypass security rules.

### Exam Scenario Example
A `Sales` table has `OrderDate`, `ShipDate`, and `DueDAte` connected to a single `Date` table.

```dax
// Default active relationship uses OrderDate
Sales by Order Date = [Total Sales]

// Activating inactive relationship for Ship Date
Sales by Ship Date = 
CALCULATE(
    [Total Sales],
    USERELATIONSHIP(Sales[ShipDate], 'Date'[Date])
)
```

---

## 3. `CROSSFILTER` (Dynamic Cross-Filtering Control)

`CROSSFILTER` dynamically modifies the cross-filtering direction of an existing physical relationship during calculation.

### Syntax
```dax
CROSSFILTER(<column1>, <column2>, <direction>)
```

### Direction Options
* **`None`:** Disables the relationship for the measure evaluation.
* **`OneWay` / `Single`:** Filters flow from the 1-side to the Many-side only.
* **`Both`:** Filters flow bi-directionally across the relationship.

### Exam Scenario Example
Enabling bidirectional filtering only when needed to prevent performance overhead across the whole model:

```dax
// Count customers who purchased products in a selected category
Active Customers in Category = 
CALCULATE(
    DISTINCTCOUNT(Sales[CustomerID]),
    CROSSFILTER(Sales[ProductID], Product[ProductID], BOTH)
)
```

---

## 4. `INTERSECT` vs. `TREATAS` (Performance Comparison)

On the PL-300 exam, you may see older DAX patterns compared against modern equivalents.

| Concept | `INTERSECT` Pattern | `TREATAS` Pattern |
| :--- | :--- | :--- |
| **Approach** | Set-based table intersection | Direct lineage transfer / virtual filter |
| **Syntax Complexity** | Verbose (requires `FILTER` + `VALUES`) | Clean single function |
| **Engine Optimization** | Slower (iterates sets) | Highly optimized in SE (Storage Engine) |
| **Recommendation** | Legacy | **Best Practice** |

---

## 5. Quick Comparison Matrix for PL-300 Exam Questions

| Scenario | Recommended Function / Approach |
| :--- | :--- |
| Apply disconnected slicer values to a dimension | `TREATAS` |
| Map aggregated target data to a granular Date table | `TREATAS` |
| Calculate metrics for `ShipDate` instead of `OrderDate` | `USERELATIONSHIP` |
| Enable bidirectional filter flow for a *single measure* | `CROSSFILTER(..., BOTH)` |
| Disable a relationship temporarily to ignore filters | `CROSSFILTER(..., NONE)` |

---

## 6. Sample PL-300 Practice Questions

### Question 1
**Scenario:** You have a `Sales` table and an `UnrelatedCategories` table that users select from a slicer. You need to write a measure that filters `Sales[Category]` based on the user's selection in `UnrelatedCategories[Category]` without creating a physical relationship in the model.

**Which DAX formula should you use?**

A.  
```dax
CALCULATE([Total Sales], USERELATIONSHIP(UnrelatedCategories[Category], Sales[Category]))
```  
B.  
```dax
CALCULATE([Total Sales], TREATAS(VALUES(UnrelatedCategories[Category]), Sales[Category]))
```  
C.  
```dax
CALCULATE([Total Sales], CROSSFILTER(UnrelatedCategories[Category], Sales[Category], BOTH))
```  
D.  
```dax
CALCULATE([Total Sales], FILTER(Sales, Sales[Category] = VALUES(UnrelatedCategories[Category])))
```

* **Correct Answer:** **B**
* **Explanation:** `TREATAS` propagates filters virtually without a physical relationship. `USERELATIONSHIP` and `CROSSFILTER` both require a pre-existing physical relationship in the data model.

---

### Question 2
**Scenario:** A model has an active relationship between `Sales[OrderDate]` and `'Date'[Date]`, and an inactive relationship between `Sales[DeliveryDate]` and `'Date'[Date]`. You need to create a visual that displays Total Sales based on the `DeliveryDate`.

**What should you do?**

A. Change the active relationship in the model schema to point to `DeliveryDate`.  
B. Use `TREATAS` to join `Sales[DeliveryDate]` with `'Date'[Date]`.  
C. Create a measure using `CALCULATE([Total Sales], USERELATIONSHIP(Sales[DeliveryDate], 'Date'[Date]))`.  
D. Enable bidirectional cross-filtering on the active relationship.

* **Correct Answer:** **C**
* **Explanation:** When an inactive physical relationship exists, `USERELATIONSHIP` is the standard and most efficient way to activate it for specific calculations.