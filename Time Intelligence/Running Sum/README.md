# Running Total UDF for Power BI

This folder contains a reusable **User‑Defined Function (UDF)** that calculates a running total (cumulative sum) over an ordered column. It supports optional partitioning and direction control, and uses a simple convention to disable partitioning when needed.

![Example of Running Total in a Power BI Table](images/runningtotal-example.png) <!-- REPLACE WITH YOUR ACTUAL SCREENSHOT -->

## ✨ Features

- **Flexible Ordering** – Works with any column that defines a logical order (date, index, month number, etc.).
- **Optional Partitioning** – Restart the running total for each group (e.g., by year, category) by passing a partition column.
- **Simple Convention for No Partition** – To get a global running total, just pass the **same column** for both the order and partition parameters.
- **Direction Control** – Choose ascending (past→present) or descending (future→past).
- **Respects User Filters** – Uses `ALLSELECTED` on the order table to keep slicers on other tables while removing direct filters on the order table itself (classic running total behavior).
- **Handles Totals Gracefully** – Returns blank at grand totals (when no single order value is present).

## 🔧 Parameters

All parameters are **required** – no blanks are accepted. Use the convention below to control partitioning.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `_measure` | `SCALAR NUMERIC EXPR` | The measure to accumulate (e.g., `[Sales]`, `[Profit]`). |
| `_orderTable` | `ANYREF EXPR` | The table containing the order column (e.g., `'Date'`). |
| `_orderBy` | `ANYREF EXPR` | The column that defines the order (e.g., `'Date'[Date]`). |
| `_partitionBy` | `ANYREF EXPR` | Column to restart the total for each group. **To disable partitioning, pass the same column as `_orderBy`.** |
| `_direction` | `STRING` | `"ASC"` for cumulative from smallest to largest; `"DESC"` for largest to smallest. |

## 📥 How to Use

1. **Enable the UDF preview feature** (if not already):  
   File → Options → Preview features → check **DAX user-defined functions** → restart.

2. **Save the UDF to your model**:  
   - Open **DAX Query View**.  
   - Paste the `UDF_RunningTotal` definition.  
   - Click **Update model with changes**.

3. **Call the UDF from any measure**:

   ```dax
   -- Year‑to‑date running total (partitioned by year)
   Sales YTD = UDF_RunningTotal( [Sales], 'Date', 'Date'[Date], 'Date'[Year], "ASC" )

   -- Global running total over all dates (no partition – same column for order and partition)
   Sales Running Total = UDF_RunningTotal( [Sales], 'Date', 'Date'[Date], 'Date'[Date], "ASC" )

   -- Descending running total (e.g., remaining sales) with partition
   Sales Remaining = UDF_RunningTotal( [Sales], 'Date', 'Date'[Date], 'Date'[Year], "DESC" )

   -- Running total within each product category (assuming a 'Product' table with a relationship)
   Sales by Category Running = 
   UDF_RunningTotal( [Sales], 'Product', 'Product'[ProductName], 'Product'[Category], "ASC" )
