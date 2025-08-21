# KPI Dashboard Migration: QuickSight → Power BI

**Goal:** Prove cross-platform BI skills by rebuilding a **Sales & Operations KPI dashboard** originally designed in **AWS QuickSight**, using **Power BI** (DAX + Power Query + SQL).

---

## 📊 KPIs
- **Revenue** = Sum of `quantity * unit_price`
- **Orders** = Distinct count of `order_id`
- **Conversion Rate** = Orders / Sessions
- **Top Products** = Ranked by Revenue or Quantity
- *(Optional)* Average Order Value, YoY Growth %

---

## 📂 Data
Sample dataset provided in both **SQL script** and **CSV format**.

**Tables:**
- `products(product_id, product_name, category)`
- `sales(order_id, order_date, region, product_id, quantity, unit_price)`
- `traffic(date, region, sessions)`

➡️ CSVs included for quick import: `products.csv`, `sales.csv`, `traffic.csv`

---

## ▶️ Run Locally

### Option A: Using SQL
1. Create a database (SQL Server / PostgreSQL).  
2. Run `sample_data.sql` to create & seed tables.  
3. In **Power BI Desktop** → *Get Data* → connect to DB → import the 3 tables.

### Option B: Using CSVs (quick start)
1. In **Power BI Desktop** → *Get Data* → *Text/CSV*.  
2. Load `products.csv`, `sales.csv`, `traffic.csv`.

---

## 🧮 Power BI Measures (DAX)

...DAX
Revenue = SUMX(sales, sales[quantity] * sales[unit_price])
Orders = DISTINCTCOUNT(sales[order_id])
Sessions = SUM(traffic[sessions])
Conversion Rate = DIVIDE([Orders], [Sessions])
Average Order Value = DIVIDE([Revenue], [Orders])
Revenue YoY % =
    DIVIDE(
        [Revenue] - CALCULATE([Revenue], DATEADD('Date'[Date], -1, YEAR)),
        CALCULATE([Revenue], DATEADD('Date'[Date], -1, YEAR))
    )
---

## 🔄 Notes on Migration (QuickSight → Power BI)

Calculated fields → DAX measures

SPICE dataset → Power BI Import Model

Filters / controls → Slicers

Top N visuals → DAX + Filters (replicates QS “Top movers”)

---

## 📸 Screenshots

Dashboard_Overview.jpg

Top_Products.jpg

---

## 📜 License

MIT
