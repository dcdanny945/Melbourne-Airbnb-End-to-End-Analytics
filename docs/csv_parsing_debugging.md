# CSV Parsing — Debugging & Resolution

## 1. Problem

During the development of `NB02_Listings_ETL`, the Melbourne Airbnb Listings CSV was incorrectly parsed because some quoted text fields contained embedded line breaks.

This produced more records than expected and introduced invalid Listing IDs.

## 2. Data Quality Investigation

| Validation | Incorrect Parsing | Corrected Parsing |
|---|---:|---:|
| Total Records | 46,525 | 25,715 |
| Invalid Listing IDs | 20,810 | 0 |
| Duplicate Listing IDs | Detected | 0 |

The initial results failed the data quality checks.

## 3. Solution

Updated the PySpark CSV reader to correctly handle multiline quoted fields using appropriate `multiLine`, `quote` and `escape` settings.

The ETL then validates:

- Valid Listing IDs
- Duplicate Listing IDs
- Record counts and required fields

Critical validation failures stop the ETL process.

## 4. Result

The corrected parser successfully processed **25,715 unique Airbnb listings**, with no invalid or duplicate Listing IDs.

**Key Learning:** Successful file ingestion does not guarantee data quality. Validating record counts and unique identifiers is essential before loading data into downstream tables.

The production implementation is available in `notebooks/NB_02_Listings_ETL_Production.ipynb`.