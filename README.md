# Microsoft Power BI — Data Technician Bootcamp

Business intelligence reports built in **Power BI Desktop** and published to the **Power BI Service**, completed across the Data Technician bootcamp. The work covers the full pipeline — cleaning and loading data, building a semantic model, writing DAX measures, and designing interactive multi-page reports on retail and sales data.

Delivered through the Microsoft **PL-300** (Data Analyst Associate) and **DP-900** (Azure Data Fundamentals) hosted lab series.

---

## Overview

| | |
|---|---|
| **Tools** | Power BI Desktop, Power BI Service, Power Query, DAX |
| **Datasets** | Adventure Works reseller sales, retail orders (customers / orders / products) |
| **Labs completed** | PL-300 Lab 02, Lab 04, Lab 08; DP-900 Lab 06 |
| **Deliverables** | Semantic model, DAX measures, published multi-page sales report |

---

## Skills demonstrated

| Area | Techniques |
|---|---|
| **Data preparation** | Power Query transformations, column typing, applied-step pipelines, loading a multi-table model |
| **Data modelling** | Semantic model across seven related tables, relationships, home tables for measures |
| **DAX** | Measures, calculated columns, `DIVIDE` for safe division, percentage formatting |
| **Report design** | Slicers, cross-filtering, hierarchy drill-down, multi-page navigation, full-screen mode |
| **Visuals** | Matrix with drill-down, bar and column charts, pie charts, map visuals |
| **Publishing** | Publishing from Desktop to a Power BI Service workspace |

---

## PL-300 Lab 02 — Clean, transform and load data

Shaping source data in **Power Query** before it reaches the model, then loading a seven-table Adventure Works reseller sales model into Power BI Desktop:

`Product` · `Region` · `Reseller` · `Sales` · `Salesperson` · `SalespersonRegion` · `Targets`

This is the stage where data quality is decided. A column left as text instead of a numeric type loads without complaint and then breaks every measure written against it two labs later — so typing and transformation happen here, not downstream.

**Status:** 100% completed.

---

## PL-300 Lab 04 — DAX calculations in semantic models

Writing **DAX measures** against the loaded semantic model to compare actual sales against target by salesperson:

```dax
Variance        = [Sum of Sales] - [Target]
Variance Margin = DIVIDE([Variance], [Target])
```

`DIVIDE` was used in place of the `/` operator because it returns blank rather than an error when the denominator is zero or blank — one missing target would otherwise break the whole visual. The measure was assigned `Targets` as its home table and formatted as a **percentage to two decimal places**.

The resulting table gives a salesperson-level view of sales against target:

| Salesperson | Sum of Sales | Target | Variance | Variance Margin |
|---|---|---|---|---|
| Brian Welcker | $77,548,570 | $221,700,000 | ($144,151,430) | −65.02% |
| Pamela Ansman-Wolfe | $30,005,939 | $53,850,000 | ($23,844,061) | −44.28% |
| Linda Mitchell | $25,634,503 | $40,850,000 | ($15,215,497) | −37.25% |

Every salesperson shows a negative variance at this stage — a reminder that a measure is only as meaningful as the filter context around it. The table has no time filter applied, so it compares partial-period sales against a full-period target. The correct reading is not "nobody is hitting target" but "this view is not yet filtered to a comparable period", which is exactly what the report-design lab addresses.

**Status:** 100% completed.

---

## PL-300 Lab 08 — Design Power BI reports

Building a multi-page interactive report and publishing it to the **Power BI Service**.

### Slicers and interactivity

- A **`Region` slicer** with ten values — Australia, Canada, Central, France, Germany, Northeast, Northwest, Southeast, Southwest, United Kingdom — driving the page
- **Cross-filtering** — selecting a mark in one visual filters the other visuals on the page
- **Full-screen mode** with page navigation controls, tested before publishing

### Hierarchy drill-down

A matrix expanding through a fiscal date hierarchy — fiscal year → quarter → month:

| Period | Orders | Sum of Sales | Sum of Cost | Profit Margin |
|---|---|---|---|---|
| FY2018 | 246 | $5,141,307 | $5,095,585 | 0.89% |
| FY2019 | 393 | $8,442,071 | $8,098,310 | 4.07% |
| FY2020 | 490 | $9,316,666 | $9,365,501 | **−0.52%** |
| **Total** | **1,129** | **$22,900,045** | **$22,559,396** | **1.49%** |

The finding the report surfaces: **order volume grew every year while profit margin collapsed** — from 4.07% in FY2019 to −0.52% in FY2020, meaning FY2020 sold more and lost money doing it. Cost grew faster than sales. This is precisely the question a `Region` slicer exists to answer next: which regions drove the loss, and is it concentrated or general?

### Report structure

Three pages with navigation — `Overview`, `Profit` and `My Performance` — published to a Power BI Service workspace and accessed at `app.powerbi.com`.

**Status:** 100% completed.

---

## DP-900 Lab 06 — Fundamentals of data visualisation with Power BI

A retail sales report built over a three-table model — `customers` (City, CountryRegion, CustomerID, Name, PostalCode), `orders` (CustomerID, OrderDate, OrderItemID, ProductID, Quantity, Revenue) and `products` (Category, ProductID, ProductName).

**Report:** `Sales Report` — combining

- A **pie chart** of `Sum of Quantity by Category` across product categories (Touring, Jerseys, Road, Mountain, Helmets, Vests, Pedals)
- A **column chart** of `Sum of Revenue by Category` at product level — Bike Racks, Bottles and Cages, Bottom Brackets, Brakes, Caps, Chains, Cleaners, Cranksets, Derailleurs
- A **map visual** keyed on `City` as the location field with `Sum of Revenue` as bubble size
- Page-level filters on `City` and `Sum of Revenue`

**Note on the map:** the map and filled map visuals were **disabled in the hosted lab environment** (Options → Global → Security), so the map rendered as a placeholder rather than a live visual. The field wells were configured correctly — `City` on Location, `Sum of Revenue` on Bubble size — but the visual itself could not display. This is an environment restriction, not an incomplete configuration.

**Status:** 93% completed (blocked at the map-rendering step by the environment restriction above).

---

## How this compares to Tableau

The same week covered Tableau, which makes the contrast useful:

| Tableau | Power BI |
|---|---|
| Dashboard filters, "Show Filter" | Slicers |
| Calculated fields | DAX measures and calculated columns |
| Quick Table Calculations | DAX with filter context |
| Dashboard | Multi-page report |
| Data source connection and field roles | Power Query transformation and semantic model |
| Tableau Public | Power BI Service workspace |

The largest difference is *where the modelling happens*. Tableau does much of the shaping at visualisation time, so a single worksheet can be productive within minutes. Power BI expects a typed semantic model to exist first, with measures written against it — slower to start, but the model is reusable across every report built on it and the measures stay consistent between pages.

---

## Repository contents

```
.
├── README.md
└── screenshots/
    ├── PL300_Lab02_Clean_Transform_Load.png
    ├── PL300_Lab04_DAX_Calculations.png
    ├── PL300_Lab08_Design_Reports.png
    └── DP900_Lab06_Sales_Report_Visualisation.png
```

The labs ran in a hosted lab environment (XtremeLabs lab VMs), so the completed work is evidenced by screenshots rather than `.pbix` files — the lab starter files contain Microsoft sample data and are not redistributed here.

---

## Key takeaways

- Model first, visualise second — Power BI rewards front-loading the transformation and typing work, and punishes skipping it
- `DIVIDE` over `/` in DAX: defensive division costs nothing and stops one blank denominator taking down a visual
- A measure is meaningless without its filter context — a variance that looks catastrophic is often a period mismatch, not a performance problem
- Volume up and margin down is the finding worth surfacing; a report that only shows revenue growth hides it
- Slicers turn a report into a tool — the point is not the default view but the question the viewer can ask next
- Know your environment's limits: a visual disabled by lab security policy is a configuration to document, not a failure to hide
