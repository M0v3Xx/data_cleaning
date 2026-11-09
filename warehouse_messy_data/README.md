# Warehouse Messy Data: Data Cleaning Project

Cleaning and preparing a deliberately messy warehouse inventory dataset (1,000 records) for analysis and modelling, with a focus on handling missing values **without distorting the data**.

## Project Overview

Inventory data is rarely analysis-ready. This project takes a raw warehouse extract with inconsistent formats, text-encoded numbers and substantial missing data, and turns it into a consistent, documented dataset, keeping every row and recording exactly where information was missing.

**Goal:** produce a clean dataset that can be used for reporting and future predictive modelling, while avoiding common cleaning mistakes (dropping rows, blind mean-filling).

## Dataset

| File | Description | Shape |
|---|---|---|
| `warehouse_messy_data.csv` | Raw data as received | 1,000 × 10 |
| `warehouse_messy_data_cleaned.csv` | Cleaned data with missing-value flags | 1,000 × 14 |

**Columns:** Product ID, Product Name, Category, Warehouse, Location, Quantity, Price, Supplier, Status, Last Restocked

## Data Quality Issues Found

| Issue | Detail |
|---|---|
| Numbers stored as text | `Quantity` contained the words "two hundred" in 160 rows instead of `200`, so the column was read as text |
| Inconsistent casing | `Category` in UPPERCASE (`ELECTRONICS`), `Status` in Title Case (`In Stock`) |
| Missing `Quantity` | 158 rows (15.8%) |
| Missing `Price` | 207 rows (20.7%) |
| Missing `Last Restocked` | 200 rows (20.0%) |
| Non-unique `Product ID` | Only 615 distinct IDs across 1,000 rows, so it cannot identify a record |
| Padded text | `Product Name` values carry leading/trailing spaces |

## Cleaning Steps

1. **Converted `Quantity` to numeric**, mapping the text value "two hundred" to `200`. All valid values are now 50, 100, 150, 200 or 300.
2. **Standardised casing** of `Category` and `Status` to lowercase.
3. **Added `Record ID`** (1–1000) as a unique row identifier, since `Product ID` repeats.
4. **Added missing-value indicator flags** (`1` = value was missing, `0` = present):
   - `Missed Quantity`
   - `Missed Price`
   - `Missed Last Restocked`
5. **Kept all 1,000 rows.** Nothing was dropped, and no duplicate rows were found.

## Key Findings & Decisions

I tested whether the missing `Price` and `Quantity` values could be predicted from the other columns, using 5-fold cross-validation with label encoding, one-hot encoding, random forest, gradient boosting and logistic regression.

- **The other columns carry almost no signal.** Every model scored at roughly chance level (about 0.25 accuracy for Price's 4 possible values, about 0.20 for Quantity's 5), no better than always guessing the most common value.
- **Missingness looks random.** No variable explains where values are missing once multiple testing is accounted for.
- **Mean-filling would be harmful.** It would shrink the standard deviation of Price by about 11% and Quantity by about 8%, and would create prices that do not exist (e.g. 28.09 when valid prices are 9.99, 19.99, 29.99 and 49.99).

**Decision:** the cleaned file keeps missing values as `NaN` and records them in the flag columns, so that the choice of imputation method stays explicit and reversible.

## Repository Structure

```
warehouse_messy_data/
├── README.md
├── warehouse_messy_data.csv           # raw data
└── warehouse_messy_data_cleaned.csv   # cleaned data
```

## Quick Start

```python
import pandas as pd

raw = pd.read_csv("warehouse_messy_data.csv")
clean = pd.read_csv("warehouse_messy_data_cleaned.csv")

print(raw.shape, clean.shape)            # (1000, 10) (1000, 14)
print(clean[["Missed Quantity", "Missed Price", "Missed Last Restocked"]].sum())
```

## Next Steps

- **Impute `Price` and `Quantity`** by drawing from the observed distribution (preserves variance and valid values), ideally with multiple imputation and pooled estimates.
- Strip whitespace from `Product Name`.
- Parse `Last Restocked` (dd/mm/yyyy) into a proper datetime type.
- Build a model with imputation fitted inside cross-validation folds only, to avoid data leakage.

## Tools

Python · pandas 
