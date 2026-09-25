# PL-300 Domain 1: Prepare the Data — True or False Study Guide

## This study guide contains 25 True or False questions covering Domain 1: Prepare the Data (25%–30%) of the PL-300 Microsoft Power BI Data Analyst certification exam.

Question 1

In DirectQuery mode, all underlying dataset data is fully loaded and stored inside the Power BI desktop file memory.

Answer: False

Explanation: DirectQuery mode queries the underlying data source in real time whenever visuals are rendered. Data remains in the source system rather than being cached inside the Power BI memory engine (VertiPaq).

Question 2

Query Folding occurs when transformation steps written in Power Query are translated into a single native query executed by the source database.

Answer: True

Explanation: Query folding offloads transformation workloads (such as filtering, grouping, or joining) directly to the source database engine (e.g., SQL Server), which significantly optimizes refresh performance.

Question 3

Merging queries in Power Query operates like a SQL UNION, combining rows from two tables with identical structures.

Answer: False

Explanation: Merging queries is equivalent to a relational JOIN (combining columns based on a key value). Appending queries is equivalent to a UNION (combining rows).

Question 4

The Appending operation in Power Query combines columns from two tables based on matching key values.

Answer: False

Explanation: Appending stacks rows from two or more tables together (like a SQL UNION). Merging is what combines columns based on matching key values.

Question 5

By default, Power Query data profiling tools analyze the entire dataset regardless of row count.

Answer: False

Explanation: By default, Power Query profiles data based on the top 1,000 rows to maintain quick performance. You can change this behavior in the status bar to profile the entire dataset if necessary.

Question 6

Replacing error values in Power Query allows a query to complete successfully without dropping entire rows.

Answer: True

Explanation: "Replace Errors" targets only the problematic cells, substituting errors with specific default values (such as 0 or BLANK), allowing the remaining row values to load normally.

Question 7

Column Distribution views in Power Query display distinct and unique value counts for selected columns.

Answer: True

Explanation: Column Distribution provides visual cues and numeric counts showing the number of distinct values (all different values) and unique values (values that appear exactly once).

Question 8

The Unpivot Columns transformation turns attribute-value pair columns into clean row-based data structures.

Answer: True

Explanation: Unpivoting converts wide tables (e.g., separate columns for every month) into tall, narrow tables with an Attribute column and a Value column, which is essential for proper star-schema modeling in Power BI.

Question 9

Dual storage mode allows a table to fulfill queries using Import or DirectQuery depending on the context.

Answer: True

Explanation: Tables set to Dual mode act as Import mode when queried alongside Import tables and act as DirectQuery when queried alongside DirectQuery tables, preventing expensive cross-source queries.

Question 10

Adding a custom step using complex M code or index columns can interrupt subsequent query folding steps.

Answer: True

Explanation: If an M operation cannot be translated into the native query language of the source database (such as adding an Index column or custom M functions), Query Folding stops at that step, and subsequent steps will run in Power Query engine memory.

Question 11

The Column Quality tool provides percentages for Valid, Error, and Empty cell values in preview data.

Answer: True

Explanation: Column Quality displays a visual bar chart at the top of each column showing the exact distribution of Valid, Error, and Empty cell percentages.

Question 12

Disabling 'Enable Load' on a Power Query query completely deletes the query and its underlying M code.

Answer: False

Explanation: Disabling "Enable Load" prevents the query results from loading into the report data model, but the query remains active in Power Query and can still be referenced by other queries.

Question 13

A Parameter in Power Query can be used to dynamically change server connections or filter incoming source rows.

Answer: True

Explanation: Parameters allow users to easily pass variables into connection strings, M code filters, or function inputs without modifying the underlying query definition directly.

Question 14

When combining multiple CSV files from a single folder, all files must share the exact same schema and column structure.

Answer: True

Explanation: For Power Query to cleanly append and merge files using the Combine Files wizard, all source files inside the targeted directory must share consistent headers and data structures.

Question 15

The 'Remove Duplicates' step in Power Query is case-sensitive when evaluating text values by default.

Answer: True

Explanation: Power Query is natively case-sensitive. Words like "Data" and "data" are considered distinct values during deduplication unless explicit text-lowercasing steps are applied first.

Question 16

DirectQuery mode supports all standard Power Query data transformation steps without limitation.

Answer: False

Explanation: DirectQuery enforces strict limits on transformations. Any transformation that cannot be folded back to the underlying database engine will throw an error or force a switch to Import mode.

Question 17

Group By in Power Query allows you to aggregate rows based on one or more columns while summarizing data.

Answer: True

Explanation: Group By performs aggregation tasks (like SUM, AVERAGE, MIN, MAX, or COUNT) across specified grouping columns directly within the Power Query transformation engine.

Question 18

Power Query M formula language is completely case-insensitive.

Answer: False

Explanation: M is strictly case-sensitive. Keywords, functions, and variable names must match the exact casing (e.g., Table.SelectRows is valid, but table.selectrows will cause a syntax error).

Question 19

'Fill Down' in Power Query replaces null values with the value from the nearest non-null cell above it.

Answer: True

Explanation: Fill Down propagates the last valid value down through consecutive null cells until it encounters a new non-null value.

Question 20

Reference query dependency creates a new query that uses the output of an existing query as its starting data source.

Answer: True

Explanation: Referencing a query branches off from the end step of the source query. If the source query changes, the referenced query automatically absorbs those updates.

Question 21

Promoting headers converts the first row of raw data into table column names in Power Query.

Answer: True

Explanation: The "Use First Row as Headers" transformation elevates the values in row 1 to serve as the formal column headers for the table.

Question 22

Data Profile settings can be updated to profile the entire dataset directly from the Power Query status bar.

Answer: True

Explanation: Clicking on "Column profiling based on top 1000 rows" in the bottom-left status bar lets you toggle to "Column profiling based on entire dataset."

Question 23

Changing a column's data type never impacts query folding capabilities.

Answer: False

Explanation: Certain data type conversions (such as changing complex string structures to custom date-time formats) cannot always be translated into native database SQL, potentially breaking the query folding chain.

Question 24

Native SQL Queries typed directly into the relational source database connector allow full downstream query folding.

Answer: False

Explanation: Specifying a custom Native SQL Query in the database connector dialog generally disables subsequent Query Folding for any additional UI transformation steps added downstream.

Question 25

Column Profile view displays value distribution histograms alongside descriptive statistics like Min, Max, Mean, and Standard Deviation.

Answer: True

Explanation: Column Profile provides deep data inspection, showing distribution charts as well as summary stats (e.g., minimum, maximum, average, standard deviation, null count) for the selected column.