LHUB Capstone Project
# Retail Sales and Customer Purchase Analysis
- Prepared by: Feng Yuanmei
- Dataset Used: Market Analysis

<p align="center">
  <img width="280" alt="Retail Insights Latin America Logo" src="https://github.com/user-attachments/assets/d2091e0c-8664-4230-9901-0f6c4110e5e0" />
</p>


## 📑 Table of Contents

- [Project Overview](#-project-overview)
- [Power BI Dashboard](#-power-bi-dashboard)
- [Project Objectives](#-project-objectives)
- [Dataset Overview](#-dataset-overview)
- [Data Understanding and Preparation](#-data-understanding-and-preparation)
- [Exploratory Data Analysis (EDA)](#-exploratory-data-analysis-eda)
- [Business Analysis and Key Findings](#-business-analysis-and-key-findings)
  - [Objective 1: Sales Trends and Seasonal Patterns](#objective-1-sales-trends-and-seasonal-patterns)
  - [Objective 2: Customer Demographic and Geographic Analysis](#objective-2-customer-demographic-and-geographic-analysis)
  - [Objective 3: Customer Value Segmentation](#objective-3-customer-value-segmentation)
  - [Objective 4: Product Performance and Revenue Concentration](#objective-4-product-performance-and-revenue-concentration)
  - [Objective 5: Discount and Purchasing Behaviour](#objective-5-discount-and-purchasing-behaviour)
- [Business Recommendations](#-business-recommendations)
- [Limitations](#️-limitations)
- [Tools and Resources](#️-tools-and-resources)
- [Conclusion](#-conclusion)

## 📋 Project Overview
As a data analyst supporting a retail company operating across four Latin American markets (Brazil, Chile, Colombia and Mexico), my goal is to analyse customer demographics, purchasing patterns, product performance and discount data to identify high-value customer segments and guide marketing and product decisions that support sales growth and encourage repeat purchases.

## 💡 Power BI Dashboard
### 1: Retail Sales & Customer Insights
<img width="1312" height="739" alt="image" src="https://github.com/user-attachments/assets/55a0e9c3-fa95-4249-8e16-94abe86277af" />


### 2: Customer & Product Deep Dive
This page explores customer value and recency, compares customer
share with sales share, and examines average units per order
across products and discount bands.
<img width="1289" height="748" alt="image" src="https://github.com/user-attachments/assets/c39f3ad4-45ec-456c-854e-21a8f16c2d33" />


## 📝 Project Objectives
* **Sales Trends and Seasonal Patterns**
  >Analyse monthly sales revenue, order volume and quantities purchased over comparable periods to identify sales trends and potential seasonal patterns, supporting sales planning and promotional scheduling.

* **Customer Demographic and Geographic Analysis**
  >Compare revenue contribution, order volume and average order value across age groups, gender, country and income levels to identify valuable customer groups and opportunities for targeted marketing.

* **Customer Value Segmentation**
  >Segment customers based on total spending and recent purchase activity to identify high-value and less recently active customers, supporting targeted engagement and repeat-purchase initiatives.
  
* **Product Performance and Revenue Concentration**
  >Compare revenue, quantities sold and revenue share across product types to identify leading and lower-performing products, supporting product priorities and portfolio decisions.

* **Discount and Purchasing Behaviour Analysis** 
  >Examine the relationship between discount rates, quantities purchased and order values within comparable product groups to inform discount and promotional decisions.

## 📊 Dataset Overview

The project uses two datasets:

| Dataset | Records | Description |
| --- | ---: | --- |
| [purchase.csv](purchase.csv) | 50,000 | Order details, products, prices, discounts, taxes, quantities and shipping information |
| [customer.csv](customer.csv) | 1,001 | Customer identifiers, demographics, countries and incomes |

- **Transaction period:** 15 August 2019–1 January 2023.
- **Markets:** Brazil, Chile, Colombia and Mexico.
- **Purchasing customers:** 1,000.
- **Product types:** 30, grouped into Home & Living, Electronics and Fashion & Apparel.
- Each purchase record represents one order.
- One customer has no recorded purchases and is retained in the customer table.

  
## 🧹 Data Understanding and Preparation

Data preparation was performed in Python using Google Colab.
The purchase and customer datasets were checked separately
before being merged for analysis.

### Missing Values and Duplicate Checks

- No missing values or duplicate rows were found in either dataset.
- Order IDs in the purchase table and customer IDs in the customer
  table were unique.
- No missing-value imputation or duplicate removal was required.

### Date Conversion and Validation

- Converted `order_date` and `shipping_date` to datetime format.
- Checked for missing or invalid dates.
- Calculated `days_to_ship` as the number of days between order
  placement and shipping.
- Checked for shipping dates earlier than order dates.

### Numerical Range and Validity Checks

Reviewed numerical summaries and checked for:

- Non-positive prices and quantities.
- Non-integer quantities and ages.
- Discounts and tax rates outside the range of 0–1.
- Negative shipping costs and incomes.
- Non-positive ages.

High-value orders were reviewed and retained because no errors
were identified during the investigation.

### Customer Matching and Data Integration

The purchase table was left-joined with the customer table using
`customer_id`, with a many-to-one relationship validated.

- All 50,000 purchase records matched a customer.
- The merged dataset retained exactly 50,000 rows.
- One customer, `CS01001`, had no recorded purchases and was retained
  in the customer master table.

Customer counts use unique customer identifiers. Customer
demographic distributions use the customer master table to avoid
counting customers repeatedly across transactions.

### Sales Metric Preparation

The following fields were calculated:

| Metric | Definition |
| --- | --- |
| Discounted Price | Price × (1 − Discount) |
| Sales Before Tax | Discounted Price × Quantity |
| Tax Amount | Sales Before Tax × Tax Rate |
| Sales After Tax | Sales Before Tax + Tax Amount |
| Order Total | Sales After Tax + Shipping Charge |
| Average Order Value | Total Sales Before Tax ÷ Unique Orders |

**Sales Before Tax (Sales Revenue) is the main revenue measure used in this
project and excludes tax and shipping** .

**Order Total** represents the calculated amount payable rather
than actual cash collections. Shipping cost is assumed to be
charged to customers without additional shipping tax.

Each purchase record represents one order, so shipping is added
once per order.

### Feature Engineering

- Created year, month, year-month and quarter fields for time-based analysis.
- Grouped ages into **18–25, 26–35, 36–45, 46–55 and 56+**.
- Grouped incomes into **Below 30K, 30K–60K, 60K–90K,
  90K–120K and 120K+**.
- Income intervals include the lower boundary and exclude the upper
  boundary. These are project-defined ranges, not official income
  classifications.
- Applied identical age and income group definitions to the customer
  and merged transaction tables.
- Grouped the 30 product types into **Home & Living, Electronics
  and Fashion & Apparel**.

  ## 🔍 Exploratory Data Analysis (EDA)

Exploratory analysis was conducted to understand numerical
distributions, investigate unusually high order values and examine
relationships between variables before the business analysis.
### Customer Group Distributions
Customer counts are compared across country, gender, age group
and income range using the customer master table.
<img width="1389" height="989" alt="1" src="https://github.com/user-attachments/assets/758a87d9-5f9b-49f8-8c2c-e8e7e001ad75" />

The chart includes all 1,001 registered customers, including one
customer without recorded purchases.

Colombia has the largest customer base, gender distribution is
nearly balanced, and the 46–55 age group and 30K–60K income range
contain the most customers within their respective classifications.

### Numerical Distributions

Descriptive statistics and distribution plots were used to examine
prices, quantities, discounts, customer ages, incomes and calculated
sales values.
<img width="1388" height="2190" alt="2" src="https://github.com/user-attachments/assets/a56b9af3-103e-48b9-826c-aca793bb16f4" />

Sales values vary substantially across orders, reflecting differences
in product prices, quantities and discounts. These differences are
considered when comparing product and customer performance.

### High-Value Order Investigation

Box plots and a review of high-value orders were used to investigate
potential outliers.

The reviewed orders were retained because no errors were identified.
High sales values alone were not treated as sufficient grounds for
removing records.

### Correlation Analysis

Correlation analysis explored relationships between numerical fields.
<img width="941" height="690" alt="3" src="https://github.com/user-attachments/assets/c28a54ae-0e7d-47dd-81c0-8527d2e7bc1a" />

- Price has a positive correlation with sales before tax.
- Quantity also has a positive correlation with sales before tax.
- Discount has a weak negative correlation with sales before tax.
- Discount and purchase quantity show almost no linear correlation.

Sales before tax is calculated from price, quantity and discount,
so its correlations with these inputs partly reflect the metric's
mathematical definition.

These correlations describe associations and do not establish
causal relationships.

## 💡 Business Analysis and Key Findings

### Objective 1: Sales Trends and Seasonal Patterns
<img width="1589" height="842" alt="1" src="https://github.com/user-attachments/assets/534af518-5ac8-4855-8c43-9a70d368460a" />
<img width="889" height="490" alt="2" src="https://github.com/user-attachments/assets/15cc6d08-45df-4794-a098-f4fbe0a83e9e" />
<img width="1089" height="490" alt="3" src="https://github.com/user-attachments/assets/08bfa5aa-2146-42e8-945a-645c2df21ba5" />



- Total sales before tax are approximately **263.69 million**,
  generated from **50,000 orders** and **274,878 units**.
- Average order value is approximately **5,274**.
- Across complete months, sales fluctuate without a clear sustained
  upward or downward trend.
- Annual sales decreased by approximately **3.55% in 2021** and
  increased by **1.01% in 2022**.
- No consistent seasonal pattern was established.
- August 2019 and January 2023 are partial months and should not
  be compared directly with complete months.

### Objective 2: Customer Demographic and Geographic Analysis
<img width="1289" height="490" alt="sales revenue by country" src="https://github.com/user-attachments/assets/29b2b508-feef-450e-af7a-421fad5e929d" />
<img width="1289" height="490" alt="sales revenue by age group" src="https://github.com/user-attachments/assets/bc1c60ed-a9b6-4bac-b170-bb7720c80fe3" />
<img width="1189" height="490" alt="sales revenue by gender" src="https://github.com/user-attachments/assets/babd19b5-7c3b-4cb6-8df9-ccc515e38bba" />
<img width="1390" height="540" alt="sales revenue by Income group" src="https://github.com/user-attachments/assets/735c9b70-f83f-4c35-9ab4-162bd3c1f991" />

- **Colombia** contributes the largest sales share at **28.20%**,
  followed by Chile, Mexico and Brazil.
- Average order values are relatively similar across countries,
  indicating that revenue differences mainly reflect customer
  and order counts.
- The **46–55 age group** contributes the highest sales share
  at **22.27%**.
- The **30K–60K income group** contributes the largest income-group
  sales share at **25.99%**.
- Revenue is nearly evenly split between male and female customers.

Demographic results describe the existing customer base. Customer
value and product preferences should also inform marketing decisions.

### Objective 3: Customer Value Segmentation
Customers are segmented using median total spending of 262,188.80
and median recency of 17 days, measured as of 2 January 2023.

<img width="1187" height="590" alt="2" src="https://github.com/user-attachments/assets/676fdae3-43b3-47fc-b1cc-1daa37f100b3" />


| Customer Segment | Customers | Customer Share | Sales Share |
| --- | ---: | ---: | ---: |
| Higher Spending / Recent | 253 | 25.30% | 28.90% |
| Higher Spending / Less Recent | 247 | 24.70% | 28.47% |
| Lower Spending / Recent | 250 | 25.00% | 21.25% |
| Lower Spending / Less Recent | 250 | 25.00% | 21.38% |

The higher-spending groups represent **50% of purchasing customers**
and contribute approximately **57.37% of total sales**.

Every purchasing customer has exactly **50 orders**, so frequency
does not distinguish customers. Average order value also gives the
same ranking as total spending.

These segments describe historical behaviour rather than predict
customer loyalty or churn.

### Objective 4: Product Performance and Revenue Concentration
<img width="1589" height="490" alt="1" src="https://github.com/user-attachments/assets/a72a4b76-6f6b-4561-8220-c00151a7a08d" />
<img width="1789" height="490" alt="2" src="https://github.com/user-attachments/assets/14dfd747-d8d9-4492-bb40-312ddfaedfde" />

<img width="1590" height="796" alt="product_pareto" src="https://github.com/user-attachments/assets/860d4f5f-17f5-4122-8a56-400cd6c4b36f" />

- **Home & Living** generates approximately **177.31 million**,
  accounting for **67.24% of total sales**.
- Quantities sold are nearly equal across the three categories;
  revenue differences mainly reflect average selling prices.
- **Curtains** generates the highest product revenue, while
  **Sports Shoes** records the highest units sold.
- The top five product types contribute **34.30%** of total sales.
- Fifteen of the thirty product types contribute **81.80%**,
  so the dataset does not follow a strict 80/20 Pareto distribution.

Product decisions should consider revenue, quantities and selling
prices together. Revenue alone does not establish profitability.

### Objective 5: Discount and Purchasing Behaviour
<img width="1690" height="490" alt="1" src="https://github.com/user-attachments/assets/9eeac7d1-a148-49d3-89a3-1cdff24b59dc" />


- Only **477 orders** have no discount; discounted transactions
  account for most orders.
- Average units per order remain broadly similar across discount bands.
- Average order value decreases as discount levels increase.
- Within individual product types, there is no consistent tendency
  for purchase quantity to rise with deeper discounts.
- No-discount averages require caution because each product has
  relatively few orders without a discount.

The analysis does not establish that deeper discounts generate
incremental demand. Product cost and margin data would be required
to assess promotional profitability.

## 🧭 Business Recommendations

### Customer Engagement

Test retention offers for higher-spending recent customers and
re-engagement campaigns for higher-spending less recent customers.
Relevant cross-selling offers could also be tested among
lower-spending recent customers.

### Marketing Decisions

Combine demographics with customer spending, recency and product
preferences when selecting campaign audiences. Lower revenue
contribution alone does not establish greater growth potential.

### Product Decisions

Monitor Home & Living closely because it contributes 67.24% of
total sales. Consider revenue, quantities sold and selling prices
together when evaluating products.

Review margins, inventory turnover and customer purchasing patterns
before reducing lower-revenue product lines or stock allocations.

### Promotional Planning

Evaluate discount offers through controlled testing. Compare
incremental sales and contribution margins before expanding
promotions, rather than assuming deeper discounts increase demand.

### Sales Planning

Compare complete periods when monitoring sales performance.
Clearly identify partial months to avoid misleading conclusions.

## ⚠️ Limitations

- Every purchasing customer has exactly 50 orders, limiting the
  usefulness of purchase frequency for segmentation.
- Average order value produces the same customer ranking as total
  spending because order counts are identical.
- Customer segments describe historical behaviour and use a fixed
  recency reference date of 2 January 2023.
- Product costs, profit margins and inventory data are unavailable.
- No-discount groups contain relatively few observations, especially
  when filtered by product or country.
- August 2019 and January 2023 contain partial-month data.
- Observed relationships do not establish causal effects.

## 🛠️ Tools and Resources

| Tool | Purpose |
| --- | --- |
| Google Colab | Python notebook development and execution |
| pandas / NumPy | Data preparation, calculations and aggregation |
| Matplotlib / Seaborn | Exploratory and business analysis charts |
| Microsoft Power BI | Data modelling, DAX measures and interactive dashboards |
| GitHub | Project documentation and file sharing |

### Project Files

- [Python Analysis Notebook](Capstone_Market_Analysis.ipynb)
- [Power BI Report](Capstone%20Project-Feng%20Yuanmei.pbix)
- [Purchase Dataset](purchase.csv)
- [Customer Dataset](customer.csv)

## 🏁 Conclusion

This project analyses retail sales and customer purchasing behaviour
across Brazil, Chile, Colombia and Mexico using Python and Power BI.

The analysis identifies major revenue contributors, compares customer
profiles, segments customers by spending and recency, and examines
product performance and discount patterns.

Home & Living contributes the largest revenue share, while the
higher-spending half of purchasing customers generates approximately
57.37% of total sales. Deeper discounts show no consistent association
with larger quantities purchased per order.

The findings provide a basis for testing targeted customer engagement,
monitoring product performance and reviewing promotions. Further
campaign testing and cost data are needed to assess their commercial
impact.
