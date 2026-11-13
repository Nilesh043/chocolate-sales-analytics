# Chocolate Sales Analytics (Excel: Power Query, Power Pivot, DAX)


An interactive Excel dashboard and analysis of a multi-country chocolate retailer: 10 lakh (1,000,000) orders from Jan 2023 to Dec 2024, across 100 stores, 200 products and 50,000 customers.

The project answers one question: **where do revenue and profit come from, and what should the business change?**

<img width="1853" height="996" alt="Animation" src="https://github.com/user-attachments/assets/fd65d179-55c3-4524-9dfb-5414b70af8bb" />

## Data

| File | Rows | Description |
|---|---|---|
| `sales.csv` | 1,000,000 | order_id, order_date, product_id, store_id, customer_id, quantity, unit_price, discount, revenue, cost, profit |
| `customers.csv` | 50,000 | age, gender, loyalty_member, join_date |
| `products.csv` | 200 | brand, category, cocoa_percent, weight_g |
| `stores.csv` | 100 | city, country, store_type |
| `calendar.csv` | 731 | date, year, month, week, day_of_week |

## Approach

1. **Power Query:** loaded and typed all five files (dates parsed as day-month-year with the en-GB locale), added helper columns (age group, loyalty status, cocoa band, day type) and a gross revenue column.
2. **Data model (Power Pivot):** star schema with `FactSales` in the centre and four dimension tables (`DimCalendar`, `DimCustomers`, `DimProducts`, `DimStores`), all one-to-many.
3. **DAX measures:** 25+ measures covering revenue, profit, margin, AOV, discount impact, year-on-year, year-to-date, shares, per-store and per-customer metrics, and inactive-customer rate.
4. **Analysis:** monthly trend, discount effectiveness, product and brand mix, store and country performance, customer segments, RFM segmentation, 2025 forecast (`FORECAST.ETS`) and a what-if scenario model with Goal Seek break-even.
5. **Dashboard:** KPI cards, seven slicers and six charts connected to the data model.

## Key findings

| # | Finding |
|---|---|
| 1 | Volume is flat: 4,99,570 orders in 2023 vs 5,00,430 in 2024 (+0.17%); revenue +0.12%. |
| 2 | The 2025 forecast is flat: ₹1,26,69,052 (−0.6% vs 2024, inside a ±1.4% monthly range). |
| 3 | Discounts gave away ₹15.2 lakh (5.6% of gross revenue) without increasing basket size (3.0 units per order at every level). Profit per order falls from ₹10.81 to ₹8.64 as the discount rises to 20%. |
| 4 | Scenario: removing the 15% and 20% tiers (losing 5% of those orders) adds ₹3.4 to ₹10.5 lakh profit; break-even is at about 17% to 44% of those orders lost. |
| 5 | Revenue follows range size: every product earns about ₹1.26 lakh, and it takes 162 of 200 products to reach 80% of revenue. |
| 6 | Stores earn about ₹2.55 lakh each in every country and format (spread under 1%). |
| 7 | Loyalty members spend no more than non-members (₹508.80 vs ₹510.65 per customer). |
| 8 | 7,054 "At Risk" customers hold ₹42.5 lakh of revenue (16.7%): the win-back priority. 8.7% of customers have not ordered in the last 90 days. |

Details and recommendations are in `Chocolate_Findings_and_DataQuality.xlsx`.

## Data quality issues found and handled

- Two product IDs (`P0000`, `P0201`) in sales are missing from the product file (0.97% of revenue); mapped to an "Unmapped" product.
- 43.7% of orders are dated before the customer's `join_date`, so join date was not used for tenure or cohorts.
- Cost is about 60% of discounted revenue in every discount tier, so margin is a constant 40%; profit per order is used instead, with a second scenario view that holds cost fixed.
- City and country conflict in the stores file; country is used.
- Unit price does not depend on pack size, so no price-per-gram analysis.
- First-order dates cluster at the start of the data window, so cohort retention was replaced by an order-frequency view.

The full log is in `Chocolate_Findings_and_DataQuality.xlsx` (sheet `Data_Quality_Log`).

## How to use

1. Put the five CSV files in one folder and update the file paths in each Power Query (Data → Queries & Connections → Edit → Advanced Editor).
2. Open the workbook in Excel for Windows (Microsoft 365 or 2016+ with Power Pivot enabled) and choose **Data → Refresh All**.
3. Use the slicers on the `Dashboard` sheet to filter every card and chart.

## Limitations

- The data looks synthetic: volume, products, stores and customers behave very uniformly, so many results are "no meaningful difference". These are reported as such.
- The what-if scenario depends on an assumed share of lost orders; it is a planning tool, not a prediction.
- Excel for Mac and Excel for the web can't build the Power Pivot data model.

## Tools

Excel (Power Query, Power Pivot, DAX, PivotCharts, slicers, Goal Seek, `FORECAST.ETS`).

## Author

Gireejesh Nilesh
