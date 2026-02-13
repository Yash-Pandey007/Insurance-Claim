# Automated Loss Data Analytics & Risk Benchmarking Tool

## Project Overview
This project is an end-to-end **Python-based analytical workflow** designed to ingest raw insurance loss data, automate data cleansing, and generate risk benchmarking reports.

Developed to simulate the **Core Analytics** function, this tool moves beyond simple prediction by calculating a **Frequency Index** against industry benchmarks and generating standardized **Excel outputs** suitable for downstream reporting (Power BI/Dashboarding).

## Key Features aligned with Core Analytics

### 1. Automated Data Ingestion & Cleansing
*   **Ingestion:** Reads raw customer loss data (`.csv`) simulating client loss runs.
*   **Data Quality Checks:** Automates the identification of missing values in critical fields (`credit_score`, `annual_mileage`).
*   **Standardization:** Implements imputations based on median distributions to ensure dataset completeness, adhering to standard operating procedures (SOPs) for data preparation.

### 2. Risk Behavior Scoring (Feature Engineering)
*   Translates raw violation data into a weighted **Risk Behavior Score**.
*   **Logic:** Assigns severity weights to distinct risk factors (e.g., Speeding: 1.5x, DUIs: 3.0x, Past Accidents: 2.0x) to quantify policyholder exposure.

### 3. Actuarial-Style Benchmarking
*   Calculates the portfolio's **Actual Claim Rate**.
*   Compares performance against a **Benchmark Rate (0.25)** to derive a **Frequency Index** (e.g., 1.25), allowing for immediate identification of underperforming segments.

### 4. Hybrid Modeling Approach
*   **Logistic Regression (Statsmodels):** Used for **interpretability**—identifying statistically significant drivers of loss (coefficients and p-values).
*   **Random Forest (Scikit-Learn):** Used for **predictive performance**—capturing non-linear relationships to maximize AUC (0.90+).

### 5. Automated Reporting & Export
*   Generates a multi-tabbed **Excel Report** (`insurance_model_output.xlsx`) containing:
    *   **Sheet 1 (Scored Data):** Policy-level predictions and risk scores.
    *   **Sheet 2 (Summary):** High-level model metrics and Benchmarking KPIs.
*   *Note: This output structure is designed for direct ingestion into Power BI or Tableau.*

---

## Technical Stack
*   **Language:** Python 3.x
*   **Data Manipulation:** Pandas, NumPy
*   **Statistical Modeling:** Statsmodels (Logit), Scikit-Learn (Random Forest)
*   **Reporting:** Matplotlib (Visualization), OpenPyXL (Excel Export)

---

## Installation & Usage

### 1. Prerequisites
Ensure you have the necessary libraries installed:
```bash
pip install pandas numpy statsmodels scikit-learn matplotlib openpyxl
```

### 2. Configuration
The script utilizes a configuration block at the top to easily adjust parameters without altering the core logic—useful for analyzing different Lines of Business (LoB).
```python
# CONFIGURATION
TEST_SIZE = 0.3
BENCHMARK_CLAIM_RATE = 0.25  # Can be adjusted based on industry standards
```

### 3. Running the Analysis
Execute the script to process data and generate reports:
```bash
python risk_benchmark_model.py
```

---

## Workflow Logic

1.  **`load_data()`**: Ingests the raw loss run file.
2.  **`preprocess_data()`**:
    *   Handles missing data (Imputation).
    *   Calculates `risk_behavior_score`.
    *   Encodes categorical variables (One-Hot Encoding).
3.  **`split_data()`**: Segregates data into training and testing sets to validate model performance.
4.  **`train_logit()` & `train_rf()`**: Fits models to identify risk drivers and predict outcomes.
5.  **Benchmarking**: Calculates the `Frequency Index` (`Actual Rate / Benchmark Rate`).
6.  **`export_results()`**: Compiles all findings into a structured Excel workbook.

---

## Business Impact & Results

*   **Efficiency:** Reduces manual data preparation time by automating the cleansing and scoring of thousands of records.
*   **Insight:** The **Frequency Index of 1.25** indicates the analyzed portfolio is performing 25% worse than the standard benchmark, signalling a need for underwriting review.
*   **Deliverable:** The automated Excel export allows consultants and stakeholders to review model outputs without needing to interact with the Python code.

---

## Future Scope (Power BI Integration)
To align with **WTW's visualization requirements**:
1.  The Excel output from this script is structured to serve as a direct data source for **Microsoft Power BI**.
2.  Future enhancements include automating the refresh of Power BI datasets using Python scripts within the Power BI environment.
