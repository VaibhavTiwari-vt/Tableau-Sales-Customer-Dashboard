# 📊 Sales & Customer Dashboard — Tableau

An interactive, dual-dashboard Tableau project built for stakeholders to analyse **sales performance** and **customer behaviour** with seamless navigation between the two views. The dashboards support year-based dynamic filtering and provide both a high-level summary and detailed drill-down views.

---

## Table of Contents

- [Project Goal](#project-goal)
- [Datasets](#datasets)
- [Data Preparation & Modelling](#data-preparation--modelling)
- [Calculated Fields & Parameters](#calculated-fields--parameters)
- [Sales Dashboard](#sales-dashboard)
- [Customer Dashboard](#customer-dashboard)
- [UX & Formatting](#ux--formatting)
- [Dashboard Assembly](#dashboard-assembly)
- [Project Workflow Summary](#project-workflow-summary)
- [How to Open](#how-to-open)
- [File Structure](#file-structure)

---

## Project Goal

Build a fully interactive Tableau dashboard that allows stakeholders to:

- Monitor **sales performance** (Sales, Profit, Quantity) across time, categories, and sub-categories.
- Understand **customer behaviour** (number of customers, orders per customer, top customers by profit).
- Compare **current year vs. previous year** at a glance.
- **Navigate** between Sales and Customer dashboards seamlessly.
- **Filter** data dynamically by year.

---

## Datasets

All four datasets are semicolon-delimited CSV files located in the `Datasets/` folder.

### `Customers.csv`
— 793 customers

| Field | Description |
|---|---|
| `Customer ID` | Unique customer identifier |
| `Customer Name` | Full name of the customer |

### `Location.csv`
— 632 location records

| Field | Description |
|---|---|
| `Postal Code` | ZIP / postal code |
| `City` | City name |
| `State` | US state |
| `Region` | Central, East, South, West |
| `Country/Region` | United States |

### `Orders.csv`
— 9,994 order line items spanning **2020–2023**

| Field | Description |
|---|---|
| `Row ID` | Unique row identifier |
| `Order ID` | Groups multiple line items per order |
| `Order Date` | Date order was placed (DD/MM/YYYY) |
| `Ship Date` | Date order was shipped |
| `Ship Mode` | First Class, Second Class, Standard Class, Same Day |
| `Customer ID` | FK → Customers |
| `Segment` | Consumer, Corporate, Home Office |
| `Postal Code` | FK → Location |
| `Product ID` | FK → Products |
| `Sales` | Revenue for the line item |
| `Quantity` | Units ordered |
| `Discount` | Discount applied |
| `Profit` | Profit for the line item |

### `Products.csv`
— 1,894 products across 3 categories and 17 sub-categories

| Field | Description |
|---|---|
| `Product ID` | Unique product identifier |
| `Category` | Furniture, Office Supplies, Technology |
| `Sub-Category` | 17 sub-categories (Phones, Chairs, Binders, etc.) |
| `Product Name` | Full product name |

**Categories & Sub-Categories:**

| Category | Sub-Categories |
|---|---|
| Furniture | Bookcases, Chairs, Furnishings, Tables |
| Office Supplies | Appliances, Art, Binders, Envelopes, Fasteners, Labels, Paper, Storage, Supplies |
| Technology | Accessories, Copiers, Machines, Phones |

---

## Data Preparation & Modelling

The initialisation followed a structured **CMBPCODN** approach:

1. **Connect** — Connected all 4 datasets in Tableau.
2. **Map** — Explored each dataset to understand dimensions and measures.
3. **Build** — Identified the fact table (`Orders`) and dimension tables (`Customers`, `Products`, `Location`).
4. **Primary Keys** — Verified the automatic relationships created by Tableau between tables.
5. **Check** — Reviewed data types for all fields.
6. **Organise** — Renamed tables and fields for better clarity.
7. **Design** — Prepared dashboard mocks before building any worksheets.
8. **Navigate** — Planned navigation flow between the two dashboards.

**Data model relationships:**

```
Orders (Fact)
  ├── Customer ID  →  Customers.Customer ID
  ├── Postal Code  →  Location.Postal Code
  └── Product ID   →  Products.Product ID
```

---

## Calculated Fields & Parameters

| Name | Type | Purpose |
|---|---|---|
| **CY (Current Year)** | Calculated Field | Filters/aggregates data for the selected year |
| **PY (Previous Year)** | Calculated Field | Filters/aggregates data for the year prior to selection |
| **Min Value** | Calculated Field | Marks the minimum monthly value on sparkline charts |
| **Max Value** | Calculated Field | Marks the maximum monthly value on sparkline charts |
| **% Difference vs PY** | Calculated Field | Calculates percentage change between CY and PY |
| **Year (Parameter)** | Parameter | Drives all CY/PY calculations dynamically — allows users to select any year |

---

## Sales Dashboard

### KPI Banners (3 BAN charts)
Three headline numbers at the top, each showing:
- **Current Year value**
- **Previous Year value**
- **% change vs. Previous Year** (with directional indicator)

| Metric | Description |
|---|---|
| Total Sales | Total revenue for the selected year |
| Total Profit | Total profit for the selected year |
| Total Quantity | Total units sold for the selected year |

### Sparkline Charts
One sparkline per KPI, plotted monthly, showing both CY and PY trend lines in a single chart. Min and max values are highlighted using the `Min Value` and `Max Value` calculated fields.

### Bar-in-Bar Chart — Sales by Category
Displays sales per product **category** (Furniture, Office Supplies, Technology) for both the current and previous year as a bar-in-bar comparison, making year-over-year changes immediately visible.

### Dual Trend Line Chart — Sales & Profit by Sub-Category
A single sheet with two trend lines (Sales and Profit) plotted across all 17 sub-categories. A **reference line** marks the average, and sub-categories are **colour-coded** to indicate whether they are above or below the average, helping stakeholders quickly identify outliers.

---

## Customer Dashboard

### KPI Banners (3 BAN charts)
Three headline numbers at the top with CY value, PY value, and % change:

| Metric | Description |
|---|---|
| Total Customers | Distinct customer count for the year |
| Total Orders | Number of orders placed in the year |
| Sales per Customer | Average revenue per customer |

### Sparkline Charts
Same sparkline approach as the Sales dashboard — monthly CY vs PY trend with min/max highlights for all three KPIs.

### Bar Chart — Orders per Customer Distribution
Visualises the distribution of how many orders individual customers have placed, giving a sense of purchase frequency and loyalty spread.

### Top 10 Customers by Profit (Table)
A ranked table showing the top 10 most profitable customers for the selected year. Uses Tableau's `INDEX()` function for ranking and dynamic sorting. Columns include customer name, number of orders, total sales, total profit, and last order date.

---

## UX & Formatting

Before assembling the dashboards, every worksheet was polished for a clean, professional look:

- Removed all **grid lines**.
- Removed **null value reminders** and **unwanted headers**.
- Built fully **custom tooltips** for every chart — tooltips display contextual detail (e.g. month, value, % change) relevant to each specific data point rather than showing default labels.
- Applied consistent **colour scheme** across both dashboards.
- Used above/below average colouring on the sub-category trend chart for instant visual insight.

---

## Dashboard Assembly

### Layout
Both dashboards were built using **horizontal and vertical containers** for precise, responsive layout control. All worksheets were inserted into containers one by one and aligned consistently.

### Navigation
**Navigation buttons** allow users to switch between the Sales Dashboard and the Customer Dashboard without going back to the Tableau start screen — the two dashboards behave like pages of a single app.

### Filters
A **Year filter** (driven by the Year parameter) is exposed on the dashboard, allowing users to switch the entire view — all KPIs, sparklines, and charts — to any year in the dataset (2020–2023) with a single click.

---

## Project Workflow Summary

```
Connect Datasets
      ↓
Explore & Understand Schema (fact vs. dimension tables)
      ↓
Verify Relationships & Data Types
      ↓
Rename Fields & Tables for Clarity
      ↓
Design Dashboard Mocks (paper/wireframe)
      ↓
Create Calculated Fields & Year Parameter
      ↓
Build Sales Dashboard Worksheets
  ├── 3 KPI BANs + Sparklines
  ├── Bar-in-Bar (Category Sales CY vs PY)
  └── Dual Trend + Reference Line (Sub-Category)
      ↓
Build Customer Dashboard Worksheets
  ├── 3 KPI BANs + Sparklines
  ├── Orders per Customer Bar Chart
  └── Top 10 Customers by Profit Table
      ↓
Format All Sheets (gridlines, tooltips, headers)
      ↓
Assemble Dashboards (containers, layout)
      ↓
Add Navigation Buttons & Year Filter
```

---

## How to Open

1. **Install Tableau Desktop** (version 2021.1 or later recommended).
2. Clone or download this repository.
3. Open the workbook file:
   ```
   Sales&Customer Dashboard.twb
   ```
4. If Tableau prompts for data source paths, re-point each source to the corresponding file in the `Datasets/` folder.
5. Use the **Year parameter** on either dashboard to switch between 2020, 2021, 2022, and 2023.
6. Use the **navigation buttons** to toggle between the Sales and Customer dashboards.

> **Note:** The `.twb` file is an XML-based workbook that references the data sources. The `Datasets/` folder must remain in the same relative location as the `.twb` file, or data source connections will need to be re-mapped on first open.

---

## Autho

```
Vaibhav Tiwari
├── Sales&Customer Dashboard.twb   # Tableau workbook
└── README.md
```
