# Currency Conversion UDF

This folder contains a reusable **DAX user‑defined function (UDF)** that retrieves the latest exchange rate from a rate table on or before a given date. It eliminates the need to rewrite the same lookup logic in every measure that requires currency conversion.

<img width="983" height="794" alt="Image" src="https://github.com/user-attachments/assets/c0db20a8-1d1e-48dd-b04f-c132a0d5f18d" />

## ✨ Features

- **As‑of‑Date Lookup** – For any date, finds the most recent exchange rate available in the rate table (on or before that date).
- **No Relationship Required** – Works independently of model relationships; performs a pure lookup using `CALCULATE` and `FILTER`.
- **Simple & Reusable** – Define once, then use in any measure that needs currency conversion – just pass the rate table, date column, rate column, and the as‑of date.
- **Handles Missing Rates Gracefully** – If no rate is found for the date, returns `BLANK()` (easily wrapped with `COALESCE` if a fallback is desired).
- **Lightweight** – Designed to be called inside iterators like `SUMX`, or even as a calculated column.

## 📥 How to Use

### 1. Enable UDF Preview (if not already)
- Go to **File → Options and settings → Options → Preview features**.
- Check **DAX user‑defined functions** and restart Power BI Desktop.

### 2. Save the UDF to Your Model
- Open **DAX Query View** (the icon next to Model view).
- Paste the following UDF definition:

```
DEFINE
    /// GetExchangeRate returns the latest exchange rate from a rate table on or before a given date.
    /// It does not rely on any relationship; it performs a direct lookup.
    ///
    /// Parameters:
    ///   rateTable     : ANYREF EXPR - Table containing exchange rates (e.g., 'EUvsUSD_Exchange').
    ///   rateDateCol   : ANYREF EXPR - Date column in the rate table (e.g., 'EUvsUSD_Exchange'[observation_date]).
    ///   rateValueCol  : ANYREF EXPR - Column with the exchange rate value (e.g., 'EUvsUSD_Exchange'[EU vs. USD]).
    ///   asOfDate      : DATETIME    - The date for which to get the rate (e.g., Orders[Order Date]).
    ///
    /// Returns: The exchange rate (scalar) or the default value.

    FUNCTION GetExchangeRate = 
        (
            rateTable     : ANYREF EXPR,
            rateDateCol   : ANYREF EXPR,
            rateValueCol  : ANYREF EXPR,
            asOfDate      : DATETIME 
        ) =>
        
        -- Find the latest date in the rate table that is <= asOfDate and has a non-blank rate
        VAR _LastDate =
            CALCULATE(
                MAX( rateDateCol ),
                FILTER(
                    rateTable,
                    rateDateCol <= asOfDate &&
                    NOT ISBLANK( rateValueCol )
                )
            )
        -- Get the rate for that date
        VAR _Rate =
            CALCULATE(
                SELECTEDVALUE( rateValueCol ),
                FILTER( rateTable, rateDateCol = _LastDate )
            )
        RETURN _Rate
