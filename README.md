LHUB CapstoneProject
- Prepared by: Feng Yuanmei
- Dataset Used: Market Analysis

<p align="center">
  <img width="280" alt="Retail Insights Latin America Logo" src="https://github.com/user-attachments/assets/d2091e0c-8664-4230-9901-0f6c4110e5e0" />
</p>


## 📋 Project Overview
As a data analyst supporting a retail company operating across four Latin American markets (Brazil, Chile, Colombia and Mexico), my goal is to analyse customer demographics, purchasing patterns, product performance and discount data to identify high-value customer segments and guide marketing and product decisions that support sales growth and encourage repeat purchases.

## 💡 Power BI Dashboard
<img width="1155" height="653" alt="image" src="https://github.com/user-attachments/assets/23aaf98c-d39e-4c4f-8430-4291b3f77d08" />




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

**Sales Before Tax** is the main revenue measure used in this
project and excludes tax and shipping.

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

### Numerical Distributions

Descriptive statistics and distribution plots were used to examine
prices, quantities, discounts, customer ages, incomes and calculated
sales values.

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

- Price has a positive correlation with sales before tax.
- Quantity also has a positive correlation with sales before tax.
- Discount has a weak negative correlation with sales before tax.
- Discount and purchase quantity show almost no linear correlation.

Sales before tax is calculated from price, quantity and discount,
so its correlations with these inputs partly reflect the metric's
mathematical definition.

These correlations describe associations and do not establish
causal relationships.
