# Pizza Sales Analysis 2015 | Power BI

A Power BI project that transforms raw pizza sales transactions into an interactive dashboard for understanding sales performance, demand patterns, product preferences, and ingredient requirements.

**Tools:** Power BI Desktop · Power Query · DAX

**Project completed:** Aug 2026 | **Data period:** 2015

**Table of Contents**

- [Problem](#problem)
- [Data Source](#data-source)
- [Steps](#steps)
  - [1. Data Preparation](#1-data-preparation)
  - [2. Data Modelling](#2-data-modelling)
  - [3. DAX Measures](#3-dax-measures)
  - [4. Dashboard Development](#4-dashboard-development)
  - [5. Explore the Project](#5-explore-the-project)
- [Result](#result)
  - [1. Overall Performance](#1-overall-performance)
  - [2. Key Findings](#2-key-findings)
  - [3. Business Recommendations](#3-business-recommendations)
  - [4. Limitations and Further Improvements](#4-limitations-and-further-improvements)

## Problem

The project uses a pizza manufacturing and distribution business scenario. Its raw sales file contains repeated product names, categories, sizes, prices, and ingredients, making it difficult to monitor performance consistently and translate transactions into operational decisions.

The objective is to build a structured Power BI model and dashboard that answer the following questions:

- How much revenue was generated, how many pizzas were sold, and what was the average order value?
- How did sales vary across days, weeks, and months, and when was order demand highest?
- Which pizza categories, individual pizzas, and sizes performed best or worst?
- Which ingredients were associated with the highest sales volumes?
- What can order size and price–volume patterns tell us about purchasing behaviour?

The analysis supports sales monitoring, staffing, menu review, and inventory planning.

## Data Source

The analysis uses the **2015 transactional dataset supplied with the project**. The project materials do not identify an external download source.

| File | Description |
| --- | --- |
| [data_pizza.csv](data_pizza.csv) | Raw sales data containing 48,620 transaction rows across 21,350 distinct orders. |
| [data_dictionary.xlsx](data_dictionary.xlsx) | Supporting definitions of the dataset fields. |
| [Requirements.txt](Requirements.txt) | Business context, analysis requirements, and expected deliverables. |

Each transaction row represents a pizza line item within an order; its quantity can exceed one.

The dataset contains ten fields:

| Field group | Columns |
| --- | --- |
| Order details | `order_id`, `date`, `time` |
| Product details | `pizza_id`, `name`, `category`, `size` |
| Sales details | `quantity`, `price` |
| Ingredient details | `ingredients` |

Revenue is calculated as **quantity × unit price**.

The dataset covers four categories—Classic, Supreme, Chicken, and Veggie—and five sizes: S, M, L, XL, and XXL.

## Steps

### 1. Data Preparation

- Imported the raw CSV into Power BI and used **Power Query** for transformation.
- Checked column quality, distribution, profiles, and data types. The report records no major missing-value or error issues.
- Trimmed and cleaned text fields to support consistent grouping.
- Created a size-sorting field so visuals display **S → M → L → XL → XXL**.
- Split comma-separated ingredient values to support analysis of individual ingredients.
- Developed preliminary order-level quantity and single/multi-item classification logic. The associated visual requires further validation, as noted under [limitations](#4-limitations-and-further-improvements).

### 2. Data Modelling

Restructured the denormalised source into a **star schema**, separating sales transactions from descriptive dimension data.

- Retained one pizza line item per row in `FactSales`.
- Stored product attributes and prices in dimension tables, including `DimPizza`.
- Used fact–dimension relationships to support consistent filtering and aggregation.
- Used date attributes for daily, weekly, monthly, and weekday analysis, and order hours for demand analysis.

### 3. DAX Measures

Created explicit measures for the main business KPIs.

The following formulas are documented in the project report:

```dax
Total Revenue =
SUMX(
    FactSales,
    FactSales[quantity] * RELATED(DimPizza[price])
)

Total Quantity Sold =
SUM(FactSales[quantity])

Total Orders =
DISTINCTCOUNT(FactSales[order_id])

Average Order Value =
DIVIDE([Total Revenue], [Total Orders])

Average Pizzas per Order =
DIVIDE([Total Quantity Sold], [Total Orders])

Average Pizza Price =
AVERAGE(DimPizza[price])
```

**Measure interpretation:**

- **Total Revenue:** Revenue calculated using pizza quantities and their corresponding prices.
- **Total Quantity Sold:** Total number of pizzas sold, accounting for quantity per transaction row.
- **Total Orders:** Number of unique orders, calculated using distinct `order_id` values.
- **Average Order Value:** Average revenue generated per order.
- **Average Pizzas per Order:** Average number of pizzas purchased per order.
- **Average Pizza Price:** Arithmetic mean of prices recorded in `DimPizza`. This is not a quantity-weighted average selling price.

### 4. Dashboard Development

Built four analytical pages with slicers, page navigation, and home buttons.

| Dashboard page | Main content |
| --- | --- |
| **Executive Overview** | Five KPI cards, monthly revenue, and category revenue share. |
| **Sales Trends** | Daily and weekly trends, revenue anomalies, and a weekday/hour order heatmap. |
| **Product Analysis** | Category rankings, top/bottom five pizzas, and size distribution, with Revenue and Quantity selection buttons. |
| **Ingredient & Order Analysis** | Ingredient rankings, average price versus sales volume, pizzas per order, and the order-type visual. |

The dashboard supports interactive exploration across sales performance, product preferences, ordering patterns, and ingredient-related demand.

**Note:** The order-type visual requires correction before its single-item and multi-item results can be used for decision-making. See [Limitations and Further Improvements](#4-limitations-and-further-improvements).

### 5. Explore the Project

1. Download or clone this repository.
2. Open [Pizza_Sales_Analysis_2015.pbix](Pizza_Sales_Analysis_2015.pbix) in Power BI Desktop.
3. If refreshing the data, update the source path to your local `data_pizza.csv` in Power Query or Data source settings, then apply the changes and refresh.
4. Use the dashboard navigation, slicers, and Revenue/Quantity buttons to explore the analysis.

For a static view or detailed explanation, open:

- [Dashboard PDF](Power_BI_Dashboard_Pizza_Sales_Analysis_2015.pdf)
- [Analysis Report](Power_BI_Pizza_Sales_Analysis_report.pdf)
- [Dashboard Presentation](Pizza_Sales%20Analysis_Dashboard.pptx)

## Result

### 1. Overall Performance

The analysis produced the following full-year performance results for 2015:

| KPI | 2015 result |
| --- | ---: |
| Total Revenue | $817,860.05 |
| Total Orders | 21,350 |
| Total Quantity Sold | 49,574 pizzas |
| Average Order Value | $38.31 |
| Average Pizzas per Order | 2.32 |

The results provide an overall view of sales volume, revenue, and customer purchasing patterns.

### 2. Key Findings

**Category performance**

| Category | Quantity sold | Revenue |
| --- | ---: | ---: |
| Classic | 14,888 | $220,053.10 |
| Supreme | 11,987 | $208,197.00 |
| Chicken | 11,050 | $195,919.50 |
| Veggie | 11,649 | $193,690.45 |

**Key analytical findings:**

- **Classic led both revenue and quantity**, contributing approximately 26.91% of total revenue.

- **Chicken generated more revenue than Veggie despite selling fewer pizzas**, showing why revenue and quantity should be evaluated together.

- **Friday was the busiest weekday**, with 3,538 orders; Sunday had the fewest, with 2,624.

- Demand concentrated around **lunch and early evening**. The 12:00 hour recorded 2,520 orders across the year.

- **July had the strongest monthly revenue**, with weaker performance around September and October.

- **The Classic Deluxe Pizza led quantity sold**, while **The Brie Carre Pizza** had the lowest quantity, at 490 pizzas.

- **Garlic and tomatoes** were associated with the highest pizza sales volumes. These figures describe pizzas containing each ingredient, rather than ingredient weight or procurement quantities.

These findings highlight how customer demand varies across time, product categories, individual pizzas, and ingredients.

### 3. Business Recommendations

Based on the descriptive analysis, the following recommendations were proposed:

- **Staffing and preparation:** Align staffing and preparation schedules with Friday demand and lunch/evening peaks.

- **Product availability:** Protect the availability of Classic pizzas and ingredients shared by high-volume products.

- **Menu optimisation:** Review low-selling pizzas using sales, ingredient costs, and menu value before deciding whether to promote, redesign, or remove them.

- **Promotional planning:** Test targeted promotions during weaker periods and measure their results.

- **Pricing analysis:** Evaluate pricing at pizza and size level. Category-level comparisons alone do not establish how price changes affect demand.

These are recommendations derived from descriptive analysis. The project does not measure the impact of implementing them.

### 4. Limitations and Further Improvements

**Order-type classification**

The existing single-item/multi-item donut chart displays approximately 48.62K for both categories, which is inconsistent with the dataset's 21,350 distinct orders.

These values should not be interpreted as valid counts of single-item and multi-item orders.

A corrected analysis should first define whether order size refers to total pizza quantity or the number of distinct pizza line items. It should then aggregate records at the order level and classify each distinct order using that definition.

The current order-type results are excluded from the project's conclusions until this calculation is validated.

**Order timing**

The project requirements request an average interval between consecutive orders, but the report does not document a validated result.

This remains an area for further verification. An interval between order placements is not equivalent to food preparation or delivery time.

**Missing business context**

The dataset does not include:

- Product and ingredient costs
- Profit margins
- Customer demographics
- Promotional activities
- Inventory levels
- Staffing information

Therefore, revenue cannot establish profitability, and unusual sales patterns cannot be attributed to specific causes using this dataset alone.

**Further development**

Future improvements could include:

- Correcting and validating order-level classification.
- Adding cost information to evaluate profitability.
- Integrating inventory and staffing data.
- Investigating product-level relationships between prices and sales volumes.
- Evaluating the effectiveness of operational recommendations using additional business data.

These improvements would strengthen the dashboard's ability to support business decision-making.

---

[Back to top](#pizza-sales-analysis-2015--power-bi)
