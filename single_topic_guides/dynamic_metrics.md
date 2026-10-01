## 1. Primary Feature: Field Parameters

**Field Parameters** allow report authors to dynamically change the fields or measures being displayed inside visuals using a slicer.

### How to Implement:

1. **Create the Parameter:**
   Navigate to the **Modeling** tab in Power BI Desktop $\rightarrow$ Click **New Parameter** $\rightarrow$ Select **Fields**.

2. **Select Metrics:**
   Add your existing measures (`[Total Sales]`, `[Margin]`, `[Profit]`) into the parameter definition box and name the parameter (e.g., `Select Metric`).

3. **Add Slicer & Visuals:**

   * Check the option to **Add slicer to this page**.

   * Drag the newly created Parameter field into the **Value** or **Axis** section of your charts/tables.

# Dynamic Metric Switching in Power BI

To allow users to switch dynamically between metrics such as **Sales**, **Margin**, and **Profit** in Power BI, the primary feature to use is **Field Parameters**.

## 

1. **User Experience:**
   When users pick a metric from the slicer, the visual updates its values, tooltips, axis labels, and formatting dynamically.

## 2. Alternative Method: DAX `SWITCH()` Pattern

Before Field Parameters were added to Power BI, developers used a custom DAX pattern using a disconnected table.

### Implementation Steps:

1. **Create a Disconnected Table:**
   Create a standalone table with a single column containing the metric names:

   ```
   Metric Table = DATATABLE(
       "Metric Name", STRING,
       {
           {"Sales"},
           {"Margin"},
           {"Profit"}
       }
   )
   
   ```

2. **Create a Dynamic DAX Measure:**
   Use the `SWITCH` and `SELECTEDVALUE` functions to conditionally evaluate the correct calculation:

   ```
   Dynamic Metric = 
   VAR SelectedMetric = SELECTEDVALUE('Metric Table'[Metric Name], "Sales")
   RETURN
       SWITCH(
           SelectedMetric,
           "Sales", [Total Sales],
           "Margin", [Margin],
           "Profit", [Profit],
           [Total Sales] -- Default fallback
       )
   
   ```

3. **Configure Visuals:**
   Put `Metric Table[Metric Name]` in a slicer and use `[Dynamic Metric]` inside your visual's value field.