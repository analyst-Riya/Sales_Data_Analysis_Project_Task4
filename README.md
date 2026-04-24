# 🛒 Superstore Sales Analysis — End to End Data Analytics Project
---
## 📋 Table of Contents
- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Tech Stack](#tech-stack)
- [Project Workflow](#project-workflow)
- [Data Cleaning](#data-cleaning)
- [Database Setup](#database-setup)
- [Data Modeling](#data-modeling)
- [DAX Measures](#dax-measures)
- [Dashboard](#dashboard)
- [Business Insights](#business-insights)
- [Conclusion](#conclusion)

---

## 📌 Project Overview

This project is a part of my internship at **MainCrafts Technology**. This is an end-to-end data analytics project built on the **Superstore Sales Dataset**. The project covers the complete data analytics pipeline — from raw data cleaning in Excel, importing into MySQL, building a star schema in Power BI, creating DAX measures, and finally delivering an executive-level interactive sales dashboard.

---

## 📂 Dataset

| Property | Details |
|----------|---------|
| Dataset | Superstore Sales Data |
| Rows | 9,986 |
| Columns | 12 |
| Period | 2014 – 2017 |
| Source | Sample Superstore (Tableau/Kaggle) |

**Columns:**
`Order_ID`, `Order_Date`, `Customer_ID`, `Customer_Name`, `Segment`, `Region`, `Product_ID`, `Category`, `Sub_Category`, `Sales`, `Quantity`, `Profit`

---

## 🛠 Tech Stack

| Tool | Purpose |
|------|---------|
| Excel | Data Cleaning |
| MySQL Workbench | Database Storage |
| Power BI Desktop | Data Modeling & Dashboard |
| DAX | Measures & Calculations |
| GitHub | Version Control |

---

## 🔄 Project Workflow

```
Raw Excel File
      ↓
Python (Data Cleaning)
      ↓
Split into 3 CSV Files (Orders, Customers, Products)
      ↓
MySQL (Import via Workbench)
      ↓
Power BI (Star Schema + DAX)
      ↓
Executive Sales Dashboard
      ↓
Business Insight Report
```

---

## 🧹 Data Cleaning

Performed using **Python (Pandas)**:

- Removed **7 duplicate Order ID + Product ID** pairs by merging and summing Sales, Quantity and Profit
- Replaced **negative profit values** with 0
- Removed **BOM characters** from CSV headers to fix MySQL import errors
- Split the master dataset into **3 normalized CSV files**:
  - `orders.csv` — 9,986 rows (fact table)
  - `customers.csv` — 793 unique customers (dimension table)
  - `products.csv` — 9,986 rows (dimension table)

---

## 🗄 Database Setup

Created a **superstore** database in MySQL with 3 tables:

```sql
CREATE TABLE Orders (
    Order_ID VARCHAR(50),
    Order_Date DATE,
    Customer_ID VARCHAR(50),
    Product_ID VARCHAR(50),
    Sales DECIMAL(10,2),
    Profit DECIMAL(10,2),
    Quantity INT
);

CREATE TABLE Customers (
    Customer_ID VARCHAR(50),
    Customer_Name VARCHAR(50),
    Region VARCHAR(50),
    Segment VARCHAR(50)
);

CREATE TABLE Products (
    Product_ID VARCHAR(50),
    Category VARCHAR(50),
    Sub_Category VARCHAR(50)
);
```

---

## 📊 Data Modeling

Built a **Star Schema** in Power BI:

```
Customers_Table
      |
      | (Many to 1)
      |
Orders_Table ——— (Many to 1) ——— Products_Table
      |
      | (Many to 1)
      |
Calendar_Table
```

**Calendar Table** created using DAX:

```dax
Calendar = 
ADDCOLUMNS(
    CALENDAR(MIN(Orders_Table[Order_Date]), MAX(Orders_Table[Order_Date])),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMM"),
    "Month Number", MONTH([Date])
)
```

---

## 📐 DAX Measures

```dax
Total Sales = SUM(Orders_Table[Sales])

Total Profit = SUM(Orders_Table[Profit])

Profit Margin = DIVIDE(SUM(Orders_Table[Profit]), SUM(Orders_Table[Sales]))

Total Orders = DISTINCTCOUNT(Orders_Table[Order_ID])

Growth % = 
DIVIDE(
    [Total Sales] - CALCULATE([Total Sales], SAMEPERIODLASTYEAR(Calendar[Date])),
    CALCULATE([Total Sales], SAMEPERIODLASTYEAR(Calendar[Date]))
)
```

---

## 📈 Dashboard
The Dashboard overview:
<img width="1438" height="814" alt="Sales_Dashboard" src="https://github.com/user-attachments/assets/9f0d42d1-5f3d-4521-a982-f03803aaf510" />


The executive dashboard includes:

**KPI Cards:**
- Total Sales: $2.30M
- Total Profit: $0.44M
- Profit Margin: 19.27%
- Total Orders: 5,010
- Growth %: 46.89%

**Visuals:**
- 📉 Line Chart — Sales Trend by Month
- 📊 Column Chart — Sales by Region
- 📊 Bar Chart — Profit by Category
- 🍩 Donut Chart — Segment Distribution
- 📋 Table — Top 10 Customers by Sales

**Slicers:**
- Year, Month, Region, Category, Segment

---

## 💡 Business Insights

1. **Business is growing consistently** — 46.89% growth rate over 4 years
2. **West region** drives the highest revenue (~$0.8M)
3. **Technology** is the most profitable category
4. **Strong Q4 seasonality** — sales peak in Nov–Dec
5. **Consumer segment** contributes 50.56% of total revenue
6. **Furniture** has lowest profit margins despite decent sales — needs pricing review

---

## 📝 Conclusion

The Superstore business shows strong and consistent growth over 2014–2017. West region and Consumer segment are the primary revenue drivers. Technology yields the highest margins while Furniture needs cost restructuring. Management should focus on Q4 promotional campaigns, top customer retention, and Technology category expansion for sustained profitability.

---

## 📁 Project Structure

```
superstore-sales-analysis/
│
├── data/
│   ├── orders.csv
│   ├── customers.csv
│   └── products.csv
│
├── notebooks/
│   └── data_cleaning.py
│
├── dashboard/
│   └── Sales_Dashboard.pbix
│
└── README.md
```

---

## 👤 Author

**Riya - Data Analyst**
- GitHub: analyst-Riya (https://github.com/analyst-Riya)
- LinkedIn: https://www.linkedin.com/in/riya-442771384?utm_source=share_via&utm_content=profile&utm_medium=member_android
---
