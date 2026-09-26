# PERFORMANCE ANALYSIS DASHBOARD
<img width="1383" height="795" alt="image" src="https://github.com/user-attachments/assets/29f11e25-e086-4c10-9ad0-40193120b780" />


## End-to-End Power BI Project Documentation

**Project:** Performance Analysis Dashboard
**Tool:** Microsoft Power BI
**Analysis Period:** January – June 2024
**Project Type:** Sales & Business Performance Analytics

---

# 1. PROJECT OVERVIEW

The **Performance Analysis Dashboard** is an interactive Power BI dashboard designed to analyze business performance across revenue, quantity sold, shipping cost, customer performance, regional performance, country-level revenue, payment methods, and target achievement.

The dashboard converts transactional data into meaningful business insights using:

* Power Query
* Data Modeling
* DAX
* Time Intelligence
* Interactive Slicers
* KPI Cards
* Charts and Graphs
* Customer and Product Analysis

The primary objective of the dashboard is to provide management with a **single interactive view of business performance** and allow users to drill down from overall KPIs to regional, country, customer, product and category-level analysis.

<img width="1392" height="802" alt="image" src="https://github.com/user-attachments/assets/7295d989-e550-4547-ad73-ecebdcfb8f5c" />


---

# 2. BUSINESS OBJECTIVE

The main objective of this dashboard is to help business users monitor sales performance and identify areas that require attention.

The dashboard answers questions such as:

1. What is the total revenue generated?
2. How many products/units were sold?
3. What is the average shipping cost?
4. What is the average order gap?
5. Which region generates the highest revenue?
6. Which countries contribute the most revenue?
7. How is revenue changing month by month?
8. Who are the top customers?
9. Which payment methods are most commonly used?
10. How much of the sales target has been achieved?
11. How does the current month compare with the previous month?
12. Which product categories are contributing to business performance?
13. How can return information be incorporated into performance analysis?

---

# 3. BUSINESS PROBLEM STATEMENT

Businesses often have sales data distributed across different tables such as orders, customers, products and returns.

Without an interactive reporting system, management may face difficulty in:

* Monitoring sales performance
* Identifying high-performing regions
* Finding high-value customers
* Understanding monthly sales trends
* Tracking sales targets
* Monitoring payment-method distribution
* Identifying potential product/category opportunities
* Understanding returns and operational performance

Therefore, this Power BI dashboard was developed to provide a centralized and interactive performance-monitoring solution.

---

# 4. DATASET / TABLES USED

The Power BI model contains the following major tables:

1. **Orders**
2. **Customers**
3. **Products**
4. **Returns**
5. **Date_Table**
6. **Image_Url**

---

# 5. DATA MODEL ARCHITECTURE

The model is designed around the **Orders table**, which acts as the primary transactional table.

### Main Structure

```text
                    Customers
                        |
                        |
                        ↓
                     Orders
                    /      \
                   /        \
                  ↓          ↓
             Products     Date_Table
                  |
                  ↓
              Image_Url

                  ↑
                  |
               Returns
```

### Table Roles

| Table      | Role                           |
| ---------- | ------------------------------ |
| Orders     | Main transaction/fact table    |
| Customers  | Customer dimension             |
| Products   | Product dimension              |
| Date_Table | Date/calendar dimension        |
| Returns    | Return transaction/event table |
| Image_Url  | Product image reference table  |

---

# 6. ORDERS TABLE

The **Orders** table contains the main transactional information.

### Columns

| Column        | Purpose                          |
| ------------- | -------------------------------- |
| CustomerID    | Connects orders with customers   |
| Discount      | Discount applied to transactions |
| Month         | Month-related field              |
| OrderDate     | Transaction/order date           |
| OrderID       | Unique order identifier          |
| PaymentMethod | Payment method used              |
| ProductID     | Connects orders with products    |
| Quantity      | Quantity sold                    |
| Region        | Sales region                     |
| Sales         | Revenue/sales amount             |
| ShippingCost  | Shipping cost                    |
| UnitPrice     | Selling price per unit           |

### Business Usage

The Orders table is used to calculate:

* Total Revenue
* Total Quantity
* Total Orders
* Average Order Value
* Shipping Cost
* Monthly Revenue
* Regional Revenue
* Payment Method Revenue
* Customer Revenue
* Product Revenue

---

# 7. CUSTOMERS TABLE

The Customers table contains customer information.

### Columns

| Column      | Purpose             |
| ----------- | ------------------- |
| Customer_ID | Customer identifier |
| First_Name  | Customer first name |
| Last_Name   | Customer last name  |
| Email       | Customer email      |
| City        | Customer city       |
| Country     | Customer country    |

### Business Usage

Used for:

* Customer-level revenue
* Top customer analysis
* Customer geography
* Customer segmentation
* Country-level analysis

---

# 8. PRODUCTS TABLE

The Products table contains product master information.

### Columns

| Column       | Purpose               |
| ------------ | --------------------- |
| Product_ID   | Product identifier    |
| Product_Name | Product name          |
| Category     | Main product category |
| Sub_Category | Product sub-category  |
| Unit_Cost    | Cost per unit         |
| Unit_Price   | Selling price         |

### Business Usage

Used for:

* Product analysis
* Category analysis
* Sub-category analysis
* Revenue analysis
* Cost analysis
* Margin analysis

---

# 9. RETURNS TABLE

The Returns table contains information about returned orders.

### Columns

| Column        | Purpose                   |
| ------------- | ------------------------- |
| Order_ID      | Returned order identifier |
| Return_Date   | Date of return            |
| Return_ID     | Return identifier         |
| Return_Reason | Reason for return         |

### Business Usage

Returns can be used to calculate:

* Return Count
* Returned Orders
* Return Rate
* Return Trend
* Return Reasons
* Product/Category Return Performance

---

# 10. DATE TABLE

The Date_Table is used for time intelligence.

The primary field is:

```text
Date
```

Recommended additional columns:

```text
Year
Month Number
Month Name
Quarter
Year-Month
Week Number
Day
Day Name
```

### Why Date Table is Important?

A dedicated Date Table allows Power BI to perform calculations such as:

* Previous Month
* Previous Year
* Month-over-Month Growth
* Year-over-Year Growth
* Rolling Average
* YTD
* MTD
* QTD

---

# 11. IMAGE_URL TABLE

The Image_Url table contains product-image information.

### Columns

| Column       | Purpose            |
| ------------ | ------------------ |
| Product Name | Product lookup key |
| Image Link   | Image URL          |

The purpose of this table is to display product images in the dashboard.

---

# 12. DATA RELATIONSHIPS

The current model contains the following relationships:

| From                   | To                      | Relationship       |
| ---------------------- | ----------------------- | ------------------ |
| Orders[CustomerID]     | Customers[Customer_ID]  | Many-to-Many shown |
| Orders[OrderDate]      | Date_Table[Date]        | Many-to-One        |
| Orders[ProductID]      | Products[Product_ID]    | Many-to-One        |
| Products[Sub_Category] | Image_Url[Product Name] | Many-to-One shown  |
| Returns[Order_ID]      | Orders[OrderID]         | One-to-One shown   |

---

# 13. RELATIONSHIP RECOMMENDATIONS

## 13.1 Customer Relationship

Current relationship:

```text
Orders[CustomerID] *:* Customers[Customer_ID]
```

If `Customers[Customer_ID]` is unique, the preferred structure should be:

```text
Customers (1) → Orders (*)
```

This follows the standard Power BI star-schema design.

---

## 13.2 Product Relationship

Recommended:

```text
Products (1) → Orders (*)
```

Using:

```text
Products[Product_ID]
        ↓
Orders[ProductID]
```

---

## 13.3 Date Relationship

Recommended:

```text
Date_Table (1) → Orders (*)
```

Using:

```text
Date_Table[Date]
        ↓
Orders[OrderDate]
```

---

## 13.4 Image Relationship

The screenshot shows:

```text
Products[Sub_Category]
        ↓
Image_Url[Product Name]
```

This appears to be a potential key mismatch.

For product images, the expected relationship would normally be:

```text
Products[Product_Name]
        ↓
Image_Url[Product Name]
```

This should be validated in the actual Power BI model.

---

## 13.5 Returns Relationship

The screenshot shows:

```text
Returns[Order_ID] 1:1 Orders[OrderID]
```

This should be validated.

If one order can have multiple return records, the relationship should instead be:

```text
Orders (1) → Returns (*)
```

---

# 14. POWER QUERY / DATA CLEANING PROCESS

The following data preparation steps should be performed before creating the dashboard.

### Step 1 — Load Data

Import:

```text
Orders
Customers
Products
Returns
Date_Table
Image_Url
```

into Power BI.

### Step 2 — Data Type Validation

Check data types for:

* OrderDate → Date
* Return_Date → Date
* Quantity → Whole Number
* Sales → Decimal Number
* ShippingCost → Decimal Number
* UnitPrice → Decimal Number
* Unit_Cost → Decimal Number
* Discount → Decimal Number

### Step 3 — Remove Extra Spaces

Use:

```text
Transform → Format → Trim
```

for important text fields.

### Step 4 — Clean Text

Standardize:

* Region
* Category
* Sub_Category
* PaymentMethod
* Product_Name

### Step 5 — Validate Keys

Check:

```text
Customer_ID
Product_ID
OrderID
Order_ID
```

for duplicates and null values.

### Step 6 — Validate Relationships

Ensure that:

```text
Every Order Customer exists in Customers
Every Order Product exists in Products
Every Return Order exists in Orders
```

---

# 15. KPI CARDS

The dashboard contains four major KPI cards.

## 15.1 Total Revenue

Displayed value:

```text
41.72M
```

Purpose:

Shows total revenue generated in the selected filter context.

### DAX

```DAX
Total Revenue =
SUM(Orders[Sales])
```

---

# 16. TOTAL QUANTITY

Displayed value:

```text
15K
```

Purpose:

Shows the total quantity sold.

### DAX

```DAX
Total Quantity =
SUM(Orders[Quantity])
```

---

# 17. AVERAGE SHIPPING COST

Displayed value:

```text
110.80
```

Purpose:

Shows the average shipping cost.

### DAX

```DAX
Avg Ship Cost =
AVERAGE(Orders[ShippingCost])
```

Note: The business definition should confirm whether this should be an average per transaction, order or order line.

---

# 18. AVERAGE ORDER GAP

Displayed value:

```text
2.34
```

Purpose:

Represents the average order interval/gap based on the project's calculation logic.

The exact DAX should be validated against the original PBIX because the screenshot only exposes the measure name, not its formula.

---

# 19. SALES TARGET

The dashboard shows a target of approximately:

```text
54.24M
```

Actual Revenue:

```text
41.72M
```

Target Achievement:

```text
41.72 / 54.24 × 100
≈ 76.9%
```

Target Gap:

```text
54.24M - 41.72M
≈ 12.52M
```

### Recommended DAX

```DAX
Sales Target =
54240000
```

However, in a production dashboard, the target should ideally come from a separate target table rather than being hard-coded.

---

# 20. TARGET ACHIEVEMENT

Recommended measure:

```DAX
Target Achievement % =
DIVIDE(
    [Total Revenue],
    [Sales Target],
    0
)
```

This measure can be displayed as a percentage.

---

# 21. LAST MONTH REVENUE

Recommended measure:

```DAX
Last Month =
CALCULATE(
    [Total Revenue],
    DATEADD(
        Date_Table[Date],
        -1,
        MONTH
    )
)
```

Purpose:

Allows comparison between the current month and previous month.

---

# 22. LAST MONTH QUANTITY

```DAX
Last Month Qty =
CALCULATE(
    [Total Quantity],
    DATEADD(
        Date_Table[Date],
        -1,
        MONTH
    )
)
```

---

# 23. QUANTITY CHANGE VS LAST MONTH

```DAX
Vs Last Month QTY =
[Total Quantity] - [Last Month Qty]
```

---

# 24. REVENUE GROWTH %

Recommended additional measure:

```DAX
Revenue Growth % =
DIVIDE(
    [Total Revenue] - [Last Month],
    [Last Month],
    0
)
```

This shows the percentage change in revenue compared with the previous month.

---

# 25. QUANTITY GROWTH %

```DAX
Quantity Growth % =
DIVIDE(
    [Total Quantity] - [Last Month Qty],
    [Last Month Qty],
    0
)
```

---

# 26. TOTAL ORDERS

Recommended measure:

```DAX
Total Orders =
DISTINCTCOUNT(Orders[OrderID])
```

This should be used instead of simply counting rows if the Orders table contains multiple rows per order.

---

# 27. AVERAGE ORDER VALUE

```DAX
Average Order Value =
DIVIDE(
    [Total Revenue],
    [Total Orders],
    0
)
```

This helps understand the average revenue generated per order.

---

# 28. TOTAL SHIPPING COST

```DAX
Total Shipping Cost =
SUM(Orders[ShippingCost])
```

---

# 29. RETURNS KPIs

## Returned Orders

```DAX
Returned Orders =
DISTINCTCOUNT(Returns[Order_ID])
```

## Return Count

```DAX
Return Count =
DISTINCTCOUNT(Returns[Return_ID])
```

## Return Rate

```DAX
Return Rate % =
DIVIDE(
    [Returned Orders],
    [Total Orders],
    0
)
```

---

# 30. DASHBOARD VISUALS

## 30.1 Quantity Sold By Date

### Visual Type

Line/Scatter style trend chart.

### Purpose

Shows daily quantity movement from January to June 2024.

### Business Use

It helps identify:

* Sales spikes
* Sales drops
* High-volume days
* Low-volume days
* Unusual demand patterns

---

# 31. TOTAL REVENUE BY REGION

### Visual Type

Donut Chart

Regions:

```text
East
North
South
West
```

Visible distribution:

| Region | Revenue Share |
| ------ | ------------: |
| East   |         24.8% |
| North  |        25.19% |
| South  |        24.96% |
| West   |        25.04% |

### Observation

The regional revenue distribution shown in the dashboard is relatively balanced, with each region contributing approximately one-quarter of total revenue.

---

# 32. TOTAL REVENUE BY COUNTRY

### Visual Type

Horizontal Bar Chart

The visible top countries include:

| Country        | Approx. Revenue |
| -------------- | --------------: |
| Romania        |            130K |
| Antarctica     |            105K |
| Macao          |             99K |
| Côte d'Ivoire  |             80K |
| Iraq           |             70K |
| Yemen          |             63K |
| Tuvalu         |             52K |
| Western Sahara |             51K |

These values should be validated against the source data before being used in formal business reporting.

---

# 33. TOTAL REVENUE BY MONTH

### Visual Type

Waterfall Chart

The chart displays monthly revenue contribution for:

```text
January
February
March
April
May
June
Total
```

The monthly values are approximately 7M-level contributions, with June appearing higher.

The displayed total rounds to approximately:

```text
42M
```

while the KPI displays:

```text
41.72M
```

This difference is consistent with visual rounding.

---

# 34. LAST MONTH VS TOTAL REVENUE

### Visual Type

Line/Area Chart

Purpose:

The visual compares monthly revenue performance and previous-month context.

It can help identify:

* Revenue growth
* Revenue decline
* Monthly peaks
* Monthly drops
* Trend direction

### Important Validation

The dashboard is titled:

```text
Jan–Jun Analysis 2024
```

However, this chart visibly contains:

```text
July 2024
August 2024
```

Therefore, the date scope should be validated.

If the dashboard is strictly Jan–Jun, July and August should normally be excluded from the reporting view.

---

# 35. TOP 10 CUSTOMER LIST

The dashboard displays:

* Customer ID
* Total Products
* Total Quantity
* Total Revenue

Example visible customers include:

```text
C1075
C1579
C1491
C1476
C1650
C1985
C1830
C1006
C1319
C1068
```

This visual helps identify high-value customers.

---

# 36. PAYMENT METHOD ANALYSIS

### Visual Type

Donut Chart

Payment methods shown include:

```text
Credit Card
COD
Debit Card
UPI
Net Banking
```

The dashboard shows a relatively balanced distribution across the visible payment methods.

### Business Use

This analysis can help the business understand:

* Customer payment preferences
* Digital payment adoption
* COD dependency
* Payment-channel distribution

---

# 37. CATEGORY ANALYSIS

The dashboard contains a category navigation panel with categories such as:

```text
Shoes
Headphones
Shirts
Furniture
Outdoor Fitness
Cookware
Mobile
Comics
Jackets
Books
Indoor Fitness
Accessories
Home & Kitchen
```

This provides an interactive way to explore product/category performance.

---

# 38. TARGET ACHIEVEMENT GAUGE

The gauge compares:

```text
Actual Revenue = 41.72M
Target = 54.24M
```

Approximate achievement:

```text
76.9%
```

Remaining gap:

```text
12.52M
```

### Business Interpretation

The gauge provides management with a quick view of progress against the defined revenue target.

---

# 39. BUSINESS INSIGHTS

Based on the dashboard snapshot:

### Insight 1 — Revenue Performance

The dashboard reports approximately:

```text
Total Revenue = 41.72M
```

This is the primary business performance KPI.

---

### Insight 2 — Target Achievement

Actual revenue:

```text
41.72M
```

Target:

```text
54.24M
```

Achievement:

```text
≈ 76.9%
```

The remaining target gap is approximately:

```text
12.52M
```

---

### Insight 3 — Regional Performance

Regional contribution is relatively balanced:

```text
East   = 24.80%
North  = 25.19%
South  = 24.96%
West   = 25.04%
```

No region contributes dramatically more than the others in the displayed snapshot.

---

### Insight 4 — Monthly Performance

Monthly revenue remains around the 7M range for several months, with June showing a higher visible contribution.

This indicates that monthly revenue should be monitored for seasonality and changes in demand.

---

### Insight 5 — Customer Performance

The Top 10 customer table shows customers contributing roughly 97K–130K in visible revenue.

A customer concentration analysis should be performed to determine how much of total revenue comes from the top 10 customers.

---

### Insight 6 — Payment Method

Payment methods show a relatively distributed mix across:

```text
Credit Card
COD
Debit Card
UPI
Net Banking
```

This indicates that the business is not visibly dependent on one payment method in the displayed snapshot.

---

# 40. DATA QUALITY RISKS

Several areas should be validated before treating the dashboard as production-ready.

## Risk 1 — Many-to-Many Customer Relationship

Current:

```text
Orders *:* Customers
```

If Customer_ID is unique in Customers, this should be:

```text
Customers 1:* Orders
```

---

## Risk 2 — Image Relationship

Current:

```text
Products[Sub_Category]
        ↓
Image_Url[Product Name]
```

Potentially incorrect.

Recommended:

```text
Products[Product_Name]
        ↓
Image_Url[Product Name]
```

---

## Risk 3 — Returns Relationship

Current:

```text
Returns 1:1 Orders
```

Validate whether one order can have multiple return records.

---

## Risk 4 — Date Scope

Dashboard title:

```text
Jan–Jun 2024
```

But one trend visual shows:

```text
Jul 2024
Aug 2024
```

This should be corrected or clearly explained.

---

## Risk 5 — Hard-Coded Target

If:

```DAX
Sales Target = 54240000
```

is used, the target cannot be dynamically maintained.

Better approach:

Create a separate target table.

Example:

| Month | Target |
| ----- | -----: |
| Jan   | Target |
| Feb   | Target |
| Mar   | Target |
| Apr   | Target |
| May   | Target |
| Jun   | Target |

---

# 41. RECOMMENDED DASHBOARD IMPROVEMENTS

## KPI Improvements

Add:

```text
Total Orders
Average Order Value
Revenue Growth %
Quantity Growth %
Return Rate %
Gross Margin %
Total Shipping Cost
```

---

# 42. RETURN ANALYSIS PAGE

Create a dedicated Returns page containing:

* Total Returns
* Returned Orders
* Return Rate
* Return Reasons
* Returns by Month
* Returns by Category
* Returns by Product
* Returns by Region

---

# 43. CUSTOMER ANALYSIS PAGE

Recommended visuals:

```text
Top 10 Customers
Revenue by Customer
Quantity by Customer
Average Order Value
Customer Revenue Contribution %
Repeat Customers
Customer Location
```

---

# 44. PRODUCT ANALYSIS PAGE

Recommended visuals:

```text
Top Products by Revenue
Top Products by Quantity
Revenue by Category
Revenue by Sub-Category
Unit Price vs Quantity
Product Margin
Product Return Rate
```

---

# 45. REGIONAL ANALYSIS PAGE

Recommended visuals:

```text
Revenue by Region
Orders by Region
Quantity by Region
AOV by Region
Return Rate by Region
Monthly Revenue by Region
```

---

# 46. POWER BI STAR SCHEMA

The recommended final model should look like:

```text
                 DimCustomer
                     |
                     | 1
                     |
                     | *
                 FactOrders
                /    |     \
               /     |      \
              /      |       \
             ↓       ↓        ↓
       DimProduct  DimDate   DimRegion
```

If Returns is a separate fact/event table:

```text
                 DimCustomer
                     |
                     ↓
                 FactOrders
                /        \
               ↓          ↓
         DimProduct     DimDate


                 FactReturns
                    |
                    ↓
                 DimDate
```

---

# 47. POWER BI DEVELOPMENT PROCESS

The complete project workflow is:

```text
Business Requirement
        ↓
Data Collection
        ↓
Data Understanding
        ↓
Data Cleaning
        ↓
Power Query
        ↓
Data Modeling
        ↓
Relationship Creation
        ↓
DAX Measures
        ↓
KPI Creation
        ↓
Dashboard Development
        ↓
Data Validation
        ↓
Business Insights
        ↓
Testing
        ↓
Publishing
        ↓
Scheduled Refresh
```

---

# 48. TESTING CHECKLIST

Before publishing the dashboard, validate:

### Data

* [ ] No duplicate Customer IDs
* [ ] No duplicate Product IDs
* [ ] No invalid Order IDs
* [ ] No invalid Product IDs
* [ ] No missing Customer IDs
* [ ] No missing Product IDs
* [ ] Correct date types
* [ ] Correct numeric types

### Relationships

* [ ] Customer relationship validated
* [ ] Product relationship validated
* [ ] Date relationship validated
* [ ] Returns relationship validated
* [ ] Image relationship validated

### DAX

* [ ] Total Revenue matches source
* [ ] Total Quantity matches source
* [ ] Total Orders matches source
* [ ] Previous Month calculation validated
* [ ] Target calculation validated
* [ ] Growth % validated

### Dashboard

* [ ] Region slicer works
* [ ] Month slicer works
* [ ] Category selection works
* [ ] All visuals cross-filter correctly
* [ ] Tooltips work
* [ ] Numbers are formatted correctly
* [ ] No unnecessary blank visuals
* [ ] Dashboard works at different screen sizes

---

# 49. PERFORMANCE OPTIMIZATION

To improve Power BI performance:

1. Remove unused columns.
2. Remove unnecessary calculated columns.
3. Prefer DAX measures where possible.
4. Use a proper star schema.
5. Avoid unnecessary many-to-many relationships.
6. Keep relationships single-direction where possible.
7. Disable Auto Date/Time when using a dedicated Date Table.
8. Use appropriate data types.
9. Reduce unnecessary visuals on a page.
10. Optimize complex DAX calculations.
11. Check Performance Analyzer.
12. Reduce high-cardinality columns where appropriate.

---

# 50. REFRESH & DEPLOYMENT

After development:

```text
Power BI Desktop
       ↓
Publish
       ↓
Power BI Service
       ↓
Workspace
       ↓
Dataset / Semantic Model
       ↓
Scheduled Refresh
       ↓
Dashboard / Report
```

### Deployment Checklist

* Configure data source credentials.
* Configure scheduled refresh.
* Validate refresh success.
* Configure workspace permissions.
* Configure Row-Level Security if required.
* Test report in Power BI Service.
* Share report with authorized users.
* Monitor refresh failures.

---

# 51. FUTURE ENHANCEMENTS

The dashboard can be further enhanced with:

### Advanced Analytics

* Customer segmentation
* Product profitability
* Sales forecasting
* Return prediction
* Customer lifetime value
* ABC analysis
* Pareto analysis
* Cohort analysis

### Advanced DAX

* YTD Sales
* MTD Sales
* QTD Sales
* YoY Growth
* Rolling 30-Day Sales
* Rolling 90-Day Sales
* Dynamic Ranking
* Contribution %
* Running Total

---

# 52. INTERVIEW PROJECT EXPLANATION

If the interviewer asks:

### "Tell me about your Power BI project."

You can answer:

> **"I developed a Performance Analysis Dashboard in Power BI to analyze sales and business performance. I worked with multiple tables including Orders, Customers, Products, Returns and Date Table. I performed data preparation and transformation, created relationships using a star-schema approach, and developed DAX measures for KPIs such as Total Revenue, Total Quantity, Average Shipping Cost, Previous Month Revenue and Target Achievement.**
>
> **The dashboard provides regional, country, monthly, customer, category and payment-method analysis. I also created interactive slicers so users can analyze performance dynamically. During the modeling stage, I focused on relationship cardinality and data validation to make sure the calculations were accurate.**
>
> **The main purpose was to convert raw transactional data into an interactive business reporting solution that helps management understand performance and identify areas requiring attention."**

---

# 53. INTERVIEW QUESTIONS YOU MAY GET

### Q1. Why did you use Power BI?

Power BI allows us to connect, transform and model data and create interactive dashboards for business decision-making.

### Q2. Why did you create a Date Table?

A dedicated Date Table allows us to perform reliable time-intelligence calculations such as previous month, YTD, MTD, YoY and rolling-period analysis.

### Q3. Why are relationships important?

Relationships allow data from different tables to interact correctly. Incorrect cardinality or filter direction can result in incorrect aggregations.

### Q4. Why use measures instead of calculated columns?

Measures are calculated dynamically according to the filter context, making them more suitable for interactive KPIs and aggregations.

### Q5. What is a star schema?

A star schema consists of a central fact table connected to multiple dimension tables.

Example:

```text
Orders = Fact Table

Customers = Dimension
Products = Dimension
Date = Dimension
```

### Q6. What is the difference between calculated column and measure?

**Calculated Column:** calculated row by row and stored in the model.

**Measure:** calculated dynamically when the visual is evaluated.

### Q7. How did you calculate Total Revenue?

```DAX
Total Revenue =
SUM(Orders[Sales])
```

### Q8. How did you calculate Total Quantity?

```DAX
Total Quantity =
SUM(Orders[Quantity])
```

### Q9. How did you calculate previous month revenue?

```DAX
Last Month =
CALCULATE(
    [Total Revenue],
    DATEADD(Date_Table[Date], -1, MONTH)
)
```

### Q10. What would you improve in this dashboard?

> "I would validate the relationship cardinalities, particularly the Customer and Returns relationships, correct the product-image mapping if required, add Return Rate and Margin KPIs, move the sales target into a dedicated target table, and ensure the reporting-period filters are consistent."

---

# 54. PROJECT OUTCOME

The Performance Analysis Dashboard provides a centralized solution for monitoring:

```text
Revenue
Quantity
Shipping Cost
Order Performance
Regional Performance
Country Performance
Customer Performance
Product Performance
Payment Methods
Target Achievement
Monthly Trends
```

The dashboard enables business users to move from **high-level KPIs to detailed performance analysis** through interactive filters and visualizations.

---

# 55. FINAL SUMMARY

This Power BI project demonstrates practical knowledge of:

```text
Power BI
Power Query
DAX
Data Modeling
Star Schema
Relationships
Time Intelligence
KPI Development
Data Visualization
Business Analysis
Data Validation
Dashboard Design
```

The project also demonstrates the ability to convert raw transactional data into a **business-focused analytical dashboard** rather than simply creating charts.

---

# 56. FINAL RECOMMENDED PROJECT STRUCTURE

For a polished portfolio/project submission, structure the Power BI file as:

```text
📁 Performance Analysis Dashboard
│
├── 📊 01 Executive Overview
│
├── 📈 02 Sales Analysis
│
├── 👥 03 Customer Analysis
│
├── 📦 04 Product Analysis
│
├── 🌎 05 Regional Analysis
│
├── 🔄 06 Returns Analysis
│
├── 🎯 07 Target & Performance
│
├── 📋 08 Data Model
│
└── 📚 Documentation
```

### Core KPI Layer

```text
Total Revenue
Total Orders
Total Quantity
Average Order Value
Revenue Growth %
Quantity Growth %
Return Rate %
Gross Margin %
Target Achievement %
```
