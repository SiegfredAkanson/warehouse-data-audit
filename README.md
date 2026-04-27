# Warehouse-data-audit

## README.md Structure
1. Section 1; Project Introduction
2. Section 2; Problem Statement
3. Section 3; Audit Report
4. Section 4; Solution Log
5. Section 5; Final Verdict

### Section 1; Project Introduction
Global Warehouse Inventory — Data Integrity Audit
**Role**: Junior Data Scientist  
**Reporting To:** Head of Operations 
**Framework:** CRISP-DM (Stages 1–3)  
**Date:** April 2026 

### Section 2; Problem statement
The Global Warehouse Division reported som critical errors between 
digital records and physical stock. Management was unable to make 
purchasing decisions because the data suggested the company was holding 
ghost inventory and losing money.

This audit was commissioned to:
* Restore trust in inventory metrics
* Identify all structural flaws in the dataset
* Produce a clean, audit-ready file for the finance team
* Confirm the data is safe for the next phase: Predictive Modeling

### Section 3; Audit report
| # | Risk | Column Affected                          | Rows Affected                                  | Business Impact |
|---|------|------------------------------------------|------------------------------------------------|-----------------|
| 1 | Naming Corruption | Product Id, Product Name, Last restocked | No rows                                        | Scripts crash silently |
| 2 | Missing Data | Price, Quantity                          | 158(Quantity), 207(Price), 200(Last restocked) | Cannot calculate inventory value |
| 3 | String Trap | Price, Quantity                          | 207(price), 158(quantity)                      | Arithmetic operations fail |
| 4 | Inventory Bloat | No columns found                         | no rows                                        | Overstated warehouse count |
| 5 | Operational Outliers | No Columns                               | No rows                                        | False financial reporting |


### Risk 1 — Naming Corruption
The columns had whitespaces. This would have made columns impossible to call in standard scripts 
and causes silent KeyErrors.

### Risk 2 — Missing Data
There was missing data across 2 columns(Price, Quantity) and across 158 rows, 207 rows, and 200 rows under price, quantity and last restocked respectively

### Risk 3 — The String Trap
The Price and Quantity columns were stored as object (text) 
dtype instead of numeric. Values like "NaN" and "two hundred" 
prevented any arithmetic, making it impossible to calculate values like;
**Total Inventory Value = Quantity × Price**

### Risk 4 — Inventory Bloat
Duplicates were found under some columns,
These ghost entries would have caused management to believe stock levels 
were higher than reality.

### Risk 5 — Operational Outliers
During the audit, there were no outliers or extreme values found in the dataset. 
These impossible values would have distorted average price calculations and 
masking true stock levels.

### Section 4; The Solution Log

### 1. Standardisation (Fixing Column Names)
All column names were stripped of whitespace and spaces replaced with underscores. This ensures no script will 
crash when calling a column by name.

### 2. Type Conversion (String Trap)
Written number words in Quantity 
were mapped to integers using a manual dictionary.

### 3. Imputation Strategy (Missing Data)
Numerical columns were filled with the mean rather than 
the median. The mean and median were found to be 
close in value, indicating the data was roughly symmetric with 
no extreme outliers significantly distorting the average. 
In this case the mean is the more precise estimate of central 
tendency since it accounts for every value in the column, making it 
a more accurate fill value than the median.

### 4. Deduplication (Ghost Inventory)
Duplicate rows were identified and removed. The index was reset 
after removal to ensure clean sequential numbering.

### 5. Outlier Fixing
Since there were no extreme values found, the only 0 values were the ones used to fill in the NaN values.

## 5. Final Verdict — Is the Data ML-Ready?

Yes. The dataset is now safe for Predictive Modeling.

The following conditions have been met:
* All column names are clean and machine-readable
* All numerical columns are stored as correct numeric types
* Missing values have been imputed using statistically sound methods
* Ghost inventory (duplicates) have been fully removed
* Impossible values have been eliminated

The clean file warehouse_clean.csv has been exported and is 
ready for the Finance team and the next CRISP-DM phase.