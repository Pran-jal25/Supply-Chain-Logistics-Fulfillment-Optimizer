# Power Query Transformation Steps
## Supply Chain Logistics & Fulfillment Optimizer

---

## How to Open Power Query Editor

1. Open Power BI Desktop
2. **Home → Get Data → Text/CSV**
3. Select `supply_chain_dataset1.csv`
4. In the preview window click **Transform Data** (not Load)
5. Power Query Editor opens — apply all steps below in order

---

## Step 1 — Set Column Data Types

Click the data type icon (ABC / 123) on each column header and set:

| Column | Data Type |
|---|---|
| Date | Date |
| SKU_ID | Text |
| Warehouse_ID | Text |
| Supplier_ID | Text |
| Region | Text |
| Units_Sold | Whole Number |
| Inventory_Level | Whole Number |
| Supplier_Lead_Time_Days | Whole Number |
| Reorder_Point | Whole Number |
| Order_Quantity | Whole Number |
| Unit_Cost | Decimal Number |
| Unit_Price | Decimal Number |
| Promotion_Flag | Whole Number |
| Stockout_Flag | Whole Number |
| Demand_Forecast | Decimal Number |

> **Why:** Power BI auto-detects types and sometimes gets them wrong (e.g. Date as Text).
> Fixing types ensures all calculations and date slicers work correctly.

---

## Step 2 — Add Custom Column: Revenue

**Add Column → Custom Column**

- **Column Name:** `Revenue`
- **Formula:**
```
= [Units_Sold] * [Unit_Price]
```

> **Why:** Pre-calculating revenue at the row level is more efficient than always using SUMX in DAX.

---

## Step 3 — Add Custom Column: Cost

**Add Column → Custom Column**

- **Column Name:** `Cost`
- **Formula:**
```
= [Units_Sold] * [Unit_Cost]
```

---

## Step 4 — Add Custom Column: Profit

**Add Column → Custom Column**

- **Column Name:** `Profit`
- **Formula:**
```
= [Units_Sold] * ([Unit_Price] - [Unit_Cost])
```

> **Why:** Profit per row enables SKU-level and date-level margin analysis in tables.

---

## Step 5 — Add Custom Column: Inventory Status

**Add Column → Custom Column**

- **Column Name:** `Inventory_Status`
- **Formula:**
```
= if [Inventory_Level] < [Reorder_Point] then "Critical"
  else if [Inventory_Level] < [Reorder_Point] * 1.2 then "Low"
  else "Healthy"
```

> **Why:** Classifies each row's stock health into 3 tiers:
> - **Critical** = Inventory below reorder threshold — immediate action needed
> - **Low** = Within 20% above reorder point — monitor closely
> - **Healthy** = Well above reorder point — no action needed
>
> Used for Red / Amber / Green conditional formatting on the Inventory page.

---

## Step 6 — Add Custom Column: Promotion Label

**Add Column → Custom Column**

- **Column Name:** `Promotion_Label`
- **Formula:**
```
= if [Promotion_Flag] = 1 then "Promoted" else "Not Promoted"
```

> **Why:** Converts binary 0/1 flag into readable text for chart legends and slicers.

---

## Step 7 — Add Custom Column: Reorder Triggered

**Add Column → Custom Column**

- **Column Name:** `Reorder_Triggered`
- **Formula:**
```
= if [Order_Quantity] > 0 then "Yes" else "No"
```

---

## Step 8 — Add Custom Column: Month-Year

**Add Column → Custom Column**

- **Column Name:** `Month_Year`
- **Formula:**
```
= Date.ToText([Date], "MMM yyyy")
```

> **Why:** Groups daily data into monthly buckets for trend charts.
> Example output: `Jan 2024`, `Feb 2024`

---

## Step 9 — Add Custom Column: Month Sort

**Add Column → Custom Column**
  
- **Column Name:** `Month_Sort`
- **Formula:**
```
= Date.Year([Date]) * 100 + Date.Month([Date])
```

> **Why:** Month_Year as text sorts alphabetically — April before January.
> Month_Sort creates a numeric value: Jan 2024 = 202401, Feb 2024 = 202402.
> You will use Sort by Column in the report to fix the ordering.

---

## Step 10 — Close & Apply

1. Click **Home → Close & Apply**
2. Power BI loads the transformed table into the data model
3. All original columns + 8 new columns appear in the Fields pane

---

## Applied Steps Summary

Your Applied Steps panel (right side of Power Query) should show:

```
Source
Changed Type
Added Revenue
Added Cost
Added Profit
Added Inventory_Status
Added Promotion_Label
Added Reorder_Triggered
Added Month_Year
Added Month_Sort
```

---

## Fix Month-Year Sort in Report View

Do this once after Close & Apply:

1. In the **Fields** pane, click `Month_Year` column
2. Go to **Column Tools** tab in the ribbon
3. Click **Sort by Column → Month_Sort**

> This ensures all time-series charts display Jan → Feb → Mar correctly.
