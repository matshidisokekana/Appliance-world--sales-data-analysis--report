
# 📊 ApplianceWorld Sales Data Analysis

## 🧠 Project Overview

This project analyzes sales data for **ApplianceWorld**, a U.S.-based company that sells **LG, Whirlpool, and Samsung appliances**.  
The goal was to combine data from multiple sources, build a proper data model, and generate business insights using **Excel and Power BI**.

This project demonstrates skills in:

- Data cleaning and preparation  
- Data modeling and relationships  
- DAX calculations and time intelligence  
- Business-focused data analysis  
- Data visualization and KPI reporting  

---

## 🗂️ Dataset Description

The dataset was provided in **ApplianceWorld.xlsx** and consisted of **6 worksheets** containing:

- Sales transactions  
- Salesperson information  
- Product (appliance) details  
- Regional and state data  
- Date-related fields (Year, Month No)

These sheets were combined to create a structured analytical model.

---

## 🧹 Step 1: Data Cleaning (Microsoft Excel)

Before importing into Power BI, the data was cleaned and standardized in **Excel**.

### Cleaning Actions Performed

- Standardized column names (e.g., `SalesPersonID`, `Product Code`, `Units Sold`, `Revenue`)
- Removed blank and duplicate rows  
- Checked for missing or invalid values  
- Ensured correct data types:
  - Revenue & Cost → Currency  
  - Units Sold → Whole Number  
  - Year & Month → Whole Number  
  - IDs → Text  

This ensured the data was consistent and ready for modeling.

---

## 🧩 Step 2: Data Modeling (Power BI)

After cleaning, the data was imported into **Power BI** and structured into a **star schema**.

### Main Tables

| Table Name   | Description |
|-------------|-------------|
| **Sales** | Transaction-level sales data |
| **SalesPerson** | Sales staff details (Region, State) |
| **Appliances** | Product details (Brand, Appliance Type) |
| **Dates** | Calendar/date dimension table |

---

### 🔗 Relationships

| From Table | Column | To Table | Column | Relationship |
|-----------|--------|----------|--------|--------------|
| Sales | SalesPersonID | SalesPerson | SalesPersonID | Many-to-One |
| Sales | Product Code | Appliances | Product Code | Many-to-One |
| Sales | SalesDate | Dates | Date | Many-to-One |

This structure allows efficient filtering by **time, product, and geography**.

---

## 📅 Date Column Creation

Since the raw data included only **Year** and **Month No**, a proper date field was created in Power BI.

```DAX
SalesDate = DATE(Sales[Year], Sales[Month No], 1)
````

A full Date Table was then created:

```DAX
Dates = 
ADDCOLUMNS(
    CALENDAR (DATE(2023,1,1), DATE(2024,12,31)),
    "Year", YEAR([Date]),
    "Month No", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMM"),
    "Quarter", "Q" & FORMAT([Date], "Q")
)
```

---

## 📐 Key Measures (DAX)

### 💰 Revenue & Profit

```DAX
Total Revenue = SUM(Sales[Revenue])

Total Cost = SUM(Sales[Cost of Sales])

Total Profit = [Total Revenue] - [Total Cost]
```

---

### 📦 Units Sold

```DAX
Total Units Sold = SUM(Sales[Units Sold])
```

---

### 📈 Year-over-Year (YoY) Growth

```DAX
Revenue Previous Year = 
CALCULATE(
    [Total Revenue],
    SAMEPERIODLASTYEAR(Dates[Date])
)

YoY Growth % = 
DIVIDE(
    [Total Revenue] - [Revenue Previous Year],
    [Revenue Previous Year]
)
```

---

## 📊 Business Questions & Analysis

### ✅ Q1. Total Revenue in Q1 2023 by Region & Salesperson

**Method:**

* Filtered Year = 2023
* Filtered Quarter = Q1
* Aggregated revenue by Region and Salesperson

**Insight:** Identifies which regions and staff drove early-year performance.

---

### ✅ Q2. Top 5 Best-Selling Appliances (2023–2024 Combined)

**Method:**

* Combined revenue from both years
* Grouped by appliance
* Ranked by Total Revenue

```DAX
Product Rank = 
RANKX(
    ALL(Appliances[Appliance]),
    [Total Revenue],
    ,
    DESC
)
```

---

### ✅ Q3. Top 3 States with Highest YoY Revenue Growth

**Method:**

* Used YoY Growth % measure
* Grouped by State
* Applied Top N filter (Top 3)

**Insight:** Highlights the fastest-growing markets, not just the biggest ones.

---

### ✅ Q4. Top 5 Salespersons in Those 3 States (Based on 2024 Profit)

**Method:**

* Filtered to Top 3 growth states
* Filtered Year = 2024
* Ranked salespersons by Total Profit

```DAX
Salesperson Profit Rank =
RANKX(
    ALL(SalesPerson[Sales Person]),
    [Total Profit],
    ,
    DESC
)
```

---

### ✅ Q5. 3 Products with Biggest Decline in Units Sold (Last 6 Months)

```DAX
Units Last 6 Months = 
CALCULATE(
    [Total Units Sold],
    DATESINPERIOD(Dates[Date], MAX(Dates[Date]), -6, MONTH)
)

Units Previous 6 Months = 
CALCULATE(
    [Total Units Sold],
    DATESINPERIOD(Dates[Date], MAX(Dates[Date]) - 180, -6, MONTH)
)

Units Decline = [Units Last 6 Months] - [Units Previous 6 Months]
```

Products were sorted ascending by **Units Decline** to identify the biggest drops.

---

## 📈 Dashboard & Visualizations

The Power BI report includes:

* KPI cards (Revenue, Profit, Units Sold)
* Bar charts for product and salesperson performance
* Line charts for time-based trends
* Matrix tables for regional breakdowns
* Top N visuals for ranking analysis

---

## 🛠️ Tools Used

| Tool                | Purpose                              |
| ------------------- | ------------------------------------ |
| **Microsoft Excel** | Data cleaning and preparation        |
| **Power BI**        | Data modeling and dashboard creation |
| **DAX**             | Measures, time intelligence, ranking |

---

## 🎯 Skills Demonstrated

* Data cleaning and transformation
* Relational data modeling
* DAX calculations and time intelligence
* Business KPI development
* Insight-driven dashboard design

---

## 📌 Conclusion

This project showcases the end-to-end analytics workflow — from raw Excel data to a fully modeled Power BI dashboard delivering actionable business insights for ApplianceWorld.

