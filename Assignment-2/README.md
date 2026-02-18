# Predicting Fast-Growing Firms (2025)
**Data Analysis 3 - Assignment 2**

**Submitted by:**
- Balint Decsi (ID: 2506626)
- Zariza Chowdhury (ID: 2500086)

## Project Overview
The objective of this assignment is to build and evaluate predictive models for identifying fast-growing firms using the Bisnode firms panel data. The analysis focuses on probability prediction, classification under an explicit loss function, and comparison of model performance across industries (Manufacturing vs. Services).

The core definition of "fast growth" is firms achieving the top 10% of sales growth year-over-year (2013 vs 2012).

## Repository Structure

```
Assignment-2/
├── Data/
│   ├── Raw/                # Contains original input data (cs_bisnode_panel.csv)
│   └── Cleaned/            # Contains processed modeling data
├── Output/
│   ├── Plots/              # Generated figures (distributions, ROC curves, confusion matrices)
│   └── Tables/             # CSVs with evaluation metrics
├── chowdhury_decsi_assignment_2_data_prep.ipynb  # Data cleaning & feature engineering
├── chowdhury_decsi_assignment_2_modeling.ipynb   # Model training, evaluation & results
├── requirements.txt        # Python dependencies
└── README.md               # This file
```

## Setup & Usage

### Prerequisites
- Python 3.10+
- `uv` (recommended) or `pip`

### Installation

1. Navigate to the assignment directory:
   ```bash
   cd Assignment-2
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   # OR if using uv
   uv pip install -r requirements.txt
   ```

### Running the Analysis

The analysis is split into two Jupyter notebooks. Run them in the following order:

1.  **Data Preparation:**
    Open and run `chowdhury_decsi_assignment_2_data_prep.ipynb`.
    -   This notebook loads the raw data, handles missing values, engineers features, defines the target variable, and saves the clean dataset to `Data/Cleaned/bisnode_firms_clean.csv`.

2.  **Modeling & Evaluation:**
    Open and run `chowdhury_decsi_assignment_2_modeling.ipynb`.
    -   This notebook loads the cleaned data.
    -   Trains Logit and Random Forest models.
    -   Evaluates performance using Cross-Validation (AUC, Brier Score).
    -   Performs classification based on a business loss function (Cost of False Negative = 4x Cost of False Positive).
    -   Analyzes performance across Industry sectors.
    -   Outputs tables and plots to the `Output/` folder.

## Reports
Two PDF reports summarizing the findings are generated in the root directory:
-   `summary_report.pdf`: Executive summary for management.
-   `technical_report.pdf`: Detailed technical methodology and results.
