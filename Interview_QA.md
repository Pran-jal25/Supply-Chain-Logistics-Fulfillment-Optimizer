# Interview Questions & Model Answers
## Supply Chain Logistics & Fulfillment Optimizer — Power BI Project

> All answers reference actual numbers from the dataset (91,250 rows).
> Use these numbers in your answers — interviewers are impressed when you know your own data.

---

## Section A — Project Walkthrough Questions

---

**Q1. Walk me through this project. What problem does it solve?**

"This is a supply chain analytics dashboard built in Power BI. The dataset has 91,250 daily records across 50 SKUs, 5 warehouses, 10 suppliers, and 4 regions. The dashboard solves three business problems:
1. Inventory risk — are we close to running out of stock?
2. Supplier reliability — which suppliers have the longest lead times?
3. Demand forecast accuracy — how well is our forecasting matching actual sales?

I built it across 2 pages: Page 1 covers revenue and demand performance, Page 2 covers inventory and supplier health."

---

**Q2. What were your key findings from the data?**

"A few things stood out:
- Total revenue is $33.4M at a 33.17% gross profit margin
- Forecast accuracy is 88.15% — good, but there's a 12% planning gap worth investigating
- Stockout rate is 0% — fulfillment is reliable
- 50 SKUs currently have inventory below their reorder point — roughly 20% of the catalog needs restocking
- Days of stock remaining averages 23.5 days — about 3.5 weeks of runway
- Regional demand is almost perfectly balanced across all 4 regions at ~25% each"

---

**Q3. What data transformations did you apply in Power Query?**

"I applied 10 transformation steps. First I fixed all column data types — Power BI had auto-detected some incorrectly. Then I added 8 custom columns:
- Revenue, Cost, and Profit per row using M arithmetic
- Inventory Status (Critical/Low/Healthy) using an if-else formula against the reorder point
- Promotion Label to convert the 0/1 flag into readable text
- Reorder Triggered to flag rows where an order was placed
- Month-Year for monthly trend grouping
- Month Sort as a numeric key to fix alphabetical date sorting"

---

**Q4. How did you calculate Forecast Accuracy %?**

"I used MAPE-based logic. The DAX measure uses SUMX to compute the absolute difference between actual units sold and forecasted demand at the row level, sums all those errors, divides by total forecast, then subtracts from 1. The result across this dataset is 88.15%. Using SUMX was necessary because ABS() has to be evaluated per row before aggregating — you can't do that with a simple SUM."

---

**Q5. Why did you use SUMX instead of SUM for revenue?**

"SUM only aggregates an existing column. Since Unit_Price and Units_Sold are separate columns in the source, there's no pre-calculated revenue to sum. SUMX iterates row by row, multiplies the two values, then sums the result. That gives $33,426,337 total revenue. Using SUM directly on either column alone would give a meaningless number."

---

**Q6. What is the Reorder Point and how did you use it in your analysis?**

"The Reorder Point is the inventory threshold that triggers a new purchase order. When inventory drops below it, there's a stockout risk. I used it in two places: in Power Query to classify each row as Critical (below reorder point), Low (within 20% above), or Healthy (well above). And in a DAX measure using DISTINCTCOUNT to count how many unique SKUs are currently below their reorder point — which turns out to be 50 SKUs, about 20% of the catalog."

---

**Q7. How did you handle the Month-Year sorting problem?**

"Month-Year as text sorts alphabetically — April before January. I created a Month_Sort column in Power Query using `Date.Year([Date]) * 100 + Date.Month([Date])`, which gives January 2024 = 202401, February = 202402, and so on. Then in the report I used Sort by Column to sort Month_Year by Month_Sort. This is a common Power BI gotcha and fixing it properly is something interviewers notice."

---

## Section B — DAX & Power Query Concepts

---

**Q8. What is the difference between CALCULATE and FILTER?**

"CALCULATE modifies the filter context of a measure — it's the most powerful DAX function. FILTER returns a table of rows matching a condition. You use FILTER inside CALCULATE when you need row-level multi-column conditions. For simple single-column filters, CALCULATE with a direct boolean is cleaner and faster. In this project I used CALCULATE with direct conditions for Stockout Rate and SKUs Below Reorder Point."

---

**Q9. What is row context vs filter context?**

"Row context is what exists inside iterating functions like SUMX — it's the current row being evaluated. Filter context is the set of filters applied to the visual from slicers, page filters, or CALCULATE. In my Forecast Accuracy measure, the ABS calculation happens in row context inside SUMX. The aggregation then respects the filter context from whatever slicer the user applies. Mixing these up is the most common source of wrong DAX results."

---

**Q10. When do you use a calculated column vs a measure?**

"Calculated columns are computed at data refresh and stored in the model — use them when you need a per-row value like Inventory Status or Profit. Measures are computed dynamically at query time based on filter context — use them for aggregations like Total Revenue or Stockout Rate. Overusing calculated columns bloats model size; measures are memory-efficient. In this project I used both: calculated columns in Power Query for row classification, measures in DAX for all KPIs."

---

**Q11. Why use DIVIDE instead of the / operator?**

"DIVIDE(numerator, denominator, alternate_result) handles zero division safely. If the denominator is zero or blank, it returns the alternate — usually 0 — instead of an error. The / operator returns an error or infinity on zero division, which breaks visuals. Every ratio measure in this project uses DIVIDE: Profit Margin %, Forecast Accuracy %, Stockout Rate %, and Days of Stock Remaining."

---

**Q12. Explain the VAR/RETURN pattern you used.**

"I used it in Promotion Uplift %. VAR stores intermediate calculation results — PromotedSales and NonPromotedSales — before the RETURN statement computes the final ratio. Without VAR I'd need to write the CALCULATE expression twice inside the DIVIDE, which is harder to read and evaluates the same expression twice. VAR also makes debugging easier — you can isolate each variable to verify it independently."

---

## Section C — Business & Design Questions

---

**Q13. What does the Inventory Status classification tell a business user?**

"It gives an at-a-glance health signal without the user needing to know what the reorder point is. The Critical (Red) category means stock is already below the safety threshold — orders need to go out immediately. Low (Amber) means we're within 20% of that threshold — monitor closely. Healthy (Green) means no action needed. In this dataset, 77.9% of records are Healthy, 16.8% Low, and 5.3% Critical."

---

**Q14. The Stockout Rate is 0% — is that a good or bad sign for the project?**

"It's good for the business but it means I need to reframe the narrative for the dashboard. A 0% stockout rate means the inventory management process is working well. However, it also means the Stockout Rate card on its own is not the most interesting KPI. I'd pivot to emphasizing the 50 SKUs below reorder point as the forward-looking risk signal — those could become stockouts if not addressed. This shows I can interpret data in business context, not just report numbers."

---

**Q15. Why did you choose a donut chart for the Inventory Status and Region breakdowns?**

"Donut charts work well for part-to-whole comparisons with a small number of categories — 3 statuses, 4 regions. They're compact and let the viewer quickly read proportions. For more than 5-6 categories or when exact values matter more than proportions, I'd switch to a bar chart. I also added data labels to show the exact percentages so it's not purely visual."

---

**Q16. How would you improve this project with more data or more time?**

"A few directions: First, I'd add a date intelligence layer using a proper calendar table for YoY and MoM comparisons using DATEADD or SAMEPERIODLASTYEAR. Second, I'd build a supplier reliability score combining lead time variance and reorder frequency — not just averages. Third, I'd add a rolling 30-day forecast error trend to make the demand planning page more actionable. Fourth, I'd add a what-if parameter to simulate the impact of different reorder point thresholds on the Critical SKU count."

---

## Section D — Quick-Fire Concept Questions

| Question | Answer |
|---|---|
| What is a star schema? | Fact table at center linked to dimension tables — improves query performance |
| What is DAX? | Data Analysis Expressions — formula language for Power BI, SSAS, Excel Power Pivot |
| What does DISTINCTCOUNT do? | Counts unique values — used to count 50 unique SKUs below reorder point |
| What is a bookmark in Power BI? | Saves current filter state — used for Reset Filters button |
| What is drill-through? | Right-click a data point to navigate to a detail page filtered to that value |
| What is BLANK() in DAX? | Represents null — DAX treats blank differently from zero |
| What is a slicer vs a filter? | Slicer is visible on canvas for users; filter is in the pane, set by developer |
| What does SUMX do? | Iterates a table row by row, evaluates an expression per row, then aggregates |
| What is filter context? | The active set of filters on a visual from slicers, page filters, or CALCULATE |
| What is MAPE? | Mean Absolute Percentage Error — measures forecast accuracy, used in this project |

---

## Interview Tips

1. **Open your `.pbix` file before the interview** — many interviewers ask you to share screen and walk through it live
2. **Know your numbers** — say "Revenue is $33.4M at a 33% margin" confidently — shows you analyzed the data
3. **Lead with business impact** — start with "this tells the operations team..." before explaining the DAX formula
4. **Expect a CALCULATE deep-dive** — it's the most tested DAX function in every interview
5. **The 50 SKUs below reorder point** is your strongest talking point for operations-focused roles — know it cold
6. **Acknowledge the 0% stockout** proactively and reframe it — shows analytical maturity
