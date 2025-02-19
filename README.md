# 📊 ReachMax Call Center Performance Dashboard- This is a simulated dashboard

## 🔍 Project Overview

As a **developer at ReachMax Call Center**, I built this **Power BI dashboard** to help the management team track **key performance metrics** and make **data-driven decisions**.

The dashboard provides insights into:
- 📈 **Total Calls** handled by agents
- 📞 **Calls Reached** (successful connections)
- 💼 **Deals Closed**
- 💰 **Deal Value** (revenue generated)
- 🏆 **Agent Performance**

To enhance decision-making, the dashboard includes a **dynamic KPI comparison feature** that allows users to track performance **vs Last Month (LM), Last Year (LY), and Last Quarter (Qtr)**.

---

## 🎯 Project Goals

We needed a **comprehensive dashboard** to:

✅ **Monitor overall call center performance** 📊  
✅ **Compare key metrics over time** ⏳  
✅ **Track individual agent performance** 🏆  
✅ **Visualize revenue trends from closed deals** 💰  
✅ **Highlight performance improvements or declines** with **conditional formatting** 🎨  

---

## 📌 Data Preparation

### **1️⃣ Creating the Calendar Table**
The dataset includes **historical call center data**, which required a well-structured **Calendar Table** for proper time-based analysis. I created the **Calendar Table** in Power BI using `CALENDARAUTO()`, adding key time attributes:

```DAX
Calendar = 
ADDCOLUMNS(
    CALENDARAUTO(),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMM"),
    "MN", MONTH([Date]),
    "Weekday", FORMAT([Date], "ddd"),
    "WN", WEEKDAY([Date], 1),  -- Sunday = 1, Saturday = 7
    "Qtr", "Q_" & QUARTER([Date]),
    "Weektype", IF(WEEKDAY([Date], 1) IN {1, 7}, "Weekend", "Weekday")
)
```

### **2️⃣ Enabling KPI Comparison (vs LM, LY, Qtr)**
To allow users to switch between different comparison periods, I created a disconnected table **"Switch for KPI"** that powers the slicer:

| Compare |
|---------|
| vs LY   |
| vs LM   |
| vs Qtr  |

This slicer enables **dynamic KPI switching** in the dashboard.

---

## 📌 DAX Measures for KPI Calculation

### 🔹 **Total Calls vs Previous Periods**
```DAX
Prev Month Calls = 
CALCULATE([Total calls], PREVIOUSMONTH('Calendar'[Date]))

Prev Qtr Calls = 
CALCULATE([Total calls], PREVIOUSQUARTER('Calendar'[Date]))

Prev Year Calls = 
CALCULATE([Total calls], SAMEPERIODLASTYEAR('Calendar'[Date]))
```

### 🔹 **Dynamic Calls Comparison Based on Slicer Selection**
```DAX
Calls Compare = 
VAR _Select = SELECTEDVALUE('Switch for KPI'[Compare])
RETURN 
SWITCH(
    _Select,
    "vs LY", [Total calls] - [Prev Year Calls],
    "vs LM", [Total calls] - [Prev Month Calls],
    "vs Qtr", [Total calls] - [Prev Qtr Calls],
    [Total calls]  -- Default: show current total calls
)
```

### 🔹 **Conditional Formatting for KPI Cards**
**Green** → Calls increased 📈  
**Red** → Calls decreased 📉  

```DAX
CF Total Calls = 
VAR _Check = SELECTEDVALUE('Switch for KPI'[Compare]) 
VAR _Switch = 
    SWITCH(
        TRUE(),
        _Check = "vs LM", [Total calls] - [Prev Month Calls],
        _Check = "vs Qtr", [Total calls] - [Prev Qtr Calls],
        _Check = "vs LY", [Total calls] - [Prev Year Calls],
        0
    )
RETURN
IF(
    _Switch > 0, "Green", "Red"
)
```

---

## 📌 Power BI Report Design

### 🏆 **Dashboard Features**
✅ **KPI Card**: Displays total calls dynamically based on slicer selection  
✅ **Slicer**: Allows users to toggle between **vs LY, vs LM, and vs Qtr**  
✅ **Agent Performance Table**: Tracks calls handled, deals closed, and revenue per agent  
✅ **Deal Value Trends**: Shows revenue generated from closed deals  
✅ **Call Volume Breakdown**: Analyzes total vs successful calls  
✅ **Conditional Formatting**: Highlights positive (**green**) and negative (**red**) trends  

---

## 📌 View the Full Dashboard
📄 **[Download Dashboard Report *https://github.com/Mutheu-nzuma/Call-centre-bi-dashboard/edit/main/README.md

---



