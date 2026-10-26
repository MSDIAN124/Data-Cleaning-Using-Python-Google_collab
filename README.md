# Data-Cleaning-Using-Python-Google_collab
# 🧹 Customer Sales Data Cleaning & Preprocessing Pipeline

An end-to-end exploratory data analysis and data cleaning pipeline built in Python to transform a messy, unstructured customer sales dataset into a clean, analysis-ready format using Pandas, NumPy, Scipy, and Seaborn.

---

## 🚀 Project Overview
Real-world datasets are rarely clean. This project demonstrates a systematic approach to handling common data quality issues—such as missing values, inconsistent categorical formatting, messy string-embedded numerical values, duplicates, and statistical outliers—to prepare a robust dataset for downstream machine learning or business intelligence workflows.

* **Original Dataset Size:** 10,200 rows × 12 columns
* **Cleaned Dataset Size:** 8,989 rows × 12 columns
* **Tech Stack:** Python, Pandas, NumPy, SciPy, Matplotlib, Seaborn

---

## 🛠️ Data Cleaning & Preprocessing Steps

### 1. Initial Inspection & Exploratory Data Analysis
* Loaded the raw dataset (`messy_customer_sales_data.csv`) and inspected data types and non-null counts via `df.info()`.
* Generated statistical summaries using `df.describe()` to spot anomalies (e.g., negative purchase amounts and extreme maximum values).
* Quantified missing values across all columns using `df.isnull().sum()`.
* Checked and isolated duplicate rows to understand duplication patterns.
* Audited low-cardinality categorical columns (`Gender`, `City`, `Feedback_Score`, `Country`) using frequency distributions (`value_counts()`).

### 2. Handling Missing Values & Data Hygiene
* **Primary Key Filtering:** Dropped rows where the critical identifier `Customer_ID` was missing.
* **Regex-Based String Cleaning (`Age` column):** 
  * Addressed messy string representations (e.g., `"51.0 years"`, `"-10"`, `"250"`) by extracting numerical digits using Regular Expressions (`re.findall`).
  * Calculated the integer median age (`43`) from valid entries, replacing erroneous string formats and missing values.
* **Imputation Strategy:**
  * Imputed missing `Purchase_Amount` values with the column median.
  * Imputed missing `Feedback_Score` and categorical features (`Gender`, `City`, `Country`) with their respective modes.
  * Applied forward fill (`ffill`) on `Last_Purchase_Date`.

### 3. Standardizing Inconsistent Categorical Formatting
* **Gender:** Converted strings to lowercase, stripped whitespaces, and mapped abbreviations (`'m'`/`'f'` to `'male'`/`'female'`).
* **City:** Strip-cleaned whitespace irregularities and standardized casing using `.str.title()` (e.g., transforming `"kolkata "`, `"KOLKATA"`, and `"kolkata"` into uniform title case).
* **Country:** Standardized variations like `"IND"`, `"india"`, and `"InDia"` into a clean, unified format (`"India"`).

### 4. Deduplication & Data Type Casting
* Dropped complete duplicate rows and filtered out duplicate `Customer_ID` entries keeping the first occurrence (`keep='first'`).
* Corrected column data types:
  * Cast `Signup_Date` and `Last_Purchase_Date` to `datetime64[ns]`.
  * Cast `Age` to `int64`.

### 5. Outlier Detection & Treatment
* Visualized feature distributions using Seaborn boxplots for `Age`, `Purchase_Amount`, and `Feedback_Score`.
* Calculated **Z-scores** via `scipy.stats.zscore` on numerical features (`Age`, `Purchase_Amount`).
* Filtered out extreme statistical outliers where the absolute Z-score exceeded 3 (`|z| > 3`), refining the dataset down to 8,989 clean records.
* Re-generated boxplots post-cleaning to verify outlier removal.

---

## 📊 Summary of Transformations

| Column | Issue Identified | Cleaning / Transformation Applied |
| :--- | :--- | :--- |
| **Customer_ID** | Missing values (1,023 rows) | Dropped rows with missing IDs |
| **Age** | Mixed types, strings (`"51.0 years"`), outliers | Extracted digits via Regex, imputed median (`43`), cast to `int64` |
| **Gender** | Varied casing (`M`, `m`, `FEMALE`) | Lowercased, stripped, mapped to `Male`/`Female`, imputed mode |
| **City** | Inconsistent casing & whitespace | Strip-cleaned and converted to Title Case |
| **Country** | Variations (`IND`, `india`, `InDia`) | Standardized to `"India"` |
| **Dates** | Object dtype | Converted to `datetime64[ns]` with forward-fill for missing values |
| **Purchase_Amount** | Skewed values & outliers | Imputed median, removed extremes via Z-score filter (`|z| > 3`) |

---

## 🚀 How to Run the Code
1. Clone the repository:
   ```bash
   git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
