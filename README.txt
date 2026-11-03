# Superstore Sales Analysis: From Raw Data to Dashboard

End-to-end analysis of Superstore sales data: data cleaning, exploratory analysis in Excel, and an interactive Power BI dashboard.

## 1. Problem Statement
Superstore's sales have grown steadily, but growth does not guarantee profitability. This project aims to answer:
- Where does the business make and lose money (products, categories, markets)?
- Does growth come from volume or from better profitability?
- What actions could improve the profit margin?

## 2. Dataset Information
- **Source:** Superstore sales dataset (https://www.kaggle.com/datasets/fatihilhan/global-superstore-dataset)
- **Size:** 51,290 order lines | 25,035 orders | 795 customers
- **Period:** 2011 to 2014
- **Markets:** APAC, EU, North America, LATAM, EMEA, Africa
- **Key columns:** Order.ID, Order.Date, Ship.Date, Customer.Name, Segment, Market2, Region, Country, Category, Sub.Category, Product.Name, Sales, Quantity, Discount, Profit, Shipping.Cost, Ship.Mode

## 3. Data Cleaning Process
file :(data/Cleaned_Superstore_data.csv)

The raw dataset was cleaned before analysis. The cleaned version has 51,252 rows and no missing values or duplicate rows.

### 3.1 Data Inspection
- Reviewed the structure, column names, data types and summary statistics of the raw data (51,290 rows, 27 columns).
- Checked each column for missing values, duplicates, inconsistent formats and unexpected values.

### 3.2 Checking Missing Values
- Found missing values in: Sales.
- Treatment: Keeping it , because it might had free on offer or high discount and because of rounding sales it has shown as 0 .
- Result: 0 missing values in the cleaned dataset.

### 3.3 Removing Duplicates
- Found 38 duplicate rows (Grouping by both order.ID and productID columns).
- Removed them, keeping the first occurrence.
- Result: 0 duplicate rows in the cleaned dataset.

### 3.4 Data Type Correction
- Converted `Order.Date` and `Ship.Date` to Date type.
- Converted `Sales`, `Profit`, `Shipping.Cost` and `Quantity` to numeric types.
- Converted `Discount` to a True/False field.

### 3.5 Data Transformation
- Created `Ship Days` = `Ship.Date` - `Order.Date`.
- Created date fields: `Order.Date (Year)`, `Order.Date (Quarter)`, `Order.Date (Month)`

Notes and limitations found during analysis:
- `Discount` is stored as True/False, so the discount percentage is not available.
- `Customer.ID` is unique per customer name per market (4,873 IDs for 795 names), so customers are counted by `Customer.Name`.
- `Sales` is rounded to whole numbers, and one row has a sales value of zero.
- Columns added for analysis: `Ship Days`, `IQR Flag`, `Profit Z`, `Z Flag`.

## 4. Analysis (Excel)
File: (analysis/Superstore_EDA)

| Variable | Mean | Median | Skewness |
|---|---|---|---|
| Sales | 246.43 | 85 | 8.15 |
| Profit | 28.58 | 9.24 | 4.16 |
| Shipping Cost | 26.37 | 7.79 | 5.86 |
| Quantity | 3.48 | 3 | 1.36 |

- **Distribution:** strongly right-skewed. A few very large orders pull the mean above the median.
- **Correlation:** Sales-Shipping Cost 0.77, Sales-Profit 0.48, Quantity-Profit 0.10.
- **Trend:** sales grew from $2.26M (2011) to $4.30M (2014), YoY +18.5%, +27.4%, +26.3%. Profit grew from $248K to $504K, YoY +23.8%, +32.5%, +23.9%. Margin stayed around 11-12%.
- **Seasonality:** November and December are the strongest months.
- **Anomalies:** IQR flagged 9,753 profit outliers (19%), Z-score flagged 864 (1.7%). IQR flags more because most profit values sit near zero. No outliers were removed, because they are genuine transactions.

## 5. Dashboard (Power BI)
File: (dashboard/dashboard.pbix)

![Overview](images/dashboard_overview.png)
![Profitability](images/dashboard_profitability.png)

- **KPIs:** Total Sales, Total Profit, Profit Margin, Total Orders, Total Customers, Avg Order Value
- **Filters:** Year, Market, Category, Segment
- **Charts:** sales over time , sales & profit margin by markets , sales by segments , category and subcategory performance 
- **Interactivity:** drill-down from year to month, cross-filtering between visuals

## 6. Key Business Insights
1. **Tables is the only loss-making sub-category.** Margin of -8.5% despite $757K in sales. Likely driven by discounts and shipping costs, so review its pricing and discount policy.
2. **Discounted orders lose money.** -$412K profit vs +$1.88M for non-discounted orders. Set a maximum discount cap.
3. **Some top sellers are unprofitable.** Motorola Smart Phone, Cordless and Hoover Stove, White rank among the top products by sales and among the least profitable.
4. **EMEA has the weakest margin.** 5.4% vs 10-13% in other markets. Investigate discounts and shipping costs there.
5. **Growth comes from volume, not profitability.** Profit roughly doubled from 2011 to 2014 while margin stayed flat at about 11-12%.

## 7. Limitations
- The discount percentage is missing, so the discount insight relies on the True/False flag only.
- Likely causes for Tables and EMEA are hypotheses and were not tested.

## 8. Tools
Excel (PivotTables, Power Query, descriptive statistics, outlier detection), Power BI (DAX, Power Query, interactive dashboard).