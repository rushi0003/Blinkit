# BlinkIT Quick Commerce Sales & Operations Analytics | Excel Dashboard

![Excel Dashboard Preview](BlinkIT%20dashboard.png)

> **Portfolio project | Microsoft Excel • Power Query • PivotTables • PivotCharts • Slicers**  
> **Data:** Synthetic Blinkit-style order data (not official Blinkit data) | **Period:** January–December 2025

## Project overview

This end-to-end Excel analytics project explores quick-commerce order performance across six Indian cities. It converts a 1,000-order demo dataset into an interactive dashboard covering sales, discounts, product categories, geographic performance, and order cancellations. The objective is to turn transactional records into clear, decision-oriented business insights.

## Business objectives

- Monitor completed-order revenue and overall order activity.
- Compare city-level sales to identify stronger and weaker markets.
- Rank product categories by revenue contribution.
- Quantify discounts and compare gross sales with net realized sales.
- Examine cancelled orders by product category to identify potential operational follow-ups.
- Provide interactive city and category filters for self-service analysis.

## Dataset

**Source:** Synthetic demonstration dataset created for learning and portfolio use; no proprietary or real Blinkit customer/order information is used.

| Property | Details |
| --- | --- |
| Records | 1,000 orders |
| Original fields | 15 |
| Date range | 2025-01-01 to 2025-12-31 |
| Cities | Bengaluru, Chennai, Delhi, Hyderabad, Mumbai, Pune |
| Granularity | One row per order; one product per order |
| Main dimensions | Date, city, dark store area, customer type, product/category, payment method, order status |
| Main measures | Quantity, unit price, discount %, delivery time |

**Original columns:** `Order_ID`, `Order_Date`, `Order_Time`, `City`, `Dark_Store_Area`, `Customer_ID`, `Customer_Type`, `Product_Name`, `Product_Category`, `Quantity`, `Unit_Price_INR`, `Discount_Pct`, `Payment_Method`, `Order_Status`, `Delivery_Time_Min`.

## Data preparation & quality assessment

The workflow is designed around Excel tables and Power Query for reproducible data preparation. The original CSV was independently checked for the following baseline quality indicators:

| Check | Observed result |
| --- | --- |
| Order IDs | 1,000 unique IDs across 1,000 rows |
| Exact duplicate rows | 0 |
| Missing values | 91 blank `Delivery_Time_Min` values; no other missing fields |
| Missing delivery-time context | All 91 blank delivery times correspond to cancelled orders |
| Date coverage | Full calendar-year range, January–December 2025 |

### Excel / Power Query cleaning workflow

1. Import the CSV through **Data → Get Data → From Text/CSV**.
2. Promote headers and assign appropriate types (date, time, text, whole number, decimal).
3. Inspect column quality, distribution, and missing values using Power Query profiling.
4. Check duplicate `Order_ID` values and duplicate records; retain valid unique transactions.
5. Verify that `Quantity` and `Unit_Price_INR` are non-negative and `Discount_Pct` falls within 0–100.
6. Preserve blank delivery times for cancelled orders rather than replacing them with zero.
7. Add calculated revenue fields, load the prepared table into Excel, and refresh downstream PivotTables.

**Documentation note:** The checks above describe independently verified source-data properties; the transformation steps describe the recommended reproducible workflow. Only claim a specific Power Query transformation was executed if it is present in your submitted workbook.

## Data model and KPI definitions

The dataset's `Unit_Price_INR` is a **per-unit price**; `Discount_Pct` is a percentage, not a rupee amount. Revenue calculations use:

```text
Gross_Sale       = Quantity × Unit_Price_INR
Discount_Amount  = Gross_Sale × Discount_Pct / 100
Net_Order_Value  = Gross_Sale − Discount_Amount
Total_Sales      = IF(Order_Status = "Delivered", Net_Order_Value, 0)
```

> **Revenue convention:** `Total_Sales` means realized sales from **delivered** orders only. Cancelled and returned orders contribute zero to this KPI. This distinction matters when comparing source-level order value with dashboard revenue.

| Dashboard KPI | Verified value | Definition |
| --- | ---: | --- |
| Total orders | **1,000** | Count of unique order IDs across all statuses |
| Gross sales (delivered orders) | **₹1,94,746.00** | Sum of gross order value for delivered orders |
| Discounts (delivered orders) | **₹13,364.25** | Sum of discount amounts for delivered orders |
| Total sales revenue | **₹1,81,381.75** | Gross sales minus discounts for delivered orders |
| Delivered orders | **870** | Orders with status `Delivered` |
| Cancelled orders | **91** | Orders with status `Cancelled` |
| Returned orders | **39** | Orders with status `Returned` |

The dashboard's revenue reconciles: **₹1,94,746.00 − ₹13,364.25 = ₹1,81,381.75**.

## PivotTable analysis

The analysis layer uses PivotTables / PivotCharts to summarize:

- **City-wise revenue:** city on rows; delivered-order sales, gross sales, and discounts as values.
- **Category-wise performance:** product category on rows; revenue and discount contribution as values.
- **Order cancellation mix:** category by order status, displayed as a percentage of each status total.
- **City revenue share:** revenue by city for relative market contribution.
- **Top category ranking:** descending order of delivered-order revenue.

Slicers shown in the dashboard include **City** and **Product Category**. Connect slicers to compatible PivotTables via **Slicer → Report Connections** to keep visuals synchronized.

## Dashboard walkthrough

The interactive Excel dashboard presents:

1. **KPI cards:** Total orders and delivered-order sales revenue.
2. **City-wise grouped bars:** Revenue, gross sales, and discounts by city.
3. **Product-category grouped bars:** Revenue, gross sales, and discounts by category.
4. **Category-wise cancellation distribution:** Percentage of cancelled orders across categories.
5. **Revenue rankings:** Top product categories and cities.
6. **City contribution pie:** Relative contribution of each city to total delivered-order revenue.
7. **Interactive slicers:** Filter visuals by city and product category.

![BlinkIT dashboard](BlinkIT%20dashboard.png)

## Key business insights

The following observations are derived from the dashboard and reconciled against the original demo data, applying the delivered-order revenue definition.

| Finding | Evidence | Business interpretation |
| --- | --- | --- |
| **Mumbai leads revenue** | ₹48,016.05 | Largest city contribution in this sample |
| **Pune ranks second** | ₹41,901.20 | Another important revenue market |
| **Staples leads product categories** | ₹44,422.70 | Core everyday essentials drive the largest category revenue |
| **Beverages is the second-largest category** | ₹24,497.50 | Strong complementary category |
| **Discounts reduce delivered-order gross sales** | ₹13,364.25 | Approximately 6.86% of delivered-order gross sales |
| **Cancellations merit attention** | 91 of 1,000 orders (9.1%) | Investigate causes before recommending specific fixes |
| **Snacks have a notable share of cancellations** | 19.78% of cancelled orders | Prioritize further investigation; this is a share of cancellations, **not** the category cancellation rate |

**Important interpretation:** A large *share of cancelled orders* does not by itself prove that a category has the highest *cancellation rate*. For the latter, divide cancelled orders in that category by **all orders in the same category**.

## Business recommendations

1. **Protect strong markets:** Track demand, inventory availability, and service levels in Mumbai and Pune to sustain delivered sales.
2. **Optimize core categories:** Monitor Staples and Beverages assortment and availability because they make substantial contributions to revenue.
3. **Review discount effectiveness:** Compare discounted and non-discounted order values, but do not infer causal discount impact from this descriptive dataset.
4. **Investigate cancellations:** Break down cancelled orders by category, city, and store area; validate whether operational constraints explain observed patterns.
5. **Expand performance monitoring:** Add monthly trends, delivery-time distribution, and order-status KPIs in future dashboard iterations.

## Tools and skills demonstrated

`Microsoft Excel` · `Power Query` · `Data Cleaning` · `Data Quality Validation` · `Calculated Columns` · `PivotTables` · `PivotCharts` · `Slicers` · `KPI Reporting` · `Data Visualization` · `Business Analysis` · `Dashboard Design`

## Repository structure

```text
BlinkIT-Excel-Sales-Analytics/
├── README.md
├── BlinkIT dashboard.png
├── data/
│   └── Blinkit_Quick_Commerce_Analytics_1000_Rows.csv
└── excel/
    └── BlinkIT_Excel_Analytics.xlsx
```

*The workbook name above is a suggested filename; rename it to match your actual uploaded Excel file.*

## How to explore the project

1. Download and open the Excel workbook in desktop Microsoft Excel.
2. Open the dashboard worksheet and review the KPI cards.
3. Use **City** and **Product Category** slicers to explore different segments.
4. Inspect the underlying PivotTables and calculated fields for metric definitions.
5. Refresh data connections / PivotTables if you replace the source dataset.

## Limitations

- The dataset is **synthetic**, so findings illustrate analytical methods rather than actual Blinkit business performance.
- It contains 1,000 orders; results are descriptive and should not be generalized to the full quick-commerce market.
- Each row represents one product/order, so the analysis does not model multi-product baskets.
- No marketing spend, inventory history, profit, delivery distance, or cancellation reason is available; causal and profitability conclusions cannot be drawn.
- The screenshot establishes the dashboard presentation; workbook-level Power Query steps and slicer connectivity should be validated in the final `.xlsx` file.

## Portfolio summary

Built an interactive Excel dashboard on **1,000 synthetic quick-commerce orders**, using data validation, calculated revenue metrics, PivotTables, PivotCharts, and slicers to analyze **₹1.81 lakh in delivered-order sales**, city and product-category performance, discount impact, and cancellations. Translated descriptive results into actionable questions for category and operations teams.

---

**Disclaimer:** This is an independent educational portfolio project and is not affiliated with, endorsed by, or based on internal data from Blinkit.
