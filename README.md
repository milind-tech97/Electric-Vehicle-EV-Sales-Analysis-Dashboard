# ⚡ Electric Vehicle (EV) Sales Analysis Dashboard

A Power BI dashboard providing a comprehensive analysis of Electric Vehicle (EV) sales across India, segmented by EV makers, fiscal years, states, and vehicle categories. The report includes dynamic visuals powered by advanced DAX measures, Top/Bottom N logic, CAGR calculations, and interactive slicers.

---

## 📝 Short Description

A detailed Power BI dashboard that analyzes EV sales growth, market share, revenue, CAGR, penetration rates, and projected sales for 2030. It features dynamic Top/Bottom N selection, state-level insights, maker-level comparisons, and trend analysis across months, quarters, and years.

---

# 📊 Project Overview

This dashboard provides two primary analytical views:

## **1️⃣ EVs Sold by Makers**  
*(Page 1 of EV Dashboard)*  
- Shows EV sales trends for major makers such as **OLA Electric, Ather, TVS, Hero Electric, Ampere**.  
- Visuals include:  
  - **Quarterly trends (2022–2024)**  
  - **Monthly trends (Apr–Dec)**  
  - **Market share vs YoY growth**  
  - **EV revenue of 2W & 4W makers**  
  - **CAGR of top EV companies (e.g., BMW India 1140%, Volvo 971%)**  
  - **Category distribution: 2W (92.6%) vs 4W (7.4%)**  
- Source reference: EV Makers page :contentReference[oaicite:4]{index=4}  

## **2️⃣ EVs Sold by States**  
*(Page 2 of EV Dashboard)*  
- Breakdown of sales across Indian states.  
- Key metrics:  
  - **Total Vehicles Sold: 57M**  
  - **Total EVs Sold: 2.1M**  
  - **State-wise penetration rate (2022–2024)**  
  - **Projected EV Sales for 2030 (54.21M)**  
  - **Comparative YoY growth for 2-wheeler EVs**  
  - **Top 5 states by penetration: Goa, Karnataka, Delhi, Kerala, Maharashtra**  
- Source reference: State Analysis page :contentReference[oaicite:5]{index=5}  

---

# 🎯 Key Features

### ⭐ Dynamic Top/Bottom N Analysis
- Works for EV makers (Top N EV Makers)  
- Works for States (Top N Penetration Rate / CAGR)

### ⭐ Market Share & YoY Difference Analysis
- Market share vs growth rate scatter chart  
- EV revenue difference: current year vs last year

### ⭐ Trend Visuals
- Quarterly sales  
- Monthly sales  
- State-wise penetration trends over 3 years

### ⭐ 2030 Projection Model
- Uses CAGR-based forecasting  
- State-wise projections  
- Maker-wise projections  

---

# 🧮 DAX Measures Used

Your project includes **40+ advanced DAX measures** from the DAX Notes files:  
✔ Top/Bottom N titles  
✔ EV Sold LY  
✔ YoY growth %  
✔ CAGR for Makers  
✔ CAGR for States  
✔ 2W & 4W revenue models  
✔ Projected 2030 EV sales  
✔ Dynamic chart titles  
✔ Market share ranking  
✔ Conditional ranking logic  

Some of the *important* DAX formulas include:

### **1️⃣ CAGR for EV Makers**
```DAX
CAGR for EV Makers =
VAR FirstYear = CALCULATE(MIN(Dates[fiscal_year]),ALL(Dates))
VAR LastYear = CALCULATE(MAX(Dates[fiscal_year]),ALL(Dates))
VAR FirstYearEVSales =
    CALCULATE([EV Sold Makers],Dates[fiscal_year]=FirstYear)
VAR LastYearEVSales =
    CALCULATE([EV Sold Makers],Dates[fiscal_year]=LastYear)
VAR NumberOfYears = LastYear-FirstYear
RETURN
IF(NumberOfYears>0,
    (DIVIDE(LastYearEVSales,FirstYearEVSales,0)^DIVIDE(1,NumberOfYears,0))-1,
    BLANK()
)
```

### **2️⃣ Projected EV Sales 2030**
```DAX
Projected EV Sales 2030 =
VAR CurrentYearEVSales =
    CALCULATE([EV_Sold_State], dates[fiscal_year]=2024)
VAR CAGRValue = [CAGR for EV States]
VAR YearsToProject = 2030 - MAX(dates[fiscal_year])
RETURN
IF(
    NOT ISBLANK([Ranking_States_Penetration]) && NOT ISBLANK(CAGRValue),
    CurrentYearEVSales * ((1 + CAGRValue) ^ YearsToProject),
    BLANK()
)
```
#### **3️⃣ Market Share Ranking (Top/Bottom N)**
``` DAX
Ranking_Makers_Marketshare =
VAR _filteredMakers =
    CALCULATETABLE(
        FILTER(ALLSELECTED(EV makers[maker]), [MarketShare EV Makers] > 0),
        ALLSELECTED(Dates)
    )
VAR _top = RANKX(_filteredMakers, [MarketShare EV Makers], , DESC, DENSE)
VAR _bottom = RANKX(_filteredMakers, [MarketShare EV Makers], , ASC, DENSE)
VAR _rank =
    SWITCH(TRUE(),
        SELECTEDVALUE('TopBottom'[Value])="Top", _top,
        SELECTEDVALUE('TopBottom'[Value])="Bottom", _bottom
    )
RETURN
IF(_rank <= SELECTEDVALUE('TopN Slicers for Makers'[TopN Makers]),
    [MarketShare EV Makers]
)
```

## 📌 Visuals in the Dashboard

### 🚗 **EV Makers Visuals (Page 1)**  
- **Quarterly Sales (2022–2024)**  
  Shows EV sales progression for top makers like OLA Electric, Ather, TVS, Hero Electric, and Ampere.  
- **Monthly Sales Trends**  
  Tracks monthly EV units sold across fiscal years to identify seasonality and demand cycles.  
- **Market Share vs YoY Growth (Scatter Chart)**  
  Compares each maker’s market share with its year-over-year growth performance.  
- **CAGR Bar Chart**  
  Displays compounded annual growth rates for top EV makers such as BMW India, Volvo India, BYD, OLA Electric, etc.  
- **Donut Chart – EV Sales by Vehicle Category**  
  Breakdown of **2-Wheelers (92.6%) vs 4-Wheelers (7.4%)**.  
- **Revenue Cards – 2W & 4W Makers**  
  Revenue insights for 2-wheeler and 4-wheeler EV manufacturers.  
- **EV Sold Ranking**  
  Ranks top EV makers based on cumulative EV sales.

---

### 🏙️ **EV State Visuals (Page 2)**  
- **State-Wise EV Penetration Rate Table (2022–2024)**  
  Displays penetration %, year-over-year changes, and decline indicators for each state.  
- **2030 Projected EV Sales**  
  Forecasts EV sales using CAGR for each state up to the year 2030.  
- **Top 5 States by Penetration Rate**  
  Highlights leading EV adoption states like Goa, Karnataka, Delhi, Kerala, and Maharashtra.  
- **YoY Decline Matrix**  
  Evaluates penetration trends and flags declining states.  
- **Revenue Analysis for 2W & 4W EVs**  
  Calculates revenue using EV units × pricing logic for each category.  
- **Scatter Chart – Penetration vs EV Sales**  
  Visual relationship between total EV sales and penetration rate among major states.  
- **Comparative “Delhi vs Karnataka” EV Sales**  
  Donut visualization comparing EV adoption in two major EV markets.

## 🏁 Conclusion

The **Electric Vehicle Sales Analysis Dashboard** provides a complete and data-driven view of India’s evolving EV ecosystem — from manufacturer performance to state-level penetration insights.  
Leveraging advanced DAX calculations, dynamic Top/Bottom N logic, interactive visuals, forecasting models, and YoY/CAGR analytics, the dashboard empowers users to uncover meaningful trends and patterns in EV adoption.

### 🔍 Key Insights Enabled by This Dashboard:
- **Identify which EV makers are growing the fastest** using YoY and CAGR metrics.  
- **Analyze EV adoption across states** through penetration rate trends (2022–2024).  
- **Understand overall EV market share distribution** among top manufacturers.  
- **Track category-wise growth** in 2-Wheelers and 4-Wheelers revenue and sales.  
- **Forecast future EV adoption** with projected EV sales up to **2030**.  

Overall, this dashboard serves as a powerful analytical tool for **manufacturers, policymakers, market researchers, investors, and EV ecosystem stakeholders**, enabling smarter decision-making backed by clear, interactive, and actionable insights.
