# KPI-Dashboard-Migration
Power BI remake of an AWS QuickSight Sales &amp; Operations KPI dashboard (Revenue, Orders, Conversion Rate, Top Products) using SQL + DAX + Power Query. Includes sample data, migration notes, and README.
# KPI Dashboard Migration: QuickSight → Power BI

**Goal:** Prove cross‑platform BI skills by rebuilding a Sales & Operations KPI dashboard originally designed in **AWS QuickSight** using **Power BI** (DAX + Power Query).

## KPIs
- **Revenue** = Sum of `quantity * unit_price`
- **Orders** = Distinct count of `order_id`
- **Conversion Rate** = `Orders / Sessions`
- **Top Products** = Ranked by Revenue or Quantity

## Data
Three tables (see `sample_data.sql` or the CSVs):
- `products(product_id, product_name, category)`
- `sales(order_id, order_date, region, product_id, quantity, unit_price)`
- `web_traffic(date, region, sessions)`

CSV copies are included for quick import: `products.csv`, `sales.csv`, `traffic.csv`.

## Run Locally (Two options)
### A) Using SQL
1. Create a database in SQL Server or PostgreSQL.
2. Run `sample_data.sql` to create tables & seed data.
3. In Power BI Desktop → **Get Data** → your DB → import the three tables.

### B) Using CSVs
Power BI Desktop → **Get Data** → **Text/CSV** → load `products.csv`, `sales.csv`, `traffic.csv`.

## Power BI Measures (DAX)
```DAX
Revenue = SUMX(sales, sales[quantity] * sales[unit_price])

Orders = DISTINCTCOUNT(sales[order_id])

Sessions = SUM(web_traffic[sessions])

Conversion Rate = DIVIDE([Orders], [Sessions])
```
_Optional_
```DAX
Average Order Value = DIVIDE([Revenue], [Orders])
Revenue YoY % = DIVIDE([Revenue] - CALCULATE([Revenue], DATEADD('Date'[Date], -1, YEAR)), CALCULATE([Revenue], DATEADD('Date'[Date], -1, YEAR)))
```

## Notes on Migration (QuickSight → Power BI)
- **Calculated fields** ↔ **DAX measures**
- **SPICE dataset** ↔ **Power BI model** (Import)
- **Filters/controls** ↔ **Slicers**
- **Top N** visuals work similarly; use DAX + filters to replicate QS “Top movers”.

## Screenshots

![Overview](Dashboard_Overview.png)
![Top Products](Top-Products.png)

## License
MIT
