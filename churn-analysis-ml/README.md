# 🤖 Churn Analysis – Machine Learning (Telco)

## Overview
Built a machine learning classification model to identify telecom customers at risk of churning, enabling proactive retention strategies and reducing customer loss.

## Tools & Technologies
- **Python** – end-to-end ML pipeline
- **Scikit-Learn** – model training, evaluation, and tuning
- **Pandas** – data preprocessing and feature engineering
- **Matplotlib** – result visualisation and reporting

## Model Performance
| Metric | Score |
|---|---|
| ROC-AUC | 0.841 |
| Recall (churners) | 81.8% |
| Customers analysed | 7,032 |
| Features used | 18 |

## Key Risk Factors Identified
- Month-to-month contracts (vs. long-term)
- Fibre optic service subscribers
- High monthly charges

## Methodology
1. Exploratory Data Analysis (EDA) and data cleaning
2. Feature engineering across 18 customer attributes
3. Model training and cross-validation
4. Hyperparameter tuning for optimal recall on churners
5. Business interpretation of results for retention strategy
