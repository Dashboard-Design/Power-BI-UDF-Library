# DAX Time Intelligence: Rolling Average UDF

This folder contains a **reusable DAX user‑defined function (UDF)** that calculates a rolling average over a specified number of periods (days, months, or years). It eliminates repetitive code and ensures consistent time intelligence logic across all your Power BI reports.

![Image](https://github.com/user-attachments/assets/6ea6c019-3a47-409e-8885-dc820ce6606b)


## ✨ Features

- **Flexible Periods** – Choose from `DAY`, `MONTH`, or `YEAR` to match your reporting granularity.
- **Single Reusable Function** – Define once, use everywhere. No more copying and pasting similar `DATESINPERIOD` logic.
- **Automatic Context Awareness** – The rolling window always ends at the last date in the current filter context (e.g., the current row in a matrix).
- **Seamless Integration** – Works like any built‑in DAX function once saved to your model.
- **Consistent Averaging** – Averages the measure **per distinct period** (e.g., average monthly sales, not average per transaction), matching common business requirements.

## ⚙️ Parameters

When you call `UDF_RollingAverage`, you must provide three arguments:

| Parameter | Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `_measure` | `SCALAR NUMERIC EXPR` | The measure you want to average (e.g., `[Sales]`, `[Profit]`). | `[Sales]` |
| `numPeriods` | `NUMERIC` | Number of periods to include (positive integer). | `3` |
| `periodUnit` | `STRING` | Unit of time: `"DAY"`, `"MONTH"`, or `"YEAR"`. Case‑insensitive. | `"MONTH"` |

## 🧠 Advanced: Using the User-Defined Function (UDF)

If you have enabled the **DAX user-defined functions** preview feature (Options > Preview features), you can use the `UDF_RollingAverage` code.

1.  **Open DAX Query View**.
2.  **Paste the UDF definition** (provided below) and click **Update Model** to save it to your model.
3.  Now you can call it from any measure like a built‑in function:

    ```dax
    -- Rolling average over the last 3 months
    Sales Rolling 3M = UDF_RollingAverage( [Sales], 3, "MONTH" )

    -- Rolling average over the last 7 days
    Sales Rolling 7D = UDF_RollingAverage( [Sales], 7, "DAY" )

    -- Rolling average over the last 3 years
    Sales Rolling 3Y = UDF_RollingAverage( [Sales], 3, "YEAR" )
    ```

## 🔧 How It Works

The function uses `DATESINPERIOD` to define a moving window that ends at the **last date visible in the current context** (e.g., the date on the current row of a matrix). Inside that window, it calculates the average of the measure **per distinct period**:

- For `DAY`: average per day (`VALUES( 'Date'[Date] )`).
- For `MONTH`: average per month (`VALUES( 'Date'[YearMonthNum] )`).
- For `YEAR`: average per year (`VALUES( 'Date'[Year] )`).

This approach matches your original manual measures and avoids diluting the average by including multiple rows from the same period.

## 💡 Tips

- **Date Table Required** – The function assumes you have a proper date table named `'Date'` with columns `[Date]`, `[YearMonthNum]`, and `[Year]`. Adjust these column names if yours differ.
- **Works in Any Visual** – Use it in matrixes, charts, or tables. The rolling average will respect slicers and filters.
- **Performance** – Because it uses `AVERAGEX` over distinct periods, it is efficient even with large fact tables.
- **Extending the Function** – You can easily add more period units (e.g., `QUARTER`) by adding another `SWITCH` branch and referencing the appropriate column.
- **Handling Incomplete Periods** – The function will average whatever periods are available. If you need a minimum number of periods, you can wrap the result in an `IF` condition.
