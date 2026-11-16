# Dashboard Layout Guide
## Supply Chain Logistics & Fulfillment Optimizer

---

## General Setup

- Open Power BI Desktop with your data loaded and all DAX measures created
- Default canvas size: **16:9 (1280 × 720)** — go to View → Page View → Fit to Page
- Theme: **View → Themes → Executive** (or choose Teal for a cleaner look)

---

# PAGE 1: Sales & Operations Overview

Click **+** at the bottom to add a new page → rename it `Sales & Operations`

---

## Slicers (top of the page)

1. Click **Slicer** icon in Visualizations pane
2. Drag the field into the **Field** well
3. Set style in Format pane → Slicer Settings → Style
4. Repeat 3 times — one slicer per field

| Slicer | Field | Style |
|---|---|---|
| Date Range | `Date` | Between |
| Region | `Region` | Dropdown |
| SKU | `SKU_ID` | Dropdown |

Available values (no filters applied):
- Date Range: `2024-01-01` to `2024-12-31`
- Regions: East, North, South, West
- SKUs: SKU_1 to SKU_50 (50 total)

---

## 4 KPI Cards (below slicers, in a row)

1. Click **Card** visual icon in Visualizations pane
2. Drag measure into the **Fields** well
3. Repeat 4 times — one card per measure

| Card | Measure to drag | Expected Value |
|---|---|---|
| 1 | Total Revenue | $33,426,337.22 |
| 2 | Gross Profit | $11,088,201.23 |
| 3 | Profit Margin % | 33.17% |
| 4 | Forecast Accuracy % | 88.15% |

Arrange all 4 in a row below the slicers.

---

## Line and Clustered Column Chart — Revenue vs Cost by Month

1. Click **Line and Clustered Column Chart** icon in Visualizations pane
2. Drag `Month_Year` → **X-axis**
3. Drag `Total Cost` measure → **Column Y-axis**
4. Drag `Total Revenue` measure → **Line Y-axis**
5. Right-click X-axis → Sort by → `Month_Sort`
6. Place on left side of page

Expected values on chart:

| Month | Total Revenue | Total Cost |
|---|---|---|
| Jan 2024 | ~$2,780,000 | ~$1,858,000 |
| Feb 2024 | ~$2,710,000 | ~$1,812,000 |
| Mar 2024 | ~$2,820,000 | ~$1,886,000 |
| Apr 2024 | ~$2,750,000 | ~$1,839,000 |
| May 2024 | ~$2,800,000 | ~$1,872,000 |
| Jun 2024 | ~$2,740,000 | ~$1,832,000 |
| Jul 2024 | ~$2,810,000 | ~$1,879,000 |
| Aug 2024 | ~$2,790,000 | ~$1,866,000 |
| Sep 2024 | ~$2,730,000 | ~$1,826,000 |
| Oct 2024 | ~$2,800,000 | ~$1,872,000 |
| Nov 2024 | ~$2,770,000 | ~$1,852,000 |
| Dec 2024 | ~$2,730,000 | ~$1,826,000 |
| **Total** | **$33,426,337** | **$22,338,136** |

> Revenue line stays consistently above cost bars — stable 33% margin throughout the year.

---

## Donut Chart — Units Sold by Region

1. Click **Donut chart** icon in Visualizations pane
2. Drag `Region` → **Legend**
3. Drag `Total Units Sold` measure → **Values**
4. Format pane → Detail labels → turn On → set to Percent of total
5. Place on right side of page (next to Revenue vs Cost chart)

Expected values:

| Region | Units Sold | Share % |
|---|---|---|
| East | 459,961 | 25.1% |
| North | 459,849 | 25.1% |
| South | 456,954 | 25.0% |
| West | 453,215 | 24.8% |
| **Total** | **1,829,979** | **100%** |

> Near-perfect 4-way split — all regions within 0.3% of each other.

---

## Line Chart — Actual vs Forecasted Demand

1. Click **Line chart** icon in Visualizations pane
2. Drag `Month_Year` → **X-axis**
3. Drag `Total Units Sold` measure → **Y-axis**
4. Drag `Demand_Forecast` column → **Y-axis** (second line)
5. Format pane → X-axis → Type → **Categorical**
6. Place below the Revenue vs Cost chart

Expected values on chart:

| Month | Units Sold (Actual) | Demand Forecast |
|---|---|---|
| Jan 2024 | ~152,500 | ~172,800 |
| Feb 2024 | ~142,300 | ~161,200 |
| Mar 2024 | ~158,700 | ~179,800 |
| Apr 2024 | ~149,200 | ~169,000 |
| May 2024 | ~155,100 | ~175,700 |
| Jun 2024 | ~148,600 | ~168,300 |
| Jul 2024 | ~154,400 | ~174,900 |
| Aug 2024 | ~153,100 | ~173,400 |
| Sep 2024 | ~148,900 | ~168,600 |
| Oct 2024 | ~153,400 | ~173,800 |
| Nov 2024 | ~146,700 | ~166,100 |
| Dec 2024 | ~151,400 | ~171,500 |
| **Total** | **~1,829,979** | **~2,074,000** |

> Overall Forecast Accuracy = **88.15%** — two lines track closely with ~12% gap throughout the year.

---

## Clustered Bar Chart — Promotion Impact

1. Click **Clustered Bar chart** icon in Visualizations pane
2. Drag `Promotion_Label` → **Y-axis**
3. Drag `Total Revenue` measure → **X-axis**
4. Drag `Total Units Sold` measure → **X-axis** (second bar)
5. Place below the Donut chart

Expected values:

| Label | Units Sold | Revenue |
|---|---|---|
| Promoted | ~457,000 | ~$9,350,000 |
| Not Promoted | ~1,373,000 | ~$24,076,000 |

> Not Promoted rows are 75% of total data — use this to explain how promotions affect per-unit revenue, not just volume.

---

## Page 1 Layout Sketch

```
┌─────────────────────────────────────────────────────────────────┐
│  [Date: 2024-01-01 → 2024-12-31]  [Region ▼]  [SKU_ID ▼]       │
├──────────────┬──────────────┬──────────────┬────────────────────┤
│ $33,426,337  │ $11,088,201  │   33.17%     │     88.15%         │
│ Total Rev    │ Gross Profit │ Profit Marg  │ Forecast Acc %     │
├──────────────┴──────────────┴──────────────┴────────────────────┤
│                                         │                        │
│  Revenue vs Cost by Month               │  Units Sold by Region  │
│  (Line + Clustered Column Chart)        │  (Donut Chart)         │
│  Rev ~$2.7–2.8M | Cost ~$1.8–1.9M/mo   │  East/North/South/West │
│                                         │  ~25% each             │
├─────────────────────────────────────────┴────────────────────────┤
│                                         │                        │
│  Actual vs Forecasted Demand            │  Promotion Impact      │
│  (Line Chart)                           │  (Clustered Bar)       │
│  Actual ~1.83M | Forecast ~2.07M        │  Promoted ~457K units  │
│  Accuracy: 88.15%                       │  Not Promo ~1.37M      │
└─────────────────────────────────────────┴────────────────────────┘
```

---

# PAGE 2: Inventory & Supplier Health

Click **+** at the bottom to add a new page → rename it `Inventory & Supplier Health`

---

## Slicers (top of the page)

1. Click **Slicer** icon in Visualizations pane
2. Drag the field into the **Field** well
3. Set style in Format pane → Slicer Settings → Style → Dropdown
4. Repeat 3 times

| Slicer | Field | Style |
|---|---|---|
| Date Range | `Date` | Between |
| Warehouse | `Warehouse_ID` | Dropdown |
| Supplier | `Supplier_ID` | Dropdown |

Available values (no filters applied):
- Warehouses: WH_1, WH_2, WH_3, WH_4, WH_5
- Suppliers: SUP_1 to SUP_10 (10 total)

---

## 4 KPI Cards (below slicers, in a row)

1. Click **Card** visual icon in Visualizations pane
2. Drag measure into the **Fields** well
3. Repeat 4 times — one card per measure

| Card | Measure to drag | Expected Value |
|---|---|---|
| 1 | Avg Inventory Level | 472 units |
| 2 | Stockout Rate % | 0.0% |
| 3 | SKUs Below Reorder Point | 50 SKUs |
| 4 | Days of Stock Remaining | 23.5 days |

Arrange all 4 in a row below the slicers.

> Stockout Rate = 0.0% means no stockouts occurred. The risk signal is the 50 SKUs already below reorder point.

---

## Line Chart — Inventory Level over Time by SKU

1. Click **Line chart** icon in Visualizations pane
2. Drag `Month_Year` → **X-axis**
3. Drag `Avg Inventory Level` measure → **Y-axis**
4. Drag `SKU_ID` → **Legend**
5. Format pane → X-axis → Type → **Categorical**
6. Place across the full width of the page (top chart)

Expected values on chart:
- Average inventory across all SKUs = **472 units**
- Each SKU line oscillates between **~350 and ~800 units**
- Lines dip before a reorder triggers, then jump when new stock arrives — sawtooth pattern
- SKUs dipping closest to reorder point = highest restocking priority

---

## Bar Chart — Stockout Rate by Supplier

1. Click **Bar chart** icon in Visualizations pane
2. Drag `Supplier_ID` → **Y-axis**
3. Drag `Stockout Rate %` measure → **X-axis**
4. Format pane → Data labels → turn On
5. Place on left side of page

Expected values:

| Supplier | Stockout Rate |
|---|---|
| SUP_1 | 0.0% |
| SUP_2 | 0.0% |
| SUP_3 | 0.0% |
| SUP_4 | 0.0% |
| SUP_5 | 0.0% |
| SUP_6 | 0.0% |
| SUP_7 | 0.0% |
| SUP_8 | 0.0% |
| SUP_9 | 0.0% |
| SUP_10 | 0.0% |

> All suppliers = 0.0%. Fulfillment was reliable across all vendors. The reorder proximity is the forward-looking risk.

---

## Horizontal Bar Chart — Avg Lead Time by Supplier

1. Click **Bar chart** icon in Visualizations pane
2. Drag `Supplier_ID` → **Y-axis**
3. Drag `Avg Lead Time (Days)` measure → **X-axis**
4. Format pane → Data labels → turn On
5. Sort by Avg Lead Time ascending
6. Place on right side of page (next to Stockout Rate chart)

Expected values:

| Supplier | Avg Lead Time | |
|---|---|---|
| SUP_5 | 7.0 days | ✅ Fastest |
| SUP_10 | 7.4 days | |
| SUP_7 | 7.6 days | |
| SUP_6 | 7.9 days | |
| SUP_1 | 8.1 days | |
| SUP_9 | 8.2 days | |
| SUP_3 | 8.3 days | |
| SUP_2 | 8.4 days | |
| SUP_8 | 8.5 days | |
| SUP_4 | 8.6 days | ⚠️ Slowest |

> SUP_4 is 1.6 days slower than SUP_5 — highest restocking delay risk.

---

## Donut Chart — Inventory Status Distribution

1. Click **Donut chart** icon in Visualizations pane
2. Drag `Inventory_Status` → **Legend**
3. Drag `SKU_ID` → **Values** → change aggregation to **Count**
4. Format pane → Colors → set manually:
   - Critical → Red `#C0392B`
   - Low → Amber `#E67E22`
   - Healthy → Green `#27AE60`
5. Place on bottom-left of page

Expected values:

| Status | Row Count | % of Total | Color |
|---|---|---|---|
| Healthy | 71,124 | 77.9% | 🟢 Green |
| Low | 15,339 | 16.8% | 🟡 Amber |
| Critical | 4,787 | 5.3% | 🔴 Red |
| **Total** | **91,250** | **100%** | |

> Donut is predominantly green — but the 5.3% red slice and 16.8% amber slice are the story to tell.

---

## Bar Chart — Order Quantity by Warehouse

1. Click **Bar chart** icon in Visualizations pane
2. Drag `Warehouse_ID` → **Y-axis**
3. Drag `Order_Quantity` column → **X-axis** → set aggregation to **Sum**
4. Format pane → Data labels → turn On
5. Place on bottom-right of page

Expected values:

| Warehouse | Total Order Qty | |
|---|---|---|
| WH_1 | 351,197 | |
| WH_2 | 352,574 | ✅ Highest |
| WH_3 | 350,716 | ⬇️ Lowest |
| WH_4 | 351,879 | |
| WH_5 | 352,249 | |

> All warehouses within ~2,000 units of each other — evenly distributed restocking across all locations.

---

## Page 2 Layout Sketch

```
┌─────────────────────────────────────────────────────────────────┐
│  [Date: 2024-01-01 → 2024-12-31]  [Warehouse ▼]  [Supplier ▼]  │
├──────────────┬──────────────┬──────────────┬────────────────────┤
│  472 units   │    0.0%      │   50 SKUs    │    23.5 days       │
│  Avg Inv     │  Stockout %  │  Below ROP   │  Days of Stock     │
├──────────────┴──────────────┴──────────────┴────────────────────┤
│                                                                   │
│     Inventory Level over Time by SKU  (Line Chart)               │
│     Avg 472 units | Sawtooth pattern | Range ~350–800 units      │
│                                                                   │
├──────────────────────────────┬──────────────────────────────────┤
│  Stockout Rate by Supplier   │  Avg Lead Time by Supplier        │
│  (Bar Chart)                 │  (Horizontal Bar Chart)           │
│  All suppliers = 0.0%        │  SUP_5: 7.0d ✅ SUP_4: 8.6d ⚠️  │
├──────────────────────────────┼──────────────────────────────────┤
│  Inventory Status Donut      │  Order Quantity by Warehouse      │
│  Healthy 77.9% 🟢            │  (Bar Chart)                      │
│  Low      16.8% 🟡           │  WH_2: 352,574 ✅ Highest         │
│  Critical  5.3% 🔴           │  WH_3: 350,716 ⬇️ Lowest          │
└──────────────────────────────┴──────────────────────────────────┘
```

---

# Final Touches

1. **Add page titles**
   - Insert → Text Box → type `Sales & Operations Overview` on Page 1
   - Insert → Text Box → type `Inventory & Supplier Health` on Page 2

2. **Add page navigation buttons**
   - Insert → Buttons → Navigator → place on both pages

3. **Add Reset Filters button**
   - Clear all slicers → Bookmarks pane → Add Bookmark → name it `Reset`
   - Insert → Button → Blank → Action → Type: Bookmark → select `Reset`

4. **Format KPI cards**
   - Click each card → Format pane → Callout Value → Font size: 28–32

5. **Save the file**
   - Ctrl + S → save as `SupplyChain_Fulfillment_Optimizer.pbix`

---

## Key Insights to Mention During Presentation

1. **$33.4M revenue at 33.17% margin** — healthy profitability across all SKUs and regions
2. **88.15% forecast accuracy** — good, but the 12% gap is an opportunity to improve demand planning
3. **Zero stockouts** — fulfillment was reliable throughout the year
4. **50 SKUs below reorder point** — ~20% of catalog needs restocking immediately
5. **23.5 days of stock remaining** — operations have ~3.5 weeks of runway before critical shortages
6. **Balanced regional demand** — all 4 regions within 0.3% of each other
7. **SUP_4 slowest at 8.6 days** — highest restocking delay risk; worth flagging to procurement
