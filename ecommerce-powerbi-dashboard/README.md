# Olist E-commerce Power BI Dashboard

## Project Objective

The goal of this project is to transform raw transactional and logistical data from the Brazilian e-commerce platform Olist into a multi-page interactive Power BI dashboard, enabling business stakeholders to monitor revenue performance, customer distribution, sales trends, and logistical efficiency in one place.

---

## Dataset

- **Source**: Brazilian E-commerce Public Dataset by Olist — sourced from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- **Scope**: Delivered orders, 2017-2018
- **Key metrics tracked**: Total revenue, order volume, customer count, average order value, delivery days, review scores

---

## Dashboard Structure

The dashboard is organised into 4 pages accessible via the navigation panel on the left:

### 1. Panoramica (Overview)
Top-level KPI cards: Total Revenue **5.39M** · Product Volume **43.1K** · Total Customers **37K** · Avg Order Value **141.20**

Visuals:
- Clustered bar chart of total revenue by top 5 product categories (2017 vs 2018 comparison)
- Customer geographic distribution map
- Transaction detail table filterable by year (2017 / 2018)

### 2. Performance di Vendita (Sales Performance)
- Monthly revenue trend line chart (Jan 2017 – Aug 2018)
- Product volume by category bar chart
- Year-over-year revenue and volume comparison per category

### 3. Geografia dei Clienti (Customer Geography)
- Top 10 Brazilian states by order count (SP: 16.27K, RJ: 4.95K, MG: 4.48K)
- Average delivery days by state bar chart — ranked from fastest (SP: ~8 days) to slowest (BA: ~20 days)
- Customer distribution world map (Microsoft Azure Maps)

### 4. Operazioni Logistiche (Logistical Operations)
KPI cards: Avg Delivery Days **12** · Avg Review Score **4.09** · Total Reviews **38K**

Visuals:
- Delivery speed trend (2017-2018): avg delivery days declining from ~12.4 to ~11.6
- On-time vs late delivery donut chart: **92.02% on time**, 7.98% late
- Avg review score comparison: on-time orders = **4.22**, late orders = **2.55**
- Avg review score by product category (horizontal bar chart)
- Delivery impact on satisfaction: avg delivery days per review score (1-5 star scale)

---

## Data Preparation

Data was cleaned and preprocessed in Python before being loaded into Power BI:
- Date columns parsed and `delivery_days` calculated (`order_delivered_customer_date` minus `order_purchase_timestamp`)
- Analysis filtered to `delivered` orders only
- `is_late` flag created by comparing actual vs estimated delivery date
- Top 5 categories selected by total revenue for focused analysis

---

## Tech Stack

- **Python** — data cleaning, feature engineering, delivery time calculation
- **Power BI Desktop** — data modelling, DAX measures, dashboard design
- **Microsoft Azure Maps** — geographic visualisation

---

## Files

| File | Description |
|---|---|
| `ecommerce_olist.pbix` | Power BI project file (open with Power BI Desktop) |
| `powerbi_dashboard.pdf` | PDF export of all 4 dashboard pages |
