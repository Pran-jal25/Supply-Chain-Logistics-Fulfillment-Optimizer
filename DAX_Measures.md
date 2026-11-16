# DAX Measures Reference
## Supply Chain Logistics & Fulfillment Optimizer

---

## Setup: Create a Measures Table

Before writing any measures, create a dedicated table:

1. Go to **Modeling → New Table**
2. Formula: `_Measures = {""}` → press Enter
3. Right-click `_Measures` → **New Measure** for each measure below

> **Why a separate table?** Keeps all measures organized in one place instead of
> being scattered inside the data table. Standard professional practice.

---

## Category 1 — Revenue & Profitability

---

### Total Revenue
```dax
Total Revenue =
SUMX(
    supply_chain_dataset1,
    supply_chain_dataset1[Units_Sold] * supply_chain_dataset1[Unit_Price]
)
```
**What it does:** Multiplies Units_Sold × Unit_Price for every row, then sums all results.
**Why SUMX:** No single Revenue column exists — SUMX iterates row by row before aggregating.
**Format:** Currency ($)

---

### Total Cost
```dax
Total Cost =
SUMX(
    supply_chain_dataset1,
    supply_chain_dataset1[Units_Sold] * supply_chain_dataset1[Unit_Cost]
)
```
**Format:** Currency ($)

---

### Gross Profit
```dax
Gross Profit =
[Total Revenue] - [Total Cost]
```
**What it does:** Subtracts total cost from total revenue using existing measures.
**Format:** Currency ($)

---

### Profit Margin %
```dax
Profit Margin % =
DIVIDE([Gross Profit], [Total Revenue], 0)
```
**What it does:** Gross Profit ÷ Total Revenue. Returns 0 if Revenue is zero.
**Why DIVIDE:** Safer than `/` — handles zero denominators without errors.
**Format:** Percentage (%)

---

## Category 2 — Sales & Demand

---

### Total Units Sold
```dax
Total Units Sold =
SUM(supply_chain_dataset1[Units_Sold])
```
**Format:** Whole Number

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
**What it does:** MAPE-based accuracy — takes absolute error per row via SUMX, divides by total forecast, subtracts from 1.
100% = perfect forecast. Lower = worse accuracy.
**Format:** Percentage (%)

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
**What it does:** Compares units sold during promotions vs non-promotions. Returns % lift.
**Why VAR/RETURN:** Avoids repeating CALCULATE twice. Cleaner and better performance.
**Format:** Percentage (%)

---

## Category 3 — Inventory

---

### Avg Inventory Level
```dax
Avg Inventory Level =
AVERAGE(supply_chain_dataset1[Inventory_Level])
```
**Format:** Whole Number

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
**What it does:** Average inventory ÷ average daily sales = days until stockout at current rate.
Example: Inventory = 500, Avg daily sales = 25 → 20 days of stock remaining.
**Format:** Decimal (1 decimal place)

---

### SKUs Below Reorder Point
```dax
SKUs Below Reorder Point =
CALCULATE(
    DISTINCTCOUNT(supply_chain_dataset1[SKU_ID]),
    supply_chain_dataset1[Inventory_Level] < supply_chain_dataset1[Reorder_Point]
)
```
**What it does:** Counts unique SKUs where inventory is currently below reorder threshold.
**Why DISTINCTCOUNT:** Multiple rows per SKU — we want product count, not row count.
**Format:** Whole Number

---

## Category 4 — Supplier & Fulfillment

---

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
)
```
**What it does:** % of days where a stockout occurred.
Example: 10 stockout days out of 100 total = 10% stockout rate.
**Format:** Percentage (%)

---

### Avg Lead Time (Days)
```dax
Avg Lead Time (Days) =
AVERAGE(supply_chain_dataset1[Supplier_Lead_Time_Days])
```
**What it does:** Average days for supplier to deliver stock. Compare across suppliers.
**Format:** Decimal (1 decimal place)

---

## Formatting Reference

| Measure | Format Setting |
|---|---|
| Total Revenue | Currency, 2 decimals |
| Total Cost | Currency, 2 decimals |
| Gross Profit | Currency, 2 decimals |
| Profit Margin % | Percentage, 1 decimal |
| Total Units Sold | Whole Number |
| Forecast Accuracy % | Percentage, 1 decimal |
| Promotion Uplift % | Percentage, 1 decimal |
| Avg Inventory Level | Whole Number |
| Days of Stock Remaining | Decimal, 1 decimal |
| SKUs Below Reorder Point | Whole Number |
| Stockout Rate % | Percentage, 1 decimal |
| Avg Lead Time (Days) | Decimal, 1 decimal |

---

## DAX Patterns Quick Reference

| Pattern | Used In |
|---|---|
| `SUMX(table, expression)` | Total Revenue, Total Cost, Forecast Accuracy % |
| `CALCULATE(expr, filter)` | Stockout Rate %, SKUs Below Reorder Point, Promotion Uplift % |
| `DIVIDE(num, den, alt)` | Profit Margin %, Forecast Accuracy %, Stockout Rate %, Days of Stock |
| `VAR ... RETURN` | Promotion Uplift % |
| `DISTINCTCOUNT(column)` | SKUs Below Reorder Point |
| `ABS(expression)` | Forecast Accuracy % |


---

## Actual Output After Calculation (from dataset)

> These are the real values your Power BI dashboard will display based on `supply_chain_dataset1.csv` (91,250 rows across all SKUs, warehouses, and regions).

---

### KPI Cards — Expected Values

| Measure | Value |
|---|---|
| **Total Revenue** | $33,426,337.22 |
| **Total Cost** | $22,338,135.99 |
| **Gross Profit** | $11,088,201.23 |
| **Profit Margin %** | 33.17% |
| **Total Units Sold** | 1,829,979 |
| **Avg Inventory Level** | 472 units |
| **Avg Lead Time (Days)** | 8.0 days |
| **Stockout Rate %** | 0.0% |
| **Forecast Accuracy %** | 88.15% |
| **Days of Stock Remaining** | 23.5 days |
| **SKUs Below Reorder Point** | 50 SKUs |

---

### Inventory Status Distribution

| Status | Row Count | % of Total |
|---|---|---|
| **Healthy** | 71,124 rows | 77.9% |
| **Low** | 15,339 rows | 16.8% |
| **Critical** | 4,787 rows | 5.3% |

> Most inventory is healthy, but 5.3% of records are in a critical state — these are the rows your Red highlight will flag on the dashboard.

---

### Units Sold by Region (Donut Chart values)

| Region | Units Sold | Revenue | Share |
|---|---|---|---|
| **East** | 459,961 | $8,398,382.40 | 25.1% |
| **North** | 459,849 | $8,411,171.01 | 25.1% |
| **South** | 456,954 | $8,320,450.82 | 25.0% |
| **West** | 453,215 | $8,296,332.99 | 24.8% |

> Demand is almost perfectly balanced across all 4 regions — no single region dominates.

---

### Supplier Performance

| Supplier | Stockout Rate | Avg Lead Time |
|---|---|---|
| SUP_1 | 0.0% | 8.1 days |
| SUP_2 | 0.0% | 8.4 days |
| SUP_3 | 0.0% | 8.3 days |
| SUP_4 | 0.0% | 8.6 days |
| SUP_5 | 0.0% | 7.0 days |
| SUP_6 | 0.0% | 7.9 days |
| SUP_7 | 0.0% | 7.6 days |
| SUP_8 | 0.0% | 8.5 days |
| SUP_9 | 0.0% | 8.2 days |
| SUP_10 | 0.0% | 7.4 days |

> SUP_5 has the fastest average lead time (7.0 days), SUP_4 is the slowest (8.6 days).

---

### Order Quantity by Warehouse

| Warehouse | Total Units Ordered |
|---|---|
| WH_1 | 351,197 |
| WH_2 | 352,574 |
| WH_3 | 350,716 |
| WH_4 | 351,879 |
| WH_5 | 352,249 |

> Order quantities are evenly distributed across all 5 warehouses — WH_2 ordered the most, WH_3 the least.

---

### Insight Summary (for interviews)

- **Revenue** of $33.4M generated at a **33.17% gross margin**
- **Forecast accuracy is 88.15%** — meaning demand planning is reasonably good but has ~12% error gap to address
- **No stockouts occurred** in this dataset — Stockout Rate = 0.0%, indicating strong inventory management
- **50 SKUs** have inventory below their reorder point — need restocking attention
- **23.5 days of stock remaining** on average — operations have about 3+ weeks of runway
- **Regional demand is balanced** — all 4 regions within 0.3% of each other in volume share
- **SUP_4 and SUP_8** have the longest lead times (8.5–8.6 days) — highest restocking delay risk
