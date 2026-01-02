# Possum Morphometrics Analysis: Cleaning, Hypothesis Testing, and Regression Modeling

## Project Overview
This project analyzes the **possum.csv** dataset to understand possum morphometrics in urban/wild contexts. It includes rigorous **data cleaning**, **outlier treatment**, **distribution checks (skewness/kurtosis)**, **two hypothesis tests (t-tests)**, and **regression modeling (3 simple + 1 multiple regression)** to identify statistically meaningful relationships and decision-ready insights.

## Dataset
**File:** `possum.csv`  
**Key fields (examples):** population (`Pop`), sex (`sex`), site (`site`), and multiple morphometric measurements such as `hdlngth`, `skullw`, `totlngth`, `taill`, `footlgth`, `earconch`, `eye`, `chest`, `belly`, plus `age` and `case`.

## What Was Done (Workflow)

### 1) Initial Data Audit
- Loaded dataset and inspected with `df.info()`, `df.head()`, `df.describe()`.
- Printed unique values per column to detect invalid entries.
- **Found issues:** numeric columns stored as `object`, missing values, non-numeric junk (`?`, `na`, `5,6`), and extreme outliers (e.g., `2500`, `1500`, and negative measurements).

### 2) Type Fixes (Numeric Coercion)
- Converted `age`, `skullw`, `footlgth`, and `belly` to numeric with invalid entries coerced to `NaN`.
- Outcome: columns became usable numerics (`float64`) and bad strings safely became missing values.

### 3) Missing Value Handling
- Filled missing `case` with `95`, then converted `case` to numeric.
- Filled missing `Pop` using **mode**.
- Imputed remaining missing values with **median** for key numeric columns:
  - `age`, `hdlngth`, `skullw`, `totlngth`, `footlgth`, `eye`, `belly`, `site`
- Outcome: dataset became **fully complete (no NaNs)**.

### 4) Outlier Treatment (Two-Stage)
**Stage A: Replace known bad values**
- Replaced obvious erroneous entries (e.g., `2500`, `1500`, `1230`, `200`, and negatives like `-12`, `-14`, `-22`) in columns such as `skullw`, `footlgth`, `belly`, `earconch`, and `taill` with column medians.

**Stage B: IQR-based treatment**
- Applied the **IQR method** on major numeric columns and replaced outliers with the column median.
- Verified improvement using post-treatment histograms/boxplots and recalculated **skewness and kurtosis**.

### 5) Standardization
- Rounded numeric columns to **1 decimal place** for consistent reporting.

## Statistical Analysis

### Hypothesis Test 1: Head Length Differences Between Populations
- Test: **Independent samples t-test**
- Groups: `Pop = Vic` vs `Pop = other`
- Variable: `hdlngth`
- Result: **t = 0.147, p = 0.884**
- Conclusion: No statistically significant difference (**fail to reject H0**, α = 0.05).

### Hypothesis Test 2: Chest Size Differences Between Sex
- Test: **Independent samples t-test**
- Groups: Male vs Female
- Variable: `chest`
- Result: **t = -1.709, p = 0.091**
- Conclusion: No statistically significant difference (**fail to reject H0**, α = 0.05).

## Regression Modeling

### Simple Linear Regression 1: Head Length ~ Total Length
- Model: `hdlngth ~ totlngth`
- R²: **0.410**
- Coefficient: `totlngth = 0.463` (**p < 0.001**)

### Simple Linear Regression 2: Head Length ~ Skull Width
- Model: `hdlngth ~ skullw`
- R²: **0.443**
- Coefficient: `skullw = 0.992` (**p < 0.001**)

### Simple Linear Regression 3: Chest ~ Belly
- Model: `chest ~ belly`
- R²: **0.273**
- Coefficient: `belly = 0.424` (**p < 0.001**)

### Multiple Linear Regression: Predicting Total Length
- Model: `totlngth ~ age + hdlngth + skullw + taill + footlgth + earconch + eye + chest + belly`
- R²: **0.719**
- Significant predictors: **hdlngth**, **taill**, **footlgth** (all **p < 0.001**)
- Interpretation: Total length is primarily driven by head, tail, and foot measurements.

## Visuals Produced
- Histograms + boxplots for major numeric columns (pre and post outlier treatment)
- Correlation heatmap across numeric features

## How to Run
1. Place `possum.csv` in the same folder as the notebook/script.
2. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy statsmodels scikit-learn
   ```
3. Run the notebook from top to bottom.

## Dependencies
- pandas
- numpy
- matplotlib
- seaborn
- scipy
- statsmodels
- scikit-learn

## Notes
- Cleaning used **coercion + median/mode imputation** to preserve row count.
- Outlier handling combined **known-error replacement** and **IQR-based treatment** for robustness.
- Statistical conclusions use α = 0.05.

---
**Author:** Parth Chetan Rajguru
