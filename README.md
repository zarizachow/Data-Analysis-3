# Building a prediction model on house prices
Data Analysis 3 - Assignment 1  
Submitted by: Zariza Chowdhury (ID: 2500086)    
Deadline: 2 February 2026

---

## Business Case

My business case is to operate a chain of Airbnbs.      
The task is to build a pricing model.

---

## Data Sources

All datasets are sourced from **Inside Airbnb**:  
https://insideairbnb.com/get-the-data/

### Datasets Used

| Dataset | City | Time Period | Purpose |
|------|------|-----------|--------|
| Base dataset | Tokyo, Japan | Q4 2024 | Model development |
| Live dataset 1 | Tokyo, Japan | Q3 2025 | Time Period validity |
| Live dataset 2 | Hong Kong, China | Latest available | Geographic validity |

All raw and cleaned datasets are stored in the GitHub repository and loaded via raw URLs to ensure full reproducibility.

## Cleaned Datasets
- 

---

## Part I. Modelling

### Step 0: Setup

- Import core Python libraries (`numpy`, `pandas`, `sklearn`, etc)
- Use pipelines for preprocessing and modelling
- Set a fixed random seed for reproducibility

---

### Step 1: Data Selection, Wrangling, and Feature Engineering

##### 1.A. Dataset Selection:
- **Source**: Inside Airbnb - *https://insideairbnb.com/get-the-data/*
- **Dataset**: *listings.csv* (loaded directly from GitHub Repo)
- **City, Country**: Tokyo, Japan
- **Time Period**: Q4 2024
- **Reproducibility**: Data is uploaded and stored in a public GitHub repo and loaded directly via a raw URL

---

#### 1.B. Data Wrangling and Feature Engineering
- Impute missing numeric values using medians
- Impute missing categorical values using `"missing"`
- Clean and convert the target variable (`price`) to numeric
- Winsorize numeric variables (1st–99th percentile)
- Drop columns with all missing values and system-generated IDs
- Extract amenities into binary indicators:
  Wifi, Kitchen, Air conditioning, Heating, Washer, Dryer, Elevator, TV
- Drop free-text, URL, and date fields
- Keep structured listing, host, location, availability, and amenity variables

All cleaning and feature engineering steps are applied consistently across datasets.

**Variable Selection for Modelling**

- Exclude id, URLs, dates, and free-text fields that are not useful for prediction  
- Keep structured listing, host, location, and amenity variables  
- Use the same variables across all datasets for out-of-sample comparison

---

### Step 2: Building Predictive Models

Five models are estimated using the same train–test split and evaluation metric (RMSE):

1. **OLS (Linear Regression)** – baseline model  
2. **LASSO** – regularized linear model  
3. **Random Forest** – ensemble tree-based model  
4. **Gradient Boosting** – boosting model  
5. **Decision Tree (CART)** – simple tree baseline  

Categorical variables are encoded inside pipelines using one-hot encoding.

---

### Step 3: Model Comparison Terms of Fit and Time (Base Dataset: Tokyo Q4 2024)

#### Horserace Table

| Model | RMSE (Test Set) | Runtime (seconds) |
|------|---------------|------------------|
| OLS | 12,740.71 | 4.5 |
| LASSO | 15,064.24 | 1.7 |
| Random Forest | 13,867.29 | 0.9 |
| Gradient Boosting | 14,187.45 | 10.3 |
| Decision Tree (CART) | 18,161.32 | 0.8 |

#### Discussion of Performance

- **OLS** performs strongly for a simple baseline, with low RMSE and low runtime.
- **LASSO** performs worse than OLS, suggesting regularization does not improve prediction here.
- **Random Forest** improves over LASSO and CART but is more computationally expensive.
- **Gradient Boosting** performs similarly to Random Forest but does not outperform OLS.
- **Decision Tree (CART)** performs the worst due to overfitting and limited generalization.

Overall, **OLS provides a strong and efficient baseline**, while more complex models add limited gains.

---

### Step 4: Analyzing Random Forest Model and Gradient Boosting Model

#### A. Random Forest – Feature Importance

Top drivers of price prediction include:
- Capacity (`accommodates`)
- Property size (`beds`, `bedrooms`, `bathrooms`)
- Location (`latitude`, `longitude`)
- Availability and host-related variables

#### B. Gradient Boosting – Feature Importance

Key drivers include:
- Capacity and property size
- Location and neighbourhood indicators
- Availability variables

#### C. Comparison of Feature Importance

- Both models emphasize **capacity and location** as main price drivers.
- Random Forest places more weight on availability and host variables.
- Gradient Boosting places more weight on neighbourhood and size variables.

---

## Part II. Validity

This section tests how well the models perform on **new (“live”) datasets**.

Two additional datasets are used:
- A later time period for Tokyo (Q3 2025)
- A different city from the same region (Hong Kong)

The same data wrangling steps and the same model specifications from Part I are applied.

---

### Step 5: Adding Live Datasets

#### A. Later Date: Tokyo (Q3 2025)

- **City, Country**: Tokyo, Japan  
- **Time Period**: Q3 2025  
- **Purpose**: Test temporal validity  

The dataset contains 27,945 listings and is cleaned using the same pipeline as the base dataset.

#### B. Another City in the Same Region: Hong Kong

- **City, Country**: Hong Kong, China  
- **Time Period**: Latest available  
- **Purpose**: Test geographic validity  

**Reasons for choosing Hong Kong**:
- Same broader region (East Asia)
- Highly urban and dense housing market
- Similar short-term rental dynamics

---

### Step 6: Model Performance on Live Datasets

#### A. Tokyo Q3 2025 – Horserace Table

| Model | RMSE | Runtime (seconds) |
|------|------|------------------|
| OLS | 11,416.21 | 6.97 |
| LASSO | 12,501.68 | 14.15 |
| Random Forest | 16,035.61 | 1.31 |
| Gradient Boosting | 10,117.58 | 27.15 |
| Decision Tree (CART) | 10,271.04 | 28.28 |

**Key insight**: Model rankings change over time, and more complex models do not always outperform simpler ones.

---

#### B. Hong Kong – Horserace Table

| Model | RMSE | Runtime (seconds) |
|------|------|------------------|
| OLS | 455.96 | 1.38 |
| LASSO | 546.48 | 0.61 |
| Random Forest | 636.64 | 0.43 |
| Gradient Boosting | 466.00 | 5.14 |
| Decision Tree (CART) | 499.91 | 1.92 |

**Key insight**: OLS and Gradient Boosting generalize better across cities than Random Forest.

---

## 5. Practical Modelling Experience

### Lessons Learned from Running Models on Live Data

- Random Forest models were extremely sensitive to dataset size
- Initial models with `n_estimators = 500` were not runnable on live datasets
- To ensure consistency, the Random Forest model was revised to use `n_estimators = 100`
- This increased RMSE slightly but made the model usable across datasets
- No changes were required for the other models

Key learning points:
- Runtime and scalability matter as much as accuracy
- Horserace tables are essential for model comparison
- A clear notebook structure makes iterative changes easier
- Well-documented pipelines save significant debugging time

---

## 6. Conclusion

- Model performance varies across time and geography
- More complex models do not automatically generalize better
- **OLS provides a strong, stable, and efficient baseline**
- Ensemble models add complexity with mixed gains
- Validity checks are essential before deploying pricing models

---

## 7. Reproducibility

- All data is stored in the repository or loaded via raw GitHub URLs
- All preprocessing is performed inside pipelines
- The project can be run end-to-end without manual intervention