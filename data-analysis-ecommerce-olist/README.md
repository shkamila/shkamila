# Olist E-commerce Data Analysis

## Project Objective

The main objective of this analysis is to transform raw transactional and logistical data from the Brazilian e-commerce platform Olist into actionable strategic insights. The project answers three key business questions:

1. What are the real revenue drivers, and how do sales volumes react to price changes across different product categories?
2. How do delivery times directly influence customer satisfaction and the probability of repurchase?
3. In which geographic areas are logistical inefficiencies most compromising the customer experience?

The final output is a data-driven action plan aimed at optimising revenue, recalibrating pricing, mitigating logistical risks, and increasing customer satisfaction.

---

## Dataset

- **Source**: Brazilian E-commerce Public Dataset by Olist — sourced from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- **Tables used**: Orders, Customers, Items, Products, Reviews

| Table | Rows | Columns |
|---|---|---|
| Orders | 110,197 | 8 |
| Customers | 99,441 | 5 |
| Items | 112,650 | 7 |
| Products | 32,951 | 9 |
| Reviews | 99,224 | 7 |

- **Analysis scope**: Only `delivered` orders, years 2017-2018 (2016 excluded — only 2 months of data)
- **Final working dataset**: 38,155 orders x 6 columns after aggregation and merge

---

## Methodology

### 1. Data Quality Assessment
Each table was profiled for dimensions, missing values, duplicates, data types, and outliers (IQR method). Key findings:
- `review_comment_message` (~59% missing) and `review_comment_title` (~88% missing) — expected behaviour, users leave star ratings without written feedback
- Price and weight outliers confirmed as legitimate transactions (e.g., luxury watches in Relogios), not data entry errors

### 2. Data Cleaning
- Date columns converted to `datetime` format (`order_purchase_timestamp`, `order_approved_at`, `order_delivered_carrier_date`, `order_delivered_customer_date`, `order_estimated_delivery_date`, `review_creation_date`)
- Full duplicate rows removed via integrity check
- Analysis restricted to `delivered` orders to ensure complete delivery and review data

### 3. Feature Engineering
- `delivery_days`: calculated as the difference between `order_delivered_customer_date` and `order_purchase_timestamp`
- `order_year` and `month_year`: extracted from `order_purchase_timestamp` for time-series analysis
- `review_score_rounded`: review scores rounded to integers for cleaner grouping in correlation analysis
- `pct_of_total`: each category's share of total revenue, calculated as `(category_total / total_revenue) * 100`

### 4. Analysis Areas

**Revenue & Pricing by Category (top 5 by revenue, 2017-2018):**

| Category | Revenue Share | Key Finding |
|---|---|---|
| beleza_saude | 9.38% | Primary organic growth driver |
| relogios_presentes | 8.98% | Price cut ~215 to ~190 spiked volume but caused ~710k revenue drop |
| cama_mesa_banho | 7.73% | High volume, lower unit price (~90), logistically exposed |
| esporte_lazer | 7.37% | Price increase ~110 to ~120 maintained growth — strong brand loyalty signal |
| informatica_acessorios | 6.80% | Discounting ~130 to ~105 grew volume to ~4,700 but weakened revenue |

**Delivery Time vs. Customer Satisfaction:**
- Pearson correlation between delivery days and review score: **-0.339** (moderate negative correlation)
- Score 1 reviews have the widest delivery time distribution with frequent outliers beyond 30 days
- Highest-risk states: Bahia (BA), Rio de Janeiro (RJ), Espirito Santo (ES) — median delivery ~16 days with extreme variance

**Geographic Logistical Analysis:**
- Delivery times segmented at state level to isolate logistical bottlenecks
- High-variance zones identified for last-mile optimisation

---

## Key Business Recommendations

1. Relogios & Presentes: absorb a modest shipping cost increase to guarantee faster delivery; category has margin headroom and fast delivery drives 5-star reviews and lifetime value
2. Beleza & Saude: maintain strict stock buffers; primary revenue source and replenishment predictability is critical
3. Cama, Mesa, Banho: most exposed to logistical problems among top 5; rigorously optimise shipping costs to protect already thin margins
4. Informatica Acessorios: discounting is not a sustainable strategy; focus on premium tech accessories with higher margins
5. Customer Experience: implement an automatic trigger to send a discount coupon when an order enters the late zone (beyond 12 days), to pre-empt a 1-star review before the customer writes it

---

## Tech Stack

Python, pandas, numpy, matplotlib, seaborn, Jupyter Notebook, Power BI

---

## Files

| File | Description |
|---|---|
| `ecommerce_olist_report.ipynb` | Full analysis: EDA, cleaning, feature engineering, analysis |
| `ecommerce_olist.pbix` | Power BI dashboard file |
| `powerbi_dashboard.pdf` | PDF export of the Power BI dashboard |
