⚽ UEFA Champions League Analysis & Cleaning Pipeline

Hey there! 👋 Welcome to my end-to-end Python data engineering and exploratory data analysis project. This project transforms messy, raw historical UEFA Champions League (UCL) performance statistics and finals match records (1955–2023) into clean, structured data for analytics and visual dashboards.
The repository features modular Jupyter Notebook workflows covering automated data quality profiling, missing value interpolation, string parsing, feature engineering, and statistical analysis.

📁 Repository Structure
Plaintext
.
├── data/
│   ├── UCL_AllTime_Performance_Table.csv  # Raw all-time historical performance table
│   ├── UCL_Finals_1955-2023.csv           # Raw records of all UCL finals (1955–2023)
│   ├── UCL_Cleaned_AllTime.csv            # Cleaned & transformed all-time performance dataset
│   └── UCL_Cleaned_Finals.csv             # Cleaned & transformed finals dataset
├── notebooks/
│   └── UCL_Data_Analysis.ipynb            # End-to-end pandas cleaning & EDA notebook
└── README.md                              # Project documentation

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

1️⃣ Assessment & Profiling 
-> Identifies 21 missing values in rank attributes and 51 unrecorded notes in finals listings. 
-> Uncovers non-standard string formats in composite fields (goals stored as scored:conceded, Score as Winner–RunnerUp). 
-> Detects formatting anomalies in string numeric values (e.g., Attendance containing comma separators). 

2️⃣ Cleaning & Data Transformation
-> Missing Value Imputation: Applies forward linear interpolation to restore missing tournament rank order and imputes missing match condition notes. 
-> Header Standardization: Renames abbreviated and non-standard column titles (# $\rightarrow$ Rank, M. $\rightarrow$ Matches, Dif $\rightarrow$ Goal_Difference, Pt. $\rightarrow$ Points). 
-> Composite Column Extraction: Splits composite score strings into discrete numerical metrics for goals scored and conceded.

# Linear interpolation for missing team ranks
df_alltime['Rank'] = df_alltime['#'].interpolate(method='linear', limit_direction='forward').astype(int)

# Extract composite goal statistics into numeric features
df_alltime['Goals_Scored'] = df_alltime['goals'].str.split(':').str.get(0).astype(int)
df_alltime['Goals_Conceded'] = df_alltime['goals'].str.split(':').str.get(1).astype(int)
df_alltime.drop(columns=['goals'], inplace=True)

3️⃣ Feature Engineering & Validation
-> Type Casting & Standardization: Converts formatted attendance values into standard numeric integer types. 
-> Deduplication: Handles historical edge cases, such as removing duplicate match replay records (e.g., 1973–74 Bayern Munich vs. Atlético Madrid replay).  

# Clean attendance figures and cast to integer
df_finals['Attendance'] = df_finals['Attendance'].astype(str).str.replace(',', '').astype(int)

# Deduplicate replay records by keeping primary final fixture
df_finals = df_finals.drop_duplicates(subset=['Season'], keep='first')

4️⃣ Exploratory Data Analysis & Insights
-> Historical Dominance: Computes total win-loss ratios and goal differentials across all participating European clubs.  Finals Efficiency: -> Evaluates win-to-appearance ratios in finals for top-performing teams. 

# Top 5 clubs by overall win percentage (min. 50 matches)
df_alltime['Win_Rate'] = (df_alltime['Wins'] / df_alltime['Matches']) * 100
top_clubs = df_alltime[df_alltime['Matches'] >= 50].sort_values(by='Win_Rate', ascending=False).head(5)

📊 Datasets & SchemaDatasetRecordsDescriptionCleaned Primary KeyKey Engineered FeaturesUCL_AllTime_Performance_Table.csv354Club-level all-time competition statistics  RankGoals_Scored, Goals_Conceded, Win_RateUCL_Finals_1955-2023.csv69Individual final match statistics (1955–2023)  SeasonWinner_Goals, Runnersup_Goals, Attendance

💡 Key Insights Discovered
-> Dominant Force: Real Madrid leads all-time historical metrics in total match victories, goals scored, and overall finals conversion rate.

🚀 How to Run
1. Clone the Repository
Bash
git clone https://github.com/your-username/ucl-data-cleaning-eda.git
cd ucl-data-cleaning-eda
2. Install Dependencies
Bash
pip install pandas numpy matplotlib seaborn openpyxl jupyter
3. Launch Notebook Pipeline
Bash
jupyter notebook notebooks/UCL_Data_Analysis.ipynb
Run all cells sequentially to execute the cleaning pipeline, visual analysis, and report generation.

🧰 Tech Stack
Language: Python 3.8+
Data Manipulation: pandas, numpy
Visualization: matplotlib

Thanks for checking out my project! Feel free to star ⭐️ the repository if you found it useful!
