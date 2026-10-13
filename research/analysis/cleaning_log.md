# Data Cleaning Log — sales_2026_messy.xlsx

This log documents all data quality issues, structural anomalies, and formatting defects identified in `sales_2026_messy.xlsx`, along with the specific programmatic fixes applied to produce the consolidated single-sheet master dataset `./analysis/sales_clean.xlsx`.

---

## 1. Sheet: `North`
* **Raw Row Count**: 324 rows (3 title/blank header rows + 1 column header row + 320 data rows)
* **Clean Row Count**: 320 data rows

### Problems Found & Fixes Applied:
1. **Metadata & Non-Data Header Banner (Rows 1–3)**:
   - *Problem*: Rows 1 to 3 contained non-tabular title text (`NORTH REGION – SALES LOG 2026 (CONFIDENTIAL)` and `exported by M. Weber, do not forward`) followed by an empty row before the actual column headers in Row 4.
   - *Fix*: Skipped top 3 metadata rows and used Row 4 as the schema header.
2. **Missing Region Identifier**:
   - *Problem*: Sheet contained sales transactions for the North region, but had no explicit `Region` column.
   - *Fix*: Created a standardized `Region` column populated with `"North"`.
3. **Discount Representation as Integer Percentage Points**:
   - *Problem*: The `Discount %` column stored discount percentages as integer whole numbers (`5`, `0`, `10`) rather than standard decimal rates.
   - *Fix*: Converted `Discount %` to float and divided by 100 to yield standardized decimal rates (`0.05`, `0.00`, `0.10`).
4. **Data Types & Whitespace**:
   - *Problem*: SKU and Order ID strings required whitespace stripping.
   - *Fix*: Stripped all leading/trailing whitespace and standardized SKU casing to uppercase.

---

## 2. Sheet: `South_Sales`
* **Raw Row Count**: 355 rows (1 header row + 354 data rows)
* **Clean Row Count**: 340 unique data rows (14 duplicate transactions removed)

### Problems Found & Fixes Applied:
1. **Inconsistent Date Formats**:
   - *Problem*: The `order_date` column contained mixed date formatting: `YYYY/MM/DD` (e.g., `2026/01/01`) and European `DD.MM.YYYY` (e.g., `03.01.2026`).
   - *Fix*: Implemented custom date parser detecting delimiters (`/` vs `.`) to normalize all dates into standard `YYYY-MM-DD` timestamps.
2. **Duplicate Order Records**:
   - *Problem*: Identified 14 duplicate order transactions (28 rows total) where the same Order ID (e.g. `S-10028`, `S-10034`, `S-10065`) was recorded twice with slight formatting variations in date string or category casing.
   - *Fix*: Deduplicated rows based on unique `Order ID`, retaining 340 valid distinct transactions.
3. **String-Formatted European Currency Prices**:
   - *Problem*: The `Price` column contained strings with currency symbols and comma decimals (e.g., `'€89,00'`, `'€129,00'`).
   - *Fix*: Stripped `'€'`, replaced comma `','` with period `'.'`, and parsed to numeric float values.
4. **String-Formatted Discount Percentages**:
   - *Problem*: The `Disc` column contained string percentages (e.g., `'5%'`, `'10%'`, `'25%'`, `'35%'`).
   - *Fix*: Stripped `'%'`, parsed to float, and divided by 100 to standardize as decimal rates.
5. **Messy Text Casing and Whitespace in SKU and Category**:
   - *Problem*: Product Code and Category strings contained trailing whitespace and inconsistent casing (e.g., `'hm-045 '`, `'EL-450'`, `'Home '`, `'ELECTRONICS'`, `'home'`).
   - *Fix*: Stripped whitespace, standardized SKUs to uppercase, and mapped categories to official catalog values.
6. **Non-Standard Column Names & Missing Region**:
   - *Problem*: Headers used non-standard labels (`order_date`, `OrderNo`, `Product Code`, `Cat.`, `Qty`, `Price`, `Disc`) and lacked a `Region` column.
   - *Fix*: Standardized to master column names and added `Region = "South"`.

---

## 3. Sheet: `West Q1-Q3`
* **Raw Row Count**: 320 rows (1 header row + 319 rows containing 300 data rows, 9 monthly subtotal rows, 9 blank separator rows, 1 grand total row)
* **Clean Row Count**: 300 data rows

### Problems Found & Fixes Applied:
1. **Embedded Subtotal and Grand Total Rows**:
   - *Problem*: The sheet contained 9 monthly subtotal rows (`Subtotal January`, `Subtotal February`, ... `Subtotal September`) with `Notes = 'auto'`, 9 completely empty separator rows, and 1 final `GRAND TOTAL` row.
   - *Fix*: Programmatically filtered out all rows where `Product` was null or started with `'Subtotal'` or `'GRAND'`.
2. **Dirty String Units ('pcs' suffix)**:
   - *Problem*: The `Units Sold` column mixed raw integer values with strings containing unit suffixes (e.g., `'2 pcs'`, `'7 pcs'`, `'4 pcs'`).
   - *Fix*: Stripped `' pcs'` suffix and cast all values to clean integers.
3. **Missing Category Column**:
   - *Problem*: The sheet lacked a `Category` column entirely.
   - *Fix*: Enriched transactions by joining `Category` from `Product Costs` using `SKU`.
4. **Different Column Names & Order**:
   - *Problem*: Used non-standard column names (`Product` for SKU, `Units Sold` for Units, `List Price` for Unit Price, `Ref` for Order ID, `Disc.` for Discount, `Sale Date` for Date).
   - *Fix*: Realigned and standardized column names to match the consolidated schema.
5. **Notes Column Inspection**:
   - *Problem*: Contains operational annotations (`returned 1`, `check price`, `promo`).
   - *Fix*: Audited monthly subtotals confirming `Units Sold` represents gross transaction volume; retained clean transaction records.

---

## 4. Sheet: `East (Jan-Sep)`
* **Raw Row Count**: 311 rows (1 header row + 310 data rows)
* **Clean Row Count**: 301 data rows (9 unquantified `'n/a'` records removed)

### Problems Found & Fixes Applied:
1. **German Column Headers**:
   - *Problem*: Columns were named in German: `Datum`, `Bestellnr.`, `Artikel`, `Menge`, `Preis`, `Rabatt`.
   - *Fix*: Mapped to standard English headers: `Date`, `Order ID`, `SKU`, `Units`, `Unit Price (€)`, `Discount`.
2. **Excel Serial Number Dates**:
   - *Problem*: The `Datum` column stored dates as unformatted Excel integer serial numbers (e.g., `46023`, `46025`).
   - *Fix*: Converted Excel serial days using origin `'1899-12-30'` to standardized `YYYY-MM-DD` date objects (e.g., `46023` -> `2026-01-01`).
3. **Invalid Missing Quantities (`Menge = 'n/a'`)**:
   - *Problem*: 9 rows had `'n/a'` in the quantity (`Menge`) column (Orders: `E-10057`, `E-10227`, `E-10261`, `E-10232`, `E-10219`, `E-10102`, `E-10016`, `E-10094`, `E-10118`), making revenue and unit calculations impossible.
   - *Fix*: Excluded the 9 invalid non-numeric records to ensure financial integrity.
4. **Missing Region and Category Columns**:
   - *Problem*: Sheet lacked `Region` and `Category` columns.
   - *Fix*: Added `Region = "East"` and mapped `Category` from `Product Costs`.

---

## 5. Sheet: `Product Costs`
* **Raw Row Count**: 16 rows (1 header + 13 SKU records + 1 empty row + 1 text footnote note)
* **Clean Lookup Table**: 13 unique SKUs mapped to Product Name, Category, and Unit Cost (€)

### Processing & Enrichment:
1. Filtered out trailing blank row and qualitative footnote note (`Note: Apparel costs rose ~8% from July...`).
2. Created reference mapping dictionaries for:
   - `SKU -> Product Name`
   - `SKU -> Category`
   - `SKU -> Unit Cost (€)`
3. Enriched all consolidated transactions with baseline Unit Cost and calculated:
   - `Revenue (€) = Units × Unit Price × (1 - Discount)`
   - `COGS (€) = Units × Unit Cost`
   - `Gross Profit (€) = Revenue - COGS`
   - `Gross Margin (%) = (Gross Profit / Revenue) × 100`

---

## 6. Consolidation & Final Dataset Summary

| Region Sheet | Raw Rows | Excluded / Deduplicated Rows | Final Clean Rows |
| :--- | :--- | :--- | :--- |
| **North** | 324 | 4 (3 title/blank + 1 header) | 320 |
| **South_Sales** | 355 | 15 (1 header + 14 duplicate orders) | 340 |
| **West Q1-Q3** | 320 | 20 (1 header + 9 subtotals + 9 blanks + 1 grand total) | 300 |
| **East (Jan-Sep)** | 311 | 10 (1 header + 9 `'n/a'` quantity rows) | 301 |
| **Total Master Dataset** | **1,310** | **49** | **1,261** |

*Output File*: `./analysis/sales_clean.xlsx` (Single sheet: `Sales_Cleaned_2026`)
