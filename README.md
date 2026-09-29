# 📈 Egyptian Stock Market — Data Science & 4-Week Return Prediction

A collaborative Data Science and Machine Learning project focused on analyzing weekly stock market data from **50 Egyptian companies**, building a reliable data preprocessing pipeline, engineering financial features, predicting **4-week forward returns**, and using the predictions to rank stocks and simulate an investment portfolio.

---

## 🎯 Project Objective

The main objective of this project is to build an end-to-end Data Science pipeline that can:

* Clean and validate historical stock market data.
* Analyze weekly price and trading-volume behavior.
* Engineer meaningful financial and time-series features.
* Predict the expected **4-week forward return** of each company.
* Rank companies according to their predicted returns.
* Identify the **Top 5** and **Bottom 5** predicted stocks.
* Simulate an investment portfolio of **100,000 EGP** distributed equally among the Top 5 companies over a 4-week holding period.

---

## 📊 Dataset

The dataset consists of weekly stock market data for **50 Egyptian companies**, covering approximately **2017–2026**.

### Dataset Statistics

| Property      |                   Value |
| ------------- | ----------------------: |
| Companies     |                      50 |
| Total Rows    |                  23,823 |
| Frequency     |                  Weekly |
| Time Period   |               2017–2026 |
| Source Format | 50 individual CSV files |

Each company initially had its own CSV file, which was merged into a unified dataset for processing and analysis.

### Raw Features

* `Company` — Company name
* `Date` — Weekly trading date
* `Price` — Closing price
* `Open` — Opening price
* `High` — Highest weekly price
* `Low` — Lowest weekly price
* `Vol.` — Trading volume
* `Change %` — Weekly percentage change

---

# 🧹 Data Preprocessing

The preprocessing pipeline was developed to transform the raw stock data into a clean and reliable dataset suitable for time-series analysis and machine learning.

### 1. Data Type Conversion

* Removed unnecessary spaces from column names.
* Converted `Date` into a proper datetime format.
* Converted OHLC price columns into numeric values.
* Converted `Change %` from strings to numerical percentages.
* Parsed trading-volume values containing `K`, `M`, and `B` suffixes into numerical values.

### 2. Deduplication & Sorting

* Removed exact duplicate rows.
* Sorted the dataset by `Company` and chronological `Date`.
* Preserved time-series continuity for each company.

### 3. Missing Value Handling

Missing trading-volume values were handled separately for each company using:

* Linear interpolation
* Forward fill (`ffill`)
* Backward fill (`bfill`)

### 4. OHLC Data Validation

The dataset was checked for structural inconsistencies such as:

* `High < Low`
* `Open > High`
* `Price > High`
* `Price < Low`

Suspicious values were corrected using logical OHLC constraints.

For example:

* Inverted `High` and `Low` values were swapped when necessary.
* `High` was adjusted to include the maximum of `High`, `Open`, and `Price`.
* `Low` was adjusted to include the minimum of `Low`, `Open`, and `Price`.

### ✅ Data Quality Improvements

| Quality Check        | Before | After |
| -------------------- | -----: | ----: |
| Missing Values       |      5 |     0 |
| Duplicate Rows       |      — |     0 |
| Suspicious OHLC Rows |    386 |     0 |

---

# ⚙️ Feature Engineering

After preprocessing, several time-series and financial features were engineered to capture historical price behavior and volatility.

### 🎯 Target Variable

**`Target_4W_Return`**

The target represents the percentage return expected over the following **4 weeks**.

### 📈 Return Features

* `Return_1W`
* `Return_2W`
* `Return_4W`
* `Return_8W`

### 📊 Volatility Features

* `Volatility_4W`
* `Volatility_8W`

Calculated using rolling standard deviation of historical returns.

### 📉 Price-Based Features

* `Range`
* `Price_to_MA_4W`
* `Price_to_MA_8W`
* `Price_Position_8W`

### 📦 Volume Features

* `Volume_Change_1W`
* `Volume_to_AvgVol_4W`

All lagged and rolling features were calculated **per company using historical information only** to reduce the risk of data leakage.

---

# 🤖 Machine Learning

The prediction task is formulated as a **Regression problem**, where the model predicts the future 4-week return:

```text
Historical Stock Data
        ↓
Data Cleaning
        ↓
Feature Engineering
        ↓
Time-Based Data Split
        ↓
Machine Learning Model
        ↓
Predicted 4-Week Return
        ↓
Stock Ranking
        ↓
Portfolio Simulation
```

The project also uses the regression predictions for a **Learning-to-Rank style stock selection process**, ranking companies according to their predicted future returns.

---

# ⏳ Time-Based Data Splitting

Because this is a financial time-series problem, the data was split chronologically rather than randomly.

| Dataset    | Percentage |
| ---------- | ---------: |
| Training   |        70% |
| Validation |        15% |
| Testing    |        15% |

No random shuffling was used in order to avoid introducing future information into the training process and to better reflect a real-world forecasting scenario.

---

# 🏆 Stock Ranking

After generating predictions, companies are ranked according to their predicted 4-week returns.

### Top 5

The five companies with the highest predicted returns are selected.

### Bottom 5

The five companies with the lowest predicted returns are identified.

This ranking provides a practical way to translate regression predictions into a stock-selection strategy.

---

# 💰 Portfolio Backtest

A simple portfolio simulation was designed using an initial capital of:

**100,000 EGP**

The investment is divided equally among the Top 5 predicted companies:

```text
Initial Capital: 100,000 EGP

Company 1 → 20,000 EGP
Company 2 → 20,000 EGP
Company 3 → 20,000 EGP
Company 4 → 20,000 EGP
Company 5 → 20,000 EGP
```

The portfolio is evaluated over a **4-week holding period** based on the realized returns.

> ⚠️ This backtest is intended for educational and analytical purposes only and does not constitute financial advice or a recommendation to buy or sell any security.

---

# 📁 Project Structure

```text
Final_Project_Data_Science-main/
│
├── Data_Set_Weekly/
│   └── 50 weekly stock CSV files
│
├── all_companies.csv
│   └── Raw merged dataset
│
├── all_companies_cleaned.csv
│   └── Cleaned dataset ready for analysis/modeling
│
├── merge_and_prepare_data.py
│   └── Automated data cleaning & quality pipeline
│
├── Data_Preprocessing.py
│   └── Data preprocessing & visual inspection
│
├── plots/
│   ├── A_missing_values.png
│   ├── B_rows_per_company.png
│   ├── C_change_pct_distribution.png
│   ├── D_suspicious_rows.png
│   └── E_quality_before_after.png
│
└── README.md
```

---

# 🛠️ Technologies Used

* **Python**
* **Pandas**
* **NumPy**
* **Scikit-learn**
* **Matplotlib**
* **Seaborn**
* **Jupyter Notebook**

---

# 👥 Team Contributions

This project was developed collaboratively by **Ali Mohamed Sheta** and **Eslam-Alrifay**.

### 👨‍💻 Ali Mohamed Sheta

**Data Preprocessing & Feature Engineering**

Responsible for the data preparation stage, including:

* Data cleaning
* Data type conversion
* Missing value handling
* Duplicate detection and removal
* OHLC data validation and correction
* Time-series sorting
* Feature engineering
* Preparing the final dataset for Machine Learning

### 👨‍💻 Eslam-Alrifay

**Machine Learning & Model Development**

Responsible for the modeling stage, including:

* Machine Learning model development
* Model training
* Model validation
* Model evaluation
* Prediction generation
* Stock ranking
* Portfolio simulation

### 🤝 Collaboration

The project was developed collaboratively by [Ali Mohamed Sheta](https://github.com/alisheta550) and [Eslam Alrifay](https://github.com/Eslam-Alrifay) as an end-to-end Data Science and Machine Learning workflow.

**Ali Mohamed Sheta** was mainly responsible for:

* Data cleaning and preprocessing.
* Data quality validation and handling inconsistencies.
* Feature engineering and preparation of modeling features.

**Eslam Alrifay** was mainly responsible for:

* Model development.
* Model training and evaluation.
* Prediction and ranking pipeline.

Both team members contributed to reviewing the overall pipeline and discussing the modeling results. The current model provides a working baseline, and we are planning to further improve and optimize its performance in the next development phase by experimenting with different modeling approaches, features, and tuning strategies.


---

# 🚀 Key Learning Outcomes

Through this project, we applied practical concepts including:

* Real-world data cleaning
* Time-series data preprocessing
* Financial data analysis
* Missing value imputation
* Data quality validation
* Feature engineering
* Time-based train/validation/test splitting
* Machine Learning regression
* Prediction-based ranking
* Portfolio backtesting
* Avoiding data leakage in time-series problems

---

## 📌 Disclaimer

This project is an educational Data Science and Machine Learning project. The predictions and portfolio simulation are based on historical data and should **not** be considered financial advice or guaranteed future performance.
