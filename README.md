# 📊 Super Store Sales & Profit Dashboard — Power BI

A multi-page interactive Power BI dashboard built on the Sample Superstore dataset, delivering deep insights into sales performance, profitability, shipping trends, regional breakdowns, and segment analysis.

---

## 📸 Dashboard Pages

### Page 1 — Sales Overview
![Sales Overview](assets/Dashboard%201.png)

### Page 2 — Sales Insight (YTD Analysis)
![Sales Insight](assets/Dashboard%202.png)

### Page 3 — Profit Analysis
![Profit Analysis](assets/Dashboard%203.png)

---

## 🎯 Project Overview

This dashboard transforms raw Super Store transactional data into a 3-page executive-ready report. Each page focuses on a distinct analytical lens — overview metrics, year-to-date sales trends, and deep-dive profitability — enabling business stakeholders to make faster, smarter decisions.

---

## 📈 Key Metrics at a Glance

| Metric | Value |
|---|---|
| Total Sales | **$2.30M** |
| Total Profit | **$286.40K** |
| Profit Margin % | **12.03%** |
| Quantity Sold | **38K units** |
| Total Discount | **$1.56K** |
| Total Orders | **9,994** |

---

## ✨ Dashboard Pages & Features

### 🔵 Page 1 — Sales Overview
- **KPI Cards** — Max, Min, and Average Sales
- **Category-wise Sales & Profit Table** — Sub-category breakdown with totals
- **Ship Mode Based Orders** — Count, Quantity, and Discount by shipping class
- **Regional Sales Bar Chart** — West (0.73M) leads, followed by East (0.68M)
- **West Region Sales KPIs** — Max, Min, Average for the West region
- **Count Data** — Total Rows, Distinct Count, DCount No Blank, Country Count

### 🌙 Page 2 — Sales Insight (YTD)
- **YTD KPI Banners** — Total Sales (3.58M), Avg Sales (1.08K), Products Sold (12K)
- **Month-to-Date Sales** — Real-time MTD tracking with YoY % change
- **Weekly Sales Trend Line** — Total sales and max point by week number
- **Ship Mode Donut** — Standard Class dominates at 54.35%
- **Segment Donut** — Consumer (45.6%), Corporate (32.52%), Home Office (21.87%)
- **State-level Map** — Product sold by US state (Bing Maps)
- **Sub-category Sales Trend Table** — YTD Avg Price, Products Sold, YoY Sales %
- **Interactive Filters** — Region, Category, Year, City slicers

### 🟡 Page 3 — Profit Analysis
- **Profit by Ship Mode** — Standard Class leads at 164K
- **Profit Analysis by Region** — Central at -24K (loss); West highest at 70K
- **Decomposition Tree** — Profit drilled by Segment → Category → Sub-category
- **Bottom 5 Cities** — Philadelphia (-14K), Houston (-10K), Chicago/Lancaster/San Antonio (-7K each)
- **Profit Analysis by State (Choropleth Map)** — State-level heat map
- **State Profit Table** — Texas (-33,688 margin), Illinois (-19,270), Pennsylvania (-5,048)
- **Profit by Category** — Technology (145K) vs Office Supplies (122K) vs Furniture (18K)
- **Filters** — Category, Sub-Category, Region, State slicers

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Power BI Desktop | Report design, visuals, interactivity |
| Power Query Editor | Data import, cleaning, type fixing |
| DAX | KPIs, YTD, MTD, YoY, profit margin measures |
| Bing Maps | Geographic visualizations |
| Data Modeling | Relationships across fact/dimension tables |

---

## 🔑 DAX Measures Used

All measures are organized under a dedicated **Calculation** table for clean model management.

### 📊 Sales Measures
```dax
Total Sales = SUM(Orders[Sales])

YTD Sales = TOTALYTD(SUM(Orders[Sales]), 'Date'[Date])

YTD Avg Sales = AVERAGEX(DATESYTD('Date'[Date]), [Total Sales])

MTD Total Sales = TOTALMTD(SUM(Orders[Sales]), 'Date'[Date])

MTD Avg Sales = AVERAGEX(DATESMTD('Date'[Date]), [Total Sales])

Sales Last Y... = CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Date'[Date]))

Average Sales = AVERAGE(Orders[Sales])
```

### 📦 Product & Quantity Measures
```dax
YTD Product S... = TOTALYTD(SUM(Orders[Quantity]), 'Date'[Date])

YoY Products ... = 
VAR CY = [YTD Product S...]
VAR PY = CALCULATE([YTD Product S...], SAMEPERIODLASTYEAR('Date'[Date]))
RETURN DIVIDE(CY - PY, PY, 0)

YTD Avg Price = DIVIDE([YTD Sales], [YTD Product S...], 0)

GT YTD Sales = CALCULATE([YTD Sales], ALL('Date'))
```

### 💰 Profit Measures
```dax
Total Profit = SUM(Orders[Profit])

Profit Margin = DIVIDE([Total Profit], [Total Sales], 0) * 100

MTD Total Pro... = TOTALMTD(SUM(Orders[Profit]), 'Date'[Date])

Overall Profit ... = CALCULATE([Total Profit], ALL(Orders))
```

### 📈 YoY & Trend Measures
```dax
YoY Sales % = 
VAR CY = [YTD Sales]
VAR PY = CALCULATE([YTD Sales], SAMEPERIODLASTYEAR('Date'[Date]))
RETURN DIVIDE(CY - PY, PY, 0)

YoY Avg Sales ... = 
VAR CY = [YTD Avg Sales]
VAR PY = CALCULATE([YTD Avg Sales], SAMEPERIODLASTYEAR('Date'[Date]))
RETURN DIVIDE(CY - PY, PY, 0)

YoY Sales Cha... = [YTD Sales] - CALCULATE([YTD Sales], SAMEPERIODLASTYEAR('Date'[Date]))

MoM% = 
VAR CM = [MTD Total Sales]
VAR PM = CALCULATE([MTD Total Sales], DATEADD('Date'[Date], -1, MONTH))
RETURN DIVIDE(CM - PM, PM, 0)
```

### 🔢 Count & Utility Measures
```dax
Total Rows = COUNTROWS(Orders)

Distinct Count = DISTINCTCOUNT(Orders[Order ID])

DCount No Bl... = CALCULATE(DISTINCTCOUNT(Orders[Order ID]), Orders[Order ID] <> BLANK())

Difference in ... = [YTD Sales] - [Sales Last Y...]

Previous YTD ... = CALCULATE([YTD Sales], SAMEPERIODLASTYEAR('Date'[Date]))

Previous avg ... = CALCULATE([Average Sales], SAMEPERIODLASTYEAR('Date'[Date]))
```

### 🎨 Conditional Formatting
```dax
colour  = IF([YoY Sales %] >= 0, "Green", "Red")
colour 1 = IF([YoY Avg Sales ...] >= 0, "Green", "Red")
colour 2 = IF([YoY Products ...] >= 0, "Green", "Red")
Colour 3 = IF([Profit Margin] >= 0, "Green", "Red")
```

---

## 💡 Key Insights

- 🏆 **West region** leads with $0.73M in sales; **Central** is the only loss-making region (-$24K profit)
- 📦 **Standard Class** shipping is most used (54.35% of orders) and generates the highest profit (164K)
- 🛍️ **Consumer segment** drives nearly half of all sales (45.6%)
- ⚠️ **Texas** has the worst profit margin at -33,688% — likely driven by high discounting
- 📉 **Tables** and **Bookcases** sub-categories consistently show negative profit margins
- 📈 **Binders** top the sub-category YoY growth chart at 45.23% increase

---

## 📁 Repository Structure

```
sales-profit-dashboard-powerbi/
│
├── Super_Store.pbix                          # Main Power BI report (3 pages)
├── data/
│   └── Sample_Superstore.xlsx               # Source dataset
├── assets/
│   ├── dashboard-page1-overview.png         # Sales Overview screenshot
│   ├── dashboard-page2-sales-insight.png    # YTD Sales Insight screenshot
│   └── dashboard-page3-profit-analysis.png  # Profit Analysis screenshot
├── .gitignore
└── README.md
```

---

## 🚀 How to Open

1. Download [Power BI Desktop](https://powerbi.microsoft.com/desktop/) — free
2. Clone this repo:
   ```bash
   git clone https://github.com/YOUR_USERNAME/sales-profit-dashboard-powerbi.git
   ```
3. Open `Super_Store.pbix` in Power BI Desktop
4. If prompted, update data source to `data/Sample_Superstore.xlsx`
5. Click **Refresh** and explore all 3 dashboard pages!

---

## 📬 Contact

**Naveen Krishna Venigandla**  
📧 naveenkrishna.v13@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/naveen-krishna-324b341bb)
