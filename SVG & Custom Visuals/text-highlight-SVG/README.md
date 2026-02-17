# SVG Visuals for Power BI: Dynamic Status Pills

This folder contains DAX measures that generate **dynamic SVG images** to create colorful, width‑adjustable "status pill" visuals directly in your Power BI reports. They are perfect for highlighting categories, statuses, or any dimension values with consistent, automatically assigned colors.

![Example of SVG Pills in a Power BI Table](images/powerbi-svg-pill-example.png) <!-- REPLACE WITH YOUR CORRECT IMAGE PATH -->

## ✨ Features

*   **Automatic Colors** – Each distinct value gets a unique color from a fixed palette, based on a simple hash of the text. No manual color mapping is needed.
*   **Dynamic Width** – The pill’s width automatically adjusts to the length of the text (character count + padding).
*   **Easy to Use** – Two versions are provided:
    1.  **Template Measure**: Just change one line to point to your dimension column.
    2.  **User‑Defined Function (UDF)**: A reusable function (once enabled) that accepts any column as a parameter.
*   **Customizable** – Font weight, padding, character width, and the color palette can be easily tweaked.


## ⚙️ Customization Options

In the template, look for the "CONFIGURATION" section:

*   `_FontWeight`: Change text thickness (e.g., "400"=normal, "700"=bold).
*   `_CharWidth`: Adjust the estimated pixels per character if the pills look too tight or too wide.
*   `_Padding`: Add more horizontal space around the text.
*   `_Color` Palette: Replace the hex codes in the `SWITCH` statement with your own brand colors.

## 🧠 Advanced: Using the User-Defined Function (UDF)

If you have enabled the **DAX user-defined functions** preview feature (Options > Preview features), you can use the `SVG_Pill_UDF.dax` code.

1.  Open **DAX Query View**.
2.  Paste the UDF definition and click **Update Model** to save it.
3.  Now you can call it from any measure like a built-in function:
    `Measure = SVG_Pill( 'YourTable'[YourColumn] )`
    You can also override the defaults: `SVG_Pill( 'Table'[Col], "700", 8, 10 )`

## 💡 Tips

*   The pills work best with **categorical data** that has a manageable number of unique values.
*   If two different values accidentally get the same color (a palette collision), you can manually adjust the `SWITCH` statement or increase the palette size.
*   The `HASONEVALUE` check ensures the SVG only renders when a single value is in context (important for totals or subtotals).

