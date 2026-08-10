# ⚽ UEFA Champions League Analysis & Cleaning Pipeline

Hey there! 👋 Welcome to my end-to-end Python data engineering and exploratory data analysis project. This project transforms messy, raw historical UEFA Champions League (UCL) performance statistics and finals match records (1955–2023) into clean, structured data for analytics and visual dashboards. The repository features modular Jupyter Notebook workflows covering automated data quality profiling, missing value interpolation, string parsing, feature engineering, and statistical analysis.

---

## 📁 Repository Structure

```text
.
├── data/
│   ├── UCL_AllTime_Performance_Table.csv # Raw all-time historical performance table
│   ├── UCL_Finals_1955-2023.csv          # Raw records of all UCL finals (1955–2023)
│   ├── UCL_Cleaned_AllTime.csv           # Cleaned & transformed all-time performance dataset
│   └── UCL_Cleaned_Finals.csv            # Cleaned & transformed finals dataset
├── notebooks/
│   └── UCL_Data_Analysis.ipynb           # End-to-end pandas cleaning & EDA notebook
└── README.md                             # Project documentation
```

🛠️ How the Pipeline Works
The data engineering workflow is structured into four sequential phases inside UCL_Data_Analysis.ipynb:

┌───────────────────────────┐     ┌───────────────────────────┐
│ 1. Data Assessment        │ ──> │ 2. Cleaning & Transformation│
│ Profiling & Anomaly Audit │     │ String Parsing & Renaming │
└───────────────────────────┘     └───────────────────────────┘
                                                │
                                                ▼
┌───────────────────────────┐     ┌───────────────────────────┐
│ 4. Exploratory Analysis   │ <── │ 3. Feature Engineering    │
│ Statistical EDA & Insights│     │ Deduplication & Type Cast │
└───────────────────────────┘     └───────────────────────────┘
