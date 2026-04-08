# Telco Customer Churn Prediction

## Project Objective

The goal of this project is to identify telecom customers likely to churn before they actually leave, enabling the commercial team to intervene proactively with targeted retention offers. Specifically, the project aims to:

- Build and compare multiple classification models to find the best-performing one on the churn problem
- Handle class imbalance, as churners represent only 26.58% of the dataset
- Optimise the model for Recall, prioritising the capture of as many at-risk customers as possible
- Provide an interpretable output usable by a business team

---

## Dataset

- **Source**: Telco Customer Churn dataset (`tcc.csv`) originally sourced from [Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)
- **Shape**: 7,043 rows x 21 columns (7,032 after removing 11 rows with tenure = 0)
- **Target variable**: `Churn` (binary: Yes / No) — churn rate: 26.58%
- **Feature categories**: Demographics (`gender`, `SeniorCitizen`, `Partner`, `Dependents`), Account info (`tenure`, `Contract`, `PaymentMethod`, `MonthlyCharges`, `TotalCharges`), Services (`PhoneService`, `InternetService`, `StreamingTV`, etc.)

---

## Methodology

### 1. Exploratory Data Analysis
Missing values, duplicates, outliers (IQR method), and data types were checked. `TotalCharges` was converted from string to float. 11 rows with `tenure = 0` were removed as they represent newly acquired customers with no billing history.

### 2. Preprocessing & Feature Engineering
- Correlation analysis: `TotalCharges` is highly correlated with `tenure` (0.83) and `MonthlyCharges` (0.65), multicollinearity confirmed via VIF and was dropped
- Categorical variables encoded via One-Hot Encoding (patsy formula interface for Logistic Regression; `LabelEncoder` + manual dummies for tree-based models)
- Redundant "No internet service" dummy columns dropped to avoid multicollinearity
- `StandardScaler` applied to numerical features (`tenure`, `MonthlyCharges`)
- Final feature matrix: 22 columns after encoding and cleaning
- Train/test split: 80/20 with `stratify=y` to preserve the original 26% churn rate in both sets

### 3. Models Trained

| Model | Accuracy | Recall | ROC-AUC |
|---|---|---|---|
| Logistic Regression | 0.80 | 0.57 | 0.8336 |
| Random Forest | — | — | ~0.81 |
| XGBoost (default) | 0.73 | 0.78 | 0.8328 |
| XGBoost (tuned) | 0.73 | **0.82** | **0.8411** |

### 4. Hyperparameter Tuning
- Framework: Optuna (Bayesian optimisation)
- 50 trials, 5-fold StratifiedKFold, optimising ROC-AUC
- Search space: `n_estimators` (100–500), `max_depth` (3–8), `learning_rate`, `subsample`, `colsample_bytree`, `reg_alpha`, `reg_lambda`

### 5. Model Interpretability

Two complementary techniques were applied to understand what drives the model's predictions:

**Feature Importance (XGBoost gain):** measures how much each feature contributes to reducing impurity across all trees. Top features by importance score: `Contract_Two year`, `tenure`, `InternetService_Fiber optic`, `Contract_One year`, `PaymentMethod_Electronic check`.

**SHAP Values (TreeExplainer):** SHAP (SHapley Additive exPlanations) assigns each feature a contribution value for every individual prediction, showing both the magnitude and direction of the effect. Key findings:
- `Contract_Two year` - high values strongly push predictions toward no churn (negative SHAP)
- `tenure` — longer tenure consistently reduces churn probability
- `InternetService_Fiber optic` strongly increases churn risk (positive SHAP)
- `PaymentMethod_Electronic check` associated with higher churn probability
- `InternetService_No` - absence of internet service reduces churn risk

---

## Results

- **Recall**: 0.8182, 82% of actual churners correctly identified
- **ROC-AUC**: 0.8411
- **Confusion matrix**: 720 TN · 306 TP · 313 FP · 68 FN

### Business Impact (ROI Estimate)
Assumptions: avg. monthly revenue $65, avg. customer lifetime 24 months, retention rate of campaign 30%, cost per contact $20.

| Metric | Value |
|---|---|
| Churners caught by model | 306 / 374 |
| Total revenue at risk | $583,440 |
| Revenue recoverable (30% retained) | $143,208 |
| Campaign cost | $6,120 |
| **Net savings** | **$137,088** |

---

## Tech Stack

Python, pandas, numpy, matplotlib, seaborn, scikit-learn, statsmodels/patsy, XGBoost, Optuna, SHAP

---

## Files

| File | Description |
|---|---|
| `telco_customer_churn_1.ipynb` | Full analysis: EDA, modelling, tuning, SHAP |
| `report.pdf` | Business report with findings and ROI |
| `telco_customer_churn_ppt.pptx` | Presentation slides |
