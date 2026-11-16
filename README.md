# Supply Chain Logistics & Fulfillment Optimizer
### Power BI Resume Project

---

## Project Summary

A 2-page interactive Power BI dashboard that analyzes supply chain operations
across SKUs, warehouses, suppliers, and regions. The dashboard helps operations
and supply chain managers answer three core business questions:

1. **Are we at risk of stockouts?** — Inventory health vs reorder points
2. **How reliable are our suppliers?** — Lead times and fulfillment gaps
3. **How accurate is our demand forecast?** — Actual vs forecasted sales

---

## Tools Used

| Tool | Purpose |
|---|---|
| Power BI Desktop | Dashboard design and visualization |
| Power Query (M) | Data cleaning and transformation |
| DAX | Business KPI measures and calculations |

---

## Dataset

- **File:** `data/supply_chain_dataset1.csv`
- **Rows:** ~365+ daily records per SKU
- **Columns:** 15 fields including Date, SKU_ID, Warehouse_ID, Supplier_ID, Region, Units_Sold, Inventory_Level, Supplier_Lead_Time_Days, Reorder_Point, Order_Quantity, Unit_Cost, Unit_Price, Promotion_Flag, Stockout_Flag, Demand_Forecast

---

## Dashboard Pages

### Page 1 — Sales & Operations Overview
- KPI Cards: Total Revenue, Gross Profit, Profit Margin %, Forecast Accuracy %
- Revenue vs Cost trend by Month
- Units Sold by Region (Donut)
- Actual vs Forecasted Demand (Line)
- Promotion Impact Analysis (Clustered Bar)

### Page 2 — Inventory & Supplier Health
- KPI Cards: Avg Inventory Level, Stockout Rate %, SKUs Below Reorder Point, Days of Stock Remaining
- Inventory Level over Time by SKU (Line)
- Stockout Rate by Supplier (Bar)
- Avg Lead Time by Supplier (Horizontal Bar)
- Inventory Status Distribution (Donut: Critical / Low / Healthy)
- Order Quantity by Warehouse (Bar)

---

## Project Structure

```
Supply Chain Logistics & Fulfillment Optimizer/
│
├── supply_chain_dataset1.csv                   # Raw dataset
├── SupplyChain_Fulfillment_Optimizer.pbix       # Power BI report file (save here)
│
├── README.md                                   # Project overview (this file)
├── Power_Query_Steps.md                        # All M transformations step by step
├── DAX_Measures.md                             # All DAX measures with explanations
├── Dashboard_Layout.md                         # Page layout and visual guide
├── Interview_QA.md                             # Interview questions and model answers
└── Resume_Bullets.md                           # Resume bullets and skills section
```

---

## Key DAX Measures Built

- `Total Revenue` — SUMX row-level multiplication
- `Gross Profit` & `Profit Margin %` — Profitability tracking
- `Forecast Accuracy %` — MAPE-based demand planning metric
- `Stockout Rate %` — Fulfillment reliability indicator
- `Days of Stock Remaining` — Inventory runway calculation
- `SKUs Below Reorder Point` — Reorder urgency signal
- `Promotion Uplift %` — VAR/RETURN promotion effectiveness
- `Avg Lead Time (Days)` — Supplier performance benchmark

---

## How to Open

1. Install **Power BI Desktop** (free) — [Download here](https://powerbi.microsoft.com/desktop/)
2. Load `supply_chain_dataset1.csv` via **Get Data → Text/CSV**
3. Follow `Power_Query_Steps.md` for transformations
4. Follow `DAX_Measures.md` to create all measures
5. Follow `Dashboard_Layout.md` to build both pages
6. Save the `.pbix` file in the project root as `SupplyChain_Fulfillment_Optimizer.pbix`

---

## Resume Value

- Demonstrates end-to-end Power BI workflow: ETL → Modeling → Visualization
- Covers 4 business domains: Revenue, Inventory, Supplier, Demand Forecasting
- Uses advanced DAX patterns: SUMX, CALCULATE, VAR/RETURN, DIVIDE, DISTINCTCOUNT
- Suitable for: Data Analyst, Business Analyst, Supply Chain Analyst, BI Developer roles
