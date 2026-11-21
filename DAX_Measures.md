# DAX Measures Reference

**Project:** Supply Chain Logistics & Fulfillment Optimizer  
**Dataset:** `supply_chain_dataset1.csv` — 91,250 rows | 50 SKUs | 5 Warehouses | 4 Regions | Full Year 2024

---

## Setup — Create a Measures Table

1. Go to **Modeling → New Table**
2. Formula: `_Measures = {""}` → press Enter
3. Right-click `_Measures` → **New Measure** for each measure below

---

## Category 1 — Revenue & Profitability

### Total Revenue
```dax
Total Revenue =
SUMX(
    supply_chain_dataset1,
    supply_chain_dataset1[Units_Sold] * supply_chain_dataset1[Unit_Price]
)
```
> SUMX iterates row by row — no pre-built Revenue column exists in the dataset.  
> **Format:** Currency ($), 2 decimals

---

### Total Cost
```dax
Total Cost =
SUMX(
    supply_chain_dataset1,
    supply_chain_dataset1[Units_Sold] * supply_chain_dataset1[Unit_Cost]
)
```
> **Format:** Currency ($), 2 decimals

---

### Gross Profit
```dax
Gross Profit =
[Total Revenue] - [Total Cost]
```
> **Format:** Currency ($), 2 decimals

---

### Profit Margin %
```dax
Profit Margin % =
DIVIDE([Gross Profit], [Total Revenue], 0)
```
> DIVIDE handles zero denominators safely — avoids divide-by-zero errors.  
> **Format:** Percentage (%), 1 decimal

---

## Category 2 — Sales & Demand

### Total Units Sold
```dax
Total Units Sold =
SUM(supply_chain_dataset1[Units_Sold])
```
> **Format:** Whole Number

---

### Forecast Accuracy %
```dax
Forecast Accuracy % =
1 - DIVIDE(
    SUMX(
        supply_chain_dataset1,
        ABS(supply_chain_dataset1[Units_Sold] - supply_chain_dataset1[Demand_Forecast])
    ),
    SUM(supply_chain_dataset1[Demand_Forecast]),
    0
)
```
> MAPE-based accuracy. 100% = perfect forecast. SUMX + ABS computes row-level error before aggregating.  
> **Format:** Percentage (%), 1 decimal

---

### Promotion Uplift %
```dax
Promotion Uplift % =
VAR PromotedSales =
    CALCULATE([Total Units Sold], supply_chain_dataset1[Promotion_Flag] = 1)
VAR NonPromotedSales =
    CALCULATE([Total Units Sold], supply_chain_dataset1[Promotion_Flag] = 0)
RETURN
    DIVIDE(PromotedSales - NonPromotedSales, NonPromotedSales, 0)
```
> VAR/RETURN avoids repeating CALCULATE twice.  
> **Format:** Percentage (%), 1 decimal

---

## Category 3 — Inventory

### Avg Inventory Level
```dax
Avg Inventory Level =
AVERAGE(supply_chain_dataset1[Inventory_Level])
```
> **Format:** Whole Number

---

### Days of Stock Remaining
```dax
Days of Stock Remaining =
DIVIDE(
    AVERAGE(supply_chain_dataset1[Inventory_Level]),
    AVERAGE(supply_chain_dataset1[Units_Sold]),
    0
)
```
> Average inventory ÷ average daily sales = days until stockout at current burn rate.  
> **Format:** Decimal, 1 decimal

---

### SKUs Below Reorder Point
```dax
SKUs Below Reorder Point =
CALCULATE(
    DISTINCTCOUNT(supply_chain_dataset1[SKU_ID]),
    supply_chain_dataset1[Inventory_Level] < supply_chain_dataset1[Reorder_Point]
)
```
> DISTINCTCOUNT counts unique SKUs, not rows — multiple rows exist per SKU.  
> **Format:** Whole Number

---

## Category 4 — Supplier & Fulfillment

### Stockout Rate %
```dax
Stockout Rate % =
DIVIDE(
    CALCULATE(
        COUNTROWS(supply_chain_dataset1),
        supply_chain_dataset1[Stockout_Flag] = 1
    ),
    COUNTROWS(supply_chain_dataset1),
    0
) + 0
```
> `+ 0` forces the KPI card to show `0.0%` instead of `--` when no stockouts exist. DIVIDE returns BLANK (not zero) on an empty numerator.  
> **Format:** Percentage (%), 1 decimal

---

### Avg Lead Time (Days)
```dax
Avg Lead Time (Days) =
AVERAGE(supply_chain_dataset1[Supplier_Lead_Time_Days])
```
> **Format:** Decimal, 1 decimal

---

## Verified Output Values

> No slicers applied — full year 2024, all regions, all SKUs, all warehouses.

### KPI Cards

| Measure | Value |
|---|---|
| Total Revenue | $33,426,337.22 |
| Total Cost | $22,338,135.99 |
| Gross Profit | $11,088,201.23 |
| Profit Margin % | 33.17% |
| Total Units Sold | 1,829,979 |
| Avg Inventory Level | 472 units |
| Avg Lead Time (Days) | 8.0 days |
| Stockout Rate % | 0.0% |
| Forecast Accuracy % | 88.15% |
| Days of Stock Remaining | 23.5 days |
| SKUs Below Reorder Point | 50 SKUs |

---

### Units Sold by Region

| Region | Units Sold | Revenue | Share % |
|---|---|---|---|
| East | 459,961 | $8,398,382 | 25.1% |
| North | 459,849 | $8,411,171 | 25.1% |
| South | 456,954 | $8,320,451 | 25.0% |
| West | 453,215 | $8,296,333 | 24.8% |

---

### Supplier Performance

| Supplier | Stockout Rate | Avg Lead Time | |
|---|---|---|---|
| SUP_5 | 0.0% | 7.0 days | ✅ Fastest |
| SUP_10 | 0.0% | 7.4 days | |
| SUP_7 | 0.0% | 7.6 days | |
| SUP_6 | 0.0% | 7.9 days | |
| SUP_1 | 0.0% | 8.1 days | |
| SUP_9 | 0.0% | 8.2 days | |
| SUP_3 | 0.0% | 8.3 days | |
| SUP_2 | 0.0% | 8.4 days | |
| SUP_8 | 0.0% | 8.5 days | |
| SUP_4 | 0.0% | 8.6 days | ⚠️ Slowest |

---

### Inventory Status Distribution

| Status | Row Count | % of Total |
|---|---|---|
| Healthy | 71,124 | 77.9% |
| Low | 15,339 | 16.8% |
| Critical | 4,787 | 5.2% |
| **Total** | **91,250** | **100%** |

---

### Order Quantity by Warehouse

| Warehouse | Total Order Qty | |
|---|---|---|
| WH_1 | 351,197 | |
| WH_2 | 352,574 | ✅ Highest |
| WH_3 | 350,716 | ⬇️ Lowest |
| WH_4 | 351,879 | |
| WH_5 | 352,249 | |
